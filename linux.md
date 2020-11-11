# System Setup for Linux (Ubuntu & Debian)

1. Click on the Ubuntu icon in the top left to open the Dash, type in "Terminal" and click on the matching application.<br><br>
   <img src="linux-1-open-terminal.png"><br>
2. Copy the following text, paste it in the terminal and hit return.<br><br>
   ```bash
   sudo snap install node --channel=14/stable --classic
   ```
   This uses Snap to install Node.js and Yarn.<br><br>
3. Copy the following text, paste it in the terminal and hit return.<br><br>
   ```bash
   sudo snap install code --classic
   ```
   This uses Snap to install VS Code.<br><br>
4. Copy each line in the following text, paste it in the terminal and hit return.<br><br>

   ```bash
   code --install-extension Cardinal90.multi-cursor-case-preserve
   code --install-extension dbaeumer.vscode-eslint
   code --install-extension esbenp.prettier-vscode
   code --install-extension thebearingedge.vscode-sql-lit
   code --install-extension jpoissonnier.vscode-styled-components
   code --install-extension kumar-harsh.graphql-for-vscode
   code --install-extension meganrogge.template-string-converter
   code --install-extension sysoev.vscode-open-in-github
   code --install-extension wix.glean
   ```

   This installs some VS Code extensions we will need.<br><br>

5. With each line below, copy the text, paste it in the terminal and hit return.<br><br>
   ```bash
   cd /tmp
   wget https://zoom.us/client/latest/zoom_amd64.deb
   sudo dpkg --install zoom_amd64.deb
   cd -
   ```
   This installs Zoom.<br><br>
6. Copy the following text, paste it in the terminal and hit return.<br><br>
   ```bash
   sudo snap install slack --classic
   ```
   This uses Snap to install Slack.<br><br>
7. Copy the following text, paste it in the terminal and hit return.<br><br>
   ```bash
   sudo snap install postman
   ```
   This uses Snap to install Postman.<br><br>
8. We recommend installing and using Chrome so that you have the same Devtools as others.<br><br>
   If you don't have Chrome installed yet, you can install it by copying each line below, pasting it in the terminal and hitting return.<br><br>
   ```bash
   cd /tmp
   wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
   sudo dpkg --install google-chrome-stable_current_amd64.deb
   cd -
   ```
   This installs Chrome.<br><br>
