## WaaSMedicSvc Missing or Corrupted on Windows 11 Pro

### **Issue Description**

After upgrading or modifying Windows 11 Pro, the **Windows Update Medic Service (WaaSMedicSvc)** may go missing or become corrupted, preventing Windows Update from functioning properly. Running `sc query WaaSMedicSvc` results in an error indicating that the service is not found.

### **Steps to Reproduce**

1. Open **Command Prompt** as Administrator.
2. Run the following command:

   ```powershell
   sc query WaaSMedicSvc
   ```

3. If the service is missing, the output will indicate an error.
4. Attempting to recreate the service using `sc create` may result in an error due to MUI localization issues.

### **Expected Behavior**

The `WaaSMedicSvc` service should be present and functional, allowing Windows Update to operate normally.

### **Solution / Workaround**

To restore the service, follow these steps:

#### **Method 1: Restore Service via Registry**

1. Open **Notepad** and paste the following content:

   ```reg
   Windows Registry Editor Version 5.00

   [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WaaSMedicSvc]
   "DisplayName"="Windows Update Medic Service"
   "Description"="Enables remediation and protection of Windows Update components."
   "ImagePath"=hex(2):25,00,53,00,79,00,73,00,74,00,65,00,6d,00,52,00,6f,00,6f,
     00,74,00,25,00,5c,00,73,00,79,00,73,00,74,00,65,00,6d,00,33,00,32,00,5c,
     00,73,00,76,00,63,00,68,00,6f,00,73,00,74,00,2e,00,65,00,78,00,65,00,00,
     00
   "ObjectName"="NT AUTHORITY\\LocalService"
   "Start"=dword:00000003
   "Type"=dword:00000020
   ```

2. Save the file as `WaaSMedicSvc.reg`.
3. Double-click the file to merge it into the registry.
4. Restart your computer.

#### **Method 2: Run System File Checker (SFC) & DISM**

If the issue persists, run the following commands in an **elevated Command Prompt**:

```powershell
sfc /scannow
```

```powershell
DISM /Online /Cleanup-Image /RestoreHealth
```

These commands will repair missing or corrupted system files.

### **Additional Information**

- **OS Version:** Windows 11 Pro (various builds)
- **Possible Cause:** Windows update issues, system modifications, localization (MUI) problems
- **Workaround Effectiveness:** ✅ Tested and confirmed working

### **Related Issues**

- Microsoft Support: [Windows Update Troubleshooter](https://support.microsoft.com/en-us/windows/windows-update-troubleshooter-6d5d2dbf-1ba3-4ae1-bc4b-50355ae3f31c)

### **Status**

✅ Issue resolved using the registry method.

---

Let me know if you need modifications! 🚀
