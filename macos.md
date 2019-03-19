1. Click on the magnifying glass at the very top right of your screen to open the Spotlight search, enter "terminal.app" and hit return:<br><br>
   <img src="./macos-1-start-terminal.png">
   <br>This will launch the macOS terminal.<br><br>
2. Copy the following text, paste it in the terminal and hit return.<br><br>
   ```sh
   /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
   ```
   This will install Homebrew, a package manager which will allow us to install and uninstall programs from the terminal.<br><br>
3. Hit return when the installer asks you to:<br><br>
   <img src="./macos-2-hit-return.png">
   <br><br>Also enter your Mac password when the installer asks you to. Note: while typing your password, you will not see the letters being entered. This is normal:<br><br>
   <img src="./macos-3-enter-password.png"><br><br>
4. The installer will take a bit of time and then show a message that "Installation successful!", signaling that it is done:<br><br>
   <img src="./macos-4-installation-successful.png"><br><br>
5. Copy the following text, paste it in the terminal and hit return.<br><br>
   ```sh
   brew install git node yarn
   ```
   This uses Homebrew to install Git, Node.js and Yarn.<br><br>
6. Copy the following text, paste it in the terminal and hit return.<br><br>
   ```sh
   brew cask install vscode
   ```
   This uses Homebrew to install Visual Studio Code.<br><br>
7. Copy the following text, paste it in the terminal and hit return.<br><br>
   ```sh
   npx create-react-app --help
   ```
   This last step will prepare a program that we will use in the course.<br><br>
8. The preparation will take a while and then respond with a message that some modules have been installed, similar to this:<br><br>
   <img src="./general-1-cra-installed.png"><br><br>
