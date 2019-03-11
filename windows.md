1. Run Command Prompt as administrator:\
   <img src="./windows-1-run-cmd-as-admin.png">
2. Copy the following text and right-click to paste it in the cmd.exe @"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
