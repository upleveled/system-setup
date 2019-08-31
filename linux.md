# System Setup for Linux (Ubuntu & Debian)

1. Click on the Ubuntu icon in the top left to open the Dash, type in "Terminal" and click on the matching application.<br><br>
   <img src="linux-1-open-terminal.png"><br>
2. Copy the following text, paste it in the terminal and hit return.<br><br>
   ```sh
   curl -sL https://deb.nodesource.com/setup_11.x | sudo -E bash -
   ```
   This prepares the system to install Node.js.<br><br>
3. With each line below, copy the text, paste it in the terminal and hit return.<br><br>
   ```sh
   curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
   ```
   ```sh
   echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
   ```
   This prepares the system to install Yarn.<br><br>
4. With each line below, copy the text, paste it in the terminal and hit return.<br><br>
   ```sh
   curl -sL https://deb.nodesource.com/setup_11.x | sudo -E bash -
   ```
   ```sh
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   ```
   ```sh
   sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main" > /etc/apt/sources.list.d/vscode.list'
   ```
   This prepares the system to install Visual Studio Code.<br><br>
5. With each line below, copy the text, paste it in the terminal and hit return.<br><br>
   ```sh
   sudo apt-get update
   ```
   ```sh
   sudo apt-get install -y apt-transport-https
   ```
   ```sh
   sudo apt-get update
   ```
   ```sh
   sudo apt-get install -y git nodejs yarn code
   ```
   This uses apt to install Git, Node.js, Yarn and Visual Studio Code.<br><br>
6. Copy the following text, paste it in the terminal and hit return.<br><br>
   ```sh
   npx create-react-app --help
   ```
   This will prepare a program that we will use in the course.<br><br>
7. The preparation will take a while and then respond with a message that some modules have been installed, similar to this:<br><br>
   <img src="./general-1-cra-installed.png"><br><br
8. For this step, we'll need to **edit some of the information in the commands** by adding our own information.<br><br>
   First of all, we will set our name, which will be the same name as on our GitHub profile:<br><br>
   <img src="./general-2-github-name.png"><br><br>
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
9. Copy the following text, paste it in the terminal and hit return.<br><br>
   ```sh
   git config --global credential.helper cache
   ```
   This step will save your GitHub password for 15 minutes so that you don't need to enter it every time.<br><br>>
