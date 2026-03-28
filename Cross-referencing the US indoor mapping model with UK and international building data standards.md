# Cross-referencing the US indoor mapping model with UK and international building data standards

**The UK Ordnance Survey ecosystem and international standards provide rich but structurally different equivalents to every layer of the US Public Safety Indoor Mapping GIS Data Model.** The most important finding is that the UK system separates building geometry (OS NGD Buildings), addressing (OS NGD Addresses / AddressBase Premium), and land use (OS NGD Land Use) into distinct but cross-linked themes — unlike the US model's integrated 6-layer approach. Vertical/floor-level information exists across multiple UK products but is stored as attributes rather than discrete floor-plan layers, creating a fundamental architectural difference that any internationalised model must bridge. The OSID (a 36-character GUID) and UPRN (a persistent numeric property identifier) together serve the role that NGUID plays in the US model, while the BS 7666 SAO/PAO addressing structure provides a more granular sub-building address decomposition than the NENA flat address fields.

---

## The six US layers mapped to UK and international equivalents

The US Public Safety Indoor Mapping model defines six spatial layers: Sites, Facilities, Levels, Details, Units, and Points. Each has direct or composite equivalents across UK and international standards. The table below provides the primary mapping.

| US Layer | Geometry | OS NGD Equivalent | OS MasterMap Equivalent | IFC Equivalent | CityGML Equivalent | INSPIRE Equivalent | IndoorGML Equivalent |
|---|---|---|---|---|---|---|---|
| **Sites** | Polygon | **Land Use → Site** (MultiPolygon) | TopographicArea (descriptiveGroup = site context) | **IfcSite** | City model context | Dataset/project level | — |
| **Facilities** | Polygon | **Buildings → Building** (Polygon) | TopographicArea (descriptiveGroup = "Building") | **IfcBuilding** | **Building / BuildingPart** | **Building / BuildingPart** | — (assumed) |
| **Levels** | Polygon | No discrete layer; `numberoffloors`, `floorlevel`, `lowestfloorlevel`, `highestfloorlevel` attributes | `physicalLevel` attribute (integer) | **IfcBuildingStorey** | **Storey** (CityGML 3.0 only) | `numberOfFloors` attribute | SpaceLayer + storey property (v2.0) |
| **Details** | Line | **Buildings → Building Line** (LineString) | TopographicLine (descriptiveGroup = "Building") | **IfcWall, IfcDoor, IfcWindow** | BoundarySurface, Door, Window | BoundarySurface, Opening (3D extended) | **CellSpaceBoundary** |
| **Units** | Polygon | **Addresses → Built Address** (Point only) + classification | No polygon equivalent | **IfcSpace** | **Room** (v2.0 LOD4) / **BuildingRoom** (v3.0) | **BuildingUnit** / Room (extended) | **CellSpace / GeneralSpace** |
| **Points** | Point | **Buildings → Building Access Location** (Point) | TopographicPoint | IfcAnnotation (non-native) | — | — | **State** (navigation node) |

**The critical gap** is the Levels layer. Only IFC provides an explicit storey entity (`IfcBuildingStorey`) with its own geometry and elevation. The UK OS system stores floor counts as integer attributes on buildings and floor levels as string attributes on addresses, but has **no discrete polygon layer representing individual floor plans**. CityGML 3.0 introduced a `Storey` class, but CityGML 2.0 and INSPIRE lack explicit storey entities entirely. An internationalised model must therefore define the Levels layer as a new construct that bridges these gaps.

The Units layer also presents a mapping challenge. The US model represents rooms and spaces as polygons, but the UK address system represents sub-building units (flats, floors, rooms) only as **point geometries with structured text attributes**. Polygon room/unit data in the UK context exists only in BIM (IFC IfcSpace) or CityGML LOD4 models, not in national mapping products.

---

## Identifier systems across standards

Every standard uses a different identifier architecture. The US model's NGUID format (used for SITE_ID, FACILITY_ID, LEVEL_ID, etc.) must map to multiple identifier types depending on context.

| US Model ID | Format | UK/International Equivalent | Format | Scope |
|---|---|---|---|---|
| SITE_ID | NGUID | **OSID** (Land Use Site) | 36-char GUID | OS NGD site features |
| FACILITY_ID | NGUID | **OSID** (Building) + **TOID** (legacy) | GUID / 'osgb' + 13-16 digits | OS NGD buildings; TOID on BuildingPart only |
| LEVEL_ID | NGUID | No equivalent | — | No discrete floor entity in UK data |
| UNIT_ID | NGUID | **UPRN** | Numeric, up to 12 digits | Every addressable property in GB |
| DETAIL_ID | NGUID | **OSID** (Building Line) | 36-char GUID | OS NGD building lines |
| POINT_ID | NGUID | **OSID** (Building Access Location) | 36-char GUID | OS NGD access points |
| — | — | **USRN** | Numeric, 8 digits | Every street in GB |
| — | — | IFC **GlobalId** | 22-char base-64 encoded UUID | Every IFC entity |
| — | — | INSPIRE **inspireId** | namespace + localId + versionId | EU INSPIRE features |
| — | — | CityGML **gml:id** | String (document-local) | GML features |

