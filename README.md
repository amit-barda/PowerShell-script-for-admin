# PowerShell-script-for-admin

# for copy member of from one to another 
Get-ADUser -Identity user (the source) -Properties memberof | Select-Object -ExpandProperty memberof | Add-ADGroupMember -Members user (the target)

if aduser not work 
Install-WindowsFeature -Name “RSAT-AD-PowerShell” -IncludeAllSubFeature
then 
Get-Module -Name ActiveDirectory -ListAvailable


# to Check if  multiple user is disable or enable 

Get-ADUser -Filter {Enabled -eq "False"} 
with all the details  about the user 

Get-ADUser -Filter {Enabled -eq "False"} | select name, enabled 
only name and status 


# to disable multiple users 

Disable-ADAccount -Identity user1 user2
 disable and check status Get-ADUser Abel.Austin | select name,enabled
 Enter the users 

# some but form txt file 

$users=Get-Content c:\it\users.txt
ForEach ($user in $users)
{
Disable-ADAccount -Identity $user
write-host "user $($user) has been disabled"
}

# Auto update windows 

# first do that shit 
Get-WindowsUpdate -AcceptAll -Install -AutoReboot

# then Schedule a reboot for 11 PM
$trigger = New-ScheduledTaskTrigger -Daily -At 11pm
$action = New-ScheduledTaskAction -Execute "shutdown /r /t 0"
Register-ScheduledTask -TaskName "DailyReboot" -Trigger $trigger -Action $action -RunLevel Highest


# Create scheduled task to run the script daily
$trigger = New-ScheduledTaskTrigger -Daily -At 11pm
$action = New-ScheduledTaskAction -Execute "PowerShell.exe" -Argument "-File C:\Path\To\UpdateAndReboot.ps1"
Register-ScheduledTask -TaskName "DailyUpdateAndReboot" -Trigger $trigger -Action $action -RunLevel Highest

# clear win update 

@ECHO OFF
echo Simple Script to Reset / Clear Windows Update
echo.
PAUSE
echo.
attrib -h -r -s %windir%\system32\catroot2
attrib -h -r -s %windir%\system32\catroot2\*.*
net stop wuauserv
net stop CryptSvc
net stop BITS
ren %windir%\system32\catroot2 catroot2.old
ren %windir%\SoftwareDistribution sold.old
ren "%ALLUSERSPROFILE%\application data\Microsoft\Network\downloader" downloader.old
net Start BITS
net start CryptSvc
net start wuauserv
echo.
echo Task completed successfully...
echo.
PAUSE
# if win update dont work that do this manually 
Write-Output "Creating Microsoft.Update.Session COM object" 
$session1 = New-Object -ComObject Microsoft.Update.Session -ErrorAction silentlycontinue

Write-Output "Creating Update searcher" 
$searcher = $session1.CreateUpdateSearcher()

Write-Output "Searching for missing updates..." 
$result = $searcher.Search("IsInstalled=0")

#Updates are waiting to be installed 
$updates = $result.Updates;

Write-Output "Found $($updates.Count) updates!" 

$updates | Format-Table Title, AutoSelectOnWebSites, IsDownloaded, IsHiden, IsInstalled, IsMandatory, IsPresent, AutoSelection, AutoDownload -AutoSize

pause