9. Install the following Chrome Extensions:
   - [React Developer tools Chrome Extension](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en)
   - [Refined GitHub Chrome Extension](https://chrome.google.com/webstore/detail/refined-github/hlepfoohegkhhmjieoechaddaejaokhf?hl=en)
   - [Web Vitals Chrome Extension](https://chrome.google.com/webstore/detail/web-vitals/ahfhijdlegdabablpippeagghigmibma?hl=en)
10. Copy the following text, paste it in the terminal and hit return.<br><br>

    ```bash
    npx create-react-app --help
    ```

    This will prepare a program that we will use in the course.<br><br>

11. The preparation will take a while and then respond with a message that some modules have been installed, similar to this:<br><br>
    <img src="./general-1-cra-installed.png"><br><br>
12. Next we will configure VS Code.<br><br>
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
    "files.autoSave": "onFocusChange",
    "explorer.openEditors.visible": 0,
    "editor.tabSize": 2,
    "files.trimTrailingWhitespace": true,
    "files.trimFinalNewlines": true,
    "[markdown]": {
      "files.trimTrailingWhitespace": false
    },
    "workbench.editor.tabSizing": "shrink",
    "workbench.editor.closeEmptyGroups": false,
    "prettier.singleQuote": true,
    "prettier.trailingComma": "all",
    ```
    If you had any previous settings beforehand, you may notice that some text above will be underlined by a squiggly yellow line. This is a warning because we pasted some duplicate properties from the code above.<br><br>
    If you have any of these warnings, we should fix them. For each one of these lines with the warnings on them, delete the full line, including the comma at the end. We usually like to select from the start of the first `"` to just before the next `"` on the next line:<br><br>
    <img src="./general-5-vscode-settings-fix-warnings.png"><br><br>
13. <a name="postgresql"></a>We will now install PostgreSQL. Copy and run each of these lines separately in the terminal:<br>

    ```bash
    sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
    wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
    sudo apt update
    sudo apt install postgresql-12
    ```

    This will install PostgreSQL and create a default user of `postgres` with no password (only authorized users can use this user).<br><br>

    Your database will be started automatically - no need to start it manually.

    Now we will connect to PostgreSQL using a tool called `psql` and add a new table, to make sure everything is working with the connection.

    Run the following command:

    ```bash
    sudo -u postgres psql
    ```

    It should look like this:<br><br>

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

    This will show the tables that you have, including the newly-created `users` table. Your screen should look like this:<br><br>

    <img src="./macos-5.3-psql.png"><br><br>

    Finally, let's delete the table again to clean up. Run this query:

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

14. Copy the following text, paste it in the terminal and hit return.<br><br>

    ```bash
    sudo snap install docker
    ```

    This uses Snap to install Docker.<br><br>

15. Test if Docker is installed by running the following command on the command line:

    ```bash
    docker run hello-world
    ```

    It should print out a welcome message like this:<br><br>
    <img src="macos-6-docker-hello-world.png"><br><br>

16. <a name="expo-react-native"></a>We will now install Expo CLI for React Native.

    Copy the following text, paste it in the terminal and hit return.

    ```bash
    npm install -g expo-cli
    ```

    On your phone, go to the app store and install Expo on your phone. Create an account and log in.

17. Install the Android Studio Emulator for Expo by following this guide: https://docs.expo.io/workflow/android-studio-emulator/
18. To verify that Expo is working with Android Studio, copy and run each of these lines separately in the terminal:
    ```bash
    cd ~
    mkdir projects
    cd projects
    npx create-react-native-app --template blank expo-test
    cd expo-test
    yarn start
    ```
    This should run an Expo demo application, which will show you the Expo Developer Tools. In the left panel, select "Run on Android device/emulator". This should open up an Android emulator. It should show the words "Universal React with Expo".
19. If you don't have one yet, create a GitHub account [here](https://github.com/join). Make sure to set a name.

    If you already have a GitHub account and you haven't set a name on GitHub yet, go to the [GitHub Profile Settings](https://github.com/settings/profile) and add a name:<br><br>
    <img src="./general-6-github-profile-settings.png"><br><br>
    We will use this name in the next step.<br><br>

20. For this step, we'll need to **edit some of the information in the commands** by adding our own information.<br><br>
    First of all, we will set our name, which will be the same name as on our GitHub profile:<br><br>
    <img src="./general-7-github-name.png"><br><br>
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
21. Copy the following text, paste it in the terminal and hit return.<br><br>
    ```bash
    git config --global credential.helper cache
    ```
    This step will save your GitHub password for 15 minutes so that you don't need to enter it every time.<br><br>
22. Copy the following text, paste it in the terminal and hit return.<br><br>
    ```bash
    git config --global init.defaultBranch main
    ```
    This step will change the default Git branch from `master` to `main` (see https://github.com/github/renaming).<br><br>
23. Go back to GitHub, and go to your profile page by clicking on your avatar at the top right and selecting **Your profile**<br><br>
    <img src="./general-8-github-your-profile.png"><br><br>
    Copy the `github.com/...` URL in the address bar of your browser, for use in the next step.
24. Open Dash and start Slack. Log in to the UpLeveled Slack. Send your GitHub profile URL to Antje.
25. On your phone, go to the app store and install Slack on your phone. Log in to the UpLeveled Slack.

## Optional Software

1. If you would like to check the spelling of all code you write in VS Code, try out [Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker). You can install on the command line with this command:
   ```bash
   code --install-extension streetsidesoftware.code-spell-checker
   ```