The **UPRN** deserves special attention. It is the single most important identifier in UK property data — a persistent, never-reused numeric reference assigned to every addressable location. The UPRN connects addressing data to building geometry via cross-reference tables. In the US model, the closest equivalent is the combination of UNIT_ID and the addressing fields, but the UPRN is more universal: it links across all government systems, utilities, and emergency services. Any internationalised model should treat UPRN as a first-class field alongside NGUID-format identifiers.

The **PARENT_UPRN** field in AddressBase Premium establishes hierarchical relationships (e.g., a block of flats has a parent UPRN, each flat has a child UPRN). This maps conceptually to the FACILITY_ID → UNIT_ID relationship in the US model, though the UK system can nest to arbitrary depth.

---

## Address field mapping: NENA vs BS 7666 / OS NGD

The US model uses NENA-style address fields with directional prefixes and suffixes typical of North American addressing. The UK uses the BS 7666 SAO/PAO structure, which handles sub-building addressing natively through a different decomposition.

| US NENA Field | Purpose | UK AddressBase Premium (BS 7666 LPI) | UK OS NGD Addresses |
|---|---|---|---|
| **AddNum** | Street number | **PAO_START_NUMBER** + PAO_START_SUFFIX | **number** |
| **AddNumSuffix** | Number suffix (A, B, etc.) | **PAO_START_SUFFIX** | Included in `number` field |
| **PreDir** | Directional prefix (N, SW, etc.) | Not used in UK addressing | Not applicable |
| **StrName** | Street name | Part of **STREET_DESCRIPTION** (via USRN → Street Descriptor) | **streetname** |
| **PostType** | Street suffix (St, Ave, Rd) | Embedded in STREET_DESCRIPTION (no separate field) | Embedded in `streetname` |
| **PostDir** | Directional suffix | Not used in UK addressing | Not applicable |
| **SubAddress** | Unit/suite/apt identifier | **SAO_TEXT** + **SAO_START_NUMBER** + SAO_START_SUFFIX | **subname** |
| **Building** | Building name | **PAO_TEXT** | **name** |
| **IncMuni** | City/municipality | **TOWN_NAME** (Street Descriptor) | **townname** |
| **County** | County | **ADMINISTRATIVE_AREA** (Street Descriptor) | Not directly included; derivable from geography |
| **State** | State/province | Not applicable (GB is small enough) | **country** (England, Scotland, Wales) |
| **Country** | Country | **COUNTRY** code (E/S/W/N) | **country** |
| **PostalCode** | ZIP/postal code | **POSTCODE_LOCATOR** (BLPU) | **postcode** |
| — | Locality/suburb | **LOCALITY** (Street Descriptor) | **locality** |
| — | Floor level | **LEVEL** (LPI field, free text up to 30 chars) | **floorlevel** (structured, e.g., "-1, 0, 1, 2, 3") |

**Key structural differences**: UK addresses have no directional components (PreDir, PostDir) because UK street naming does not use compass directions as systematic elements. Street types (Road, Street, Avenue, Lane, Close, Crescent, etc.) are embedded in the street name string rather than parsed into a separate field. The UK system is fundamentally **name-based** rather than **grid-based**, reflecting centuries of organic development rather than planned grid layouts.

The **SAO/PAO decomposition** is the UK's most distinctive addressing feature. The SAO (Secondary Addressable Object) identifies a sub-division within a building — a flat, floor, room, or named unit — using five structured fields: `SAO_START_NUMBER`, `SAO_START_SUFFIX`, `SAO_END_NUMBER`, `SAO_END_SUFFIX`, and `SAO_TEXT`. The PAO (Primary Addressable Object) identifies the building itself using the same five-field structure. This is more granular than the US model's single `SubAddress` field and maps naturally to indoor unit identification.

