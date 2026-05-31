Requirement

2. A brief report describing the project idea, user study, background research on related products, finalized system specs, system architecture, component design details (include any CAD files, link to permanent repository of source code, circuit diagrams, etc), results and tests on the final product.

Draft 
Final Development Report: SAVE4223 RFID Smart Inventory Cabinet

1. Project Vision and Problem Definition

In high-traffic maker spaces such as HKUST Room 4223, maintaining an accurate inventory of hundreds of small hand tools represents a significant logistical burden. Traditional manual sign-out sheets are inherently slow and error-prone, creating a friction-heavy environment that inevitably leads to a breakdown in compliance. As tools are borrowed and returned dozens of times daily, the administrative overhead of manual tracking fails to scale. The SAVE4223 project marks a strategic shift toward a "zero-friction" accountability model, utilizing automated edge-computing and RFID sensing to ensure that every item is tracked the instant a drawer closes, requiring zero active effort from the user.

The system addresses three core operational failures identified in the current lab environment:

* Lost Tools (P-01): Items left on project desks are frequently forgotten because the lab lacks an automated reminder system for outstanding loans.
* Volume Tracking (P-02): The sheer volume of inventory makes paper or spreadsheet-based tracking a functional impossibility as the system scales.
* Staff Visibility (P-03): Lab administrators currently operate in a data vacuum, with no real-time inventory levels or historical usage patterns to inform procurement.

Our primary mission is the implementation of a seamless "Tap. Take. Tracked." workflow. This architecture balances the creative freedom required by student researchers with the rigorous administrative control necessary for sustainable laboratory management.

2. User Study and Background Research

The development of SAVE4223 was driven by a "local-first" iterative design process. The system evolved from a Semester 1 prototype—a basic proof-of-concept—to the current industrial-grade architecture. This transition was dictated by the need to resolve fundamental engineering flaws discovered during initial field tests, ensuring the system could survive the "knock-and-tolerate" environment of a professional shop.

Key takeaways from member interviews, lab-staff consultations, and field tests include:

* Optimal Tech Stack: The combination of UHF RFID for bulk item tracking and NFC for secure user authentication provides the most seamless hands-off experience.
* Latency Requirements: Users demand immediate feedback; Semester 1 testing showed that high latency in processing leads to "unclosed drawer" errors.
* Isolation Standards: Reliable tracking requires 100% read accuracy inside the volume and 0% "phantom detections" outside the enclosure.

The Semester 1 prototype utilized plastic drawers which, while easy to assemble, were a failure of isolation. Because plastic is RF transparent, waves leaked through the back and sides of the stack, leading to massive interference. Market research confirmed that while a mass-production route using stock steel cabinets is viable, the prototype route—using aluminium extrusions and composite panels—was necessary to validate our bespoke hardware integration and UI feedback loops.

3. Finalized System Specifications and Architecture

For a system to be mission-critical in a laboratory setting, it must prioritize a "local-first, cloud-synced" architecture. This ensures that the cabinet remains fully functional during network outages while maintaining a global ground truth via the cloud.

System Specifications

* Form & Dimensions: 620 x 522 x 850 mm (Height matched to standard workbench height).
* Load Capacity: 200 kg total capacity supported by an industrial aluminium skeletal frame.
* Storage Configuration: Dual drawer depths (120 mm and 170 mm) to accommodate everything from precision tweezers to heavy power drills.
* Mobility: Mounted on heavy-duty casters for single-person maneuverability.

Product Architecture & Hardware Stack

A Raspberry Pi acts as the central orchestrator, managing edge device logic locally to eliminate the high latency found in cloud-only builds.

Component	Technical Specification	Strategic Purpose
Main Controller	Raspberry Pi	Local logic orchestrator; manages auth and cloud sync.
UHF Reader	4-channel, 33 dBm, EPC Gen2	Full-volume coverage; high-gain sensing across all drawers.
Locks	4× Electromagnetic locks	Independent per-drawer permissions and security.
Sensors	4× Infrared (IR) sensors	Real-time physical closure detection (Independent of lock).
UI Interface	Touchscreen + QR Scanner	Bilingual guidance and secure identity verification.

4. Material Science and Shielding Engineering

The SAVE4223 cabinet design manages a fundamental "Core Conflict": the external enclosure must serve as a perfect RF shield (Faraday cage), while the internal drawer structures must be RF transparent to allow for full-volume scanning.

The Physics of Shielding

