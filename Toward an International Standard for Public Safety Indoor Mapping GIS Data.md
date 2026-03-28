# Toward an International Standard for Public Safety Indoor Mapping GIS Data

**A framework for interoperable indoor spatial data, mediated through international BIM and 3D city model standards, to serve emergency response and public safety worldwide.**

---

## 1. Introduction and rationale

Indoor environments are where most emergency incidents occur, yet no international standard exists for the GIS data that supports public safety operations inside buildings. The US Public Safety Indoor Mapping GIS Data Model (v1.0, June 2025) is the most developed national effort, extending the Esri ArcGIS Indoors Information Model with NENA 9-1-1 addressing fields to support Next Generation 9-1-1 call routing, dispatchable address estimation, and first responder visualisation. The UK Ordnance Survey National Geographic Database (OS NGD) provides a rich but structurally different ecosystem of building geometry, addressing, and land use data across separate but cross-linked themes. Neither model was designed for international use, and neither maps cleanly onto the other.

However, a set of mature international standards already defines the spatial, semantic, and topological concepts needed for indoor mapping. IFC (ISO 16739-1:2024) provides the most complete indoor spatial hierarchy through its `IfcSite → IfcBuilding → IfcBuildingStorey → IfcSpace` structure. CityGML 3.0 (OGC) bridges the BIM-to-GIS gap with `Storey`, `BuildingRoom`, and `BuildingUnit` classes available at any level of detail. IndoorGML 2.0 (OGC) models indoor space as cellular decomposition with a dual graph for navigation. INSPIRE Buildings defines four profiles of increasing detail for pan-European building data harmonisation. ISO 19160 provides a conceptual model for addressing that is independent of any national scheme.

This document proposes that these international standards should form the **central translation layer** through which any national indoor mapping implementation — US, UK, or otherwise — can achieve interoperability for public safety purposes. Rather than extending one national model to fit others, the architecture defines an international core that national models map into.

---

## 2. Architecture: international standards as the mediation layer

The fundamental design principle is that **national data models are profiles of an international core**, not the other way around. National implementations add country-specific attributes (addressing formats, emergency service identifiers, regulatory fields) on top of a common spatial and semantic framework drawn from existing international standards.

```
┌─────────────────────────────────────────────────────────────────────┐
│                    NATIONAL IMPLEMENTATIONS                        │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐             │
│  │  US NENA /   │  │  UK OS NGD / │  │  [Future     │             │
│  │  Esri Indoors│  │  BS 7666 /   │  │  national    │             │
│  │  Public      │  │  AddressBase │  │  profiles]   │             │
│  │  Safety Model│  │  Premium     │  │              │             │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘             │
│         │                 │                 │                      │
│         ▼                 ▼                 ▼                      │
│  ┌─────────────────────────────────────────────────────────┐      │
│  │          INTERNATIONAL TRANSLATION LAYER                 │      │
│  │                                                         │      │
│  │  SPATIAL STRUCTURE         IFC (ISO 16739-1)            │      │
│  │  IfcSite → IfcBuilding → IfcBuildingStorey → IfcSpace  │      │
│  │                                                         │      │
│  │  3D CITY INTEGRATION       CityGML 3.0 (OGC)           │      │
│  │  Building → Storey → BuildingRoom / BuildingUnit        │      │
│  │                                                         │      │
│  │  INDOOR NAVIGATION         IndoorGML 2.0 (OGC)         │      │
│  │  CellSpace, CellSpaceBoundary, State, Transition        │      │
│  │                                                         │      │
│  │  ADDRESSING                ISO 19160 (ISO/TC 211)       │      │
│  │  AddressableObject, AddressComponent, profiles          │      │
│  │                                                         │      │
│  │  SPATIAL SCHEMA            ISO 19107:2019               │      │
│  │  GM_Point, GM_Curve, GM_Surface, GM_Solid               │      │
│  │                                                         │      │
│  │  BUILDING CLASSIFICATION   ISO 12006-2 / UniClass       │      │
│  │                                                         │      │
│  │  EMERGENCY ALERTING        OASIS CAP 1.2 / ITU-T X.1303│      │
│  │                                                         │      │
│  │  EU HARMONISATION          INSPIRE Buildings            │      │
│  │                                                         │      │
│  │  PROVENANCE / METADATA     ISO 19115 / ISO 19157        │      │
│  └─────────────────────────────────────────────────────────┘      │
└─────────────────────────────────────────────────────────────────────┘
```

This architecture means that a US indoor map and a UK indoor map do not need to understand each other's national fields directly. Instead, both map their data into the international core, and interoperability is achieved through that shared framework.

---

## 3. The six canonical layers and their international anchors

The US model defines six spatial layers that represent a practical and well-tested decomposition of indoor space. These six layers map naturally to entities already defined in international standards. The international standard should adopt this six-layer structure while grounding each layer in its IFC, CityGML, and IndoorGML equivalents.

### 3.1 Sites (Polygon)

**Purpose**: Represents the boundary or top-level organisational unit — a campus, complex, or property containing one or more buildings.

| International Standard | Equivalent Entity | Notes |
|---|---|---|
| **IFC** | `IfcSite` | Contains one or more `IfcBuilding` entities; carries postal address via `IfcPostalAddress`; has `RefElevation` for site datum |
| **CityGML 3.0** | City model context / `AbstractCityObject` aggregation | No explicit "site" entity; sites are modelled as spatial aggregations of buildings |
| **INSPIRE** | Dataset / project level | No explicit site entity in core profiles |
| **ISO 19160** | `AddressableObject` (site-level profile) | A site can be an addressable object with its own address components |

**US implementation**: SITE_ID (NGUID), NAME, with NENA provenance fields (DiscrpAgID, SrcLastEd, ProvAgID).