| SAO/PAO Example | SAO Fields | PAO Fields | Full Address |
|---|---|---|---|
| Flat 3, 10 High Street | SAO_START_NUMBER=3, SAO_TEXT='FLAT' | PAO_START_NUMBER=10 | Flat 3, 10 High Street |
| First Floor, Rose Cottage | SAO_TEXT='FIRST FLOOR' | PAO_TEXT='ROSE COTTAGE' | First Floor, Rose Cottage |
| Units 1-6, Building A | SAO_START_NUMBER=1, SAO_END_NUMBER=6 | PAO_TEXT='BUILDING A' | Units 1-6, Building A |

The OS NGD Addresses theme simplifies this into just `subname` (≈ SAO_TEXT), `name` (≈ PAO_TEXT), and `number` (≈ PAO_START_NUMBER), trading granularity for usability.

---

## How height, elevation, and vertical order are handled

The US model stores vertical information through `LEVEL_NUMBER`, `VERTICAL_ORDER`, `HEIGHT_RELATIVE`, and `HAE_FLOOR` (Height Above Ellipsoid). UK and international systems distribute this information differently.

**OS NGD Buildings height attributes** (on both Building and BuildingPart features):

| OS NGD Attribute | US Model Equivalent | Description |
|---|---|---|
| `height_absolutemin_m` | Related to HAE_FLOOR (ground level) | Height above Ordnance Datum Newlyn of ground surface |
| `height_absoluteroofbase_m` | — | Height above ODN where roof meets walls |
| `height_absolutemax_m` | — | Height above ODN of highest point (chimneys, etc.) |
| `height_relativeroofbase_m` | HEIGHT_RELATIVE (approximate) | Ground to roof base, in metres |
| `height_relativemax_m` | — | Ground to highest point, in metres |
| `height_confidencelevel` | — | Qualitative: High, Moderate, Low, Incomplete |
| `numberoffloors` | Related to LEVEL_NUMBER range | Integer count of occupiable floors at/above ground |
| `basementpresence` | Related to negative LEVEL_NUMBERs | Present / Not Present / Unknown |

**Critical datum difference**: The US model uses **HAE (Height Above Ellipsoid)** referenced to WGS84, while the UK uses **Ordnance Datum Newlyn (ODN)**, a geoid-based orthometric height. The difference between WGS84 ellipsoid height and ODN orthometric height in the UK varies from approximately **48 to 56 metres** depending on location. Any cross-referencing must apply an appropriate geoid separation model (OSGM15 in the UK).

**OS MasterMap physicalLevel** encodes vertical position using an integer scheme where **50 = ground level**, values below 50 indicate underground features, and values above 50 indicate overhead structures. This maps conceptually to VERTICAL_ORDER but uses a different baseline (50 vs 0).

| physicalLevel Value | Meaning | US VERTICAL_ORDER Equivalent |
|---|---|---|
| -1 or below | Underground | Negative values |
| 0 | Obscured below ground | Negative values |
| **50** | Ground/surface level | **0** |
| 51 | First overhead level | 1 |
| 52+ | Higher overhead levels | 2+ |

**IFC** stores elevation explicitly on `IfcBuildingStorey.Elevation` (relative to building datum) and on `IfcBuilding.ElevationOfRefHeight` (above sea level). This is the closest match to the US model's per-level height fields.

**CityGML** provides `measuredHeight`, `storeysAboveGround`, `storeysBelowGround`, and uniquely, `storeyHeightsAboveGround` — an ordered list of individual storey heights that could reconstruct per-level elevation data.

---

## OS NGD Buildings: the full data model in detail

The OS NGD Buildings theme contains four feature types that collectively represent the physical building stock of Great Britain. The **Building** feature (polygon) is a merged footprint aggregating one or more **BuildingPart** features. BuildingParts carry TOIDs for backward compatibility with OS MasterMap; Buildings carry only OSIDs.

The schema is notably rich. As of version 4.1 (March 2025), a Building feature carries over **70 attributes** spanning geometry, classification, connectivity, construction material, building age, basement presence, number of floors, five height measurements, roof material, roof shape with eight compass-direction area measurements, and address counts broken down by residential/commercial/other. Key attributes for indoor mapping include:

- **`buildinguse`** — up to 2 coded values from a controlled list, with "Mixed Use" assigned when more than 2 uses exist. Maps to the US model's USE_TYPE field
- **`description`** — values like "Detached House", "Mid-Terraced House", "End-Of-Terrace", "Mixed", "Static Caravan"
- **`connectivity`** — Detached, Semi-Detached, Terraced. No US equivalent but valuable for understanding building relationships
- **`buildinguse_addresscount_total/residential/commercial/other`** — counts of UPRNs within the building, providing a measure of internal subdivision density
- **`primarysiteid`** and **`mainbuildingid`** — cross-links to Land Use Sites, establishing the Site → Building relationship equivalent to the US SITE_ID → FACILITY_ID link

