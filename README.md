# PowerShell 7-Zip

Downloads the latest 7-Zip:

```powershell
$response = Invoke-WebRequest -Uri https://www.7-zip.org/download.html
# Take the first link of three: stable, old and beta
$match = $response.Content | Select-String -Pattern '<A href="(?<url>a\/7z\d+-x64\.msi)">Download<\/A>'
$url = "https://www.7-zip.org/" + $match.Matches[0].Groups['url'].Value
Invoke-WebRequest -Uri $url -OutFile 7zip.msi
$process = Start-Process msiexec -ArgumentList "/a 7zip.msi /qn TARGETDIR=`"$(Get-Location)\7zip`"" -PassThru
Wait-Process -Id $process.id
Move-Item 7zip/Files/7-Zip/7z.exe 7z.exe -Force
Move-Item 7zip/Files/7-Zip/7z.dll 7z.dll -Force
Remove-Item –path 7zip –Recurse
Remove-Item –path 7zip.msi
```

## Running

```powershell
& .\7zip.ps1
```
