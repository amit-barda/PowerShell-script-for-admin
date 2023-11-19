# PowerShell-script-for-admin

#for copy member of from one to another 
Get-ADUser -Identity user (the source) -Properties memberof | Select-Object -ExpandProperty memberof | Add-ADGroupMember -Members user (the target)

if aduser not work 
Install-WindowsFeature -Name “RSAT-AD-PowerShell” -IncludeAllSubFeature
then 
Get-Module -Name ActiveDirectory -ListAvailable


#to Check if  multiple user is disable or enable 

Get-ADUser -Filter {Enabled -eq "False"} 
with all the details  about the user 

Get-ADUser -Filter {Enabled -eq "False"} | select name, enabled 
only name and status 


#to disable multiple users 

Disable-ADAccount -Identity user1 user2
 disable and check status Get-ADUser Abel.Austin | select name,enabled
 Enter the users 

some but form txt file 

$users=Get-Content c:\it\users.txt
ForEach ($user in $users)
{
Disable-ADAccount -Identity $user
write-host "user $($user) has been disabled"
}
