How to "undervolt" AMD RYZEN 5800X3D Guide with PBO2 tuner.

Get the Most out of your 5800X3D using PBO curve optimizer!


# Table of Contents
1. [What are we doing?](#1-what-are-we-doing)
2. [Why are we doing it? What are the Benefits?](#2-why-are-we-doing-it-what-are-the-benefits)
3. [Options on what we can do](#3-options-on-what-we-can-do)
4. [What do we need?](#4-what-do-we-need)
5. [How to set PBO2 Tuner manually](#5-how-to-set-pbo2-tuner-manually)
6. [How to set PBO2 Tuner automatically with every System boot/restart/wakeup](#6-how-to-set-pbo2-tuner-automatically-with-every-system-bootrestartwakeup)
7. [(Optional) How to create a custom Alert for every time your PBO2 Tuner settings have been applied automatically](#7-optional-how-to-create-a-custom-alert-for-every-time-your-pbo2-tuner-settings-have-been-applied-automatically)
8. [What else can we do to improve CPU Clockspeeds and Thermals?](#8-what-else-can-we-do-to-improve-cpu-clockspeeds-and-thermals)
9.  [Resources used](#9-resources-used)


# 1) What are we doing? 
We are using PBO2 Tuner to set a CO curve (PBO 2.0 Curve Optimizer)

# 2) Why are we doing it? What are the benefits?
Because the 5800X3D is very much locked down on most Motherboards thanks to AMD and the Motherboard Manufacturers, most people won't be able to set CPU Ratio and/or use PBO in their Bios to get the most out of their 5800X3D. And if you just apply an offset undervolt, your performance will decrease, not increase, while some motherboards still don't allow using Curve Optimizer on the 5800X3D.
With this guide, you will at least be able to tweak things a little inside of Windows for potentially better thermals and clockspeeds that stay longer and more often at the highest they can be.

# 3) Options on what we can do
Set PBO 2.0 CO curve manually after every change in Windows powerstate
     
Set PBO 2.0 CO curve automatically with every system boot/restart/wakeup (means after every change in Windows powerstate)
     
**Optional** possibility to create custom BurnedToast alert whenever the CO curve has been set automatically. This way, you will always have the assurance that it actually applied.
    
# 4) What do we need?
[Debug-cli.7zp](https://www.overclock.net/threads/corecycler-tool-for-testing-curve-optimizer-settings.1777398/page-45#post-28999750) from PJVol from Overclocker.net thread

Monitoring software to confirm CO curve has been set([HwInfo](https://www.hwinfo.com/download/) or [RyzenMaster](https://www.amd.com/en/technologies/ryzen-master))

BurntToast module in PowerShell

Allow execution policies change in PowerShell

PowerShell script for BurntToast alert

Any PNG of your choice for the alert

# 5) How to set PBO2 Tuner manually
First download [Debug-cli.7zp](https://www.overclock.net/threads/corecycler-tool-for-testing-curve-optimizer-settings.1777398/page-45#post-28999750).

Get monitoring software like [HwInfo](https://www.hwinfo.com/download/) or [RyzenMaster](https://www.amd.com/en/technologies/ryzen-master). You will need this to see your CPU voltage and temperature and to later confirm that your settings work.

Run a CPU stress test like OCCT or benchmark like Cinebench R20/R23 to push your CPU to ~100% Utilization. See how high your CPU boosts and what temperatures you get before any changes to it.

Now open PBO2 Tuner.exe from the folder we just downloaded and select the curve tab

Start by setting a low negative offset like -10 and decrease it in decrements of -5 to a minimum of -30.

Make sure to stress test it each step to see if it is stable.

Now you're done! But remember, you will have to do this again every time you restart or wake from sleep.

# 6) How to set PBO2 Tuner automatically with every system boot/restart/wakeup
Do all the steps in [Chapter 5](#5-how-to-set-pbo2-tuner-manually) until you're satisfied with the offsets you can get.

Now open Windows Task Scheduler. Go to Actions and Create Task.

**Use the provided Screenshots to configure the task correctly.**

Set general settings.

![Chapter 6 p1_LI](https://user-images.githubusercontent.com/106172193/170519308-df13b85a-00c8-4e57-ad12-6d9b118ed1ac.jpg)

Create the Trigger for login.
You can set it to All users but it's safer to set it to trigger on a specific user's login, as in the event of an unstable curve on logging in, you could log in as another user and remove or edit the task.

![Chapter 6 p2](https://github.com/Krzeszny/How-to-undervolt-AMD-RYZEN-5800X3D-Guide-with-PBO2-Tuner/assets/10529134/72f25374-a605-4d6a-bc33-3ea56756dd27)

Create the Trigger for wakeup.

![Chapter 6 p3](https://user-images.githubusercontent.com/106172193/170519673-90d6766b-6397-4638-a2b8-08c8028839e9.png)

Create the Action for PBO2 tuner.exe to start with the offsets as startup arguments.

![Chapter 6 p4_LI](https://user-images.githubusercontent.com/106172193/170519780-4835e046-82fc-49fe-8a9f-e1ded1144638.jpg)

Set the Conditions and Settings tabs.

![Chapter 6 p5](https://user-images.githubusercontent.com/106172193/170519955-8fd6d69b-36ec-4e1c-b7b8-906fda4a28ac.png)

![Chapter 6 p6](https://user-images.githubusercontent.com/106172193/170520002-83e3a008-9177-4a35-98be-f5005b559db9.png)


**Make sure to set the path to your PBO2 tuner.exe since it most likely won't be the same as mine.**

**Customize the arguments to your previously tested Offsets.**

Click OK to save it. Congratulations, you are done!

# 7) (Optional) How to create a custom alert for every time your PBO2 Tuner settings have been applied automatically
If you are as paranoid as me and have the urge to check PBO2 Tuner after every reboot or wakeup to see if your offset has been set, then I have something to ease your mind. A custom alert tied to the Task Scheduler we just created above to inform you that the task has been executed.

![Chapter 7 p1](https://user-images.githubusercontent.com/106172193/170520421-a21eec68-2311-4eb7-ad44-d695410b9a1c.png)

Find a logo and save into the folder with PBO2 tuner.exe in it.

Open a text file and paste this into it:

```pwsh
Import-Module BurntToast;
$pngPath="C:\Users\REPLACE_THIS_WITH_THE_PATH_TO_THE_PNG_FILE\whatever.png";
New-BurntToastNotification -Text "PBO Offset has been applied!", 'PBO Offset has been applied!' -AppLogo $pngPath
```

Save as `Alert.ps1`, select All Files and save in the same folder as PBO2 tuner.exe.

Open Windows PowerShell as administrator and execute this command:

```pwsh
Install-Module -Name BurntToast
```

You will, likely, get a message that says you need to install the NuGet provider. If you do, simply type in Y to proceed and PowerShell will take care of the rest. Once it’s installed, run the above command again. This time, you will likely get a message saying you’re installing a module from an untrusted repository. Again, type Y to proceed. The module will now be installed.

Now paste

```pwsh
Set-ExecutionPolicy -ExecutionPolicy Unrestricted
```

into PowerShell and run the command. Press Y to proceed. This is necessary for `Alert.ps1` to work in the future.

**DO THIS AT YOUR OWN RISK**, since you're disabling a security feature of windows.

Now open Task Scheduler again and look for the task we've created in [Chapter 6](#6-how-to-set-pbo2-tuner-automatically-with-every-system-bootrestartwakeup).

Right click it, select Properties, go to Actions and click New.

**Use the provided screenshot to set up correctly.**

![Chapter 7 p2_LI](https://user-images.githubusercontent.com/106172193/170520568-ed331286-d0be-4b65-9539-7949ad0334ce.jpg)

**Edit Add arguments to the path of your Alert.ps1 file**

Click okay and save all changes.

We're done!
    
You can test it by right-clicking the task in Task Scheduler and selecting run.

# 8) What else can we do to improve CPU clockspeeds and thermals?
Pretty much nothing without the support from AMD and or motherboard manufacturers.

Get a better CPU cooler.

(For enthusiasts with an AIO or custom water cooling block) Get an AM4 cooler offset bracket (like from DerBauer); reportedly around 5-10 degrees improvement on core temps.

Set higher Baseclock (really not recommended if you don't exactly know what it does and what problems can be caused by it)
# 9) Resources used
- https://www.overclock.net/threads/corecycler-tool-for-testing-curve-optimizer-settings.1777398/

- https://www.overclock.net/threads/5800x3d-owners.1798046/