To prevent "Slit Active Leaky Wave" interference, we engineered the enclosure based on Skin Depth (\delta) and Geometric Seam requirements. The skin depth is determined by the formula: \delta = 1 / \sqrt{\pi f \mu \sigma} Where f is frequency (915 MHz), \mu is permeability, and \sigma is conductivity. While the calculated skin depth for Aluminium is \sim 2.8 \mu m, effective shielding requires a material thickness several multiples of this; we selected 0.15 mm Aluminium Composite (ACP) to ensure total attenuation. Furthermore, to prevent wave leakage, all geometric seams were kept at d < \lambda/30 (approx. 1 cm) and bridged with conductive tape to maintain galvanic continuity.

The Architecture of Compromise

Materials were categorized into three functional regimes:

* Structural Frame (Aluminium T-slot Extrusion): Chosen for its ability to bear 200kg loads and provide modular attachment points for accessories.
* RF Enclosure (Aluminium Composite Panel): Provides the necessary Faraday shielding while allowing for V-groove folding into a crisp industrial form.
* Internal Components (PC Hollow Sunboard & Plywood): Sunboard provides the RFID transparency and ribbed stiffness required for drawer sides, while Plywood serves as the load-bearing drawer base and the tactile "safe touch" top surface.

5. Software Stack and User Experience

Our software stack (NiceGUI, SQLite, Next.js, Supabase) was selected to provide a high-performance edge UI without the need for a separate API layer for local hardware control.

Layer	Technology	Rationale
Edge UI	NiceGUI (Python)	Eliminates separate API layers; runs alongside device logic.
Edge DB	SQLite	Network-resilient local storage for mission-critical reliability.
App/API	Next.js + Vercel	Streamlined hosting for the save4223.isd-hub.com portal.
Cloud	Supabase	Managed PostgreSQL and object storage for global visibility.

The User Journey

Users register at save4223.isd-hub.com to generate a unique pairing QR code. At the cabinet, they scan the QR and tap their HKUST student card (NFC) to pair. Once paired, the user simply taps to unlock. Real-time feedback is provided via the LED strip:

* Red: Warning—Drawer is physically unclosed.
* Green: Success—Drawer is safely locked and transaction recorded.
* Orange: Processing—UHF RFID scanning in progress (approx. 5s).

The web-based "Tool Library" provides administrators with live item counts and historical logs, transforming the previously "invisible" inventory into actionable data.

6. Component Design and Technical Documentation

We prioritized "Design for Manufacturability" (DfM). While this prototype uses an extrusion-and-panel method for rapid iteration, the mass-production route will utilize stock steel cabinets. Steel is an excellent UHF shield, and its higher skin depth requirement (\sim 0.78 mm) is naturally met by standard-gauge industrial steel filing cabinets.

Hardware Integration & Repository

* Rear Hardware Bay (Fig. 02): The UHF reader is centrally mounted for balanced cable runs to the antennas. The Raspberry Pi is mounted on the left, with PSUs separated on the right to manage thermal load and electromagnetic interference.
* Source Code Repository: [PLACEHOLDER: Link to Permanent Repo]
* CAD and Fabrication Files: [PLACEHOLDER: Shared Folder Link]

7. Results, Testing, and Performance Evaluation

Empirical validation was required to ensure survival in a maker space environment.

Mechanical and Surface Testing:

* Three-Point Bending Test: Confirmed structural integrity under full load.
* Hardness Evaluation: We performed Mohs Hardness and ASTM D3363 Pencil Hardness tests. We determined that PC Hollow Sunboard, while excellent for RF transparency, failed the touch-surface test (scratched by a fingernail). Consequently, Sunboard was relegated to internal use, with Plywood used for all touchable surfaces.

RFID Validation: The engineering goal was a 100% internal read rate and 0% external "phantom detections." By maintaining a sealed Faraday cage and using conductive tape to bridge galvanic discontinuities, we successfully isolated the 915 MHz signal. The hybrid material regime (shielding outer shell/transparent inner drawers) achieved 100% accuracy in identifying items during the 5-second scan cycle.

8. Roadmap and Future Improvements

The SAVE4223 is currently a fully functional working prototype, ready for expanded deployment in HKUST Room 4223.

Phased Improvements:

* Short term (3-4 weeks): Finalize door back-panels, optimize mobile UI data-fetching, and implement concurrency handling for multi-user scenarios.
* Long term (5+ weeks): Deployment of the full admin platform and the "Materials System"—a Taobao MCP integration that will auto-generate expense reports and push ordered items directly to inventory.

Project Team:

* He Tianlun: Lead Systems Design
* Li Tsz Yuk: Hardware Integration
* Zhang Yue: Software Architecture
* Zhang Yunxin: User Experience & Research

