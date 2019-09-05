1. Open the Start menu and type "cmd.exe". Right-click on the item "Command Prompt" or "Eingabeaufforderung" that appears and choose "Run as administrator":<br>
   <br>
   <img src="./windows-1-run-cmd-as-admin.jpg">
   <br>This will run the Command Prompt as an administrator user<br><br>
2. Copy the following text (be sure you select all of it, it's very long) and right-click in the black middle part of the Command Prompt window to paste the text. Hit enter.<br><br>
   ```sh
   @"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
   ```
   This will install Chocolatey, a package manager which will allow us to install and uninstall programs from the command prompt.
   <br>
3. The installer will take a bit of time and then show a message that "Chocolatey is ready", signaling that it is done:<br><br>
   <img src="./windows-2-chocolatey-installed.png"><br><br>
4. Close the Command Prompt and open it again as administrator (like in step 1)<br><br>
5. Copy the following text and right-click in the black middle part of the Command Prompt window to paste the text. Hit enter.<br><br>
   ```sh
   choco install git nodejs yarn vscode hyper -y
   ```
   This uses Chocolatey to install Git, Node.js, Yarn, Visual Studio Code and Hyper.<br><br>
6. We recommend installing and using Chrome so that you have the same Devtools as others.<br><br>
   If you don't have Chrome installed yet, you can install it with Homebrew. To do this, copy the following text and right-click in the black middle part of the Command Prompt window to paste the text. Hit enter.<br><br>
   ```sh
   choco install chrome -y
   ```
   This uses Chocolatey to install Chrome.<br><br>
7. Close the Command Prompt and open it again as administrator (like in step 1)<br><br>
8. Copy the following text and right-click in the Command Prompt window to paste the text. Hit enter.<br><br>
   ```sh
   npx create-react-app --help
   ```
   This will prepare a program that we will use in the course.<br><br>
9. The preparation will take a while and then respond with a message that some modules have been installed, similar to this:<br><br>
   <img src="./general-1-cra-installed.png"><br><br>
10. Next we will configure VS Code.<br><br>
    Open VS Code and then press the keys <kbd>Ctrl</kbd>-<kbd>Shift</kbd>-<kbd>P</kbd>. Type in "Settings" and select the item that says `Preferences: Open Settings (JSON)`:<br><br>
    <img src="./general-2-vscode-settings.png"><br><br>
    Once the settings file is open, we will want to add the settings below.<br><br>
    First of all, identify whether your settings file is empty or not. This is what an empty file looks like:<br><br>
    <img src="./general-3-vscode-settings-empty.png"><br><br>
    If your file is **not empty** (if there is more text within the curly brackets), then **we will need to do something extra** - add a comma on the second to last line:<br><br>
    <img src="./general-4-vscode-settings-comma.png"><br><br>
    Now in both cases you will want to paste the following settings **before the closing curly bracket (before the `}`)**:<br><br>
    ```json
     "editor.wordWrap": "on",
     "editor.minimap.enabled": false,
     "editor.formatOnSave": true,
     "files.autoSave": "onFocusChange",
     "explorer.openEditors.visible": 0,
     "editor.tabSize": 2,
     "workbench.editor.tabSizing": "shrink",
     "workbench.editor.closeEmptyGroups": false,
     "prettier.singleQuote": true,
     "prettier.trailingComma": "all",
     "[markdown]": {
       "files.trimTrailingWhitespace": false
     },
    ```
    If you had any previous settings beforehand, you may notice that some text above will be underlined by a squiggly yellow line. This is a warning because we pasted some duplicate properties from the code above.<br><br>
    If you have any of these warnings, we should fix them. For each one of these lines with the warnings on them, delete the full line, including the comma at the end. We usually like to select from the start of the first `"` to just before the next `"` on the next line:<br><br>
    <img src="./general-5-vscode-settings-fix-warnings.png"><br><br>
11. On Windows we will also configure Hyper.<br><br>
    Open Hyper and then select Edit -> Preferences, which will open a text file in an editor:<br><br>
    <img src="./windows-3-hyper-preferences.png"><br><br>
    In this file, we will do three things:<br><br>

    1. Find `shell: '',` and replace it with `shell: 'C:\\Program Files\\Git\\git-cmd.exe',`
    2. Find `shellArgs: ['--login'],` and replace it with `shellArgs: ['--command=usr/bin/bash.exe', '-l', '-i'],`
    3. Find `env: {},` and replace it with `env: { TERM: 'cygwin' },`

    Then save the file and close and restart Hyper.

12. If you haven't set a name on GitHub yet, go to the [GitHub Profile Settings](https://github.com/settings/profile) and add a name:<br><br>
    <img src="./general-6-github-profile-settings.png"><br><br>
    We will use this name in the next step.<br><br>
13. For this step, we'll need to **edit some of the information in the commands** by adding our own information.<br><br>
    First of all, we will set our name, which will be the same name as on our GitHub profile:<br><br>
    <img src="./general-7-github-name.png"><br><br>
    Copy your name from your profile, **add it in quotes** in the command (replace `Mona Lisa Octocat`) and run the command:<br><br>
    ```sh
    git config --global user.name "Mona Lisa Octocat"
    ```
    You can test whether the name was set correctly with the next command (if it worked, it will print the name on the next line):<br><br>
    ```sh
    git config --global user.name
    ```
    <br>For running the next command, **add your email in quotes**:<br><br>
    ```sh
    git config --global user.email "monalisaoctocat@example.com"
    ```
    You can test whether the email was set correctly with with the next command (if it worked, it will print the email on the next line):<br><br>
    ```sh
    git config --global user.email
    ```
    This prepares `git` so that your work is attributed correctly to you.<br><br>
14. Copy the following text, paste it in the terminal and hit return.<br><br>
    ```sh
    git config --global credential.helper wincred
    ```
    This step will save your GitHub password so that you don't need to enter it every time.<br><br>