**UK implementation**: OS NGD Land Use → Site (MultiPolygon) with OSID, TOID, OS Land Use Tier A/B, NLUD classification, `primaryuprn`, `mainbuildingid`.

**International core fields**: Site identifier (UUID), name, geometry (MultiPolygon), classification, provenance metadata.

### 3.2 Facilities (Polygon)

**Purpose**: Represents individual buildings or major physical structures within a site.

| International Standard | Equivalent Entity | Notes |
|---|---|---|
| **IFC** | `IfcBuilding` | Carries `ElevationOfRefHeight`, `ElevationOfTerrain`; supports `IfcPostalAddress`; contains `IfcBuildingStorey` entities |
| **CityGML 3.0** | `Building` / `BuildingPart` | Supports `storeysAboveGround`, `storeysBelowGround`, `storeyHeightsAboveGround`, `measuredHeight`, `function`, `usage`, `roofType` |
| **INSPIRE** | `Building` / `BuildingPart` | Core 2D: polygon/point with `numberOfFloors`, `currentUse`, `heightAboveGround`. Extended: adds `BuildingUnit`, materials, roof type |
| **IndoorGML** | Assumed (not modelled explicitly) | Building is the implicit container for indoor spaces |
| **ISO 19160** | `AddressableObject` (building-level profile) | A building is an addressable object; address components attach here |

**US implementation**: FACILITY_ID (NGUID), full NENA address decomposition (AddNum, StrName, PreDir, PostType, PostDir, IncMuni, County, State, Country, PostalCode), HEIGHT_RELATIVE, HAE_FLOOR, Type.

**UK implementation**: OS NGD Buildings → Building (Polygon) with OSID, `buildinguse`, `description`, `connectivity`, `numberoffloors`, `basementpresence`, five height measurements, `constructionmaterial`, `buildingageperiod`, address counts. Address fields via cross-reference to OS NGD Addresses (UPRN linkage). Full NENA-style address decomposition is not applicable; the UK uses BS 7666 SAO/PAO structure.

**International core fields**: Facility identifier (UUID), name, geometry (Polygon), building type/use classification (referencing ISO 12006-2 or national classification), height above datum, number of floors, address (structured per ISO 19160 profile), provenance metadata.

### 3.3 Levels (Polygon) — *the critical layer*

**Purpose**: Represents vertical subdivisions of a facility — physical floors, mezzanines, basements.

This is the **weakest point of international equivalence** and the layer where the international standard adds the most value. Only IFC provides an explicit storey entity with geometry and elevation. National mapping products (including UK OS NGD) store floor counts as attributes on buildings, not as discrete spatial layers.

| International Standard | Equivalent Entity | Notes |
|---|---|---|
| **IFC** | **`IfcBuildingStorey`** | **Primary anchor.** Carries `Elevation` (relative to building datum); contains `IfcSpace` entities; has own geometric representation |
| **CityGML 3.0** | **`Storey`** (new in v3.0) | A `BuildingSubdivision` with `sortOrder`; can contain `BuildingRoom` entities; `storeyHeightsAboveGround` on parent `Building` gives per-storey heights |
| **INSPIRE** | `numberOfFloors` attribute only | No discrete storey entity; Extended 3D profile references CityGML LOD4 for interior modelling |
| **IndoorGML 2.0** | `SpaceLayer` with storey property | Version 2.0 includes level/storey information as a property of `CellSpace` |
| **ISO 19107** | `GM_Surface` (2D) / `GM_Solid` (3D) | Provides geometry types for floor-plan polygons or volumetric storey representations |

**US implementation**: LEVEL_ID (NGUID), LEVEL_NUMBER, VERTICAL_ORDER, HEIGHT_RELATIVE, HAE_FLOOR, AREA_GROSS, SHORT_NAME.

**UK implementation**: No discrete layer. `numberoffloors` and `basementpresence` on OS NGD Building features; `floorlevel` (comma-separated list, e.g., "-1, 0, 1, 2, 3"), `lowestfloorlevel`, `highestfloorlevel` on OS NGD Address features; `LEVEL` free-text field on AddressBase Premium LPI records. `physicalLevel` integer attribute on OS MasterMap features (50 = ground level).

**International core fields**: Level identifier (UUID), facility identifier (FK), level number (integer), vertical order (integer, 0 = ground), name, short name, geometry (Polygon — floor-plan outline), height above datum (metres, referenced to a specified vertical datum), relative height (metres above ground level), gross area (m²), provenance metadata.

**Design requirement**: The international standard MUST define Levels as a required discrete polygon layer with clear guidance for deriving it from attribute-only systems (such as UK OS NGD). The derivation process should use `numberoffloors`, `basementpresence`, building height measurements, and `storeyHeightsAboveGround` (where available from BIM/CityGML sources) to construct level polygons.

### 3.4 Details (Line)

**Purpose**: Contains fine-grained static features — walls, doors, stairs, columns, furnishings — that provide visual and spatial context.

| International Standard | Equivalent Entity | Notes |
|---|---|---|
| **IFC** | `IfcWall`, `IfcDoor`, `IfcWindow`, `IfcStair`, `IfcColumn`, `IfcFurnishingElement` | Rich semantic decomposition of building elements with material properties, parametric geometry, and openings |
| **CityGML 3.0** | `BoundarySurface`, `Door`, `Window`, `BuildingInstallation`, `BuildingFurniture` | Building boundaries and installations at various LODs |
| **INSPIRE** | `BoundarySurface`, `Opening` (Extended 3D profile) | Wall, roof, floor surfaces and door/window openings |
| **IndoorGML** | **`CellSpaceBoundary`** | Boundary geometry of a `CellSpace` — may be a surface (3D) or curve (2D) |

