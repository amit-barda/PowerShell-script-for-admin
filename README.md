# PowerShell-script-for-admin

#for copy member of from one to another 
Get-ADUser -Identity user (the source) -Properties memberof | Select-Object -ExpandProperty memberof | Add-ADGroupMember -Members user (the target)
if aduser not work 
Install-WindowsFeature -Name “RSAT-AD-PowerShell” -IncludeAllSubFeature
then 
Get-Module -Name ActiveDirectory -ListAvailable


#to Check if user is disable or enable 
