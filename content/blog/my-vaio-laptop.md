---
title: My VAIO Laptop
date: 2026-04-22T02:17:32.372Z
---





**PowerShell**
```
Get-ComputerInfo | Select-Object OsName, OsVersion, OsBuildNumber, OsArchitecture, WindowsInstallationType, CsProcessors, CsTotalPhysicalMemory, CsSystemType, BiosBIOSVersion, CsManufacturer, CsModel
```

**Result**
```
OsName : Microsoft Windows 11 Pro
OsVersion : 10.0.26200
OsBuildNumber : 26200
OsArchitecture : 64-bit
WindowsInstallationType : Client
CsProcessors : {Intel(R) Core(TM) Ultra 7 155H}
CsTotalPhysicalMemory : 68108480512
CsSystemType : x64-based PC
BiosBIOSVersion : {VAIO - 20260209, R0481HF, Phoenix Technologies - 12345678}
CsManufacturer : VAIO Corporation
CsModel : VJS4R1
```

**Bash**
```
#!/bin/bash

# ============================================================================
# Linux 等效脚本：模拟 Get-ComputerInfo 选定属性的输出格式
# 用法: chmod +x sysinfo.sh && ./sysinfo.sh   (部分信息需 sudo 权限)
# ============================================================================

# 辅助函数：提取系统制造商/型号/BIOS版本 (需要 root)
get_dmi_info() {
    local keyword="$1"
    if command -v dmidecode &>/dev/null; then
        sudo dmidecode -s "$keyword" 2>/dev/null || echo "N/A (requires root)"
    else
        echo "N/A (dmidecode not installed)"
    fi
}

# 1. 操作系统名称 (模拟 OsName)
if [ -f /etc/os-release ]; then
    . /etc/os-release
    OsName="$NAME $VERSION"
else
    OsName="$(uname -s) $(uname -r)"
fi

# 2. 内核版本 (模拟 OsVersion)
OsVersion="$(uname -r)"

# 3. 内核构建号 (模拟 OsBuildNumber，Linux无直接对应，使用内核版本作为替代)
OsBuildNumber="$(uname -v | cut -d' ' -f1)"

# 4. 系统架构 (模拟 OsArchitecture)
OsArchitecture="$(uname -m)"

# 5. 安装类型 (模拟 WindowsInstallationType，Linux无直接对应，显示发行版类别)
WindowsInstallationType="${ID:-Linux}"

# 6. CPU 信息 (模拟 CsProcessors)
if command -v lscpu &>/dev/null; then
    # 提取型号名称并去重
    CsProcessors="$(lscpu | grep "Model name" | cut -d':' -f2 | sed 's/^[ \t]*//' | uniq | paste -sd ', ')"
else
    CsProcessors="$(grep -m1 "model name" /proc/cpuinfo | cut -d':' -f2 | sed 's/^[ \t]*//')"
fi

# 7. 总物理内存 (字节) (模拟 CsTotalPhysicalMemory)
if [ -f /proc/meminfo ]; then
    mem_kb=$(grep "^MemTotal:" /proc/meminfo | awk '{print $2}')
    CsTotalPhysicalMemory=$((mem_kb * 1024))
else
    CsTotalPhysicalMemory="N/A"
fi

# 8. 系统类型 (模拟 CsSystemType)
CsSystemType="$(uname -m)-based PC"

# 9. BIOS 版本 (模拟 BiosBIOSVersion)
BiosBIOSVersion="$(get_dmi_info bios-version)"

# 10. 制造商 (模拟 CsManufacturer)
CsManufacturer="$(get_dmi_info system-manufacturer)"

# 11. 型号 (模拟 CsModel)
CsModel="$(get_dmi_info system-product-name)"

# ============================================
# 输出格式：完全模拟 PowerShell 的列表格式
# ============================================
cat <<EOF

OsName                  : $OsName
OsVersion               : $OsVersion
OsBuildNumber           : $OsBuildNumber
OsArchitecture          : $OsArchitecture
WindowsInstallationType : $WindowsInstallationType
CsProcessors            : $CsProcessors
CsTotalPhysicalMemory   : $CsTotalPhysicalMemory
CsSystemType            : $CsSystemType
BiosBIOSVersion         : $BiosBIOSVersion
CsManufacturer          : $CsManufacturer
CsModel                 : $CsModel

EOF
```

**Result**
```

OsName                  : Fedora Linux 43 (Workstation Edition)
OsVersion               : 6.17.1-300.fc43.x86_64
OsBuildNumber           : #1
OsArchitecture          : x86_64
WindowsInstallationType : fedora
CsProcessors            : Intel(R) Core(TM) Ultra 7 155H
CsTotalPhysicalMemory   : 66804535296
CsSystemType            : x86_64-based PC
BiosBIOSVersion         : R0481HF
CsManufacturer          : VAIO Corporation
CsModel                 : VJS4R1
```


