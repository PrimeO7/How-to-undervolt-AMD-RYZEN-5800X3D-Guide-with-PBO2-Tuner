How to "undervolt" AMD RYZEN 5800X3D Guide with PBO2 Tuner.

Get the Most out of your 5800X3D using PBO Curve Optimizer!


# Table of Contents
1. [What are we doing?](#1-what-are-we-doing)
2. [Why are we doing it? What are the Benefits?](#2-why-are-we-doing-it-what-are-the-benefits)
3. [Options on what we can do](#3-options-on-what-we-can-do)
4. [What do we need?](#4-what-do-we-need)
5. [How to set PBO2 Tuner manually](#5-how-to-set-pbo2-tuner-manually)
6. [How to set PBO2 Tuner automatically with every System boot/restart/wakeup](#6-how-to-set-pbo2-tuner-automatically-with-every-system-bootrestartwakeup)
7. [(Optional) How to create a custom Alert for every time your PBO2 Tuner settings have been applied automatically](#7-optional-how-to-create-a-custom-alert-for-every-time-your-pbo2-tuner-settings-have-been-applied-automatically)
8. [What else can we do to improve CPU Clockspeeds and Thermals?](#8-what-else-can-we-do-to-improve-cpu-clockspeeds-and-thermals)
9. [Share your Results!](#9-share-your-results)
10. [Resources used](#10-resources-used)


# 1) What are we doing? 
We are using PBO2 Tuner to set an OC Curve (AMD Curve Optimizer).

# 2) Why are we doing it? What are the Benefits?
Because the 5800X3D is very much locked down on most Motherboards thanks to AMD and the Motherboard Manufacturers, most People won't be able to set CPU Ratio and/or CPU Voltage in their Bios to get the most out of their 5800X3D.
With this Guide you will at least be able to tweak Things a little inside of Windows for potentially better Thermals and Clockspeeds that stay longer and more often on the highest they can be.

# 3) Options on what we can do
Set PBO OC Curve manually after every change in Windows powerstate.
     
Set PBO OC Curve automatically with every system boot/restart/wakeup (means after every change in Windows powerstate).
     
**Optional** possibility to create custom BurnedToast Alert whenever the CO Curve has been set automatically. This way you will always have the assurance that it actually applied.
    
# 4) What do we need?
[Debug-cli.7zp](https://www.overclock.net/threads/corecycler-tool-for-testing-curve-optimizer-settings.1777398/page-39#post-28979881) from PJVol from Overclocker.net thread

Monitoring software to confirm CO Curve has been set([HwInfo](https://www.hwinfo.com/download/) or [RyzenMaster](https://www.amd.com/en/technologies/ryzen-master))

BurntToast Module in PowerShell

Allow Execution Policies change in PowerShell

PowerShell script for BurntToast Alert

Any PNG of your Choice for the Alert

# 5) How to set PBO2 Tuner manually
First download [Debug-cli.7zp](https://www.overclock.net/threads/corecycler-tool-for-testing-curve-optimizer-settings.1777398/page-39#post-28979881).

Get a Monitoring Software like [HwInfo](https://www.hwinfo.com/download/) or [RyzenMaster](https://www.amd.com/en/technologies/ryzen-master). You will need this to see your CPU Voltage and Temperature and to later confirm that your Settings work.

Run a CPU Stress Test like OCCT or Benchmark like Cinebench R20/R23 to push your CPU to ~100% Utilization. See how high your CPU boosts and what Temperatures you get before any changes to it.

Now open PBO2 Tuner.exe from the Folder we just downloaded and select the Curve tab.

Start by setting a low negative offset like -10 and decrease it in Decrements of -5 to a Minimum of -30.

Make sure to stress test it each step to see if it is stable.

Now you're done! But remember you will have to do this again every time you restart or wake from sleep.

# 6) How to set PBO2 Tuner automatically with every System boot/restart/wakeup
Do all the Steps in [Chapter 5](#5-how-to-set-pbo2-tuner-manually) until you're satisfied with the offsets you can get.

Now Open Windows Task Scheduler. Go to Actions and Create Task.

**Use the provided Screenshots to configure the Task correctly.**

![Chapter 6 p1_LI](https://user-images.githubusercontent.com/106172193/170519308-df13b85a-00c8-4e57-ad12-6d9b118ed1ac.jpg)

![Chapter 6 p2](https://user-images.githubusercontent.com/106172193/170519471-b033ca84-48ea-4822-88a4-a9daecc33b7f.png)

![Chapter 6 p3](https://user-images.githubusercontent.com/106172193/170519673-90d6766b-6397-4638-a2b8-08c8028839e9.png)

![Chapter 6 p4_LI](https://user-images.githubusercontent.com/106172193/170519780-4835e046-82fc-49fe-8a9f-e1ded1144638.jpg)

![Chapter 6 p5](https://user-images.githubusercontent.com/106172193/170519955-8fd6d69b-36ec-4e1c-b7b8-906fda4a28ac.png)

![Chapter 6 p6](https://user-images.githubusercontent.com/106172193/170520002-83e3a008-9177-4a35-98be-f5005b559db9.png)


Make sure to set the path to your PBO2 tuner.exe since it most likely won't be the same as mine.

Customize the Arguments to your previously tested Offsets.

Click OK to save it. Congratulations you are done!

# 7) (Optional) How to create a custom Alert for every time your PBO2 Tuner settings have been applied automatically
If you are as paranoid as me and have the urge to check PBO2 Tuner after every reboot or wakeup to see if your offset has been set, then I have something to ease your Mind. A Custom Alert tied to the Task Scheduler we just created above to inform you that the Task has been executed.

![Chapter 7 p1](https://user-images.githubusercontent.com/106172193/170520421-a21eec68-2311-4eb7-ad44-d695410b9a1c.png)

Find a Logo and save into the folder with PBO2 tuner.exe in it.

Open a Text file and paste this into it **Import-Module BurntToast; New-BurntToastNotification -Text "PBO Offset has been applied!", 'PBO Offset has been applied!' -AppLogo (The Path to the PNG you will be using Example: C:\Users\Username\Desktop\Debug\name.png)**

Don't forget to remove the **()** after -AppLogo.

Save as Alert.ps1, select All Files types and save in the same folder as PBO2 tuner.exe.

Open Windows PowerShell as Administrator and execute this command **Install-Module -Name BurntToast**

You will, likely get a message that says you need to install the NuGet provider. If you do, simply type in Y to proceed and PowerShell will take care of the rest. Once it’s installed, run the above command again. This time, you will likely get a message saying you’re installing a module from an untrusted repository. Again, type Y to proceed. The module will now be installed.

Now paste **Set-ExecutionPolicy -ExecutionPolicy Unrestricted** into PowerShell and run the command. Press Y to proceed. This is necessary for Alert.ps1 to work in the future.

**DO THIS AT YOUR OWN RISK** since you're disabling a security feature of windows.

Now Open Task Scheduler again and look for the Task we've created in [Chapter 6](#6-how-to-set-pbo2-tuner-automatically-with-every-system-bootrestartwakeup).

Right click it, select Properties, go to Actions and click New.

**Use the provided Screenshot to set up correctly.**

![Chapter 7 p2_LI](https://user-images.githubusercontent.com/106172193/170520568-ed331286-d0be-4b65-9539-7949ad0334ce.jpg)

Edit Add arguments to the path of your Alert.ps1 file.

Click okay and save all changes.

We're done!
    
You can test it by right-clicking the Task in Task Scheduler and selecting run.

# 8) What else can we do to improve CPU Clockspeeds and Thermals?
Pretty much nothing without the support from AMD and or Motherboard Manufacturers.

Get a better CPU cooler.

(For Enthusiasts with an AIO or Custom Water cooling Block) Get an AM4 Cooler offset bracket(like from DerBauer); reportedly around 5-10 degrees improvement on Core temps.

Set higher Baseclock (really not recommended if you don't exactly know what it does and what Problems can be caused by it).

# 9) Share your Results!
Let everyone else see what Improvements you got out of your 5800X3D by sharing it with us through this.
https://forms.gle/dcs6czetRqnHb1G78

# 10) Resources used
- https://www.overclock.net/threads/corecycler-tool-for-testing-curve-optimizer-settings.1777398/

- https://www.overclock.net/threads/5800x3d-owners.1798046/