**US implementation**: DETAIL_ID (NGUID), LEVEL_ID (FK), USE_TYPE, HEIGHT_RELATIVE, FEATNAME, HAE_FLOOR.

**UK implementation**: OS NGD Buildings → Building Line (LineString) with OSID, TOID. Represents walls, internal divisions, and overhanging edges.

**International core fields**: Detail identifier (UUID), level identifier (FK), geometry (LineString or MultiLineString), feature type/use (from controlled vocabulary aligned to IFC element types), name, height above datum, provenance metadata.

### 3.5 Units (Polygon)

**Purpose**: Represents discrete, navigable spaces — rooms, suites, offices, corridors — that are the primary targets for dispatchable address estimation and indoor positioning.

| International Standard | Equivalent Entity | Notes |
|---|---|---|
| **IFC** | **`IfcSpace`** | **Primary anchor.** Carries `ElevationWithFlooring`, `LongName`; associated with `IfcSpaceType` for classification; bounded by `IfcRelSpaceBoundary` |
| **CityGML 3.0** | **`BuildingRoom`** (replaces CityGML 2.0 `Room`) / **`BuildingUnit`** | `BuildingRoom` for individual interior spaces; `BuildingUnit` for functional subdivisions (apartments, offices) with `sortOrder` |
| **INSPIRE** | **`BuildingUnit`** (Extended profiles) / `Room` (Extended 3D) | `BuildingUnit`: subdivision with own lockable access, functionally independent, separately sellable. `Room`: interior space within Extended 3D profile |
| **IndoorGML** | **`CellSpace`** / `GeneralSpace` | Basic unit of cellular indoor space — room, corridor, hall. Contains GML identifier; geometry is `GM_Surface` (2D) or `GM_Solid` (3D) |
| **ISO 19160** | `AddressableObject` (unit-level profile) | A unit can be an addressable object; sub-building address components (floor, unit, room) attach here |

**US implementation**: UNIT_ID (NGUID), LEVEL_ID (FK), NAME, USE_TYPE, AREA_GROSS, plus NENA sub-building address elements (Floor, Wing, Unit_PreType, Unit_Value, Room, Section, Row, Seat, Addtl_Loc), HAE_FLOOR.

**UK implementation**: OS NGD Addresses → Built Address (Point only) with UPRN, `subname` (≈ SAO_TEXT), `name` (≈ PAO_TEXT), `number`, `floorlevel`. No polygon geometry in national addressing products; polygon room data exists only in BIM (IFC) models. AddressBase Premium provides BS 7666 SAO/PAO decomposition: SAO_START_NUMBER, SAO_START_SUFFIX, SAO_END_NUMBER, SAO_END_SUFFIX, SAO_TEXT for sub-building identification.

**International core fields**: Unit identifier (UUID), level identifier (FK), name, geometry (Polygon), use type (from controlled vocabulary), gross area (m²), address (structured per ISO 19160 profile — must support both NENA-style flat decomposition and BS 7666 SAO/PAO structure as parallel representations), height above datum, provenance metadata.

### 3.6 Points (Point)

**Purpose**: Represents discrete geolocated features — navigation waypoints, emergency exits, fire extinguishers, sensors, access points.

| International Standard | Equivalent Entity | Notes |
|---|---|---|
| **IFC** | `IfcAnnotation` (non-native fit); better modelled as placed products (`IfcFireExtinguisher`, `IfcSensor`, etc.) with `IfcLocalPlacement` | IFC's rich element library provides typed point features with properties |
| **CityGML 3.0** | `BuildingInstallation` (point placement) | Interior or exterior installations with point geometry |
| **IndoorGML** | **`State`** (navigation node) / `Transition` (navigation edge) | `State` represents a position in the dual-space graph for navigation; associated with a `CellSpace` via the duality relation |
| **OASIS CAP** | Alert location (`<area>` element with `<circle>`, `<polygon>`, or `<geocode>`) | Emergency alerts can reference indoor point locations for incident positioning |

**US implementation**: POINT_ID (NGUID), NAME, USE_TYPE, with conditional FKs to SITE_ID, FACILITY_ID, LEVEL_ID, UNIT_ID.

**UK implementation**: OS NGD Buildings → Building Access Location (Point) with OSID, access type attribution. Added March 2025; marks entrances to major public buildings.

**International core fields**: Point identifier (UUID), name, geometry (Point with Z coordinate), use type (from controlled vocabulary — should include emergency-specific types such as fire exit, AED, emergency phone, assembly point), conditional FK to facility/level/unit, provenance metadata.

---

## 4. Addressing: the ISO 19160 bridge

The most intractable interoperability challenge is addressing. The NENA system (used in the US model) decomposes addresses into directional prefixes/suffixes, street name types, and sub-building identifiers that reflect the planned grid-based address system of North America. The BS 7666 system (used in UK addressing) uses a fundamentally different SAO/PAO structure reflecting centuries of organic, name-based addressing. Japanese addresses use hierarchical administrative area references without thoroughfares. Many countries have no formal addressing system at all.

**ISO 19160-1:2015** provides the bridge. It defines a conceptual model for addressing that is independent of any national scheme, built around the concepts of `Address`, `AddressComponent`, `AddressableObject`, and `AddressLifecycle`. National addressing systems are represented as **profiles** of this conceptual model. Country profiles for ISO 19160-1 have been published (or are under development) for South Africa, Denmark, and others, and additional profiles can be defined for NENA and BS 7666 addressing.

The international indoor mapping standard should:

