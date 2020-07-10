# Windows Subsystem for Linux
![alt text](https://betanews.com/wp-content/uploads/2017/09/linux_windows_logos-600x400.jpg)
---

## Preparing Windows for your new Linux kernel!

1. #### Updating windows 
Your Windows 10 version must be *Build 19041* or higher. To check, open System Information. The second line will say 'Version'. To update, follow this link:
> https://www.microsoft.com/en-us/software-download/windows10
2. #### Enable WSL And Virtual Machine Platform
Open PowerShell as Administrator (search 'PowerShell', right click, and select 'Launch as Administrator') and run paste in this command (the following commands will be done in the same way as well):
>`dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart`

>`dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart`
3. #### Update the WSL 2 Linux kernel
We'll be using WSL 2 for it's better file system integration and performance boosts. Download and run this update:
>https://docs.microsoft.com/en-us/windows/wsl/wsl2-kernel
4. ##### Set WSL 2 as your default version
Back in your PowerShell, run this command:
>`wsl --set-default-version 2`

## Installing your Linux distro!
We'll be going with Ubuntu, because of its stability, user-friendliness, and performance, but there are other options out there!
[Download Ubuntu 20.04 LTS from the Microsoft Store](https://www.microsoft.com/en-ca/p/ubuntu-2004-lts/9n6svws3rx71?rtc=1&activetab=pivot:overviewtab)

Once it's finished downloading and installing, click Launch to open a new terminal! At first, it will take a second to install and get set up, but eventually it will ask you for a new username and password. These will be your credentials for your new Linux environment, and can be the same, or different than your Windows credentials.

The first thing we want to do with our fresh-off-the-shelf distro is check for and run updates! In Ubuntu, our package manager (like the app store, or Microsoft store) is called `apt`, and we'll use it a lot to get some key things installed. First of all, we're going to use it to update and upgrade our new system by first entering `sudo apt update`, which will fetch all of our available updates. 
> You'll notice that you're prompted for your password. This is because we're using `sudo`, which is our linux equivilent of 'Running as Administrator'. You'll also notice that your password does not appear on screen as you type it. Don't worry, this is just to protect you against people peering over your shoulder, the password is still being typed. 

Once that has completed (it could take a minute), we're going to run all of our new updates with `sudo apt upgrade`. This will prompt you with a `[y/n]`, so just type `y` and hit enter. 

You're all up to date! Now we can work on setting up our programming environment. 

## Installing Windows Terminal

The Ubuntu shell is great and all, but it can get a little buggy when we try to do things like clear the screen. Luckily, Windows finally has a Terminal program that can go toe-to-toe with all the others! It has a bunch of really awesome features that you normally wouldn't get with just the WSL (multiple tabs, tiling/panes) [Start off by downloading Windows Terminal.](https://www.microsoft.com/en-us/p/windows-terminal/9n0dx20hk701)

When we launch it, you'll see the familiar command prompt. This, obviously, is not our new Ubuntu shell. If you click the downwards arrow in the top bar, you'll see a list of the different shells that Windows Terminal can open, which includes your new linux distro! Go ahead and click on "Ubuntu 20.04", and you'll see your familiar bash shell, but with one thing different. Beside our name, we don't see `~`, but instead `/mnt/c/Users/userName`. This is no longer the home directory for Ubuntu but instead is our home directory in Windows. Looks like we need to do some digging into the settings, so go ahead and open them with the shortcut `Ctrl ,`. It will ask your how you want to open the settings, for now, we'll just select Notepad. 

The settings will appear as a JSON object (more on those later in the course), but we'll just be here editing a couple of lines. Take notice to the value `"defaultProfile"`, because we'll be coming back to that. Scroll down until you see an object that looks like this

```json
            {
                "guid": "{07b52e3e-de2c-5db4-bd2d-ba144ed6c273}",
                "hidden": false,
                "name": "Ubuntu-20.04",
                "source": "Windows.Terminal.Wsl",
            }
```

First things first, we're going to change the starting directory when you open the shell. After the first line, paste this into the object:
```
"startingDirectory": "//wsl$/Ubuntu-20.04/home/[YOUR_UBUNTU_USERNAME]",
```
Secondly, we'll change it so that when you open Windows Terminal, the Ubuntu shell will be the one that opens. Take the value of the `"guid"` key, copy & paste it into the `"defaultProfile"` line we looked at earlier. Save and quit the settings file, and restart your terminal. You should be greeted by your familiar Ubuntu shell at it's true home directory. 

### Directory Structure
As of right now, this Ubuntu environment is not connected to our usual User folders (Documents, Downloads, etc.). Organizationally, you might find it better to have this environment stand outside of your Windows environment, but if you'd rather keep them tied together, we can create a symbolic link between your Ubuntu home directory and Windows. To correctly do this, you must have your Windows Username correct. To double check, navigate to `C:/Users`. Among 'Default', and 'Public', you'll see your Username.

Back in your Ubuntu terminal, type in the following command:
>`ln -s /mnt/c/Users/[YOUR USERNAME HERE]/Documents Documents`

This will create a symbolic link between a directory in our Ubuntu home called Documents, and the Windows folder Documents. The same can be done with Downloads and Desktop:
>`ln -s /mnt/c/Users/[YOUR USERNAME HERE]/Downloads Downloads`

>`ln -s /mnt/c/Users/[YOUR USERNAME HERE]/Desktop Desktop`

We'll also want to set up a folder where we'll store the bulk of our General Assembly course material, assignments, projects, and homework; so lets set that up with `mkdir GA`. 

Now if we type `ls` into our terminal, we'll see our directories created.

## The Good Stuff
First things first, we're going to need an IDE (Integrated Development Environment), which is what we'll be using to write our code! 
#### Visual Studio Code
This will be our IDE of choice. It actually (being a Microsoft product) has marvelous integrations with the WSL. Go ahead and download it here:
>https://code.visualstudio.com/

As you go through the installer, you'll see that it has checked off `Add to PATH (requires shell restart)`. Leave this checked as it will be extremely useful later on. 

Once it's finished installing, *don't launch it yet!* We'll want to launch it from our Ubuntu shell. You might be wondering, "but we downloaded that for Windows, how is it going to work in Linux?", which is a solid question. The short answer is, it won't! But what it will do, is connect to our WSL, and integrate it into our Windows GUI! In your terminal, we're going to want to create a file, and open it in vs code. First to create a file in bash, we'll use the `touch` command, followed by the name of our new file, `test.txt`. Now, if we `ls`, we should see our new text file there. All we need to do to open it, is use the VS Code shell command (that was the option that was checked off in our installer), and the name of our file: `code test.txt`.

You'll be greeted with a welcome page, and maybe a couple pop-ups in the bottom right corner letting you know about an Extension that is recommended. 

Extension will be our friends in this adventure. They're basically ways in which we can improve our editor. So lets install a couple. To open the Extensions panel, hit `ctrl + shift + x`, and start typing the name to see the results. 

##### Remote - WSL
This is the extension that VSCode recommends to users using the WSL. It allows for you to use the WSL terminal right in VS Code! 
##### Prettier - Code formatter
This extension will greatly help you write cleaner, easier to read code. When you save a file, it will touch up any indenting or line breaks. 

### Git
Git will be our go-to for version control and making our code more widely accessable. Back in your Ubuntu terminal, type the following command to install Git
> `sudo apt install git`

## Unit 2 Tools and Programs
---
### Node.js
NodeJS will be used in our second unit, but lets get it installed and set up right now with the following command:
> `sudo apt install nodejs`

### MongoDB
Our first Database! We'll install it similarly to how we install everything else:
> `sudo apt install mongodb`

To make sure that it installed correctly, run `mongod --version`. You should see the version number for what you just installed. Finally, we'll set MongoDB up to run when we start up our WSL service with this command:
> `sudo /etc/inid.d/mongodb enable mongod`

## Unit 3 Tools and Programs
---

### Python3
We'll be installing and using Python3, since Python2 is no longer supported, and Python3 has been widely integrated. 
> `sudo apt install python3.8`
To make sure it installed, we'll check both the version of python (`python3 --version`) as well as pip, which we'll be using to install Python packages (`pip3 --version`)

### PostgreSQL
This is going to be our second Database that we'll be using. We'll install it along with the -contrib package, which has some extra utilities. 
> `sudo apt install postgresql postgresql-contrib`

### Django
