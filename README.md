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