1. **Define addressing at the Facility and Unit layers using ISO 19160 profiles** rather than hard-coding any national format.
2. **Require at minimum** a building-level address (on Facilities) and a sub-building address (on Units).
3. **Provide parallel address representation fields** for the two most developed sub-building addressing schemes: NENA (Floor, Wing, Unit_PreType, Unit_Value, Room, Section, Row, Seat) and BS 7666 (SAO_START_NUMBER, SAO_TEXT, PAO_TEXT, PAO_START_NUMBER, LEVEL).
4. **Allow national profiles** to add additional address fields as extensions.
5. **Support the ISO 19160-4 postal address template language** for rendering addresses in country-appropriate formats.

---

## 5. Identifiers: a composite architecture

Every standard uses a different identifier scheme. The international model must support multiple coexisting identifiers without privileging any one.

| Scope | International Core | US Equivalent | UK Equivalent | BIM Equivalent | EU Equivalent |
|---|---|---|---|---|---|
| Primary key (all layers) | **UUID** (ISO/IEC 9834-8 / RFC 4122) | NGUID (NENA format) | OSID (36-char GUID) | IFC GlobalId (22-char base-64 UUID) | INSPIRE inspireId (namespace + localId + versionId) |
| Property/unit | — | UNIT_ID (NGUID) + address fields | **UPRN** (numeric, up to 12 digits) | — | — |
| Street | — | — | **USRN** (numeric, 8 digits) | — | — |
| Building (legacy) | — | — | **TOID** ('osgb' + 13-16 digits) | — | — |
| Hierarchical link | Parent ID FK fields | SITE_ID → FACILITY_ID → LEVEL_ID → UNIT_ID | **PARENT_UPRN** (arbitrary nesting depth) | `IfcRelAggregates` (object relationship) | — |

**Design**: The international standard should define a **UUID primary key** on every feature (compatible with IFC GlobalId when base-64 decoded, with OSID format, and with NGUID when structured as a URN). A **national identifier** field (text, optional) should carry the country-specific identifier (UPRN, NGUID, inspireId, etc.) to enable linkage to national systems.

---

## 6. Height, elevation, and vertical datum

Indoor mapping requires precise vertical positioning for floor-level estimation, 3D visualisation, and emergency caller location. Different standards use fundamentally different vertical datums.

| Datum Type | Used By | Description |
|---|---|---|
| **Ellipsoid height (HAE)** | US model (WGS84), GNSS positioning | Height above the WGS84 reference ellipsoid. Directly measured by GPS/GNSS. |
| **Orthometric height** | UK (Ordnance Datum Newlyn), EU (EVRS), most national mapping | Height above mean sea level (geoid). Requires a geoid separation model to convert from HAE. |
| **Building-relative** | IFC (`IfcBuildingStorey.Elevation`), ArcGIS Indoors (`HEIGHT_RELATIVE`) | Height relative to a building datum (typically ground floor slab level). |

The difference between WGS84 ellipsoid height and ODN orthometric height varies from approximately 48 to 56 metres across the UK (via the OSGM15 geoid model). This is not a trivial offset.

**Design**: The international standard should:

1. **Store height above ellipsoid (HAE)** as the primary vertical field on Levels and Facilities, referenced to the WGS84 ellipsoid — this is the datum used by GNSS positioning systems and is therefore directly comparable to emergency caller device locations.
2. **Include a building-relative height field** (metres above ground level) for use in visualisation and floor-level estimation.
3. **Require documentation of the vertical CRS** used, per ISO 19111.
4. **Support per-storey height data** following the CityGML 3.0 `storeyHeightsAboveGround` model (an ordered list of individual floor heights).

---

## 7. Coordinate reference systems

| Standard | Primary CRS | Notes |
|---|---|---|
| US model | WGS84 (EPSG:4326) | Required by NENA for received emergency call locations |
| UK OS NGD | British National Grid (EPSG:27700) | ETRS89 increasingly provided alongside BNG |
| IFC | Local coordinates + `IfcMapConversion` | Georeferencing via a map conversion entity; can target any CRS |
| CityGML | Any CRS (typically national) | CRS declared in GML header |
| INSPIRE | ETRS89 (mandatory for EU) | Geographic or projected |

**Design**: The international standard should:

1. **Require WGS84 (EPSG:4326) as the interchange CRS** — this is the CRS of GNSS positioning and is universally convertible.
2. **Allow storage in any national CRS** with mandatory declaration of the CRS identifier per ISO 19111.
3. **Require a vertical CRS declaration** separate from the horizontal CRS.

---

## 8. Classification and use types

Building and space classification varies enormously between countries. The international standard should define a **minimal common classification** while allowing national extensions.

| Classification System | Scope | Granularity | Standard |
|---|---|---|---|
| **ISO 12006-2** | Construction classification framework | Framework only — defines table structure | ISO |
| **UniClass 2015** (UK) | Full construction lifecycle | 15 tables, hundreds of codes (e.g., SL_25_20_04) | NBS / compliant with ISO 12006-2 |
| **OmniClass** (US/Canada) | Full construction lifecycle | 15 tables parallel to UniClass | CSI / CSC |
| **IFC classification** | Building elements and spaces | Via `IfcClassification` / `IfcClassificationReference` | buildingSMART |
| **CityGML `function`/`usage`** | Building and room function | Code lists (national profiles) | OGC |
| **INSPIRE `currentUse`** | Building use | Controlled vocabulary (residential, agriculture, industrial, commerceAndServices, ancillary) | EU |
| **AddressBase Classification** (UK) | Property use | 4-level hierarchy, 9 primary categories, hundreds of specific codes | GeoPlace |
| **NENA `PlaceType`** | Building type | IANA Location Types Registry values | NENA/IANA |

**Design**: The international standard should:

