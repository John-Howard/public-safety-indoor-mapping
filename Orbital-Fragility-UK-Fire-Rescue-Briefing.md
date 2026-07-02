# Orbital Fragility and the UK Fire and Rescue Sector

**Briefing paper**

*Potential impact of a low Earth orbit collision cascade and severe space-weather event on supply chain, command-and-control, and operational response*

| Field | Detail |
| --- | --- |
| **Title** | Orbital Fragility and the UK Fire and Rescue Sector |
| **Author** | John Howard, Data Services Manager, Hertfordshire Fire and Rescue Service |
| **Document type** | Analytical thought experiment / horizon-scanning briefing |
| **Version** | 1.3 (added air-asset and UAV analysis) |
| **Date** | 1 July 2026 |
| **Status** | Draft for discussion; not an official risk assessment |
| **Scenario basis** | Thiele et al. (2026), arXiv:2512.09643, and later CRASH Clock updates, plus published UK dependency evidence |
| **Intended use** | Risk-register input and tabletop-exercise seed material |
| **Scope** | Comprehensive scenario (LEO cascade plus GNSS/space-weather), acute phase emphasised |

> **How to read this document.** This is a structured thought experiment, not a formal service risk assessment. It draws on a recent scientific paper on orbital collision risk and on published evidence about the UK's dependence on satellite services. Bracketed numbers `[n]` cite the sources listed in Appendix A. Validate the judgements against each service's own systems, contracts and continuity plans before using them in planning.

## Contents

