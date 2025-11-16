Before starting the setup, check that your hardware, operating system and account is compatible:

### Minimum RAM: 8GB

Make sure that your machine has at least 8GB RAM

### Minimum OS Version: Windows 10

Make sure that you're running Windows 10 or later - earlier versions such as Windows 7 or 8.1 are not supported

### Spaces in Windows User Name

Open the Start menu and type "cmd" and click on "Command Prompt". When PowerShell starts, type in `echo %USERPROFILE%`. It should display something like `C:\Users\Karl` (without any spaces).

If you see spaces in the path (eg. `C:\Users\Karl Horky`) then you should create a new Windows account:

- Start menu -> Settings -> Accounts -> Family & other users -> Add other user -> Add account
- Enter a name **without spaces** in the user name (the box beneath the question "Who's going to use this PC?")
- Once you create the new account, log in to the new account and try the command above again

Make sure to use an account without spaces in the user name for the course.

---

With those compatibility things out of the way, you're ready to start the system setup:

1. Open the Start menu, type "Windows Update" and click on the result named Windows Update. Make sure that you have all of the latest updates.
2. Open the Start menu and type "powershell". Right-click on the item "Windows PowerShell" that appears and choose "Run as administrator":<br>
   <br>
   <img src="./windows-1-run-powershell-as-admin.png">
   <br>This will run Powershell as an administrator user<br><br>