1. Define a **required top-level use type** with a small controlled vocabulary (Residential, Commercial, Industrial, Institutional, Mixed Use, Infrastructure, Unclassified) — compatible with INSPIRE `currentUse`.
2. Allow a **secondary classification code** from any ISO 12006-2-compliant system (UniClass, OmniClass, or national equivalent).
3. For the **Units layer**, define a required **space function** field with a minimal vocabulary for public safety (Office, Classroom, Corridor, Stairwell, Elevator, Lobby, Restroom, Storage, Mechanical, Server Room, Assembly, Kitchen, Medical, Laboratory, Hazardous Materials, Residential Unit, Retail, Unknown).

---

## 9. Provenance and metadata

The US model uses record-level provenance (DiscrpAgID, SrcLastEd, ProvAgID, Effective, Expire). The UK OS NGD system uses per-attribute provenance with separate `_updatedate`, `_evidencedate`, `_source`, and `_capturemethod` fields for major attributes.

| Concept | ISO Standard | US Equivalent | UK Equivalent |
|---|---|---|---|
| Data quality reporting | **ISO 19157** (Data quality) | — | `height_confidencelevel` |
| Metadata | **ISO 19115** (Metadata) | NENA-ADM-001.5b-2022 (Document Terminology) | OS NGD metadata attributes |
| Data steward | — | DiscrpAgID | `localcustodiancode` |
| Last edited | — | SrcLastEd | `versiondate`, attribute-specific `_updatedate` |
| Data provider | — | ProvAgID | Attribute-specific `_source` |
| Effective date | — | Effective | `versionavailablefromdate` |
| Expiration date | — | Expire | `versionavailabletodate` |
| Change type | — | — | `changetype` (Insert, Update, Delete, Reclassification) |
| Evidence date | — | — | Attribute-specific `_evidencedate` |
| Capture method | — | — | Attribute-specific `_capturemethod` |

**Design**: The international standard should:

1. **Require record-level provenance** on all features: data steward identifier, source last edited timestamp, data provider identifier, effective date, expiration date.
2. **Recommend (SHOULD) attribute-level provenance** for height and classification fields, following the OS NGD pattern. This is particularly important for height data where accuracy directly affects floor-level estimation for emergency response.
3. Align provenance metadata with **ISO 19115** (Metadata) and **ISO 19157** (Data quality).

---

## 10. Emergency services integration

The standard should explicitly support integration with emergency alerting and response systems.

**OASIS Common Alerting Protocol (CAP) 1.2** — approved as ITU-T Recommendation X.1303 — provides the international format for emergency alerts. CAP's `<area>` element supports geographic targeting via polygon, circle, or geocode. Indoor maps can provide the spatial context for CAP alerts, enabling floor-specific alert targeting. The international indoor mapping standard should define how indoor features (particularly Units and Points) can be referenced from CAP alert messages.

**PIDF-LO (RFC 4119 / RFC 5139)** — the Presence Information Data Format for Location Objects — is the format used in NG9-1-1 (US) and NG1-1-2 (EU) to convey emergency caller location. PIDF-LO supports civic address elements and geodetic coordinates. The international indoor mapping standard should ensure that its addressing fields can populate a PIDF-LO civic address, and that its spatial data can be used to validate PIDF-LO locations against indoor features.

---

## 11. IndoorGML's multi-layered space model for emergency response

IndoorGML 2.0's **multi-layered space model** is particularly valuable for public safety applications. It allows different semantic interpretations of the same physical space to coexist as separate `SpaceLayer` instances. For emergency response, this means a single building can support:

- A **topographic layer** (rooms, corridors, stairs as-built)
- A **security zone layer** (access-controlled areas, restricted zones)
- A **hazard layer** (hazardous material storage, high-voltage areas)
- A **evacuation route layer** (designated egress paths, assembly points)
- A **sensor coverage layer** (fire alarm zones, CCTV coverage areas)

Each layer is a separate cellular decomposition of the same physical space, with its own topology graph. The navigation module's `State` and `Transition` entities provide routing between spaces within and across layers.

The international standard should **recommend** that implementations support at least two IndoorGML space layers: a topographic layer (derived from the Units and Details layers) and a navigation layer (derived from the Points layer). Additional layers for security, hazard, and evacuation purposes should be supported as optional extensions.

---

## 12. BIM-to-GIS conversion pathway

A practical international standard must define how existing BIM models (which are abundant for new construction) can be converted to GIS-compatible indoor maps.

The conversion pathway from IFC to the international indoor mapping model is:

| IFC Source Entity | International Layer | Conversion Notes |
|---|---|---|
| `IfcSite` | Sites | Direct mapping; convert local coordinates via `IfcMapConversion` to WGS84 |
| `IfcBuilding` | Facilities | Extract `ElevationOfRefHeight` for HAE_FLOOR; extract `IfcPostalAddress` for addressing |
| `IfcBuildingStorey` | Levels | Extract `Elevation` for per-level height; project storey boundary to 2D polygon for floor plan |
| `IfcWall`, `IfcDoor`, `IfcWindow`, `IfcStair`, `IfcColumn` | Details | Project 3D geometry to 2D lines on each level; classify by IFC type |
| `IfcSpace` | Units | Project 3D boundary to 2D polygon; extract `LongName` for naming; classify by `IfcSpaceType` |
| Placed products (`IfcFireExtinguisher`, `IfcSensor`, etc.) | Points | Extract placement point; classify by IFC type |

This conversion pathway aligns with the existing Esri **Import IFC To Indoor Dataset** tool and with CityGML 3.0's improved BIM integration, providing practical tooling support.

---

## 13. Design principles for the international standard

1. **International standards at the centre**: IFC, CityGML 3.0, IndoorGML 2.0, ISO 19160, and ISO 19107 form the canonical semantic and spatial framework. National models are profiles that extend this core.

2. **Six required spatial layers**: Sites, Facilities, Levels, Details, Units, Points — grounded in IFC's spatial hierarchy and compatible with CityGML and IndoorGML concepts.

