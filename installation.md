## Installation Instructions
*updated 3/29/17*


### All
Please download the course files onto your machine's Desktop from the [Software Carpentry Unix Shell lesson](http://swcarpentry.github.io/shell-novice/data/shell-novice-data.zip) at http://swcarpentry.github.io/shell-novice/data/shell-novice-data.zip. Please unzip / extract these files as well into a folder also on your Desktop.


### Mac & Linux
No preparation needed, as we'll be using the built-in Terminal application.

### PC/Windows OS

**Nota Bene!** Windows 10 users, like Mac OS users, now get to enjoy both a rich GUI interface for typical work, and a Unix environment under the hood. Windows 10 Subsystem for Unix can be installed and used for most of the commands we showcase here. Please see these pages: [MS DevNet Unix on Windows documentation](https://msdn.microsoft.com/en-us/commandline/wsl/about) and How-To Geek's [Window's 10 New Bash](https://www.howtogeek.com/265900/everything-you-can-do-with-windows-10s-new-bash-shell/).

For PC folks, I recommend that you download and install [Git for Windows](https://git-for-windows.github.io/), commonly called *Git Bash*. This will give you a bash-like environment on your Windows system.

Seems complicated, but the steps are short and easy:
1. Download the Git for Windows installer from https://git-for-windows.github.io/.
2. Run the installer and follow the steps below:
    1. Click on "Next".
    2. Click on "Next".
    3. Keep "Use Git from the Windows Command Prompt" selected and click on "Next". If you forgot to do this programs that you need for the workshop will not work properly. If this happens rerun the installer and select the appropriate option.
    4. Click on "Next".
    5. Keep "Checkout Windows-style, commit Unix-style line endings" selected and click on "Next".
    6. Keep "Use Windows' default console window" selected and click on "Next".
    7. Click on "Install".
    8. Click on "Finish".
3. If your "HOME" environment variable is not set (or you don't know what this is):
    1. Open command prompt (Open Start Menu then type cmd and press [Enter])
    2. Type the following line into the command prompt window exactly as shown:<br>
         `setx HOME "%USERPROFILE%"`<br>
 
    3. Press [Enter], you should see SUCCESS: Specified value was saved.
    4. Quit command prompt by typing exit then pressing [Enter]


To verify that you've installed this correctly, double-click on the Git Bash icon on your Desktop (if you missed or forgot this option, Git > Git Bash should be in Programs section of your Start menu or search for 'git bash'). A window should open up and display something like

`yourname@YOURNAME92E4 MINGW64 ~
$
`

*For 3-hr hands-on workshops*: If for any reason you did not get this prompt, please arrive 30 minutes early so that we can troubleshoot your installation. If you do not arrive with enough lead time, we may ask you to use an on-site computer instead.

Finally, please download and install the [Software Carpentry Windows Installer](http://training.rcs.hbs.org/files/hbstraining/files/swcarpentryinstaller-win8win10.exe), which will add a few extra programs to the Git Bash installation. That installer can be found at http://training.rcs.hbs.org/files/hbstraining/files/swcarpentryinstaller-win8win10.exe.
