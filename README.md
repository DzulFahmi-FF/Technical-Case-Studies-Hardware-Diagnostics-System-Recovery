 # Technical-Case-Studies-Hardware-Diagnostics-System-Recovery
This repository documents my independent research and troubleshooting methodologies for complex computer system failures. My approach focuses on Root Cause Analysis (RCA) and systematic validation.

## Case Study 1: Thermal Performance & Pump-Out Effect analysis
**Device:**
HP Pavilion Gaming 15 (Intel Core i7-9750H, NVIDIA GTX 1650) Context: Investigating chronic thermal throttling despite high-end thermal paste application.

**Problem Statement:**
After applying Kingpin KPx thermal paste, the CPU reached 98°C under load (Cinebench R15) within just one week, causing severe performance throttling.

**Investigation:**
Performed comparative analysis between day-1 results (79°C) and day-7 results (98°C). Researched material properties and discovered the "Pump-Out Effect" prevalent in direct-die applications where low-viscosity paste migrates out due to thermal expansion cycles.

**Solution:**
Re-applied thermal interface material using Thermalright TFX, a high-viscosity paste specifically designed for high-pressure/direct-die environments.

**Validation:**
Successfully maintained stable temperatures at 78°C-79°C consistently over a long-term period, effectively eliminating the throttling issue.

Note: This case study was originally shared and discussed within a local hardware community on Facebook to provide educational insights for other users.
<details>
  <summary><b>Click to view benchmark screenshots</b></summary>
  
  #### Phase 1: Kingpin KPx (Low Viscosity - Pump-Out Identified)
  ![Kingpin Results idle day 1](assets/case_1/kingpin_idle_day_1.jpg)
  *Image 1: Initial Idle temperatures with Kingpin KPx.*
  ![Kingpin Results load day 1](assets/case_1/kingpin_load_day_1.jpg)
  *Image 2: Day 1 Cinebench R15 Stress Test. Maximum temperature reached 79°C, showing excellent initial heat transfer.*
  ![Kingpin Results load day 7](assets/case_1/kingpin_load_day_7.jpg)
  *Image 3: Day 7 Cinebench R15 Stress Test. Maximum temperature spiked to 98°C, confirming performance degradation due to the "Pump-Out" effect.*
  
  #### Phase 2: Thermalright TFX (High Viscosity - Stability Verified)
  ![TFX Results idle day 1](assets/case_1/tfx_idle_day_1.jpg)
  *Image 4: Idle temperatures with Thermalright TFX.*
  ![TFX Results load day 1](assets/case_1/tfx_load_day_1.jpg)
  *Image 5: Day 1 Cinebench R15 Stress Test. Maximum temperature reached 79°C, matching the initial performance of previous high-end pastes.*
  ![TFX Results load day 7](assets/case_1/tfx_load_day_7.jpg)
  *Image 6: Day 7 Cinebench R15 Stress Test. Maximum temperature remained stable at 78°C. This confirms that high-viscosity material successfully prevents pump-out on direct-die applications.*
</details>


## Case Study 2: Diagnosing "5-Second Power Cutoff" & Peripheral Short-Circuit Recovery
### 1. Problem Identification
#### Status: 
System failed to POST (No Boot).
#### Symptom: 
Upon pressing the power button, the LED stayed solid for exactly 5 seconds, then the system abruptly shut down (Power Cutoff). No fan spin or screen activity.
#### Expert Verdict: 
A professional technician diagnosed this as a fatal Motherboard Short-Circuit or CPU Failure.

### 2. The 14-Day Persistence (Trial & Error Phase)
I spent two weeks performing a deep-level investigation, refusing to accept the "dead motherboard" verdict. My troubleshooting steps included:
#### Electrical Validation: 
With a technical colleague, I verified that the main power flow from the AC adapter to the Power Management IC was normal, ruling out the power supply.
#### Isolation Testing: 
I stripped the laptop down, testing RAM and resetting the CMOS, but the "5-second shutdown" loop persisted.
#### The Frustration: 
The consistent timing of the shutdown (always 5 seconds) suggested a specific timeout or protection mechanism was being triggered.

### 3. The "Eureka" Moment (The Discovery)
After nearly giving up, I decided to remove the original SSD to secure my data. As a final experiment:
#### The Action: 
I inserted a spare SSD containing a Linux OS.
#### The Result: 
The system immediately powered on, passed the 5-second mark, and booted successfully into Linux.
#### Verification: 
Re-inserting the original SSD caused the "5-second power cutoff" to return immediately.

### 4. Technical Root Cause Analysis (RCA)
#### The Culprit: 
The original SSD had developed a Short-to-Ground fault or internal controller failure.
#### The Mechanism: 
The faulty SSD was pulling down the 3.3V Power Rail. During the initial 5-second boot sequence, the Embedded Controller (EC) detected this voltage drop/irregularity.
#### Protection Mode: 
To prevent motherboard damage, the EC triggered an emergency "Over-Current Protection" (OCP) shutdown, cutting power instantly. This was mistakenly diagnosed by others as a mainboard failure.

### 5. Professional Reflection
This case demonstrated that a single shorted peripheral can mimic a dead motherboard. By persisting through a 14-day diagnostic process, I identified the true fault and saved the system from being scrapped.

<details>
<summary><b>Click to view diagnostic videos and photos</b></summary>

#### Phase 1: Symptom Documentation (5-Second Power Cutoff)
![Power Button Failure](assets/case_2/power-failure.mp4)
*Video 1: Documentation of the 5-second power-on symptom followed by an immediate system hard shutdown (Protection Mode).*

#### Phase 2: Teardown & Board Inspection
![Teardown Process](assets/case_2/teardown-process.jpg)
*Image 1: Full disassembly process for motherboard inspection and component cleaning prior to deep isolation testing.*

#### Phase 3: Hardware Layout & Success Verification
![Internal Layout](assets/case_2/internal-layout.jpg)
*Image 2: Internal view of the system. During this phase, the original SSD was identified as the component triggering the power failure; swapping to a spare drive successfully restored full functionality.*

</details>

