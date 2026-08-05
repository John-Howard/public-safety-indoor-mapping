# Orbital Fragility and the UK Fire and Rescue Sector

*An invisible dependency: how a low Earth orbit collision cascade and severe space weather would reach the fire and rescue sector, and what to test before it does*

> **Briefing paper.** Version 1.5.2, issued 4 August 2026. Horizon-scanning paper, not a formal risk assessment.

## Document control

| Field | Detail |
|---|---|
| **Title** | Orbital Fragility and the UK Fire and Rescue Sector |
| **Document type** | Analytical thought experiment / horizon-scanning briefing |
| **Version** | 1.5.2 |
| **Status** | Issued |
| **Date issued** | 4 August 2026 |
| **Author and owner** | John Howard, Data Services Manager, Hertfordshire Fire and Rescue Service. The views expressed are the author's own. |
| **Classification** | Unrestricted. Suitable for circulation within the sector and to partner agencies. |
| **Evidence base** | Thiele et al. (2026a), Acta Astronautica; the Outer Space Institute CRASH Clock; National Risk Register 2026; published UK dependency evidence |
| **Intended use** | Risk-register input and tabletop-exercise seed material |
| **Scope** | Comprehensive scenario (orbital cascade plus GNSS and space weather), acute phase emphasised |
| **Review trigger** | A material change in the CRASH Clock value, publication of the next National Risk Register, or a significant orbital fragmentation event |
| **Next scheduled review** | February 2027, or earlier if a review trigger occurs |

## Version history

| Version | Date | Summary of change |
|---|---|---|
| **1.0** | 1 July 2026 | First issue. Structured thought experiment covering supply chain, communications and command and control, and operational response and firefighter safety. |
| **1.1** | 1 July 2026 | Plain-language edit. No change to analysis or conclusions. |
| **1.2** | 1 July 2026 | CRASH Clock values updated to the May 2026 figure. Single-collision tail risk expanded into its own section. |
| **1.3** | 1 July 2026 | Air assets added as a fourth dimension, covering drones and crewed aircraft in a degraded satellite environment. |
| **1.4** | 18 July 2026 | National Risk Register 2026 incorporated following its publication on 14 July 2026. |
| **1.5** | 4 August 2026 | Restructured around a five-step central argument. Referencing converted to Harvard. Source paper updated to the peer-reviewed Acta Astronautica version. Emergency Services Network timetable, National Risk Register matrix scores and the deliberate-disruption risk entry added. Sources audited and weakest references replaced. |
| **1.5.1** | 4 August 2026 | Corrected the National Fire Chiefs Council reference. The cited ukfrs.com address no longer resolves following migration of the guidance to the NFCC site, and the citation now points to the Fire Control Guidance framework. No change to analysis or conclusions. |
| **1.5.2** | 4 August 2026 | Primary source obtained. Section 5.1 verified against the full NFCC guidance and revised: the guidance states that automatic vehicle location works from GPS, which strengthens the dependency argument, and its current wording on reassessing a proposed attendance replaces the earlier reference to manual override. Advanced Mobile Location accuracy and the degraded-location fallback now cited to the primary source. |

> **How to read this document.** This is a structured thought experiment, not a formal service risk assessment. It builds a single argument in five steps, set out in Section 3, and each following section takes one step. Sources are cited in Harvard style and listed in full under References. Validate the judgements against each service's own systems, contracts and continuity plans before using them in planning.

## Contents

