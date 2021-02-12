# How to install chrome in EC2 Windows instance

The powershell script to install chrome is:

```powershell
$LocalTempDir = $env:TEMP; $ChromeInstaller = "ChromeInstaller.exe"; (new-object    System.Net.WebClient).DownloadFile('http://dl.google.com/chrome/install/375.126/chrome_installer.exe', "$LocalTempDir\$ChromeInstaller"); & "$LocalTempDir\$ChromeInstaller" /silent /install; $Process2Monitor =  "ChromeInstaller"; Do { $ProcessesFound = Get-Process | ?{$Process2Monitor -contains $_.Name} | Select-Object -ExpandProperty Name; If ($ProcessesFound) { "Still running: $($ProcessesFound -join ', ')" | Write-Host; Start-Sleep -Seconds 2 } else { rm "$LocalTempDir\$ChromeInstaller" -ErrorAction SilentlyContinue -Verbose } } Until (!$ProcessesFound)
```

So to install it on windows server simply paste it on windows powershell by launching it. Or we can use as bootstrap script as follows:-

* **Step-1**

![AMI](/EC2/assets/ami.png)

* **Step-2**

I linke to prefer **`t2.xlarge`** instange for windows

![Machine Type](/EC2/assets/type.png)

* **Step-3**
Enter the following scripts in **Advanced Details** > **User Data** > **As text**, paste the following

```powershell
<powershell>
$LocalTempDir = $env:TEMP; $ChromeInstaller = "ChromeInstaller.exe"; (new-object    System.Net.WebClient).DownloadFile('http://dl.google.com/chrome/install/375.126/chrome_installer.exe', "$LocalTempDir\$ChromeInstaller"); & "$LocalTempDir\$ChromeInstaller" /silent /install; $Process2Monitor =  "ChromeInstaller"; Do { $ProcessesFound = Get-Process | ?{$Process2Monitor -contains $_.Name} | Select-Object -ExpandProperty Name; If ($ProcessesFound) { "Still running: $($ProcessesFound -join ', ')" | Write-Host; Start-Sleep -Seconds 2 } else { rm "$LocalTempDir\$ChromeInstaller" -ErrorAction SilentlyContinue -Verbose } } Until (!$ProcessesFound)
</powershell>
```

It will look like this

![Useer Data](/EC2/assets/UserData.png)

* **Step-4**

Click **`Review and Launch`** button

![Launch](/EC2/assets/launch.png)

**Wait for 5-minutes, and login. Your screen wil be like this.**

Your final instance will be like this.

![EC2-WINDOWS](/EC2/assets/final.png)