**Building Part** features (polygon) represent physically distinguishable portions of buildings and carry the most detailed classification: `oslandusetiera`, `oslandusetierb`, `oslandcovertiera`, `oslandcovertierb`, plus all five height measurements. These are the closest equivalent to the US Facilities layer's building footprints with attribution.

**Building Line** features (linestring) represent walls, internal divisions, and overhanging edges — the direct equivalent of the US Details layer's line features for walls, doors, and architectural elements.

**Building Access Location** features (point, added March 2025) mark entrances to major public buildings with access type attribution — mapping to a subset of the US Points layer.

Cross-reference tables link Buildings to their constituent BuildingParts, to containing Land Use Sites, and to UPRNs (addresses). The **UPRN Reference table** is the critical bridge between building geometry and address data.

---

## AddressBase Premium and BS 7666 as the sub-building addressing backbone

AddressBase Premium is the direct commercial implementation of the BS 7666 British Standard. Its relational structure — BLPU records linked to LPI records linked to Street records — provides the most granular UK addressing available. The schema supports **multiple addresses per property** (one BLPU can have multiple LPIs with different logical statuses: Approved, Alternative, Historical) and **hierarchical nesting** (PARENT_UPRN linking child properties to parent buildings).

The **Classification Record** (Type 32) assigns each BLPU a code from the AddressBase Classification Scheme — a 4-level hierarchy with 9 primary categories (Commercial, Land, Military, Other, Parent Shell, Residential, Unclassified, Dual Use, Object of Interest) expanding to hundreds of specific codes. For example, `RD02` = Residential Dwelling, Detached; `CH01YH` = Commercial Hotel, Youth Hostel. This maps to the US model's USE_TYPE field but with much finer granularity.

The **LEVEL field** in the LPI record (Type 24) stores floor-level information as free text up to 30 characters, entered by Local Authority custodians. The OS NGD Addresses theme improves on this with structured `floorlevel` (a comma-separated list like "-1, 0, 1, 2, 3"), `lowestfloorlevel` (float), and `highestfloorlevel` (float) attributes. These enable systematic identification of which floors an address occupies — essential for mapping sub-building units to the US model's Levels and Units layers.

---

## Land use classification and the site concept

The OS NGD Land Use theme's **Site** feature type (MultiPolygon) is the closest UK equivalent to the US Sites layer. It represents discrete areas of consistent land use — airports, hospitals, schools, shopping centres, residential estates, industrial parks — with over 100 description values. Each Site carries three parallel classification systems:

- **OS Land Use Tier A/B** — OS's own two-level classification (e.g., "Health" → "Hospital")
- **NLUD classification** — National Land Use Database codes with 13 orders and 41 groups
- **AddressBase classification** — derived from addresses within the site extent

Sites link to Buildings via `mainbuildingid` (identifying the principal building) and `primarysiteid` (on the Building, identifying its containing site). Site Access Location points mark entrance locations with routing network connectivity — analogous to the US model's access points.

---

## UniClass and the BIM-GIS bridge

**UniClass 2015** is the UK construction industry's unified classification system, compliant with ISO 12006-2. It contains 15 tables covering the full lifecycle from strategy (Complexes, Entities) through design (Spaces/Locations, Elements/Functions) to construction (Systems, Products). Codes follow a structured pattern: a two-letter table prefix followed by numeric group/subgroup/section/object pairs (e.g., `SL_25_20_04` = Artists' studios).

The **Spaces/Locations (SL)** table is most relevant to indoor mapping, classifying interior spaces by function. The **Entities (En)** table classifies building types. **No formal mapping exists between UniClass and OS NGD classification**, but both can describe building types from different perspectives — UniClass for design/construction and OS for geographic/statistical purposes. IFC models reference UniClass via `IfcClassification` entities, making it the natural bridge between BIM indoor models and GIS-based indoor mapping.

---

## International standards provide the indoor modelling depth the UK lacks

IFC (ISO 16739-1) offers the most complete indoor mapping equivalent through its spatial hierarchy: `IfcSite → IfcBuilding → IfcBuildingStorey → IfcSpace`, with building elements (`IfcWall`, `IfcDoor`, `IfcWindow`) contained within spatial structures. Every entity carries a **22-character GlobalId** (compressed 128-bit UUID) and supports `IfcPostalAddress` for addressing. The `IfcBuildingStorey.Elevation` attribute provides per-floor height data that directly maps to the US model's HAE_FLOOR.