- [1. Executive summary](#1-executive-summary)
- [2. Purpose and scope](#2-purpose-and-scope)
- [3. The argument in outline](#3-the-argument-in-outline)
- [4. Step one: the margin in orbit is now measured in days](#4-step-one-the-margin-in-orbit-is-now-measured-in-days)
- [5. Step two: the dependency is wider than service risk registers show](#5-step-two-the-dependency-is-wider-than-service-risk-registers-show)
  - [5.1. Communications and command and control](#51-communications-and-command-and-control)
  - [5.2. Operational response and firefighter safety](#52-operational-response-and-firefighter-safety)
  - [5.3. Supply chain](#53-supply-chain)
- [6. Step three: the fallbacks fail alongside what they replace](#6-step-three-the-fallbacks-fail-alongside-what-they-replace)
- [7. Step four: disruption would arrive with demand, and possibly without warning](#7-step-four-disruption-would-arrive-with-demand-and-possibly-without-warning)
- [8. Step five: what holds, and the case against alarmism](#8-step-five-what-holds-and-the-case-against-alarmism)
- [9. Testing the argument: questions for a tabletop exercise](#9-testing-the-argument-questions-for-a-tabletop-exercise)
- [10. Caveats and limitations](#10-caveats-and-limitations)
- [References](#references)

## 1. Executive summary

The central argument of this paper is short. The UK fire and rescue sector consumes space services without owning any space assets. That single fact makes its exposure real, indirect, and at service level largely unexamined. The orbital margin for error is now thin enough, and the plausible warning time short enough, that the exposure is worth testing before an event rather than during one.

- The margin in orbit is now measured in days. The CRASH Clock, the expected time to a catastrophic collision in low Earth orbit if satellites lost the ability to manoeuvre, has fallen from 164 days in 2018 to 2.5 days in May 2026. The underlying research has now completed peer review and is published in Acta Astronautica (Thiele et al., 2026a; Outer Space Institute, 2026).

- The Government has reached a similar conclusion. The National Risk Register 2026 makes a major orbital collision the reasonable worst case for disruption to space-based services, scores it at a 5 to 25 per cent likelihood over five years with significant impact, and names the emergency services among the cascading effects (Cabinet Office, 2026).

- The dependency is wider than service risk registers show. It runs through 999 caller location, nearest-appliance mobilising, the radio and cellular backbone, satellite mapping for wildfire and flood, and the road logistics behind fuel and equipment.

- The fallbacks are coupled to the things they replace. Drones, the natural substitute for lost satellite imagery, depend on the same satellite positioning, so the aerial-survey fallback weakens in the same event and may be in higher demand as it does so.

- Disruption would arrive with demand, and possibly without warning. The storm that degrades the space layer also drives fires and flooding, and a triggering event could produce a collision within hours, so plans should not assume days of notice.

- Much of the sector's core holds. Entry control, crew accountability and mayday procedures run on voice and paper. Network timing has holdover, TETRA remains in service into 2029 or beyond, and a national grid collapse is assessed as very unlikely (National Audit Office, 2023; TelcoTitans, 2026).

- The credible worst case is a compound, multi-day degradation coinciding with surge demand, not a permanent loss of capability. The recommended response is a set of low-cost questions set out in Section 9, worth asking even if the triggering event never occurs.

## 2. Purpose and scope

This paper tests the UK fire and rescue sector against the orbital-fragility scenario described by Thiele et al. (2026a), taken together with the severe space-weather event most likely to trigger it. It is intended as horizon-scanning input to service risk registers and as seed material for a tabletop exercise. It covers three operational dimensions, namely communications and command and control, operational response and firefighter safety, and supply chain, and adds air assets as a fourth because they are the fallback the other three quietly assume. The acute phase, from the first hours to a few weeks, receives most of the attention, with prolonged degradation over months to years treated as secondary.

This version differs from earlier versions in structure rather than conclusions. It is organised around a single argument instead of topic by topic, after feedback that the analysis needed a clearer spine.

## 3. The argument in outline

The case runs in five steps, and Sections 4 to 8 take one step each.

1.  The margin for error in low Earth orbit has collapsed, and the most plausible trigger is a severe solar storm that would also reach the ground.

2.  The sector's dependency on space services is broader and deeper than its risk registers record.

3.  The parts of the plan assumed to back each other up would fail together, because they share the same satellite layer.

4.  The event would arrive with its own demand surge and potentially with almost no warning.

5.  The sector's most safety-critical functions are inherently resilient, and the remaining exposure can be tested cheaply and now.

## 4. Step one: the margin in orbit is now measured in days

The threat environment has changed, and recent orbital-mechanics research puts a number on it. Thiele et al. (2026a) propose the CRASH Clock, the expected time to a catastrophic collision in low Earth orbit if satellites lost the ability to manoeuvre or operators lost the situational awareness needed to direct them. At the start of 2018, before the mega-constellation era, the figure was 164 days. The Outer Space Institute (2026) tracker records 5.5 days in June 2025, 3.8 days in January 2026, 3 days in March 2026 and 2.5 days in May 2026. Satellites are now launched at roughly 100 per week. Separately, Lewis and Kessler (2025) find that the main Starlink shell near 550 km is the only altitude below 800 km sitting above the collisional runaway threshold, which means a single collision there could have long-term consequences.

Two features matter more to emergency planners than the headline number. The first is the tail. The clock is an average, and the authors' simulations show that after a widespread loss of control a first collision might take weeks, or might occur within a few hours. A single collision, even one that never becomes a runaway, would stress the orbital environment for months, because cataloguing even half the debris from a fragmentation event typically takes around 100 days. Fragmentation already happens for ordinary reasons, such as the internal break-up of a Starlink satellite in March 2026 (Thiele et al., 2026b).

The second is the trigger list. The authors name an extreme solar storm, a defective software update and a cybersecurity event as plausible causes of the control loss the clock assumes. The researchers are careful to say the clock measures shrinking margin rather than an imminent event, and that it is not a countdown to Kessler syndrome, which develops over decades. They also argue the trajectory is not inevitable and could be slowed by coordinated space-traffic management, which currently has no equivalent to the system used in civil aviation (Radisic and Lawler, 2026; IEEE Spectrum, 2026). This paper adopts both cautions. The narrower planning conclusion is that the orbital system now depends on continuously errorless operation, and the most plausible single point of failure is an event that would strike systems on the ground at the same time.

One point of standing worth noting is that this work is no longer a preprint. It has completed peer review and is published in Acta Astronautica, and the metric has been presented to the UN Committee on the Peaceful Uses of Outer Space (Boley, 2026). It is reasonable for a service to treat it as established research rather than speculation.

## 5. Step two: the dependency is wider than service risk registers show

The second step traces where the event would actually reach a fire and rescue service. The key distinction is between two loss pathways that share one trigger. A severe geomagnetic storm acts directly on ground systems. It degrades satellite positioning and timing through ionospheric disturbance, disrupts HF radio, and puts the power grid under strain at the margins (Hapgood et al., 2021). Separately and more slowly, an orbital collision cascade would remove low Earth orbit assets over weeks to months, chiefly the Earth-observation constellations used for wildfire and flood mapping. GPS and Galileo would survive a cascade physically, because they orbit far above the congested shells, so their acute vulnerability is to the storm rather than the debris (Thiele et al., 2026a).

The Government no longer treats these as two separate problems. The National Risk Register 2026 scores both severe space weather and disruption to space-based services at a 5 to 25 per cent likelihood over five years, with significant impact. Its severe space weather entry goes further. It states that the catalogue of tracked objects in orbit would be significantly impacted, raising the risk of on-orbit collisions. The register also carries a loss of positioning, navigation and timing risk, whose variations expressly include severe space weather and whose reasonable worst case names the emergency services, and a fourth entry covering deliberate disruption of UK space systems by a state or proxy. The register is the public version of the National Security Risk Assessment and now lists 95 risks in total (Cabinet Office, 2026; Jones, 2026).

The table below summarises the dependency before each dimension is examined.

| Dimension | Main dependency at risk | Acute-phase effect | Built-in resilience |
|---|---|---|---|
| Communications and command and control | GNSS timing for cellular networks; 999 caller location; automatic vehicle location | Weaker caller location; loss of automatic nearest-appliance dispatch; cellular timing holds only hours | TETRA robust and in service for years yet; gazetteer, GIS and manual override survive |
| Operational response and firefighter safety | In-cab navigation; Earth observation for wildfire and flood; any GPS crew tracking | Slower dispatch and routing; loss of near-real-time fire and flood mapping; higher radio load | Entry control, accountability and mayday run on voice and paper and do not fail with GNSS |
| Air assets (Section 6) | GNSS for drone position hold, navigation, return-to-home and mapping | Drones drift, enter fail-safe or ground; the aerial-survey fallback is weakened | Crewed aircraft retain inertial and ground-based navigation and visual flight |
| Supply chain | GNSS-timed road logistics, fuel and payments; utility SCADA; power | Friction in fuel and parts resupply; possible localised water-pressure or telemetry issues | Station fuel reserves; stockholding absorbs short outages; grid collapse assessed very unlikely |

### 5.1 Communications and command and control

The clearest dependency is at the call-handling stage. The largest single emergency-service benefit from GNSS comes from public-safety answering points using satellite-derived handset location to shorten call and search times (London Economics, 2017; Inside GNSS, 2018). In the UK this arrives as Advanced Mobile Location, retrieved through the enhanced information service for emergency calls, and national guidance puts its accuracy at about three metres, available within roughly twenty seconds of the call information being received (National Fire Chiefs Council, 2026). The fallback is far coarser. The same guidance notes that where no valid calling line identity exists, the location available is a cell or zone code that identifies the general area for routing the call but does not establish where the caller actually is. A district is not a doorway. Call times lengthen and crews are sent out to search.

Control-room mobilising leans on satellite positioning but is not captive to it. National guidance describes mobilising systems that build a predetermined attendance from an address-based gazetteer and the incident type, then use an automatic vehicle location system to calculate travel times and propose the nearest appropriate resource, with fire control able to reassess that proposal if something closer becomes available. The same guidance states plainly that automatic vehicle location works from GPS, which is what couples this stage of mobilising to the scenario (National Fire Chiefs Council, 2026). The predetermined attendance itself is not coupled to it. The gazetteer, the incident-type list and the risk layers held in the geographic information system, including site-specific risk information, hydrant data, tactical plans and pre-planned rendezvous points, are stored data and survive an outage. What degrades is the travel-time calculation that identifies the nearest appliance, leaving the predetermined attendance and human judgement, a difference worth roughly 15 to 25 per cent on response times where services have measured it (RadioMobile, 2026).

The radio backbone is where timing matters most, and the timetable has moved. Airwave, the TETRA network, remains the operational backbone for all 108 police, fire and ambulance services in England, Scotland and Wales (National Audit Office, 2023). Its replacement, the Emergency Services Network, is designed to run over commercial 4G and 5G infrastructure instead of a dedicated network (Home Office, 2024), and is now expected to go live in mid-2028, with mass transition completing around 2030 and Airwave closure pushed to late 2029 (TelcoTitans, 2026; CSL Group, 2026). That timetable cuts both ways. It means the sector keeps the more robust technology for several years yet, because TETRA depends on precise satellite timing far less than LTE does. It also means the exposure is arriving rather than receding: LTE and 5G base stations draw synchronisation from GNSS and can hold accurate time for only a few hours, up to about a day, after losing the signal, and solar activity is among the named causes of such outages ('6G resilience', 2025). The data-rich services the new network is meant to unlock, from body-worn video to live mapping, would fail before push-to-talk voice does.

### 5.2 Operational response and firefighter safety

Dispatch and navigation both slow. Nearest-appliance selection reverts to manual and in-cab routing degrades, which affects on-call and retained crews and mutual-aid responders crossing unfamiliar ground most acutely.

Crew safety and accountability are better protected, and this is the reassuring part of the picture. Breathing-apparatus entry control, crew-accountability procedures and mayday protocols are built on voice and paper and do not fail when satellite positioning does. The concern is about the direction of travel, not the present position. The sector is moving toward GPS-tagged personnel tracking and telemetry, already operational internationally, where satellite systems track a firefighter's position and relay it to a control centre while taking load off congested radio channels (European Space Agency, no date). Anything that comes to rely on that tracking degrades in this scenario, and does so while radio load is rising.

Wildfire and flood response is where the loss of low Earth orbit assets bites most directly. The Copernicus Emergency Management Service (no date) supplies satellite-based flood and wildfire mapping to authorised responders, and Scotland's Satellite Emergency Mapping Service draws on international satellite tasking arrangements, with Scottish Fire and Rescue an interested partner (Scottish Environment Protection Agency, 2024). Much of that near-real-time revisit capability comes from the dense low-orbit constellations a cascade threatens (Inside Ecology, 2026). Losing it pushes perimeter mapping, flood-extent intelligence and evacuation planning back onto aerial survey and ground observation, which are slower and less complete, and more dangerous when a fire front is moving. Section 6 explains why that fallback is itself weakened.

### 5.3 Supply chain

Here the sector is a downstream casualty of the role satellite timing plays in logistics and utilities, weighted toward the acute phase. The Government study identifies road, maritime and emergency services as bearing the brunt of a GNSS disruption, with transport a primary dependent domain (London Economics, 2017). Diesel resupply to stations depends on road-freight scheduling, telematics and forecourt-payment systems that use satellite-derived timing. A short outage is absorbed from station reserves; a prolonged one, or one coinciding with panic-buying, is the more serious vulnerability.

Equipment, personal protective equipment and consumables move on the same just-in-time, port-dependent logistics. Foam concentrate, breathing-apparatus consumables, medical oxygen and vehicle parts cause little stress over a few days but become a real concern if degradation runs into weeks. Water for firefighting depends on utility SCADA and telemetry, which use precise timing and sit on top of the power and communications layers already discussed. Localised power loss combined with degraded telemetry is a plausible route to reduced hydrant pressure, at the point when demand is highest. The common factor is interdependency: because fire and rescue sits downstream of power and telecommunications, its supply-chain risk is largely the combined risk of those two sectors coming under strain together.

## 6. Step three: the fallbacks fail alongside what they replace

The third step is the least obvious and, for planners, the most important. Layers of the response plan that look independent are coupled through the same satellite dependency.

The clearest example is aerial survey. If satellite wildfire and flood mapping were lost, the natural response is to put aircraft and drones over the incident. Most UK services now operate drones for reconnaissance, thermal imaging and incident overview; London Fire Brigade (no date) uses them to give commanders a live aerial picture and to reach areas unsafe for firefighters, and Lancashire Fire and Rescue Service (2024) has trialled autonomous fixed-wing and swarm drones for early wildfire detection. But most commercial small drones depend on satellite positioning for stable position hold, waypoint navigation, return-to-home and georeferenced mapping, and many carry no inertial or optical backup capable of sustaining a flight without it ('A survey of security challenges', 2026). In a degraded environment a typical drone drifts, enters a fail-safe mode or grounds, and cannot fly the autonomous mapping missions that make it useful at a large incident. Solar flares and geomagnetic storms are documented causes of precisely this degradation (UAV Navigation, no date).

Crewed aircraft are more resilient, retaining inertial and ground-based navigation and the option of flying visually, so basic tasking continues. Even so, the same space weather degrades satellite-based approaches, disrupts HF and satellite communications and raises radiation at altitude, which is why civil aviation runs a dedicated space-weather advisory service and adjusts operations during severe events (SKYbrary, no date; Federal Aviation Administration, no date). The tasks that suffer are the precise, night-time and beyond-line-of-sight ones.

The lesson generalises beyond aircraft. GPS-tagged personnel tracking degrades in the same event that raises radio load. Data services carried over commercial cellular fail before voice does. Capabilities acquired separately, each for good operational reasons, converge on a single upstream dependency that no individual procurement decision examined. Air support should therefore be treated as a degraded capability in this scenario rather than a clean workaround, and it may be in higher demand exactly when it becomes less reliable.

## 7. Step four: disruption would arrive with demand, and possibly without warning

A severe geomagnetic storm is not a quiet backdrop to a technical outage. The same event drives electrical faults and fires, and the extreme terrestrial weather featuring in national worst-case scenarios can add flooding and spate conditions. The realistic picture is many simultaneous incidents attended while caller location, nearest-appliance dispatch, cellular data and satellite mapping all run degraded. Compound timing of this kind is structurally underweighted by assessments that score hazards one at a time, because the severity here lives almost entirely in the coincidence.

The register gives that condition a duration. Its severe space weather reasonable worst case is a Carrington-scale event lasting one to two weeks, with several solar phenomena recurring across the period (Cabinet Office, 2026). Severe space weather has been on the register since 2011, and the May 2024 Gannon storm prompted a cross-government review and continuing scrutiny of national preparedness (Royal Society, 2026; National Audit Office, 2026).

Warning time compounds the problem. Space-weather forecasting gives hours to a few days of notice for a major storm, and the single-collision tail described in Section 4 means the orbital consequence could begin within hours of the trigger. Continuity plans that implicitly assume days of notice, time to fuel vehicles, print maps, brief crews and stand up fallback procedures, are assuming a courtesy this event may not extend. Fallbacks therefore need to be rehearsed in advance, because there may be no window in which to rehearse them.

## 8. Step five: what holds, and the case against alarmism

An argument for attention is not an argument for alarm, and credibility with sceptical readers depends on stating plainly what does not break.

The grid is more robust than often assumed. National Grid's transmission assessment concluded that widespread transformer damage and a collapse of the interconnected grid are very unlikely. A Carrington-type storm would damage a small number of transformers, at high cost to the operator but with little effect on end users (European Commission Joint Research Centre, no date). Planning should assume localised rather than national power loss.

Timing has holdover. The Government's economic study found timing applications comparatively resilient across a five-day GNSS outage where holdover clocks or alternative sources exist (London Economics, 2017). The National Timing Centre is being built to carry telecommunications and the emergency services through exactly this event. It is a network of atomic clocks independent of satellite signals, and it attracted a further £180 million in 2026. Behind it sits a deliberate national shift away from dependence on satellites toward a mix of timing sources (Resilient Navigation and Timing Foundation, 2026; UK Government, 2023).

The sector's safety-critical core holds, for the reasons given in Section 5.2: the procedures that keep firefighters accounted for do not touch a satellite at any point. Gazetteer and GIS mapping and manual dispatch override also survive an outage, and TETRA voice stays in service for several more years while remaining far less timing-dependent than its successor.

The honest characterisation of the credible worst case is therefore a compound, multi-day degradation coinciding with surge demand: slower call handling, manual dispatch, thinner communications, lost mapping and weakened air support, all at once, for days. That is serious. It is not a collapse, and describing it as one would discredit the argument.

## 9. Testing the argument: questions for a tabletop exercise

If the argument is accepted, the practical response does not begin with procurement. It begins with questions that are cheap to ask, that convert an invisible dependency into a documented one, and that are worth asking even if the triggering event never occurs, because they also cover mundane satellite loss from jamming, interference or equipment failure.

6.  How long can control-room and radio timing hold without satellite input before services degrade, and is that figure documented and tested rather than assumed?

7.  What is the written manual fallback for nearest-appliance dispatch, and when was it last rehearsed under realistic load?

8.  How are 999 callers located without Advanced Mobile Location, and what does that do to call and search times?

9.  What is the fireground plan if any GPS-based crew tracking is lost while radio load is already high?

10. Where do satellite wildfire and flood feeds sit in the incident-command picture, and what non-satellite backup exists for perimeter and flood-extent intelligence?

11. What is the plan for drone operations in a degraded environment, including manual-flight competency and non-satellite georeferencing, and how far does the command picture rely on drones as the mapping fallback?

12. What are station fuel-holding levels, and at what point does degraded resupply constrain operations?

13. Which supplier and utility dependencies, including fuel, water and SCADA, and protective-equipment consumables, share the same power and telecommunications single points of failure?

14. Does any continuity plan assume days of notice that a fast-moving orbital or space-weather event might not provide?

15. Does the service risk register reference the relevant National Risk Register 2026 entries, namely disruption to space-based services, loss of positioning, navigation and timing, severe space weather, and deliberate disruption of UK space systems, and are local plans consistent with their reasonable worst cases?

16. Has the service considered responding to the Government's call for views on the Civil Contingencies Act 2004, where dependency of this kind is directly relevant?

Most services will find they can answer some of these immediately and others not at all. The gaps are the finding.

## 10. Caveats and limitations

This analysis reasons beyond its primary source. Thiele et al. (2026a) present an orbital-mechanics argument and make no claims about satellite navigation, the emergency services or the UK. The bridge to fire-service impact runs through the shared solar-storm trigger and separately evidenced UK dependency data. That bridge is now partly official, since the National Risk Register 2026 treats a major orbital collision as the reasonable worst case for disruption to space-based services and links severe space weather to raised on-orbit collision risk. What remains the researchers' claim alone is the specific timeline, the 2.5-day clock and its hours-scale tail.

This is a calibrated assessment, not an alarmist one. The credible worst case is a compound, multi-day degradation coinciding with surge demand, not a permanent loss of capability, and several of the sector's most safety-critical functions are inherently resilient to the failure modes described. The single-collision tail is included as a low-probability, high-impact possibility, not a forecast.

Figures need local validation. The numbers on network holdover, mobilising fallbacks and supply reserves are indicative, and several are drawn from general or international sources, not fire-sector evidence. The risk-matrix scores quoted from the National Risk Register are read from the plotted positions in the published matrices, which do not state numerical values in the text. Each service should confirm the operational figures against its own systems, contracts and continuity plans before relying on this analysis.

Some sources are undated web pages, marked accordingly in the reference list, and the currency of the CRASH Clock figure should be checked against the Outer Space Institute tracker before reuse, since it has been revised several times a year.

## References

References follow the Harvard author-date system. Where a source carries no publication date, it is marked 'no date' rather than assigned an estimated year.

Boley, A.C. (2026) *The CRASH Clock: a key environmental indicator for the orbital environment*. Technical presentation to the UN Committee on the Peaceful Uses of Outer Space, Scientific and Technical Subcommittee, Vienna, 11 February. Available at: <https://www.unoosa.org/documents/pdf/copuos/stsc/2026/List_of_TP/Wednesday11/AM/PDF_Slides_OSI_Aaron_Boley_Item_11.pdf> (Accessed: 4 August 2026).

Cabinet Office (2026) *National Risk Register 2026*. London: Cabinet Office. Available at: <https://www.gov.uk/government/publications/national-risk-register-2026> (Accessed: 4 August 2026).

Copernicus Emergency Management Service (no date) *On-demand mapping*. Available at: <https://mapping.emergency.copernicus.eu/> (Accessed: 4 August 2026).

CSL Group (2026) *The UK's Emergency Services Network: the delays and what it means for connectivity resilience*. Available at: <https://www.csl-group.com/blogs/uk-emergency-services-network-connectivity-resilience-2026/> (Accessed: 4 August 2026).

European Commission Joint Research Centre (no date) *Space weather and power grids: findings and outlook*. Luxembourg: Publications Office of the European Union. Available at: <https://publications.jrc.ec.europa.eu/repository/bitstream/JRC86658/lbna26370enn.pdf> (Accessed: 4 August 2026).

European Space Agency (no date) *Using satellites in the fight against forest fires*. Available at: <https://www.esa.int/Applications/Connectivity_and_Secure_Communications/Using_satellites_in_the_fight_against_forest_fires> (Accessed: 4 August 2026).

Federal Aviation Administration (no date) *Space weather*. Available at: <https://www.faa.gov/nextgen/programs/weather/awrp/space-weather> (Accessed: 4 August 2026).

Hapgood, M., Angling, M.J., Attrill, G. et al. (2021) 'Development of space weather reasonable worst-case scenarios for the UK National Risk Assessment', *Space Weather*, 19(4), e2020SW002593. Available at: <https://agupubs.onlinelibrary.wiley.com/doi/10.1029/2020SW002593> (Accessed: 4 August 2026).

Home Office (2024) *Accounting officer memorandum: Emergency Services Mobile Communications Programme*. Available at: <https://www.gov.uk/government/publications/home-office-major-programmes-accounting-officer-assessments/accounting-officer-memorandum-emergency-services-mobile-communications-programme-esmcp> (Accessed: 4 August 2026).

IEEE Spectrum (2026) 'Could a solar storm trigger a satellite collision crisis?', *IEEE Spectrum*. Available at: <https://spectrum.ieee.org/kessler-syndrome-crash-clock> (Accessed: 4 August 2026).

Inside Ecology (2026) 'Improving wildfire, flood and disaster response with real-time satellite imagery', *Inside Ecology*. Available at: <https://insideecology.com/2026/01/09/improving-wildfire-flood-and-disaster-response-with-real-time-satellite-imagery/> (Accessed: 4 August 2026).

Inside GNSS (2018) 'UK study indicates just how costly a GNSS disruption can be', *Inside GNSS*. Available at: <https://insidegnss.com/uk-study-indicates-just-how-costly-a-gnss-disruption-can-be/> (Accessed: 4 August 2026).

Jones, D. (2026) *National resilience: annual statement*. Hansard, House of Commons debate, 14 July. Available at: <https://hansard.parliament.uk/Commons/2026-07-14/debates/26071462000017/NationalResilienceAnnualStatement> (Accessed: 4 August 2026).

Lancashire Fire and Rescue Service (2024) *Lancashire Fire and Rescue Service tests drones with Windracers for wildfire prevention*. Available at: <https://www.lancsfirerescue.org.uk/news-and-events/lancashire-fire-and-rescue-service-tests-drones-with-windracers-for-wildfire-prevention> (Accessed: 4 August 2026).

Lewis, H.G. and Kessler, D.J. (2025) 'Critical number of spacecraft in low Earth orbit: a new assessment of the stability of the orbital debris environment', *9th European Conference on Space Debris*. Bonn: ESA Space Debris Office. Available at: <https://conference.sdo.esoc.esa.int/proceedings/sdc9/paper/305/SDC9-paper305.pdf> (Accessed: 4 August 2026).

London Economics (2017) *The economic impact to the UK of a disruption to GNSS*. London: Innovate UK and the UK Space Agency. Available at: <https://londoneconomics.co.uk/wp-content/uploads/2017/10/LE-IUK-Economic-impact-to-UK-of-a-disruption-to-GNSS-SHOWCASE-PUBLISH-S2C190517.pdf> (Accessed: 4 August 2026).

London Fire Brigade (no date) *Drones*. Available at: <https://www.london-fire.gov.uk/about-us/services-and-facilities/vehicles-and-equipment/drones/> (Accessed: 4 August 2026).

National Audit Office (2023) *Progress with delivering the Emergency Services Network*. London: National Audit Office. Available at: <https://www.nao.org.uk/wp-content/uploads/2023/03/progress-with-delivering-the-emergency-services-network.pdf> (Accessed: 4 August 2026).

National Audit Office (2026) *The UK's resilience to severe space weather*. London: National Audit Office. Available at: <https://www.nao.org.uk/reports/the-uks-resilience-to-severe-space-weather/> (Accessed: 4 August 2026).

National Fire Chiefs Council (2026) 'Emergency call management and mobilising', *Fire Control Guidance*. 54pp. Downloaded as PDF, 4 August 2026. The guidance carries no publication date, so the year given is the year of download, and NFCC states that the content is valid only at the time of download. Access requires a free NFCC account. Previously published in part as 'Use technology to mobilise fire and rescue service resources' on ukfrs.com, which no longer resolves. Available at: <https://nfcc.org.uk/fire-control-guidance/> (Accessed: 4 August 2026).

Outer Space Institute (2026) *CRASH Clock*. Vancouver: University of British Columbia. Available at: <https://outerspaceinstitute.ca/crashclock/> (Accessed: 4 August 2026).

Radisic, G. and Lawler, S. (2026) 'Too many satellites? Earth's orbit is on track for a catastrophe, but we can stop it', *The Conversation*. Available at: <https://theconversation.com/too-many-satellites-earths-orbit-is-on-track-for-a-catastrophe-but-we-can-stop-it-275430> (Accessed: 4 August 2026).

RadioMobile (2026) *How AVL improves fire and EMS department response times*. Available at: <https://radiomobile.com/how-avl-improves-fire-ems-department-response-times/> (Accessed: 4 August 2026).

Resilient Navigation and Timing Foundation (2026) 'UK invests £180 million in National Timing Centre to back up GNSS', *RNT Foundation*. Available at: <https://rntfnd.org/2026/03/11/uk-invests-180-million-in-national-timing-centre-to-back-up-gnss-inside-gnss/> (Accessed: 4 August 2026).

Royal Society (2026) 'The May 2024 geomagnetic storm: UK experience and perspective', *Royal Society Open Science*, 13(4), 251943. Available at: <https://royalsocietypublishing.org/rsos/article/13/4/251943/481540/The-May-2024-geomagnetic-storm-UK-experience-and> (Accessed: 4 August 2026).

Scottish Environment Protection Agency (2024) *SEPA launch innovative Satellite Emergency Mapping Service*. Available at: <https://beta.sepa.scot/news/2024/sepa-launch-innovative-satellite-emergency-mapping-service-to-boost-environmental-crisis-response/> (Accessed: 4 August 2026).

SKYbrary (no date) *Impact of space weather on aviation*. Available at: <https://skybrary.aero/articles/impact-space-weather-aviation> (Accessed: 4 August 2026).

TelcoTitans (2026) 'UK's long-delayed ESN gets a new set of realistic but ambitious targets', *TelcoTitans*. Available at: <https://www.telcotitans.com/btwatch/uks-long-delayed-esn-gets-a-new-set-of-realistic-but-ambitious-targets/10318.article> (Accessed: 4 August 2026).

Thiele, S., Heiland, S.R., Boley, A.C. and Lawler, S.M. (2026a) 'An orbital house of cards: frequent satellite close conjunctions', *Acta Astronautica*, in press. Available at: <https://doi.org/10.1016/j.actaastro.2026.06.023> (Accessed: 4 August 2026).

Thiele, S., Heiland, S., Boley, A. and Lawler, S. (2026b) 'A new CRASH Clock measures the chance of satellite collisions, and it's ticking down fast', *The Conversation*. Available at: <https://theconversation.com/a-new-crash-clock-measures-the-chance-of-satellite-collisions-and-its-ticking-down-fast-283481> (Accessed: 4 August 2026).

UAV Navigation (no date) *Resilient autopilot for GPS-denied environments*. Available at: <https://uavnavigation.com/company/blog/resilient-autopilot-gps-denied-environments> (Accessed: 4 August 2026).

UK Government (2023) *Positioning, navigation and timing: overview*. Available at: <https://www.gov.uk/guidance/positioning-navigation-and-timing-overview> (Accessed: 4 August 2026).

'A survey of security challenges and solutions for UAS traffic management and small unmanned aerial systems' (2026) *arXiv*:2601.08229. Available at: <https://arxiv.org/pdf/2601.08229> (Accessed: 4 August 2026).

'6G resilience: white paper' (2025) *arXiv*:2509.09005. Available at: <https://arxiv.org/pdf/2509.09005> (Accessed: 4 August 2026).