3. **Levels as a mandatory discrete layer**: The standard must define explicit floor-plan polygon geometry for each storey, bridging the gap between BIM models (which have this) and national mapping products (which generally do not).

4. **Address-agnostic core with national profiles**: ISO 19160 provides the addressing framework. NENA, BS 7666, and other national schemes are profiles. The standard supports parallel address representations.

5. **UUID primary keys with national identifier fields**: Every feature has a UUID primary key. National identifiers (NGUID, UPRN, OSID, inspireId) are carried as optional foreign-key fields.

6. **WGS84 HAE as the vertical interchange datum**: Stored alongside building-relative heights and with mandatory CRS declaration.

7. **Emergency services integration**: Explicit support for OASIS CAP alert targeting, PIDF-LO location validation, and IndoorGML multi-layered space models for emergency response.

8. **Extensibility without breaking interoperability**: National profiles may add layers and fields but must preserve the six core layers and their required fields.

9. **Per-attribute provenance for safety-critical fields**: Height, classification, and address data should support attribute-level lineage tracking following ISO 19115/19157.

10. **Open and vendor-neutral**: Built on open international standards (ISO, OGC, OASIS) with implementation possible in any GIS platform — not tied to any single vendor's data model.

---

## 14. Mapping national implementations to the international core

### 14.1 US Public Safety Indoor Mapping Model → International Core

The US model maps cleanly to the international core because it was designed with the same six-layer structure. The primary mapping work involves:

- Replacing NGUID format identifiers with UUIDs (or defining NGUID as a UUID profile)
- Restructuring NENA address fields as an ISO 19160-1 profile
- Expressing HAE_FLOOR as WGS84 ellipsoid height (already the case)
- Mapping USE_TYPE to the international classification vocabulary
- Carrying NENA-specific fields (ESN, MSAG Community Name, Legacy Street Name fields) as US-profile extensions

### 14.2 UK OS NGD / BS 7666 → International Core

The UK implementation requires more structural work because OS NGD separates building geometry, addressing, and land use into distinct themes:

- **Sites**: OS NGD Land Use → Site maps directly, with OSID as national identifier
- **Facilities**: OS NGD Buildings → Building maps directly, with OSID as national identifier and UPRN cross-reference for addressing
- **Levels**: Must be constructed as a new layer, derived from `numberoffloors`, `basementpresence`, building heights, and `floorlevel`/`lowestfloorlevel`/`highestfloorlevel` from OS NGD Addresses
- **Details**: OS NGD Buildings → Building Line maps directly
- **Units**: Must be constructed by combining OS NGD Address point locations with room/unit classification, or imported from BIM (IFC) models where available. UPRN serves as national identifier
- **Points**: OS NGD Buildings → Building Access Location maps to a subset of Points
- **Addressing**: BS 7666 SAO/PAO fields map to the ISO 19160-1 profile as a parallel representation alongside any NENA-style decomposition
- **Height datum**: Convert from ODN to WGS84 HAE using the OSGM15 geoid separation model
- **CRS**: Transform from BNG (EPSG:27700) to WGS84 (EPSG:4326) for interchange

### 14.3 Future national profiles

The international core is designed to accommodate future national profiles. Countries implementing indoor mapping for public safety would:

1. Define an ISO 19160-1 profile for their national addressing system
2. Map their national building/property identifiers to the national identifier field
3. Map their national building classification to the international use type vocabulary
4. Add country-specific extension fields as needed (e.g., earthquake zone, bushfire risk rating)

---

## 15. Conclusion

An international standard for public safety indoor mapping is both feasible and necessary. The building blocks already exist in IFC, CityGML, IndoorGML, ISO 19160, and INSPIRE. What is missing is the binding that ties them together for the specific purpose of emergency response and public safety operations inside buildings.

The US Public Safety Indoor Mapping GIS Data Model provides the right spatial decomposition (six layers) and the right operational focus (dispatchable address estimation, floor-level determination, first responder visualisation). The UK OS NGD provides the richest national building data ecosystem with sophisticated provenance tracking and cross-referencing. The international BIM and 3D city model standards provide the semantic and spatial framework that makes both — and any future national implementation — interoperable.

