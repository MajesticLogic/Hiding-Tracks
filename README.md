# Hiding-Tracks

# Clearing Event Logs

1. Open an elevated Command Prompt window. To do this, type "cmd" in the search bar, right-click on the Command Prompt icon and select "Run as administrator".

2. Enter the command wevtutil.exe el /q to clear all events in the event log.

3. To clear a specific log, type in the command wevtutil.exe cl [LogName] to clear only the log of your choice. Replace [LogName] with the name of the log you want to clear.

4. To confirm that the log has been cleared, use the command wevtutil.exe qe [LogName] to query the events in the log. This will return an empty list if the log has been cleared successfully.

# Hiding Windows Files

The attrib command can be used to hide files in Windows. To do this, open the Command Prompt (cmd.exe) and type "attrib +h [path\filename]". This will add the hidden attribute to the file, making it invisible in Windows Explorer. To make the file visible again, type "attrib -h [path\filename]" in the Command Prompt.

