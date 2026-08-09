# Purdue Model Mapping — Simplified Oil & Gas Network Segmentation

## Objective
Map a simplified oil & gas industrial network against the Purdue Enterprise 
Reference Architecture (Purdue Model) to demonstrate understanding of IT/OT 
segmentation principles used in critical infrastructure environments, and 
apply IEC 62443 zone/conduit concepts to justify the design.

## Why This Project
Entry-level security candidates almost universally only demonstrate IT 
security skills (endpoint, network, cloud). Oil & gas hiring managers need 
candidates who understand that OT environments (SCADA, PLCs, HMIs) cannot 
be treated like IT networks — the same tools and techniques (aggressive 
scanning, patching cadence, exploitation testing) that are safe in IT can 
cause physical safety incidents in OT. This project demonstrates that 
judgment before I have OT hardware access.

## Environment / Tools
- Draw.io (or similar) for the architecture diagram
- Cisco Packet Tracer for the IT-side network simulation
- Reference: Purdue Enterprise Reference Architecture (PERA), IEC 62443 
  zone/conduit model

## What I Did
1. Diagrammed a simplified 6-level Purdue Model network (Level 0 physical 
   process through Level 5 enterprise IT), labeling each level with 
   representative oil & gas assets (e.g., PLCs/RTUs at Level 0-1, HMI/
   SCADA servers at Level 2, historian/MES at Level 3, corporate IT at 
   Level 4-5).
2. Identified the Level 3.5 "Industrial DMZ" as the critical segmentation 
   boundary and explained its purpose.
3. Applied IEC 62443 zone/conduit terminology to define trust boundaries 
   between zones.
4. Built a companion IT-side topology in Packet Tracer showing VLAN 
   segmentation and ACLs enforcing the Level 3.5 DMZ boundary.

## Findings / Key Design Decisions
- The Level 3.5 Industrial DMZ enforces a strict one-way data flow: Level 3 (OT) pushes replicated data out to the DMZ, and Level 4 (IT) retrieves it from there — the two levels never open a direct session with each other. The critical design choice isn't just that OT can push out, but that the DMZ is never permitted to push into OT. If the DMZ could initiate a connection back into the control network, that would require an access point on the OT side for it to connect through — and that access point is exactly what an attacker would need. Because that access point simply doesn't exist, even a fully compromised DMZ leaves an attacker with nowhere further to go: no path, no way in.

Without this boundary, the IT/OT line disappears entirely. A single phishing click on the corporate network becomes the starting point for an attacker to move straight across into the control network, with no gatekeeper in between. From there, HMIs are the highest-value target — they command the PLCs that control physical equipment, so a compromised corporate workstation could give an attacker a direct path to manipulating real physical processes, not just stealing data.

## What I'd Do Differently in Production
- This is a conceptual/simplified mapping. A real OT security assessment 
  would require asset inventory tools appropriate for OT (e.g., passive 
  network monitoring like Claroty/Nozomi-class tools), not active 
  scanning, and would involve OT engineers, not just IT/security staff, 
  in any change to the environment.

## Screenshots
[https://github.com/carmelin-neto/ot-purdue-model-mapping/blob/main/screenshots/purdue-model-oilgas..drawio.png, packet-tracer-topology.png]
