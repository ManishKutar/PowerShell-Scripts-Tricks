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