**CityGML 3.0** significantly improved indoor support by removing the LOD4 restriction — interior modelling (via `BuildingRoom`, `Storey`, `BuildingUnit`) is now possible at any LOD level. The new `Storey` and `BuildingUnit` classes map directly to the US Levels and Units layers respectively. CityGML's `storeyHeightsAboveGround` attribute (an ordered list of individual floor heights) is unique among standards and valuable for reconstructing per-level geometry.

**INSPIRE Buildings** defines four profiles of increasing detail. The Extended 3D profile includes `BuildingUnit` (apartments, office suites — mapping to Units) and `Room` (interior spaces). The `heightAboveGround` complex attribute stores height with explicit reference surfaces (top/bottom of construction). INSPIRE's `inspireId` (namespace + localId + versionId) provides a robust persistent identification system comparable to NGUID.

**IndoorGML** takes a fundamentally different approach, modelling indoor space as cellular decomposition with a dual graph for navigation. `CellSpace` maps to Units, `CellSpaceBoundary` to Details, and `State` (navigation nodes) to Points. Its unique **multi-layered space model** allows different semantic interpretations of the same physical space to coexist — an approach the US model could adopt for representing different emergency response views of the same building.

---

## Provenance and metadata field mapping

The US model includes provenance fields (DiscrpAgID, SrcLastEd, ProvAgID, Effective, Expire) drawn from NENA standards. The UK equivalents are distributed across version management and evidence tracking attributes.

| US Provenance Field | Purpose | OS NGD Equivalent |
|---|---|---|
| DiscrpAgID | Data steward agency ID | `localcustodiancode` (Local Authority ID) |
| SrcLastEd | Source last edited date | `versiondate` / `geometry_updatedate` / attribute-specific `_updatedate` fields |
| ProvAgID | Data provider agency ID | Attribute-specific `_source` fields (e.g., `height_source`, `numberoffloors_source`) |
| Effective | Record effective date | `versionavailablefromdate` |
| Expire | Record expiration date | `versionavailabletodate` |
| — | Change type | `changetype` (Insert, Update, Delete, Reclassification) |
| — | Evidence collection date | Attribute-specific `_evidencedate` fields |
| — | Capture method | Attribute-specific `_capturemethod` fields |

The OS NGD system is notably more granular in provenance tracking — individual attributes carry their own update dates, evidence dates, capture methods, and sources, rather than the US model's record-level provenance. This per-attribute lineage supports more sophisticated data quality assessment but adds schema complexity.

---

## Geometry types and coordinate reference systems

| Standard | CRS | Building Geometry | Address Geometry | Line Geometry |
|---|---|---|---|---|
| US Indoor Model | WGS84 (EPSG:4326) | Polygon | Point | LineString |
| OS NGD Buildings | British National Grid (EPSG:27700) | Polygon (Building, BuildingPart) | — | LineString (Building Line) |
| OS NGD Addresses | BNG + ETRS89 | — | Point only | — |
| OS MasterMap | BNG (EPSG:27700) | GM_Surface (polygon) | — | GM_Curve (line) |
| AddressBase Premium | BNG + ETRS89 | — | Point (X,Y + Lat,Long) | — |
| IFC | Local coordinates + IfcMapConversion | Brep, SweptSolid, Tessellation | — | Various |
| CityGML | Any CRS (typically national) | MultiSurface, Solid (LOD-dependent) | — | — |
| INSPIRE | ETRS89 (mandatory) | GM_Surface, GM_Solid | — | — |

The CRS difference is significant: the US model uses WGS84 geographic coordinates while UK products use British National Grid (a Transverse Mercator projection). ETRS89 coordinates are increasingly provided alongside BNG in modern OS products. An internationalised model should support configurable CRS with WGS84 as the interchange format.

---

## Conclusion: design principles for an internationalised model

Five key insights emerge from this cross-reference. **First**, the UK's separation of building geometry, addressing, and land use into distinct cross-linked themes is architecturally different from the US model's integrated layers — but the cross-reference keys (OSID, UPRN, USRN, TOID) make it possible to reconstruct equivalent integrated views. An internationalised model should support both integrated and federated data architectures.

**Second**, the Levels layer is the weakest point of international equivalence. Only IFC provides explicit storey entities with geometry and elevation. The UK's `numberoffloors`, `floorlevel`, and `basementpresence` attributes provide the *data* but not the *geometry* needed for floor-plan mapping. The internationalised model should define Levels as a required construct with clear guidance for deriving it from attribute-only systems.

**Third**, the UK SAO/PAO addressing structure is more granular than NENA for sub-building identification but fundamentally different in structure. The internationalised model should include both NENA-style fields and BS 7666-style SAO/PAO fields as parallel address representations, with a mapping table defining how to translate between them.

