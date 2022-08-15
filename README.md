# PowerShell
## PowerShell Scripts - Tips and Tricks
- Get running scheduled tasks on a Windows system. Replacing `running` with `ready` or `disabled` gives more scheduled task lists.
```
(get-scheduledtask).where({$_.state -eq 'running'})
```
- Warnings on PowerShell
```
Write-Warning "This is only a test warning."
```
- Shows the command of the PowerShell command
```
Show-Command -Name "Invoke-Command"
```
- Get the clipboard content on Windows
```
Get-Clipboard
```
- Find command from registered repos for modules
```
Find-Command -Repository PSGallery | Select-Object -First 10
```
- Get the member of any variable that can be used further.
```
$byteArray = Get-Content -Path C:\temp\test.txt -AsByteStream -Raw
Get-Member -InputObject $bytearray
```
## PowerShell Scripts - BC / NAV
- Compile objects
```
$Version = 110
if ([Environment]::Is64BitProcess)
{
    $RtcFolder = 'HKLM:\SOFTWARE\Wow6432Node\Microsoft\Microsoft Dynamics NAV\' + $Version + '\RoleTailored Client'
}
else
{
    $RtcFolder = 'HKLM:\SOFTWARE\Microsoft\Microsoft Dynamics NAV\' + $Version + '\RoleTailored Client'
}

Test-Path $RtcFolder
$IdeModulePath = (Join-Path (Get-ItemProperty $RtcFolder).Path Microsoft.Dynamics.Nav.Ide.psm1)
Import-Module $IdeModulePath -Force | Out-Null

Write-Host "Compiling the modified objects >>" -ForegroundColor Blue -BackgroundColor Yellow

$filter = "Modified=1" //$filter = "Type=$objectType;Version List=<>*Test*"
$NAVSERVER = "<NAV Server name>"
$NAVINSTANCE = "<NAV Server instance>"
$SQLSERVER = "<SQL Server name>"
$SQLINSTANCE = ""
$DB = "<Db name>"
$SYNC = "Yes"
$USER = "<User ID>"
$PASS = "<Password>"
$PathOfLogs = 'C:\<path of log>\'

Compile-NAVApplicationObject -navservername $NAVSERVER -navserverinstance $NAVINSTANCE -databaseserver $SQLSERVER\$SQLINSTANCE -databasename $DB -synchronizeschemachanges $SYNC -username $USER -password $PASS -filter $filter –recompile -LogPath $PathOfLogs
```
- Check installed modules
```
$module = 'Microsoft.Dynamics.Nav.Management'
if (Get-Module | Where-Object {$_.Name -eq $module}) {
    Write-Host "Module $($module) is imported" -ForegroundColor Green
}
else {
    Write-Host "Module $($module) is not imported. Importing it further.." -ForegroundColor Red
    Import-Module "${env:ProgramFiles}\Microsoft Dynamics NAV\110\Service\NavAdminTool.ps1" -force -DisableNameChecking -Verbose | Out-Null
}
```
## PowerShell Scripts - Docker Container and Images
- Upload license
```
Import-BcContainerLicense -licenseFile <file_path_of_license_file> -containerName <Container_name> -restart
```
- Microsoft Dynamics NAV or Business Central Container Wizard
```
New-BcContainerWizard
```
- Inspect BC docker for settings
```
docker inspect <Container_name>
```
- Logs of BC docker (can be used to get details like Vsix files for compiler on container)
```
docker log <Container_name>
```
Example of getting compiler details of container: (Reference: https://www.powershellgallery.com/packages/Tecman.Tfs.Tools/1.1.54.0/)
```
$Logs = docker logs <Container_name>
$VsixUrl = $Logs.Item($Logs.IndexOf('Files:') + 1)
```
