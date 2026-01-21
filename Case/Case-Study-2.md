# Case Study 2: Diagnosing "5-Second Power Cutoff" & Peripheral Short-Circuit Recovery

## Summary

This case study documents a **real-world investigation** into a laptop system that exhibited a consistent **“5-second power-on shutdown”** behavior, a condition commonly diagnosed as a **fatal motherboard or CPU failure**.

The issue was **not random power instability or permanent board damage**, but a **deterministic protection mechanism** triggered during the **early Power-On Self-Test (POST) phase**, causing the system to shut down at a **fixed time interval** with **no fan spin or display output**.

The purpose of this case study is to demonstrate **deep-level diagnostic persistence**, **systematic isolation techniques**, and **firmware-aware reasoning** when dealing with **power protection loops** and **peripheral-induced logic lock states**.

Through **controlled component elimination**, **power rail validation**, and **peripheral reinitialization**, this case reveals how a **single storage device** can **mimic catastrophic hardware failure**, and how understanding **Embedded Controller (EC) behavior** can lead to **full system recovery without component replacement**.

---

## 1. Problem Identification
* **Status:** System failed to POST (No Boot).
* **Symptom:** Upon pressing the power button, the LED indicator remained solid for exactly 5 seconds, then the system abruptly shut down (Power Cutoff). No fan spin or display activity was observed.
* **Expert Verdict:** A professional technician diagnosed this as a fatal **Motherboard Short-Circuit or CPU Failure**, suggesting the device was beyond repair.

---

## 2. The 14-Day Persistence (Systematic Investigation Phase)
I spent two weeks performing a deep-level investigation to better understand the system behavior. My methodology focused on a rigorous process of elimination:

### Electrical & Power Rail Validation
* **Primary Input Test:** Verified the AC adapter output stability using a multimeter to ensure consistent power delivery.
* **Logic Board Inspection:** With the help of a technical colleague, I analyzed the power flow from the DC-in jack to the **Power Management IC (PMIC)**. Observations confirmed that the main power rails were receiving and distributing the correct voltages, ruling out a simple charging port or adapter failure.

### Advanced Isolation Protocol (Minimal Boot Configuration)
* **Component Stripping:** I stripped the motherboard down to its bare essentials (CPU, a single known-good RAM stick, and internal display) to isolate the core logic from external interference such as keyboard shorts or faulty IO ribbons.
* **Capacitance Discharge:** Performed a deep "Hard Reset" by disconnecting all power sources (primary and CMOS batteries) and dissipating residual capacitance to ensure no "frozen" logic states were affecting the **Embedded Controller (EC)**.
* **Result:** Despite these efforts, the "5-second power-on" loop persisted, pointing towards a deeper conflict within the high-speed data peripherals.

### The Analytical Lead
* **Pattern Recognition:** The consistent timing of the shutdown—exactly 5 seconds every time—suggested that this wasn't a random hardware "death," but a specific **Over-Current Protection (OCP)** or timeout mechanism being triggered during the Power-On Self-Test (POST) handshake.

---

## 3. The "Eureka" Moment (The Discovery)
After nearly giving up, I decided to remove the primary NVMe SSD to secure my personal data. As a final experiment:
* **The Action:** I inserted a spare SSD containing a **Linux OS**.
* **The Result:** The system immediately bypassed the 5-second mark and booted successfully into the Linux environment.
* **The Twist (Verification):** Upon re-inserting the original SSD, the system functioned perfectly and booted into the original Windows OS. The "5-second shutdown" loop was gone, confirming that the motherboard was not the issue, and the isolation testing had cleared the system's protection latch.

---

## 4. Technical Root Cause Analysis (RCA)
* **The Culprit:** A **Transient Logic Latch** or an **Electrical Handshake Conflict** between the original NVMe SSD and the motherboard's Power Management system.
* **The Mechanism:** The system was likely stuck in a "Protection Loop." During the initial 5-second Power-On Self-Test (POST) window, the **Embedded Controller (EC)** detected a non-compliant power state or a communication timeout from the storage peripheral.
* **Protection Mode Trigger:** To prevent potential hardware damage, the EC triggered an emergency **Over-Current Protection (OCP)** or **System Guard** shutdown. This state became "latched", meaning the system remembered the error and refused to boot even if the error was no longer present.
* **The Resolution Logic:** Introducing a different peripheral (the Linux SSD) forced the **Power Management IC (PMIC)** to re-initialize the power-up sequence and perform a fresh hardware handshake. This successfully cleared the "Critical Error" flag in the EC, allowing the system to recognize the original SSD as a valid device again.

---

## 5. Professional Reflection
This case demonstrated that a single shorted peripheral can mimic a dead motherboard. By persisting through a 14-day diagnostic process and relying on systematic isolation rather than initial assumptions, I identified the true fault and saved a high-value system from being unnecessarily scrapped.

<details>
<summary><b>Click to view diagnostic videos and photos</b></summary>

#### Phase 1: Symptom Documentation (5-Second Power Cutoff)
![Power Button Failure](../assets/case_2/power-failure.mp4)
*Video 1: Documentation of the 5-second power-on symptom followed by an immediate system hard shutdown (Protection Mode).*

#### Phase 2: Teardown & Board Inspection
![Teardown Process](../assets/case_2/teardown-process.jpg)
*Image 1: Full disassembly process for motherboard inspection and component cleaning prior to deep isolation testing.*

#### Phase 3: Hardware Layout & Success Verification
![Internal Layout](../assets/case_2/internal-layout.jpg)
*Image 2: Internal view of the system. During this phase, the original SSD was identified as the component triggering the power failure; swapping to a spare drive successfully restored full functionality.*

</details>