**Fourth**, identifier portability requires a composite approach. The model should define a universal NGUID-format primary key while maintaining foreign key fields for national identifiers (UPRN, USRN, TOID, OSID for the UK; IFC GlobalId for BIM imports; inspireId for EU data).

**Fifth**, the UK's per-attribute provenance model (with separate `_updatedate`, `_evidencedate`, `_source`, and `_capturemethod` fields for each major attribute) is more rigorous than the US model's record-level provenance. An internationalised model should support at minimum record-level provenance but allow optional attribute-level lineage tracking for high-assurance applications like emergency response.

---

## Appendix: References

All URLs verified accessible as of March 2026.

### US Public Safety Indoor Mapping Model and NENA Standards

1. **A Public Safety Indoor Mapping GIS Data Model** — v1.0, June 17, 2025. The primary US indoor mapping data model analysed in this cross-reference. An extension of the Esri ArcGIS Indoors Information Model with NENA 9-1-1 addressing and public safety attributes. Indoor Dataset feature classes licensed under Creative Commons CC BY 4.0 (Copyright © 2024 Esri).

2. **NENA-STA-006.2-2022: NENA Standard for NG9-1-1 GIS Data Model** — National Emergency Number Association. Defines the GIS data structure for use in Next Generation 9-1-1, including standardised data fields, spatial reference requirements, NGUID format, and address field definitions referenced throughout the US indoor mapping model.
   - Standard PDF: https://cdn.ymaws.com/www.nena.org/resource/resmgr/standards/nena-sta-006.2-2022_ng9-1-1_.pdf
   - GIS Data Model Templates (GitHub): https://github.com/NENA911/NG911GISDataModel
   - NENA Standards catalogue: https://www.nena.org/page/standards

3. **NENA-STA-004.2-2024: NENA NG9-1-1 United States Civic Location Data Exchange Format (CLDXF-US)** — National Emergency Number Association. Defines the civic location data exchange format including sub-building address elements (Floor, Wing, Section, Row, Seat, Unit Pre Type, Unit Value) referenced in the Units layer.
   - Standard PDF: https://cdn.ymaws.com/www.nena.org/resource/resmgr/standards/NENA-STA-004.2-2024_CLDXF_20.pdf

4. **ArcGIS Indoors Information Model** — Esri. The base data model extended by the US public safety model. Defines the spatial hierarchy of Sites, Facilities, Levels, Units, Details, and Points feature classes with fields including AREA_GROSS, HEIGHT_RELATIVE, VERTICAL_ORDER, USE_TYPE, and all ID fields.
   - Indoor dataset schema: https://pro.arcgis.com/en/pro-app/latest/help/data/indoors/indoor-dataset-schema.htm
   - Full Indoors Information Model: https://pro.arcgis.com/en/pro-app/latest/help/data/indoors/arcgis-indoors-information-model.htm
   - Getting started with ArcGIS Indoors: https://pro.arcgis.com/en/pro-app/latest/help/data/indoors/get-started-with-arcgis-indoors.htm

### UK Ordnance Survey National Geographic Database (OS NGD)

5. **OS NGD Buildings Theme** — Ordnance Survey. Contains four feature types: Building (polygon), Building Part (polygon), Building Line (linestring), and Building Access Location (point). Over 70 attributes per Building feature including height measurements, floor counts, building use, connectivity, construction material, and building age.
   - Theme overview: https://docs.os.uk/osngd/data-structure/buildings
   - Building feature type schema: https://docs.os.uk/osngd/data-structure/buildings/building-features/building
   - Building Part feature type schema: https://docs.os.uk/osngd/data-structure/buildings/building-features/building-part
   - Building Features collection: https://docs.os.uk/osngd/data-structure/buildings/building-features
   - Introductory guide (Building Feature Type): https://docs.os.uk/more-than-maps/os-ngd-data-demonstrators/os-ngd-data/os-ngd-buildings/building-feature-type

6. **OS NGD Address Theme** — Ordnance Survey. Contains Built Address, Historic Address, Pre-Build Address, Non-Addressable Object, Royal Mail Address, and Street Address feature types. Key identifiers: UPRN, USRN, OSID. Includes structured floor-level information (`floorlevel`, `lowestfloorlevel`, `highestfloorlevel`) and sub-building address components (`subname`, `name`, `number`).
   - Theme overview: https://docs.os.uk/osngd/data-structure/address
   - Built Address feature type: https://docs.os.uk/osngd/data-structure/address/gb-address/built-address
   - Introductory guide: https://docs.os.uk/more-than-maps/os-ngd-data-demonstrators/os-ngd-data/os-ngd-address