- [1 Executive summary](#1-executive-summary)
- [2 Purpose and scope](#2-purpose-and-scope)
- [3 The scenario and its mechanism](#3-the-scenario-and-its-mechanism)
  - [3.1 The source paper](#31-the-source-paper)
  - [3.2 One trigger, two loss pathways](#32-one-trigger-two-loss-pathways)
  - [3.3 The single-collision tail risk](#33-the-single-collision-tail-risk)
- [4 Calibration: what breaks and what holds](#4-calibration-what-breaks-and-what-holds)
- [5 Impact assessment](#5-impact-assessment)
  - [5.1 Communications and command-and-control](#51-communications-and-command-and-control)
  - [5.2 Operational response and firefighter safety](#52-operational-response-and-firefighter-safety)
  - [5.3 Air assets: drones and crewed aircraft](#53-air-assets-drones-and-crewed-aircraft)
  - [5.4 Supply chain](#54-supply-chain)
- [6 Disruption coinciding with rising demand](#6-disruption-coinciding-with-rising-demand)
- [7 Existing mitigations and inherent resilience](#7-existing-mitigations-and-inherent-resilience)
- [8 Recommended lines of enquiry for a tabletop exercise](#8-recommended-lines-of-enquiry-for-a-tabletop-exercise)
- [9 Caveats and limitations](#9-caveats-and-limitations)
- [Appendix A: Sources](#appendix-a-sources)

## 1 Executive summary

The UK fire and rescue sector operates none of the satellites this scenario threatens, yet it is still materially exposed. While the exposure is indirect, it runs through the sector's everyday reliance on satellite positioning and timing, on the cellular networks that increasingly carry emergency communications, and on satellite imagery used in wildfire and flood response. The main summary points follow.

- The exposure is indirect but real. The sector depends on services (satellite positioning and timing, commercial cellular networks, Earth observation and satellite communications) that a severe space-weather event, or a low Earth orbit collision cascade, would degrade. [1] [2]
- The timeline is short and shrinking. The CRASH Clock, the expected time to a first collision if satellites lost the ability to manoeuvre, has fallen from 164 days in 2018 to about 2.5 days by May 2026. An extreme solar storm, a bad software update or a cyberattack could each cause the loss of control that starts that clock. [1] [26] [27]
- GNSS would not be physically destroyed. GPS and Galileo orbit well above the crowded low Earth orbit shells, so a cascade does not knock them out. The acute risk to positioning and timing comes from the storm, while the cascade mainly threatens Earth observation and low Earth orbit communications over the following weeks and months. [1]
- Most effects are delays and degradations rather than blackouts. Safety-critical fireground procedures run on voice and paper, network timing has holdover (the ability to retain precise clock accuracy when sync is lost), and a national grid collapse is assessed as very unlikely. [7] [15] [19]
- Air assets are a degraded fallback, not a workaround. Drones and, to a lesser extent, crewed aircraft depend on GNSS for stable flight, navigation and georeferenced mapping, so the aerial survey that would substitute for lost satellite imagery is itself weakened by the same event and may be in higher demand just as it becomes less reliable. [29] [31]
- A single collision is the tail risk to plan for. The CRASH clock is an average, and one collision, even without a full runaway, could degrade low Earth orbit services for weeks or months. A triggering event could produce that collision within hours, so any warning could be very short and continuity plans should not assume days of notice. [1] [26] [28]
- The credible worst case is a compound one. A multi-day loss of dispatch quality, caller location, communications and mapping would most plausibly arrive at the same time as a surge in demand caused by the same storm. [18] [20]
- The priority actions are practical; confirm how long control-room and radio timing can hold without GNSS, rehearse the manual fallbacks, and map which current routine capabilities quietly depend on satellite services. [5] [7]

## 2 Purpose and scope

This paper tests the UK fire and rescue sector against the orbital-fragility scenario set out by Thiele and colleagues, taken together with the severe space-weather event most likely to trigger it. It is meant as horizon-scanning input to service risk registers and as seed material for a tabletop exercise. It covers the three dimensions requested; supply chain, communications and command-and-control (C2) infrastructure, operational response and firefighter safety. The acute phase, from the first hours to a few weeks, receives most of the attention, with prolonged degradation over months to years treated as a secondary concern.

## 3 The scenario and its mechanism

### 3.1 The source paper

Thiele et al. propose the CRASH Clock, the expected time to a catastrophic collision in low Earth orbit if satellite manoeuvring stopped or situational awareness were lost. It has fallen sharply as mega-constellations have grown, from 164 days at the start of 2018 to about 5.5 days by mid-2025 and, on the paper authors' later updates, to roughly 3.8 days in January 2026 and about 2.5 days by May 2026, with satellites now launched at a rate of around 100 per week. The densest Starlink shell, near 550 km altitude, already sits within the threshold for long-term runaway debris growth, so a single collision could have immediate consequences. The authors name an extreme solar storm, a bad software update or a cybersecurity event as the plausible triggers for the widespread loss of control the clock assumes. [1] [26] [27]

### 3.2 One trigger, two loss pathways

Of the possible triggers, the severe solar storm matters most for the fire and rescue sector, because it acts on two fronts at once. A software fault or a cyberattack could cause a comparable loss of control in orbit, but a geomagnetic storm also reaches the ground, and that is what ties the orbital and terrestrial risks together.

The first is the direct space-weather pathway. A severe geomagnetic storm degrades systems the sector relies on today; GNSS timing and positioning (through ionospheric scintillation and position uncertainty of metres to kilometres), HF radio, and, at the margins, the power grid. The UK National Risk Assessment already treats geomagnetic storms as a contributor to worst-case scenarios for power grids, GNSS, HF radio and satellite drag. [18]

The second is the low Earth orbit cascade itself, which is a more gradual process. Over weeks to months, it removes the low Earth orbit assets a cascade would degrade, chiefly the Earth-observation satellites used for wildfire and flood mapping and the low Earth orbit satellite communications used as a fallback and in remote operations. [1] [24]

### 3.3 The single-collision tail risk

The CRASH clock is an average, and the average hides the risk that matters most here. The paper authors' own simulations show that after a loss of control a collision might take days or weeks, or it might happen within a few hours. At a clock value of only a few days the chance of a collision in the first 24 hours is already high; the paper put it near 30 per cent at about 2.8 days, and the research team treats clock values with a greater than 50 per cent chance within 24 hours as a danger zone. A single collision, even one that never grows into a runaway cascade, would immediately stress the orbital environment, and cataloguing even half of the resulting debris typically takes around 100 days, so the uncertainty and the degraded service that comes with it would persist for months. Fragmentation events already happen for ordinary reasons, such as the internal break up of a Starlink satellite in March 2026. For planning purposes, the important point is that this is a low-probability, high-impact tail; unlikely in any given month, more likely each year as the clock shrinks, and capable of arriving with little or no warning. [1] [26] [28]

Two qualifications keep this in proportion. The researchers are explicit that the clock measures how little margin remains, not that a collision is imminent, and that it does not by itself measure Kessler syndrome, which can take decades to build. They also argue the trajectory is not inevitable and could be slowed by coordinated space-traffic management, which at present has no equivalent to the system used in civil aviation. [25] [28]

## 4 Calibration: what breaks and what holds

There are three factors that reduce the severity of this scenario in the UK context and separate the real vulnerabilities from the overstated ones.

Timing has holdover (the ability to retain precise clock accuracy when sync is lost). The UK Government's economic study found timing applications relatively resilient to a five-day GNSS outage where holdover clocks or alternative sources are in place. The National Timing Centre, a distributed network of atomic clocks independent of GNSS with a further £180 million committed in 2026, is being built precisely to keep telecommunications, finance and the emergency services running through GNSS outages. [2] [5]

The grid is more robust than often assumed. National Grid's transmission assessment concluded that widespread transformer damage and a collapse of the interconnected grid are very unlikely. A Carrington-type storm would damage a small number of transformers, at high cost to the operator but with little effect on end users. Planning should therefore assume localised power loss rather than national. [19]

GNSS is not in the debris zone. GPS (about 20,200 km) and Galileo (about 23,200 km) orbit in medium Earth orbit, well above the low Earth orbit shells where the cascade risk is concentrated. The cascade threatens low Earth orbit communications and imaging, not the satellite-navigation constellations directly. [1]

## 5 Impact assessment

The three dimensions are summarised below, then examined in turn.

| Dimension | Main dependency at risk | Acute-phase effect | Built-in resilience |
| --- | --- | --- | --- |
| Communications and C2 | GNSS timing for cellular networks; caller location; AVLS dispatch | Weaker 999 caller location; loss of automatic nearest-appliance dispatch; cellular timing holds for only hours | TETRA robust; gazetteer, GIS and manual dispatch survive; voice and paper procedures |
| Operational response and firefighter safety | In-cab navigation; Earth observation for wildfire and flood; any GPS crew tracking | Slower dispatch and routing; loss of near-real-time fire and flood mapping; higher radio load | BA entry control, accountability and mayday run on voice and paper and do not fail with GNSS |
| Air assets (drones and aircraft) | GNSS for drone position-hold, navigation, return-to-home and mapping; GNSS approaches and satcom for crewed aircraft | Drones drift, drop into fail-safe or ground; autonomous and mapping missions degrade; crewed precision and night tasks affected; aerial-survey fallback weakened | Crewed aircraft keep inertial and ground-based nav and visual flight; some drones offer GNSS-denied optical or inertial navigation |
| Supply chain | GNSS-timed road logistics, fuel and payments; utility SCADA; power | Friction in fuel and parts resupply; possible localised water-pressure or telemetry issues | Station fuel reserves; stockholding absorbs short outages; grid collapse assessed as very unlikely |

### 5.1 Communications and command-and-control

The clearest dependency is at the call-handling stage. The largest single emergency-service benefit from GNSS comes from public-safety answering points using GNSS-derived handset location, which in the UK is Advanced Mobile Location, to shorten call and search times. If GNSS degrades, caller location falls back to cell-sector triangulation, which fixes a caller to a district rather than a doorway. Call times lengthen and more resources may be required. [3]

Control-room mobilising leans on GPS but is not captive to it. National guidance describes systems that use an address-based gazetteer and an automatic vehicle location system to present the nearest resource, with fire control able to override the recommendation by hand. The gazetteer, the GIS risk layers, the hydrant data and the pre-planned rendezvous points all survive a GNSS outage. What is lost is automatic nearest-appliance selection, which reverts to station-area dispatch and human judgement. That loss matters, because automatic vehicle location is worth roughly 15 to 25 per cent on response times where services have measured it. [16] [17]

The radio backbone is where timing issues are felt the most; Airwave, the TETRA network, remains the operational backbone, while the LTE-based Emergency Services Network meant to replace it has slipped repeatedly and runs over EE's commercial 4G and 5G infrastructure, carrying around 300,000 frontline users. That architecture is significant here. LTE and 5G base stations depend on precise timing far more than TETRA does. They draw synchronisation from GNSS and can hold accurate time for between 4 hours to 72 hours after losing the signal, after which services degrade, and solar activity is one of the named causes of such outages. The further the sector migrates onto the new network, the more it inherits the commercial network's exposure to GNSS timing. In this failure mode, the relative simplicity of TETRA is an advantage. [9] [10] [11] [12] [13] [14] [15]

The data-rich services the new network is meant to unlock, such as body-worn video, live mapping, database lookups and telemetry, are the first to fail when cellular coverage degrades. Push-to-talk voice survives longer than any of them.

### 5.2 Operational response and firefighter safety

Dispatch and navigation will both slow down. Nearest-appliance selection reverts to a manual process, and in-cab routing degrades, which affects on-call and retained crews and mutual-aid responders crossing unfamiliar ground the most. [16]

Crew safety and accountability are better protected. Breathing-apparatus entry control, crew-accountability procedures and mayday protocols are built on voice and paper and do not fail when GNSS does. This may become an issue in the future. The sector is moving toward GPS-tagged personnel tracking and telemetry, which is already operational elsewhere, where satellite systems track a firefighter's position and relay it to a control centre while taking load off congested radio channels. Anything that has quietly come to rely on that kind of tracking, or on data links to locate crews, is exactly what degrades in this scenario, and it degrades while radio load is also rising. [23]

Wildfire and flood response is where the loss of low Earth orbit assets is likely to be felt most directly. UK responders increasingly use Earth observation for these incidents. The Copernicus Emergency Management Service provides satellite-based flood and wildfire mapping to authorised responders, and Scotland's Satellite Emergency Mapping Service draws on the International Charter's constellation, with Scottish Fire and Rescue an interested partner. Much of that near-real-time revisit capability comes from dense low Earth orbit constellations, which are exactly the assets a cascade threatens. Losing them pushes perimeter mapping, flood-extent intelligence and evacuation planning back onto aerial survey and ground observation, which are slower and less complete, and more dangerous when a fire front is moving. As Section 5.3 sets out, that aerial-survey fallback is itself weakened when GNSS is degraded, so it only partly fills the gap. [21] [22] [24]

### 5.3 Air assets: drones and crewed aircraft

Air assets are now a standard part of the sector's toolkit. Most UK services operate drones for aerial reconnaissance, thermal imaging, incident overview, search and rescue, hazardous-materials assessment and, increasingly, wildfire detection and monitoring. London Fire Brigade uses them to give incident commanders a live overhead view and to reach areas that are unsafe for firefighters, and Lancashire has trialled autonomous fixed-wing and swarm drones for early wildfire detection and suppression. Crewed aircraft add to this, with police, coastguard and contracted helicopters supporting wildfire, flood and rescue work. Much of the value of these assets is that they keep people out of danger, so degrading them also removes a safety margin. [29] [30]

Drones are the more exposed of the two. Most commercial small drones rely on GNSS for stable position hold, waypoint navigation, return-to-home and georeferenced mapping, and many carry no inertial or optical backup able to sustain a flight without it. When the satellite signal is lost or degraded, a typical drone drifts and drops into a fail-safe mode; it hovers, attempts a return-to-home that can itself fail without a position fix, or lands. It also cannot fly the autonomous survey and mapping missions that make it useful at a large incident, and any georeferenced mapping loses accuracy, which matters most when the drone is standing in for lost satellite imagery. Solar flares and geomagnetic storms are named causes of exactly this kind of GNSS degradation, so the event that starts the wider scenario also reaches the drone fleet. Purpose-built platforms with optical or inertial navigation can fly through a GNSS outage, but these are not yet standard across UK fire fleets. [31] [32]

Crewed aircraft are more resilient, because they keep inertial and ground-based navigation, line-of-sight VHF radio and the option of flying visually, so basic tasking continues. Even so, the same space weather degrades satellite-based approaches and surveillance, can black out HF and satellite communications, and raises radiation at altitude and latitude, which is why civil aviation runs a dedicated space-weather advisory service and reroutes or adjusts flights during severe events. The tasks that suffer are the precise, night-time and beyond-line-of-sight ones. The practical point for the sector is that the aerial-survey fallback the earlier sections rely on is itself weakened by the same event, most sharply for drones. Air support should be treated as a degraded capability in this scenario rather than a clean workaround, and it may be in higher demand at exactly the moment it becomes less reliable. [33] [34]

### 5.4 Supply chain

In the supply chain the sector is mostly a downstream casualty of the role GNSS plays in logistics and utilities, and any effects are weighted toward the acute phase.

Fuel and road logistics are the first concern. The Government study identifies road, maritime and emergency services as bearing the brunt of a GNSS disruption, with transport a primary GNSS-dependent domain. Diesel resupply to stations depends on road-freight scheduling, telematics and forecourt-payment systems that use GNSS-derived timing. A short outage is absorbed from station reserves. A prolonged one, or one that coincides with panic-buying, is the more serious vulnerability. [2] [3]

Equipment, PPE and consumables move on the same just-in-time, port-dependent logistics. Foam concentrate, breathing-apparatus consumables, medical oxygen and vehicle parts cause little stress over a few days, but become a real concern if degradation runs into weeks.

Water for firefighting depends on utility SCADA and telemetry, which use precise timing and sit on top of the power and communications layers already discussed. Localised power loss combined with degraded telemetry is a plausible route to reduced hydrant pressure in an affected area, at the point when demand is highest. [8]

The common factor is interdependency. Because fire and rescue services sit downstream of power and telecommunications, their supply-chain risk is largely the combined risk of those two sectors coming under strain at the same time.

## 6 Disruption coinciding with rising demand

The event that degrades the space layer is the same event that raises demand. A severe geomagnetic storm, or the extreme terrestrial weather that often accompanies these scenarios, drives electrical faults and fires, and if it coincides with flooding it produces spate conditions; many simultaneous incidents while dispatch, caller location, communications and mapping all run in a degraded state. The UK has treated severe space weather as a national risk since 2010, and the May 2024 Gannon storm prompted a UK review that made fourteen recommendations across four government departments to prepare for this low-probability, high-impact reasonable-worst-case. The greatest strain comes when the outage and the surge in demand arrive together. [18] [20]

## 7 Existing mitigations and inherent resilience

Several features already reduce this exposure and deserve credit in any assessment:

- TETRA voice is more robust than LTE data;
- the safety-critical fireground procedures run on voice and paper;
- gazetteer and GIS mapping, and manual dispatch override, survive a GNSS outage; [16]
- holdover clocks sit in both network and control-room timing; [15]
- the National Timing Centre and the wider UK Positioning, Navigation and Timing programme are deliberately moving the country from reliance on satellites toward a mix of positioning and timing sources. [5] [7] [8]

## 8 Recommended lines of enquiry for a tabletop exercise

If it is run as an exercise, these questions are some of the most useful:

- How long can control-room and radio timing hold before services degrade, and is that figure documented and tested?
- What is the written manual fallback for nearest-appliance dispatch, and when was it last rehearsed under load?
- How are 999 callers located with no Advanced Mobile Location, and what does that do to call and search times?
- What is the fireground plan if any GPS-based crew tracking is lost while radio load is already high?
- Where do satellite wildfire and flood feeds sit in the incident-command picture, and what non-satellite backup exists for perimeter and flood-extent intelligence?
- What are station fuel-holding levels, and at what point does degraded resupply constrain operations?
- Which supplier and utility dependencies (fuel, water and SCADA, PPE consumables) share the same power and telecommunications single points of failure?
- How much warning would the service realistically get, and does any continuity plan assume days of notice that a fast-moving orbital or space-weather event might not provide?
- What is the plan for drone operations in a GNSS-degraded environment, including manual-flight competency and non-satellite georeferencing, and how much does the incident-command picture rely on drones as a mapping fallback?

## 9 Caveats and limitations

This analysis reasons beyond its source. Thiele et al. present an orbital-mechanics argument about low Earth orbit collision risk; the paper makes no claims about GNSS, the emergency services or the UK. The link from a 2.5-day CRASH Clock to a fire-service impact runs through the shared solar-storm trigger and through separately evidenced data on UK dependency. [1]

The severity is calibrated, not alarmist. The credible worst case is a compound, multi-day degradation that coincides with surge demand, not a permanent loss of capability. Most individual effects are delays and degradations, and several of the sector's most safety-critical functions are inherently resilient to the failure modes described. The single-collision tail is included as a low-probability, high-impact possibility rather than a forecast; the CRASH Clock indicates shrinking margin, not an imminent event, and it does not by itself measure a Kessler cascade.

The figures need local validation. The numbers on network holdover, mobilising-system fallback and supply-chain reserves are indicative, and several are drawn from general or international sources. Each service should confirm them against its own systems, contracts and continuity plans before relying on this analysis.

## Appendix A: Sources

Bracketed numbers in the text correspond to the entries below. Sources that are encyclopaedia entries, vendor pages or trade publications are used for orientation and corroboration. The primary evidence on UK dependency comes from the government-commissioned and academic references [1], [2], [3], [18], [19] and [20]. The updated CRASH Clock values come from the authors' later writing and the Outer Space Institute tracker [26] and [27], with further context from [25] and [28]. The air-asset section draws on [29] to [34].

**[1]** Thiele, S., Heiland, S. R., Boley, A. C., & Lawler, S. M. (2026). An Orbital House of Cards: Frequent Megaconstellation Close Conjunctions. arXiv:2512.09643. <https://arxiv.org/abs/2512.09643>

**[2]** London Economics (2017). The economic impact to the UK of a disruption to GNSS (commissioned by Innovate UK / UK Space Agency). <https://londoneconomics.co.uk/wp-content/uploads/2017/10/LE-IUK-Economic-impact-to-UK-of-a-disruption-to-GNSS-SHOWCASE-PUBLISH-S2C190517.pdf>

**[3]** Inside GNSS (2018). UK Study Indicates Just How Costly a GNSS Disruption Can Be. <https://insidegnss.com/uk-study-indicates-just-how-costly-a-gnss-disruption-can-be/>

**[4]** GPS World (2020). Timing center to protect UK from risk of satellite failure. <https://www.gpsworld.com/timing-center-to-protect-uk-from-risk-of-satellite-failure/>

**[5]** Resilient Navigation & Timing Foundation / Inside GNSS (2026). UK Invests £180 Million in National Timing Centre to Back Up GNSS. <https://rntfnd.org/2026/03/11/uk-invests-180-million-in-national-timing-centre-to-back-up-gnss-inside-gnss/>

**[6]** GOV.UK (2023). Critical services to be better protected from satellite data disruptions through new Position, Navigation and Timing framework. <https://www.gov.uk/government/news/critical-services-to-be-better-protected-from-satellite-data-disruptions-through-new-position-navigation-and-timing-framework>

**[7]** GOV.UK. Positioning, Navigation and Timing: Overview (guidance). <https://www.gov.uk/guidance/positioning-navigation-and-timing-overview>

**[8]** Digital Forensics Magazine (2025). UK Acts on Weak Link in Modern Infrastructure. <https://digitalforensicsmagazine.com/uk_acts_on_weak_link_in_modern_infrastructure/>

**[9]** Panorama Antennas. UK Emergency Services Network (ESN) ready antennas. <https://panorama-antennas.com/uk-esn-antenna/>

**[10]** Emergency Services Network (Wikipedia entry). <https://en.wikipedia.org/wiki/Emergency_Services_Network>

**[11]** CSL Group (2026). The UK's Emergency Services Network: The Delays and What It Means for Connectivity Resilience. <https://www.csl-group.com/blogs/uk-emergency-services-network-connectivity-resilience-2026/>

**[12]** Airwave Solutions (Wikipedia entry). <https://en.wikipedia.org/wiki/Airwave_Solutions>

**[13]** Furuno. Timing & Synchronization technology adopted base station for mobile telecommunications (case study). <https://www.furuno.com/en/gnss/case/furuno02>

**[14]** 6G Resilience: White Paper (2025). arXiv:2509.09005. <https://arxiv.org/pdf/2509.09005>

**[15]** Meshed GPS time synchronized network (US Patent 10,021,661): LTE base-station GPS timing and holdover behaviour. <https://patents.google.com/patent/US10021661B2/en>

**[16]** National Fire Chiefs Council / UKFRS. Use technology to mobilise fire and rescue service resources (guidance). <https://www.ukfrs.com/guidance/search/use-technology-mobilise-fire-and-rescue-service-resources>

**[17]** RadioMobile (2026). How AVL Improves Fire & EMS Department Response Times. <https://radiomobile.com/how-avl-improves-fire-ems-department-response-times/>

**[18]** Hapgood, M. et al. (2021). Development of Space Weather Reasonable Worst-Case Scenarios for the UK National Risk Assessment. Space Weather, 19. <https://agupubs.onlinelibrary.wiley.com/doi/10.1029/2020SW002593>

**[19]** European Commission Joint Research Centre. Space Weather and Power Grids: Findings and Outlook (including the National Grid transmission assessment). <https://publications.jrc.ec.europa.eu/repository/bitstream/JRC86658/lbna26370enn.pdf>

**[20]** Royal Society Open Science (2026). The May 2024 geomagnetic storm: UK experience and perspective. <https://royalsocietypublishing.org/rsos/article/13/4/251943/481540/The-May-2024-geomagnetic-storm-UK-experience-and>

**[21]** Copernicus Emergency Management Service. On-Demand Mapping. <https://mapping.emergency.copernicus.eu/>

**[22]** Scottish Environment Protection Agency (2024). SEPA launch innovative Satellite Emergency Mapping Service. <https://beta.sepa.scot/news/2024/sepa-launch-innovative-satellite-emergency-mapping-service-to-boost-environmental-crisis-response/>

**[23]** European Space Agency. Using satellites in the fight against forest fires (REMSAT). <https://www.esa.int/Applications/Connectivity_and_Secure_Communications/Using_satellites_in_the_fight_against_forest_fires>

**[24]** Inside Ecology (2026). Improving Wildfire, Flood, And Disaster Response With Real-Time Satellite Imagery. <https://insideecology.com/2026/01/09/improving-wildfire-flood-and-disaster-response-with-real-time-satellite-imagery/>

**[25]** Radisic, G. & Lawler, S. / The Conversation (2026). Too many satellites? Earth's orbit is on track for a catastrophe, but we can stop it. <https://theconversation.com/too-many-satellites-earths-orbit-is-on-track-for-a-catastrophe-but-we-can-stop-it-275430>

**[26]** Thiele, S., Heiland, S., Boley, A. & Lawler, S. / The Conversation (2026). A new CRASH Clock measures the chance of satellite collisions, and it's ticking down fast. <https://theconversation.com/a-new-crash-clock-measures-the-chance-of-satellite-collisions-and-its-ticking-down-fast-283481>

**[27]** Outer Space Institute. CRASH Clock (running values and methodology). <https://outerspaceinstitute.ca/crashclock/>

**[28]** IEEE Spectrum (2026). Could a Solar Storm Trigger a Satellite Collision Crisis? (interview with the CRASH Clock authors). <https://spectrum.ieee.org/kessler-syndrome-crash-clock>

**[29]** London Fire Brigade. Drones (aerial survey and thermal imaging at incidents). <https://www.london-fire.gov.uk/about-us/services-and-facilities/vehicles-and-equipment/drones/>

**[30]** Lancashire Fire and Rescue Service (2024). Lancashire Fire and Rescue Service tests drones with Windracers for wildfire prevention. <https://www.lancsfirerescue.org.uk/news-and-events/lancashire-fire-and-rescue-service-tests-drones-with-windracers-for-wildfire-prevention>

**[31]** A Survey of Security Challenges and Solutions for UAS Traffic Management and small Unmanned Aerial Systems (2026). arXiv:2601.08229 (loss of GNSS position fix triggers UAS fail-safe modes; most commercial small UAS rely on GPS). <https://arxiv.org/pdf/2601.08229>

**[32]** UAV Navigation (Grupo Oesia). Resilient Autopilot for GPS-Denied Environments (lists solar flares and geomagnetic storms among causes of GPS degradation). <https://uavnavigation.com/company/blog/resilient-autopilot-gps-denied-environments>

**[33]** SKYbrary. Impact of Space Weather on Aviation (GNSS, HF and satellite communications, radiation, and available backups). <https://skybrary.aero/articles/impact-space-weather-aviation>

**[34]** Federal Aviation Administration. Space Weather (GNSS-based landing degradation; instrument landing systems as a GPS backup). <https://www.faa.gov/nextgen/programs/weather/awrp/space-weather>

---

*Draft for discussion; not an official risk assessment.*
