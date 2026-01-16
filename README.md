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