7. **OS NGD Land Use Theme** — Ordnance Survey. Contains Site (MultiPolygon) and Site Access Location (Point) feature types. Sites represent discrete areas of consistent land use with OS Land Use Tier A/B, NLUD classification, and AddressBase classification. Cross-references to Buildings via `mainbuildingid` and `primarysiteid`.
   - Theme overview: https://docs.os.uk/osngd/data-structure/land-use
   - Land Use Features collection: https://docs.os.uk/osngd/data-structure/land-use/land-use-features
   - Site feature type schema: https://docs.os.uk/osngd/data-structure/land-use/land-use-features/site
   - Introductory guide: https://docs.os.uk/more-than-maps/os-ngd-data-demonstrators/os-ngd-data/os-ngd-land-use

8. **OS NGD Documentation Portal** — Ordnance Survey. Central documentation hub covering all OS NGD themes, data schema versioning, unique identifiers (OSID, TOID, UPRN, USRN), coordinate reference systems, and access methods.
   - Main documentation: https://docs.os.uk/osngd/
   - Data schema versioning: https://docs.os.uk/osngd/getting-started/os-ngd-fundamentals/data-schema-versioning
   - Change log: https://docs.os.uk/osngd/os-ngd-news/change-log

9. **AddressBase Premium** — Ordnance Survey / GeoPlace. Commercial implementation of BS 7666. Relational structure: BLPU records (with UPRN, coordinates, classification) linked to LPI records (with SAO/PAO address components, LEVEL field) linked to Street records (via USRN). Supports multiple addresses per property and hierarchical nesting via PARENT_UPRN. Data now incorporated into and accessible through the OS NGD Address Theme.
   - OS NGD Address Theme (incorporates AddressBase Premium data): https://docs.os.uk/osngd/data-structure/address

10. **OS MasterMap Topography Layer** — Ordnance Survey. Legacy product providing building footprints as TopographicArea features with `physicalLevel` attribute (integer scheme where 50 = ground level). Building lines as TopographicLine features. TOIDs provide backward-compatible identifiers for Building Part and Building Line features in OS NGD.
    - Referenced via OS NGD Buildings documentation: https://docs.os.uk/osngd/data-structure/buildings

### UK British Standards

11. **BS 7666: Spatial datasets for geographical referencing** — British Standards Institution. Multi-part standard defining the SAO/PAO addressing structure used in UK land and property gazetteers.
    - Part 0: General model for gazetteers (BS 7666-0:2006)
    - Part 1: Specification for a street gazetteer (BS 7666-1:2019)
    - Part 2: Specification for a land and property gazetteer (BS 7666-2:2020)
    - BS 7666 series: https://landingpage.bsigroup.com/LandingPage/Series?UPI=BS+7666
    - BS 7666 Guidelines (Association for Geographic Information): https://www.agi.org.uk/bs-7666-guidelines/
    - BS 7666-2:2020 details: https://knowledge.bsigroup.com/products/spatial-datasets-for-geographical-referencing-specification-for-a-land-and-property-gazetteer-1

### International Standards — BIM and 3D City Models

12. **IFC (Industry Foundation Classes) — ISO 16739-1:2024** — buildingSMART International / ISO. Open international standard for Building Information Model data exchange. Defines the spatial hierarchy `IfcSite → IfcBuilding → IfcBuildingStorey → IfcSpace` with building elements (`IfcWall`, `IfcDoor`, `IfcWindow`). Every entity carries a 22-character GlobalId. Current version: IFC 4.3 (ISO 16739-1:2024).
    - buildingSMART IFC overview: https://technical.buildingsmart.org/standards/ifc/
    - IFC Schema Specifications: https://technical.buildingsmart.org/standards/ifc/ifc-schema-specifications/
    - buildingSMART International IFC page: https://www.buildingsmart.org/standards/bsi-standards/industry-foundation-classes/
    - ISO 16739-1:2024 catalogue entry: https://www.iso.org/standard/84123.html
    - IfcBuildingStorey specification: https://standards.buildingsmart.org/IFC/DEV/IFC4_2/FINAL/HTML/schema/ifcproductextension/lexical/ifcbuildingstorey.htm
    - IfcSpace specification: https://standards.buildingsmart.org/IFC/DEV/IFC4_2/FINAL/HTML/schema/ifcproductextension/lexical/ifcspace.htm