The path forward is to develop this framework through an appropriate international standards body — most likely OGC (which already hosts CityGML and IndoorGML) in collaboration with ISO/TC 211 (which hosts ISO 19160 and ISO 19107) and buildingSMART (which hosts IFC). The OASIS Emergency Management Technical Committee provides the emergency alerting integration. National emergency services standards bodies (NENA in the US, the UK's Joint Emergency Services Interoperability Principles programme) would contribute operational requirements.

The result would be a standard that any country can implement, knowing that their indoor maps will be interoperable with those of any other country — a critical capability for multinational emergency response, cross-border incidents, and the global safety of buildings used by international travellers.

---

## Appendix A: Complete references

All URLs verified accessible as of March 2026 unless otherwise noted.

### A.1 US Public Safety Indoor Mapping Model and NENA Standards

1. **A Public Safety Indoor Mapping GIS Data Model** — v1.0, June 17, 2025. Editors: Keri Brennan, Brooks Shannon. An extension of the Esri ArcGIS Indoors Information Model with NENA 9-1-1 addressing and public safety attributes. Indoor Dataset feature classes licensed under Creative Commons CC BY 4.0 (Copyright © 2024 Esri).

2. **NENA-STA-006.2-2022: NENA Standard for NG9-1-1 GIS Data Model** — National Emergency Number Association. Defines the GIS data structure for NG9-1-1, including standardised data fields, spatial reference requirements, NGUID format, and address field definitions.
   - Standard PDF: https://cdn.ymaws.com/www.nena.org/resource/resmgr/standards/nena-sta-006.2-2022_ng9-1-1_.pdf
   - GIS Data Model Templates (GitHub): https://github.com/NENA911/NG911GISDataModel
   - NENA Standards catalogue: https://www.nena.org/page/standards

3. **NENA-STA-004.2-2024: NENA NG9-1-1 United States Civic Location Data Exchange Format (CLDXF-US)** — National Emergency Number Association. Defines sub-building address elements (Floor, Wing, Section, Row, Seat, Unit Pre Type, Unit Value).
   - Standard PDF: https://cdn.ymaws.com/www.nena.org/resource/resmgr/standards/NENA-STA-004.2-2024_CLDXF_20.pdf

4. **ArcGIS Indoors Information Model** — Esri. Defines the Sites, Facilities, Levels, Units, Details, and Points feature class hierarchy.
   - Indoor dataset schema: https://pro.arcgis.com/en/pro-app/latest/help/data/indoors/indoor-dataset-schema.htm
   - Full Indoors Information Model: https://pro.arcgis.com/en/pro-app/latest/help/data/indoors/arcgis-indoors-information-model.htm

### A.2 UK Ordnance Survey and British Standards

5. **OS NGD Buildings Theme** — Ordnance Survey. Four feature types: Building (polygon), Building Part (polygon), Building Line (linestring), Building Access Location (point). Over 70 attributes per Building feature.
   - Theme overview: https://docs.os.uk/osngd/data-structure/buildings
   - Building feature type schema: https://docs.os.uk/osngd/data-structure/buildings/building-features/building
   - Building Part schema: https://docs.os.uk/osngd/data-structure/buildings/building-features/building-part
   - Building Features collection: https://docs.os.uk/osngd/data-structure/buildings/building-features
   - Introductory guide: https://docs.os.uk/more-than-maps/os-ngd-data-demonstrators/os-ngd-data/os-ngd-buildings/building-feature-type

6. **OS NGD Address Theme** — Ordnance Survey. UPRN-keyed addressing with structured floor-level information. Built Address, Historic Address, Pre-Build Address, Royal Mail Address, Street Address feature types.
   - Theme overview: https://docs.os.uk/osngd/data-structure/address
   - Built Address feature type: https://docs.os.uk/osngd/data-structure/address/gb-address/built-address

7. **OS NGD Land Use Theme** — Ordnance Survey. Site (MultiPolygon) and Site Access Location (Point) feature types with OS Land Use Tier A/B, NLUD classification.
   - Theme overview: https://docs.os.uk/osngd/data-structure/land-use
   - Site feature type: https://docs.os.uk/osngd/data-structure/land-use/land-use-features/site

8. **OS NGD Documentation Portal** — Ordnance Survey. Central documentation hub.
   - Main documentation: https://docs.os.uk/osngd/
   - Data schema versioning: https://docs.os.uk/osngd/getting-started/os-ngd-fundamentals/data-schema-versioning

9. **BS 7666: Spatial datasets for geographical referencing** — British Standards Institution. Part 1: Street gazetteer (BS 7666-1:2019). Part 2: Land and property gazetteer (BS 7666-2:2020). Defines SAO/PAO addressing structure.
   - BS 7666 series: https://landingpage.bsigroup.com/LandingPage/Series?UPI=BS+7666
   - BS 7666 Guidelines (AGI): https://www.agi.org.uk/bs-7666-guidelines/

### A.3 International BIM Standards

10. **IFC (Industry Foundation Classes) — ISO 16739-1:2024** — buildingSMART International / ISO. Open international standard for BIM data exchange. Spatial hierarchy: `IfcSite → IfcBuilding → IfcBuildingStorey → IfcSpace`. Current version: IFC 4.3.
    - buildingSMART IFC overview: https://technical.buildingsmart.org/standards/ifc/
    - IFC Schema Specifications: https://technical.buildingsmart.org/standards/ifc/ifc-schema-specifications/
    - buildingSMART International: https://www.buildingsmart.org/standards/bsi-standards/industry-foundation-classes/
    - ISO 16739-1:2024: https://www.iso.org/standard/84123.html
    - IfcBuildingStorey specification: https://standards.buildingsmart.org/IFC/DEV/IFC4_2/FINAL/HTML/schema/ifcproductextension/lexical/ifcbuildingstorey.htm
    - IfcSpace specification: https://standards.buildingsmart.org/IFC/DEV/IFC4_2/FINAL/HTML/schema/ifcproductextension/lexical/ifcspace.htm

### A.4 OGC Standards

11. **OGC CityGML 3.0 Conceptual Model Standard** — Open Geospatial Consortium. 3D city model standard with `Storey`, `BuildingRoom`, `BuildingUnit` classes. `storeyHeightsAboveGround` attribute.
    - OGC standard page: https://www.ogc.org/standards/citygml/
    - Users Guide: https://docs.ogc.org/guides/20-066.html
    - Approval announcement: https://www.ogc.org/announcement/ogc-membership-approves-the-citygml-v30-conceptual-model-as-official-ogc-standard/
    - Building XSD (GitHub): https://github.com/opengeospatial/CityGML-3.0/blob/master/Encoding/building.xsd
    - TU Munich project page: https://www.asg.ed.tum.de/en/gis/projects/citygml-30/
    - Kutzner et al. (2020), "CityGML 3.0: New Functions Open Up New Applications," *PFG*: https://link.springer.com/article/10.1007/s41064-020-00095-z

12. **OGC IndoorGML 2.0 Part 1: Conceptual Model Standard** — Open Geospatial Consortium. Indoor spatial data model with `CellSpace`, `CellSpaceBoundary`, `State`, `Transition`, multi-layered space model. Published August 2025.
    - OGC standard page: https://www.ogc.org/standards/indoorgml/
    - Community site: https://www.indoorgml.net/
    - Publication announcement: https://www.ogc.org/announcement/ogc-publishes-indoorgml-2-0-part-1-conceptual-model-standard/
    - Li et al. (2017), "A Standard Indoor Spatial Data Model—OGC IndoorGML and Implementation Approaches," *ISPRS Int. J. Geo-Inf.*, 6(4), 116: https://www.mdpi.com/2220-9964/6/4/116

### A.5 ISO/TC 211 Geographic Information Standards

13. **ISO 19160-1:2015: Addressing — Part 1: Conceptual model** — ISO/TC 211. Defines `Address`, `AddressComponent`, `AddressableObject` with lifecycle and provenance. National addressing systems are profiles.
    - ISO/TC 211 project page: https://committee.iso.org/sites/tc211/home/projects/projects---complete-list/iso-19160-1.html
    - Country profiles: https://committee.iso.org/sites/tc211/home/standards-in-action/addressing/Profiles-to-iso-19160---1.html
    - Blog on international addressing standards: https://committee.iso.org/sites/tc211/home/standards-in-action/blog/2021-04-29-international-address.html

14. **ISO 19160-4:2017: Addressing — Part 4: International postal address components and template language** — ISO/TC 211 (jointly with UPU). Defines postal address components (elements, constructs, segments) and rendering rules.
    - ISO/TC 211 project page: https://committee.iso.org/sites/tc211/home/projects/projects---complete-list/iso-19160-4.html

15. **ISO 19107:2019: Geographic information — Spatial schema** — ISO/TC 211. Conceptual schemas for spatial characteristics of geographic entities. Defines `GM_Point`, `GM_Curve`, `GM_Surface`, `GM_Solid` geometry types used by IndoorGML, CityGML, and INSPIRE.
    - ISO catalogue: https://www.iso.org/standard/66175.html
    - ISO/TC 211 project page: https://committee.iso.org/sites/tc211/home/projects/projects---complete-list/iso-19107.html

16. **ISO 19115: Geographic information — Metadata** — ISO/TC 211. Defines metadata elements for geographic datasets and services.

17. **ISO 19157: Geographic information — Data quality** — ISO/TC 211. Defines data quality elements and measures. ISO 19160-3 (Address data quality) is a profile of ISO 19157.

18. **ISO 19111: Geographic information — Referencing by coordinates** — ISO/TC 211. Defines coordinate reference systems. Basis for EPSG codes used throughout this document.

### A.6 EU INSPIRE Directive

19. **INSPIRE Data Specification on Buildings (D2.8.III.2)** — European Commission. Four profiles: Core 2D, Core 3D, Extended 2D, Extended 3D. Extended 3D includes `BuildingUnit`, `Room`, boundary surfaces. `heightAboveGround` complex attribute. `inspireId` persistent identification.
    - INSPIRE Buildings theme: https://inspire.ec.europa.eu/Themes/126/2892
    - Technical Guidelines (HTML): https://inspire-mif.github.io/technical-guidelines/data/bu/dataspecification_bu.html
    - Buildings Base schema (XSD): https://inspire.ec.europa.eu/schemas/bu-base/4.0/BuildingsBase.xsd

### A.7 Emergency Services Standards

20. **OASIS Common Alerting Protocol (CAP) v1.2** — OASIS Emergency Management TC. International format for emergency alerts, also approved as ITU-T Recommendation X.1303. `<area>` element supports geographic targeting via polygon, circle, or geocode.
    - OASIS standard page: https://www.oasis-open.org/standard/cap/
    - CAP v1.2 specification (HTML): https://docs.oasis-open.org/emergency/cap/v1.2/CAP-v1.2-os.html
    - CAP v1.2 specification (PDF): https://docs.oasis-open.org/emergency/cap/v1.2/CAP-v1.2-os.pdf

21. **IETF RFC 4119: A Presence-based GEOPRIV Location Object Format (PIDF-LO)** and **RFC 5139: Revised Civic Location Format for PIDF-LO** — Internet Engineering Task Force. Defines the location conveyance format used in NG9-1-1 (US) and NG1-1-2 (EU) for emergency caller positioning. Supports civic address elements and geodetic coordinates.

### A.8 Construction Classification Standards

22. **ISO 12006-2: Building construction — Organization of information about construction works — Part 2: Framework for classification** — ISO. Defines the framework for construction classification systems; UniClass 2015 (UK) and OmniClass (US/Canada) are compliant implementations.

23. **UniClass 2015** — NBS (Byggfakta Group). UK construction classification with Spaces/Locations (SL) table for interior spaces and Entities (En) table for building types.
    - Wikipedia: https://en.wikipedia.org/wiki/Uniclass
    - UniClass overview (REBIM): https://rebim.io/classification-systems-uniclass-2015/
    - UniClass and IFC (BibLus): https://biblus.accasoftware.com/en/uniclass-2015-ifc-object-classification/

### A.9 Geodetic References

24. **EPSG Geodetic Parameter Registry** — International Association of Oil and Gas Producers. CRS codes referenced in this document: EPSG:4326 (WGS 84), EPSG:27700 (OSGB36 / British National Grid).

25. **OSGM15 Geoid Model** — Ordnance Survey. Geoid separation model for converting between WGS84 ellipsoid heights and Ordnance Datum Newlyn orthometric heights across the UK (separation varies ~48–56 m).

26. **EVRS (European Vertical Reference System)** — Used by INSPIRE as the recommended vertical CRS for European building data.

### A.10 INSPIRE Identifier and Provenance

27. **INSPIRE Identifier Best Practices** — wetransform. Explains `gml:id`, `gml:identifier`, and `inspireId` (namespace + localId + versionId).
    - https://wetransform.to/news-and-events/inspireid-best-practices/
