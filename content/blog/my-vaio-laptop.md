---
title: My VAIO Laptop
date: 2026-04-22T02:17:32.372Z
---


PowerShell
```
Get-ComputerInfo | Select-Object OsName, OsVersion, OsBuildNumber, OsArchitecture, WindowsInstallationType, CsProcessors, CsTotalPhysicalMemory, CsSystemType, BiosBIOSVersion, CsManufacturer, CsModel
```

**Result**

- OsName : Microsoft Windows 11 Pro
- OsVersion : 10.0.26200
- OsBuildNumber : 26200
- OsArchitecture : 64-bit
- WindowsInstallationType : Client
- CsProcessors : {Intel(R) Core(TM) Ultra 7 155H}
- CsTotalPhysicalMemory : 68108480512
- CsSystemType : x64-based PC
- BiosBIOSVersion : {VAIO - 20260209, R0481HF, Phoenix Technologies - 12345678}
- CsManufacturer : VAIO Corporation
- CsModel : VJS4R1