13. **OGC CityGML 3.0 Conceptual Model Standard** — Open Geospatial Consortium. International standard for storage and exchange of virtual 3D city models. Version 3.0 introduced `Storey`, `BuildingRoom`, and `BuildingUnit` classes for indoor modelling at any LOD level. Includes `storeyHeightsAboveGround` attribute (ordered list of individual floor heights).
    - OGC CityGML standard page: https://www.ogc.org/standards/citygml/
    - CityGML 3.0 Users Guide: https://docs.ogc.org/guides/20-066.html
    - CityGML 3.0 approval announcement: https://www.ogc.org/announcement/ogc-membership-approves-the-citygml-v30-conceptual-model-as-official-ogc-standard/
    - CityGML 3.0 building XSD schema (GitHub): https://github.com/opengeospatial/CityGML-3.0/blob/master/Encoding/building.xsd
    - TU Munich CityGML 3.0 project page: https://www.asg.ed.tum.de/en/gis/projects/citygml-30/
    - Kutzner, T. et al. (2020). "CityGML 3.0: New Functions Open Up New Applications." *PFG – Journal of Photogrammetry, Remote Sensing and Geoinformation Science*: https://link.springer.com/article/10.1007/s41064-020-00095-z

14. **OGC IndoorGML** — Open Geospatial Consortium. Standard data model and XML schema for indoor spatial information, focusing on navigation. Core concepts: `CellSpace` (rooms/spaces), `CellSpaceBoundary` (walls/doors), `State` (navigation nodes), `Transition` (navigation edges), and multi-layered space model. Version 2.0 Part 1 published August 2025.
    - OGC IndoorGML standard page: https://www.ogc.org/standards/indoorgml/
    - IndoorGML community site: https://www.indoorgml.net/
    - IndoorGML 2.0 publication announcement: https://www.ogc.org/announcement/ogc-publishes-indoorgml-2-0-part-1-conceptual-model-standard/
    - Li, K.-J. et al. (2017). "A Standard Indoor Spatial Data Model—OGC IndoorGML and Implementation Approaches." *ISPRS International Journal of Geo-Information*, 6(4), 116: https://www.mdpi.com/2220-9964/6/4/116

15. **INSPIRE Data Specification on Buildings (D2.8.III.2)** — European Commission. Defines four profiles for EU building data harmonisation: Core 2D, Core 3D, Extended 2D, and Extended 3D. The Extended 3D profile includes `BuildingUnit`, `Room`, boundary surfaces, and openings. Includes `heightAboveGround` complex attribute and `inspireId` persistent identification (namespace + localId + versionId).
    - INSPIRE Buildings theme page: https://inspire.ec.europa.eu/Themes/126/2892
    - Technical Guidelines (HTML): https://inspire-mif.github.io/technical-guidelines/data/bu/dataspecification_bu.html
    - Technical Guidelines (PDF): https://inspire-mif.github.io/technical-guidelines/data/bu/dataspecification_bu.pdf
    - Buildings Base schema (XSD): https://inspire.ec.europa.eu/schemas/bu-base/4.0/BuildingsBase.xsd
    - INSPIRE Identifier best practices (wetransform): https://wetransform.to/news-and-events/inspireid-best-practices/

### UK Construction Classification

16. **UniClass 2015** — NBS (part of Byggfakta Group), compliant with ISO 12006-2. Unified classification system for the UK construction industry with 15 tables covering the full asset lifecycle. The Spaces/Locations (SL) table classifies interior spaces by function; the Entities (En) table classifies building types. Codes follow a structured pattern: two-letter table prefix + numeric group/subgroup/section/object pairs (e.g., `SL_25_20_04`). IFC models reference UniClass via `IfcClassification` entities.
    - UniClass on Wikipedia: https://en.wikipedia.org/wiki/Uniclass
    - UniClass overview (REBIM): https://rebim.io/classification-systems-uniclass-2015/
    - UniClass and IFC classification (BibLus): https://biblus.accasoftware.com/en/uniclass-2015-ifc-object-classification/

### Geodetic References

17. **OSGM15 Geoid Model** — Ordnance Survey. The geoid separation model required to convert between WGS84 ellipsoid heights (HAE, used in the US model) and Ordnance Datum Newlyn orthometric heights (used in UK OS data). The separation varies from approximately 48 to 56 metres across the UK. Part of the OSTN15/OSGM15 coordinate transformation suite maintained by Ordnance Survey.

18. **EPSG Geodetic Parameter Registry** — International Association of Oil and Gas Producers. Defines coordinate reference system codes referenced in this document:
    - EPSG:4326 — WGS 84 (geographic 2D, used by the US model)
    - EPSG:27700 — OSGB36 / British National Grid (projected, used by OS NGD and OS MasterMap)
    - ETRS89 — European Terrestrial Reference System 1989 (increasingly provided alongside BNG in OS products; mandated by INSPIRE)
