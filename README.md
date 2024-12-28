# Fixing Windows 11 Boot Issues

This tutorial provides steps to fix common boot issues in Windows 11 using various command-line tools.

## Steps to Access Command Prompt in Windows Recovery Environment (WinRE)

1. **Boot into WinRE**:
   - Restart your computer.
   - As soon as it starts booting up, press and hold the `Shift` key and click on **Restart**.
   - This will take you to the **Advanced Startup Options**.

2. **Access Command Prompt**:
   - In the Advanced Startup Options, select **Troubleshoot**.
   - Go to **Advanced options**.
   - Select **Command Prompt**.

## Commands to Fix Boot Issues

### 1. System File Checker (SFC)

Scans and repairs corrupted system files.

```bash
sfc /scannow
```

### 2. Deployment Imaging Service and Management Tool (DISM)

Checks and repairs the system image.

- Check the health of your system image:
  ```bash
  DISM /Online /Cleanup-Image /CheckHealth
  ```

- Scan the health of your system image:
  ```bash
  DISM /Online /Cleanup-Image /ScanHealth
  ```

- Restore the health of your system image:
  ```bash
  DISM /Online /Cleanup-Image /RestoreHealth
  ```

### 3. Check Disk Utility (CHKDSK)

Scans and fixes file system errors.

```bash
chkdsk C: /f /r /x
```
Replace `C:` with the drive letter you want to check.

### 4. Bootrec Commands

Fixes boot issues.

```bash
bootrec /fixmbr
bootrec /fixboot
bootrec /scanos
bootrec /rebuildbcd
```

### 5. Bootsect Command

Fixes the boot sector.

```bash
bootsect /nt60 sys
```

### 6. Rebuild the Boot Configuration Data (BCD)

```bash
bcdedit /export C:\BCD_Backup
attrib C:\boot\bcd -h -r -s
ren C:\boot\bcd bcd.old
bootrec /rebuildbcd
```

### 7. Use Diskpart to Assign a Drive Letter to the EFI Partition

1. Open Command Prompt and type the following commands:

```bash
diskpart
list disk
select disk 0  # Select the disk where Windows is installed
list vol
select vol X  # Select the EFI partition (usually around 100MB)
assign letter=V:  # Assign a drive letter to the EFI partition
exit
```

2. Then, type the following command to fix the boot sector:

```bash
bcdboot C:\Windows /s V: /f UEFI
```

## Reset Windows Update Components

1. Open Command Prompt as an administrator.
2. Type the following commands one by one and press Enter after each:

```bash
net stop wuauserv
net stop cryptSvc
net stop bits
net stop msiserver
ren C:\Windows\SoftwareDistribution SoftwareDistribution.old
ren C:\Windows\System32\catroot2 catroot2.old
net start wuauserv
net start cryptSvc
net start bits
net start msiserver
```

## System Restore

1. Open Command Prompt as an administrator.
2. Type `rstrui.exe` and press Enter. This will open the System Restore wizard.