3. Copy the following text (be sure you select all of it, it's very long) and right-click in the blue middle part of the PowerShell window to paste the text. Hit enter.<br><br>
   ```bash
   Set-ExecutionPolicy AllSigned -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
   ```
   This will install Chocolatey, a package manager which will allow us to install and uninstall programs from the command line.
   <br>
4. The installer will take a bit of time and then show a message that "Chocolatey is ready", signaling that it is done:<br><br>
   <img src="./windows-2-chocolatey-installed.png"><br><br>
5. Close PowerShell and open it again as administrator (like in step 2)<br><br>
6. Copy the following text and right-click in the blue middle part of the PowerShell window to paste the text. Hit enter.<br><br>
   ```bash
   choco install git nodejs-lts vscode hyper httpie flyctl --yes
   ```
   This uses Chocolatey to install Git, Node.js, Visual Studio Code, Hyper, HTTPie and `flyctl`.<br><br>
   <!--
      **Note:** If you are using Windows 7, you may have encountered a problem with installing Node.js because [the latest versions no longer support Windows 7](https://github.com/nodejs/node/issues/33000). To get around this, run this separately: `choco install nodejs --yes --version 13.6.0`<br><br>
   -->
   If you don't have Zoom installed yet, run this to install it:<br>
   ```bash
   choco install zoom --yes
   ```
   If you don't have Slack installed yet, run this to install it:<br>
   ```bash
   choco install slack --yes
   ```
7. Close PowerShell and open it again as administrator (like in step 2). Copy the following text and right-click in the blue middle part of the PowerShell window to paste the text. Hit enter.<br><br>

   ```bash
   corepack enable
   corepack prepare pnpm@latest --activate
   Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass -Force
   pnpm setup
   pnpm config set minimumReleaseAge 10080 --global
   perl -i -pe '$exists ||= /^minimum-release-age-exclude\[\]=@upleveled/*$/; $_ .= "minimum-release-age-exclude[]=@upleveled/*\n" if eof && !$exists' "$LOCALAPPDATA/pnpm/config/rc"
   perl -i -pe '$exists ||= /^minimum-release-age-exclude\[\]=eslint-config-upleveled$/; $_ .= "minimum-release-age-exclude[]=eslint-config-upleveled\n" if eof && !$exists' "$LOCALAPPDATA/pnpm/config/rc"
   perl -i -pe '$exists ||= /^minimum-release-age-exclude\[\]=stylelint-config-upleveled$/; $_ .= "minimum-release-age-exclude[]=stylelint-config-upleveled\n" if eof && !$exists' "$LOCALAPPDATA/pnpm/config/rc"
   ```

   This uses Corepack to install `pnpm`, configures `pnpm`'s global bin directory and prevents installation of packages newer than 7 days to mitigate supply chain security risks.<br><br>
   
   Close PowerShell and open it again as administrator (like in step 2).
   
   Install `@upleveled/preflight`, a program we will use in the course, to verify that the previous commands were successful: copy the following text, paste it in the terminal and hit return.<br><br>

   ```bash
   pnpm add --global @upleveled/preflight
   ```

   Verify that your output looks similar to the "Successful" output below. You can ignore any differences in version numbers and package names - the important part is "Done in ..." on the last line.

   If your output looks very different (either like the "Failing" output below or like some other different output), repeat the pnpm installation commands at the start of this step.

   <table>
     <tr>
       <th>Successful</th>
       <th>Failing (ERR_PNPM_NO_GLOBAL_BIN_DIR)</th>
     </tr>
     <tr>
       <td valign="top">

   ```bash
    WARN  6 deprecated subdependencies found:
   @oclif/screen@3.0.8, glob@6.0.4, glob@7.1.6,
   osenv@0.1.5, rimraf@2.4.5, rimraf@3.0.2
   Packages: +21 -32
   +++++++++++++++++--------------------------
   Progress: resolved 773, reused 772,
   downloaded 1, added 21, done

   /Users/k/Library/pnpm/global/5:
   + @upleveled/preflight 7.0.8

   Done in 3.3s
   ```

   </td>
   <td valign="top">

   ```bash
   ERR_PNPM_NO_GLOBAL_BIN_DIR  Unable to find the
   global bin directory

   Run "pnpm setup" to create it automatically,
   or set the global-bin-dir setting, or the
   PNPM_HOME env variable. The global bin
   directory should be in the PATH.
   ```

   </td></tr></table>

8. Copy the following text, right-click in the blue middle part of the PowerShell window to paste the text and hit enter.

   ```bash
   choco install python visualstudio2022-workload-vctools --yes
   ```

   This may take some time (possibly up to 15-20 minutes). This uses Chocolatey to install Python and Visual Studio build tools, which are required for installing Node.js native modules.

9. <a name="vs-code-extensions"></a> Copy each line in the following text, right-click in the blue middle part of the PowerShell window to paste the text and hit enter.<br><br>

   ```bash
   code --install-extension bradlc.vscode-tailwindcss
   code --install-extension Cardinal90.multi-cursor-case-preserve
   code --install-extension dbaeumer.vscode-eslint
   code --install-extension dozerg.tsimportsorter
   code --install-extension esbenp.prettier-vscode
   code --install-extension frigus02.vscode-sql-tagged-template-literals-syntax-only
   code --install-extension kumar-harsh.graphql-for-vscode
   code --install-extension mattpocock.ts-error-translator
   code --install-extension meganrogge.template-string-converter
   code --install-extension styled-components.vscode-styled-components
   code --install-extension stylelint.vscode-stylelint
   code --install-extension sysoev.vscode-open-in-github
   code --install-extension tamasfe.even-better-toml
   code --install-extension unional.vscode-sort-package-json
   code --install-extension viijay-kr.react-ts-css
   code --install-extension vitaliymaz.vscode-svg-previewer
   code --install-extension vunguyentuan.vscode-css-variables
   code --install-extension wix.glean
   ```

   This installs some VS Code extensions we will need.<br><br>

10. We recommend installing and using Chrome so that you have the same DevTools as others.<br><br>
    If you don't have Chrome installed yet, you can install it with Chocolatey. To do this, copy the following text and right-click in the blue middle part of the PowerShell window to paste the text. Hit enter.<br><br>
    ```bash
    choco install googlechrome --yes
    ```
    This uses Chocolatey to install Chrome.<br><br>
11. Install the following Chrome Extensions:
    - [React Developer tools Chrome Extension](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en)
    - [Refined GitHub Chrome Extension](https://chrome.google.com/webstore/detail/refined-github/hlepfoohegkhhmjieoechaddaejaokhf?hl=en)
    - [Socket Security Chrome Extension](https://chrome.google.com/webstore/detail/socket-security/jbcobpbfgkhmjfpjjepkcocalmpkiaop?hl=en)
    - [Web Vitals Chrome Extension](https://chrome.google.com/webstore/detail/web-vitals/ahfhijdlegdabablpippeagghigmibma?hl=en)
12. <a name="vs-code-settings"></a> Next we will configure VS Code.<br><br>
    Open VS Code and then press the keys <kbd>Ctrl</kbd>-<kbd>Shift</kbd>-<kbd>P</kbd>. Type in "Settings" and select the item that says `Preferences: Open User Settings (JSON)`:<br><br>
    <img src="./general-vscode-settings.png"><br><br>
    Once the settings file is open, we will want to add the settings below.<br><br>
    First of all, identify whether your settings file is empty or not. This is what an empty file looks like:<br><br>
    <img src="./general-vscode-settings-empty.png"><br><br>
    If your file is **not empty** (if there is more text within the curly brackets), then **we will need to do something extra** - add a comma on the second to last line:<br><br>
    <img src="./general-vscode-settings-comma.png"><br><br>
    Now in both cases you will want to paste the following settings **before the closing curly bracket (before the `}`)**:<br><br>

    ```json5
    "editor.wordWrap": "on",
    "editor.minimap.enabled": false,
    "editor.linkedEditing": true,
    "editor.tabSize": 2,
    "workbench.editor.tabSizing": "shrink",
    "workbench.editor.closeEmptyGroups": false,
    "workbench.tree.enableStickyScroll": true,
    "terminal.integrated.stickyScroll.enabled": true,
    "terminal.integrated.defaultProfile.windows": "Git Bash",
    "terminal.integrated.suggest.enabled": true,
    "terminal.integrated.suggest.quickSuggestions": {
      "commands": "off",
      "arguments": "off",
      "unknown": "off"
    },
    "files.insertFinalNewline": true,
    "files.trimFinalNewlines": true,
    "files.trimTrailingWhitespace": true,
    "[markdown]": {
      "files.trimTrailingWhitespace": false
    },
    "files.autoSave": "onFocusChange",
    "editor.formatOnSave": true,
    "editor.codeActionsOnSave": [
      // Sort package.json keys with https://marketplace.visualstudio.com/items?itemName=unional.vscode-sort-package-json
      "source.sortPackageJson"
    ],
    "tsImportSorter.configuration.groupRules": ["^node:", {}, "^[.]"],
    "tsImportSorter.configuration.keepUnused": [".*"],
    "tsImportSorter.configuration.emptyLinesBetweenGroups": 0,
    "tsImportSorter.configuration.wrappingStyle": "prettier",
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "[html]": {
      "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[javascript]": {
      "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[javascriptreact]": {
      "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[typescript]": {
      "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[typescriptreact]": {
      "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[json]": {
      "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[jsonc]": {
      "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "prettier.singleQuote": true,
    "prettier.trailingComma": "all",
    "prettier.documentSelectors": [
      // Enable prettier-vscode to format *.sql files (eg. with prettier-plugin.sql)
      // https://github.com/prettier/prettier-vscode/issues/3248#issuecomment-1956209714
      "**/*.sql"
    ],
    "eslint.runtime": "node",
    "totalTypeScript.hideAllTips": true,
    ```

    After you have pasted the settings, save the file with `File` -> `Save` in the top menu.<br><br>
    If you had any previous settings beforehand, you may notice that some text above will be underlined by a squiggly yellow line. This is a warning because we pasted some duplicate properties from the code above.<br><br>
    If you have any of these warnings, we should fix them. For each one of these lines with the warnings on them, delete the full line, including the comma at the end. We usually like to select from the start of the first `"` to just before the next `"` on the next line:<br><br>
    <img src="./general-vscode-settings-fix-warnings.png"><br><br>
    If you made any further changes to the file, save the file again with `File` -> `Save` in the top menu.<br><br>

13. To verify that the VS Code configuration was successful, select `Terminal` -> `New Terminal` in the top menu:<br><br>
    <img src="./general-vscode-terminal-new-terminal.avif"><br><br>

    Once the terminal appears, copy the following text, paste it into the terminal and hit return:

    ```bash
    echo -e "VS Code Config:\n  Shell: $SHELL\n  Terminal: $TERM"
    ```

    This should display the following output on Windows:

    ```bash
    VS Code Config:
      Shell: /usr/bin/bash
      Terminal: xterm-256color
    ```

    Check your output carefully - if line 2 (`Shell: ...`) and line 3 (`Terminal: ...`) on your screen aren't exactly the same as the output above, return to the previous step and check if everything was completed fully.

14. Now we will configure Hyper.<br><br>
    Open Hyper and then select Edit -> Preferences, which will open a text file in an editor:<br><br>
    <img src="./windows-3-hyper-preferences.png"><br><br>
    In this file, we will do three things:

    1. Find `shell: '',` and replace it with `shell: 'C:\\Program Files\\Git\\bin\\bash.exe',`
    2. Find `env: {},` and replace it with `env: { TERM: 'cygwin' },`

    Then save the file and close and restart Hyper.

15. To verify that the Hyper configuration was successful, copy the following text, paste it into Hyper and hit return:

    ```bash
    echo -e "Hyper Config:\n  Shell: $SHELL\n  Terminal: $TERM"
    ```

    This should display the following output:

    ```bash
    Hyper Config:
      Shell: /usr/bin/bash
      Terminal: cygwin
    ```

    Check your output carefully - if line 2 (`Shell: ...`) and line 3 (`Terminal: ...`) on your screen aren't exactly the same as the output above, return to the previous step and check if everything was completed fully.

16. <a name="postgresql"></a>We will now install PostgreSQL. Search for Hyper in the start menu, then right click on it and choose "Run as Administrator".

    Copy the following text, paste it in Hyper and hit return.

    ```bash
    choco install postgresql17 --yes --params '/Password:postgres'
    ```

    This will install PostgreSQL and create a default user of `postgres` and a password of `postgres`. Remember this password and use it any time it asks from now on.

    After the installation is complete, close Hyper and reopen it (just as a normal user - not as an administrator).

    Now let's set an environment variable to tell PostgreSQL where to find the programs and where to put the data. Copy and run each of these lines separately in Hyper:

    ```bash
    echo -e "\nexport PATH=\$PATH:\"/c/Program Files/PostgreSQL/17/bin\"" >> ~/.bash_profile
    echo "export PGDATA=\"/c/Program Files/PostgreSQL/17/data\"" >> ~/.bash_profile
    echo "export PSQL_PAGER=\"less --chop-long-lines --header 1\"" >> ~/.bash_profile
    echo "export LANG=en_US.UTF-8" >> ~/.bash_profile
    source ~/.bash_profile
    perl -i -pe "s/^[#\s]*(timezone|log_timezone)\s*=.+$/\1 = 'UTC'/" "$PGDATA/postgresql.conf"
    perl -i -pe "s/^logging_collector = on/logging_collector = off/" "$PGDATA/postgresql.conf"
    ```

    <!--

    You may encounter some "Permission denied" warnings during some of these steps, which should be no problem:

    <img src="./windows-4-permission-denied.png"><br><br>

    Test whether the previous command worked by running the following command:

    ```bash
    cat "$USERPROFILE/.bash_profile"
    ```

    It should print out something that looks like the following (although the `17` number may be different for you):

    ```bash
    export PATH=$PATH:"/c/Program Files/PostgreSQL/17/bin"
    export PGDATA="/c/Program Files/PostgreSQL/17/data"
    ```

    -->

    We can now test whether PostgreSQL has been correctly installed by starting the database. To do this, we can run the following command:

    ```bash
    postgres
    ```

    If it worked, it should print out some messages and end with the rectangular cursor on the left side of the screen:

    <img src="./windows-5-postgres.avif"><br><br>

    You will need to run this every time you want to use your database.

    When you want to stop PostgreSQL again, just stop it like any other command line program using the shortcut <kbd>control</kbd>-<kbd>C</kbd>.

    Now we will connect to PostgreSQL using a tool called `psql` and add a new table, to make sure everything is working with the connection.

    Open a new tab in Hyper using <kbd>control</kbd>-<kbd>shift</kbd>-<kbd>T</kbd> and run the following command:

    ```bash
    psql -U postgres
    ```

    It will ask you for a password - enter `postgres` (the password will not be visible). Now it should look like this:<br><br>

    <img src="./macos-5.1-psql.png"><br><br>

    If your screen looks like the above screenshot, type in or copy and paste the following query (this is a language called SQL):

    ```sql
    CREATE TABLE users(
      id serial PRIMARY KEY,
      first_name VARCHAR (100) NOT NULL,
      last_name VARCHAR (100) NOT NULL
    );
    ```

    It should print `CREATE TABLE` on the line after running the query. Your screen should look like this:<br><br>

    <img src="./macos-5.2-psql.png"><br><br>

    Now let's check that the table has been created. Run this query:

    ```
    \dt
    ```

    This will show the tables that you have, including the newly-created `users` table. Your screen should look like this (although the owner may be `postgres` on your screen):<br><br>

    <img src="./macos-5.3-psql.png"><br><br>

    Finally, let's delete the table again to clean up. Exit the table view by typing <kbd>q</kbd> on your keyboard and then run this query:

    ```sql
    DROP TABLE users;
    ```

    It should print `DROP TABLE` on the line after running the query. Your screen should look like this:<br><br>

    <img src="./macos-5.4-psql.png"><br><br>

    Great, PostgreSQL is set up! 🚀 Now you can exit from `psql` again by writing `exit` and hitting return:

    ```
    exit
    ```

    It should exit and send you back to the command line. Your screen should look similar to this (the last line will not be exactly the same):<br><br>

    <img src="./macos-5.5-psql.png"><br><br>

    Now close the new tab in Hyper with <kbd>control</kbd>-<kbd>W</kbd>, and stop PostgreSQL again using <kbd>control</kbd>-<kbd>C</kbd>. PostgreSQL should shut down - your screen should look similar to this (the last line will not be exactly the same):<br><br>

    <img src="./macos-5.6-psql.png"><br><br>

17. <a name="docker"></a>We will now install Docker.

    **Option A - Windows 10/11 Pro:**

    1. Search for Hyper in the start menu, then right click on it and choose "Run as Administrator".
    2. Copy the following text and paste it into Hyper. Hit enter.

       ```bash
       choco install wsl2 --yes
       choco install wsl-ubuntu-2004 --yes
       choco install docker-desktop --yes
       ```

    3. Open start menu and search for "Docker Desktop". Run it. This will set up and start Docker.<br><br>
       You will need to run this every time you want to work with Docker after you restart.

    **Option B - Windows 10/11 Home:**

    1. **Windows 10 only:** Click on the start menu, type in "winver" to the search and verify you have at least Windows 10 version 1903. If your number is lower than 1903, run Windows Update.<br><br>
       <img src="windows-6-winver.jpg"><br><br>
    2. Search for Hyper in the start menu, then right click on it and choose "Run as Administrator".
    3. Copy the following text and paste it into Hyper. Hit enter.

       ```bash
       choco install wsl2 --yes
       choco install wsl-ubuntu-2004 --yes
       choco install docker-desktop --yes
       ```

    4. Open the start menu and search for "Ubuntu". Start it - it should ask you to create a user with a password. This will be your user to log in to your Ubuntu Linux Subsystem - note down the username and password somewhere secure to make sure you do not forget it.
    5. Open the start menu and search for "Docker Desktop". Start it and go to the Settings. Under the General tab, you will find an option called "Use WSL 2 based engine". Make sure this is checked.

    When opening Docker Desktop during Option A or Option B, an error message `Docker Desktop - Windows Hypervisor is not present` may appear:<br><br>

    <img src="windows-7-docker-desktop-error.avif"><br><br>

    If this appears for you, follow the next steps to enable virtualization on your machine (if you don't receive the error, you can skip to the Docker testing step).

    1. Close PowerShell and open it again as administrator (like in step 2). Copy the following text and right-click in the blue middle part of the PowerShell window to paste the text. Hit enter.

       ```bash
       Get-ComputerInfo -Property HyperVRequirementVMMonitorModeExtensions,HyperVRequirementVirtualizationFirmwareEnabled | Format-List
       ```

       This should display the following output:

       ```bash
       HyperVRequirementVMMonitorModeExtensions: True
       HyperVRequirementVirtualizationFirmwareEnabled: False
       ```

       The output indicates that your machine supports virtualization, but it is not enabled. Ensure that your output is the same before proceeding with the steps below to enable virtualization.

    2. Restart your machine
    3. As soon as the monitor turns black during restart, press the BIOS key or UEFI key for your machine repeatedly. If you're not sure what that key is, either try to read the key on the screen quickly as your machine restarts or refer to BIOS key documentation online eg. [the guide by the University of Wisconson-Madison](https://kb.wisc.edu/helpdesk/page.php?id=58779) (common keys are <kbd>Delete</kbd>, <kbd>Esc</kbd>, <kbd>F1</kbd>, <kbd>F2</kbd>, <kbd>F9</kbd>, <kbd>F10</kbd> or <kbd>F12</kbd>)
    4. On some machines, you will need to find and select the option to enter the BIOS / UEFI after hitting the hotkey
    5. Find the virtualization option for your machine and enable it. If you're not sure what the option is called, refer to the [virtualization options here](https://support.microsoft.com/en-us/windows/enable-virtualization-on-windows-11-pcs-c5578302-6e43-4b4b-a449-8ced115f58e1) (often in `Advanced` settings, common names are `Virtualization`, `VMX`, `VT-x`, `VT-d`, `AMD-V`, or `SVM`)
    6. Find and select the option to save changes and exit the BIOS / UEFI
    7. Open Docker Desktop again (as instructed in Option A or Option B) to verify that the error has been resolved

18. Test if Docker is installed by running the following command on the command line:

    ```bash
    docker run hello-world
    ```

    It should print out a welcome message like this:<br><br>
    <img src="macos-7-docker-hello-world.png"><br><br>

19. <a name="expo-react-native"></a>We will now install EAS CLI for React Native. Search for Hyper in the start menu, then right click on it and choose "Run as Administrator".

    Copy the following text, paste it in Hyper and hit return.

    ```bash
    pnpm add --global eas-cli --allow-build=dtrace-provider
    ```

    You can ignore the lines marked `WARN` - these do not indicate problems:<br><br>

    <img src="./general-expo-init.avif"><br><br>

    Lastly, we'll install Expo on your phone, so that you can also test on a real device.

    On your phone, go to the app store and install Expo on your phone ([Android](https://play.google.com/store/apps/details?id=host.exp.exponent&hl=en&gl=US), [iOS](https://apps.apple.com/us/app/expo-go/id982107779)). Create an account and log in.

20. Next we will set up some dependencies for Expo and React Native.

    Copy each line in the following text, paste it in Hyper and hit return.

    ```bash
    choco install androidstudio --yes
    echo "export PATH=$HOME/AppData/Local/Android/Sdk/platform-tools:\$PATH" >> ~/.bash_profile
    source ~/.bash_profile
    ```

    This will install Android Studio, for creating and running Android virtual devices in an emulator.

    Open Android Studio by finding it using the Start menu.

    If it asks to import Android Studio Settings, choose **Do not import settings**.

    When prompted, choose a **Custom** install:<br><br>

    <img src="./general-android-studio-setup-install-type-custom.png"><br><br>

    Leave the default JDK installation location as-is and click **Next**:<br><br>

    <img src="./general-android-studio-setup-jdk-location.png"><br><br>

    Uncheck the box next to **Android Virtual Device** (we will install our own manually):<br><br>

    <img src="./general-android-studio-setup-uncheck-virtual-device.png"><br><br>

    For each of the licenses with red stars next to them, click on the license name and then accept the agreement, finally clicking on **Finish** when you have accepted all agreements:<br><br>

    <img src="./general-android-studio-setup-license-agreement.png"><br><br>

    This will download components, which may take a few minutes:<br><br>

    <img src="./general-android-studio-setup-downloading-components.png"><br><br>

    Next will be getting a virtual device installed. Click on **More Actions** and select **Virtual Device Manager**:<br><br>

    <img src="./general-android-studio-virtual-device-dropdown.png"><br><br>

    Click on the **Create device** button on the top left of the window:<br><br>

    <img src="./general-android-studio-virtual-device-create-device.png"><br><br>

    Select the Pixel 3a as the hardware device.

    Under the **Recommended** tab (the default tab), locate the system image named **S** (API level 31) and click on the downward arrow icon (download button) next to it. In the window that pops up, accept the license agreement and click **Next**:<br><br>

    <img src="./general-android-studio-virtual-device-system-image-download.png"><br><br>

    Once the download completes, select the image you just downloaded and click on **Next** through the rest of the steps until the virtual device has been created:<br><br>

    <img src="./general-android-studio-virtual-device-system-image-select.avif"><br><br>

    The device will now show up in the Device Manager. Click on the triangular play button to launch the virtual device in the emulator. An emulator window will appear showing the screen of the virtual device:<br><br>

    <img src="./general-android-studio-virtual-device-launch.avif"><br><br>

    If a message pops up in the virtual device that the "System UI isn't responding" at any point during these steps, you can click on "Wait".

    Before running the first Expo app, test that the Android Studio `adb` (Android Debug Bridge) program has been set up properly, by running the following in a new Hyper command line (open a new tab):

    ```bash
    adb
    ```

    It should print the version and help information:<br><br>

    <img src="./general-android-studio-adb.png"><br><br>

21. To verify that Expo is working with the Android Studio virtual device copy and run each of these lines separately in Hyper:

    <!-- TODO: Check if we can remove the `echo ...` and `pnpm install ...` steps below when Expo supports RN 0.72 with symlinks https://github.com/upleveled/system-setup/issues/28 -->

    ```bash
    cd ~
    mkdir -p projects
    cd projects
    pnpm create expo-app@latest --template blank-typescript expo-test
    cd expo-test
    pnpm start --android
    ```

    This will create a new Expo demo app and start it.

    If this step doesn't work, it's possible that you may not have the emulator running - check the last part of the previous step to see how to launch the emulator.

    The first thing that you will see is the installation of Expo Go on the virtual device:<br><br>

    <img src="./general-expo-start-expo-go-install.png"><br><br>

    Next, the Metro bundler will bundle the JavaScript for the device, which may take some time. You will see a loading bar in the command line and a loading screen on the virtual device:<br><br>

    <img src="./general-expo-start-metro-bundling-cli.png"><br><br>

    <img src="./general-expo-start-metro-bundling-emulator.png"><br><br>

    After the bundling has completed, the simple app should show up in the virtual device, with the words "Open up App.js to start working on your app!" (if it instead says "Universal React with Expo", this is also ok):<br><br>

    <img src="./general-expo-start-app-loaded.png"><br><br>

    Click on the small `x` at the top right of the virtual device frame to stop the virtual device - this will save a snapshot to make starting the virtual device faster in the future.

22. If you don't have one yet, create a Google account [here](https://accounts.google.com/signup?hl=en). Make a note of the email address associated with this account for usage in later steps.
23. If you don't have one yet, create a GitHub account [here](https://github.com/join). Make sure to set a name.

    If you already have a GitHub account and you haven't set a name on GitHub yet, go to the [GitHub Profile Settings](https://github.com/settings/profile) and add a name:<br><br>
    <img src="./general-github-profile-settings.png"><br><br>
    We will use this name in the next step.<br><br>

24. For this step, we'll need to **edit some of the information in the commands** by adding our own information.<br><br>
    First of all, we will set our name, which will be the same name as on our GitHub profile:<br><br>
    <img src="./general-github-name.png"><br><br>
    Copy your name from your profile, **add it in quotes** in the command (replace `Mona Lisa Octocat`) and run the command:<br><br>
    ```bash
    git config --global user.name "Mona Lisa Octocat"
    ```
    You can test whether the name was set correctly with the next command (if it worked, it will print the name on the next line):<br><br>
    ```bash
    git config --global user.name
    ```
    <br>For running the next command, **add your email in quotes**:<br><br>
    ```bash
    git config --global user.email "monalisaoctocat@example.com"
    ```
    You can test whether the email was set correctly with with the next command (if it worked, it will print the email on the next line):<br><br>
    ```bash
    git config --global user.email
    ```
    This prepares `git` so that your work is attributed correctly to you.<br><br>

<!-- 25. Copy the following text, paste it in the terminal and hit return.<br><br>
    ```bash
    git config --global credential.helper wincred
    ```
    This step will save your GitHub password so that you don't need to enter it every time.<br><br> -->

25. Copy the following text, paste it in Hyper and hit return.<br><br>
    ```bash
    git config --global init.defaultBranch main
    ```
    This step will change the default Git branch from `master` to `main` (see https://github.com/github/renaming).<br><br>
26. Copy and run each of these lines separately in Hyper.<br><br>
    ```bash
    git config --global core.autocrlf false
    git config --global core.eol lf
    ```
    This step will improve line breaks compatibility on Windows.
27. Go back to GitHub, and go to your profile page by clicking on your avatar at the top right and selecting **Your profile**<br><br>
    <img src="./general-github-your-profile.png"><br><br>
    Copy the `github.com/...` URL in the address bar of your browser, for use in the next step.
28. Open the Start menu and start Slack. Log in to the UpLeveled Slack. Send your GitHub profile URL to [Karl](slack://user?team=TFFSPKL92&id=UFG252SH0). Also send your Google Account email address to Karl (if you haven't already).
29. <a name="specs"></a>Open the start menu, type "Settings", open the app (or click on the cog on the left) and copy and send two sets of details:
    - Select "System" and "About". Under "Device specifications", click the Copy button and paste this to Karl
    - Under "Windows specifications", click the Copy button and paste this to Karl
30. On your phone, go to the app store and install Slack on your phone. Log in to the UpLeveled Slack.

## Optional Software

1. If you would like to check the spelling of all code you write in VS Code, try out [Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker). You can install on the command line with this command:

   ```bash
   code --install-extension streetsidesoftware.code-spell-checker
   ```

2. If you want to easily capture screenshots and draw and write on them, try Flameshot:

   ```bash
   choco install flameshot --yes
   ```

3. If you need to record mp4 videos of your screen with sound, try out [Loom](https://www.loom.com/).

   An alternative without the limitations of Loom is Screen to Gif (however, it does not record audio):

   ```bash
   choco install screentogif --yes
   ```

4. If you need a clipboard manager to keep a history of things that you have copied, this is an awesome option:

   ```bash
   choco install ditto --yes
   ```

5. To simultaneously test your web design in multiple mobile viewports, try Responsively App:

   ```bash
   choco install responsively --yes
   ```

6. To remove secrets, large files or other undesirable files from your Git repository, try BFG Repo-Cleaner:

   ```bash
   choco install bfg-repo-cleaner --yes
   ```

7. If you're running out of space on your computer, you can use WinDirStat to analyze your hard drive and show a chart of which items are taking up how much space:

   ```bash
   choco install windirstat --yes
   ```

8. To add [an assortment of new features](https://www.fourth-wall.co.uk/post/powertoys-11-awesome-features-microsoft-won-t-add-to-windows) to Windows such as "pinning" a window to stay on top of all others, quickly renaming or resizing multiple files, splitting your running apps into regions of the screen and more, try Microsoft PowerToys:

   ```bash
   choco install powertoys --yes
   ```

## Software Upgrades

Most software upgrades can be performed with `choco upgrade <package name>`, but some software upgrades require additional steps:

1. Node.js with pnpm
   ```bash
   choco upgrade nodejs-lts --yes
   corepack disable
   corepack enable
   corepack prepare pnpm@latest --activate
   ```
