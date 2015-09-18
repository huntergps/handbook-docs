# $(Getting Started)

Welcome to Fuse!

## $(Setup)

Getting up and running with Fuse is super easy. We'll just need to install a few things, then we're good to go!

We're always working hard on making this process smooth as silk, but occasionally things can still go wrong. If something happens, please let us know!

Setup instructions for OS X: https://www.fusetools.com/developers/guides/setup/install-osx

Setup instructions for Windows: https://www.fusetools.com/developers/guides/setup/install-win

## Tutorial

Congratulations on your fresh Fuse install! 

If you want to get started using Fuse by following an example from creation of a new project all the way until you run it on your handheld device, you can find such a tutorial here: https://www.fusetools.com/developers/guides/tutorial

## Preview

Fuse has excellent support for getting a live preview of the edits you make, both in a desktop based simulator and on the devices you decide to target. These previews can run simultaneously, so you no longer need to build your project for specific devices between edits; just save and they will appear instantly on the device(s). 

> ### iOS

For instructions on how to enable iOS preview, go here: https://www.fusetools.com/developers/guides/previewandexport/devicepreview

> ### Android

For instructions on how to enable preview on an Android device, go here: https://www.fusetools.com/developers/guides/previewandexport/devicepreview

> ### Desktop

Starting a preview of your project on your desktop can be done in a couple of ways. If you have Sublime Text 3 installed with the supported plugin, you can right click on the UX-file and select "Begin Fuse preview" and select the "Local" option. Preview will the open the simulator, and it will sync automatically as you edit your files. 

Alternatively, you can navigate on the command line to your project root directory and type `fuse preview`.

You can read more about starting Fuse preview on the desktop here: https://www.fusetools.com/developers/guides/previewandexport/toolpreview 

## Export

When running preview, Fuse will create a shell application on your device that connects to the Fuse daemon running on your system to quickly display changes you make to the project. However, if you want to bring the device with you to a meeting or to show to people away from your development environment, you need to export the project to the device you are targetting.

> ### iOS

On iOS, you need to have a machine running OS X and Xcode installed (as for preview). You also need an Apple developer account. In the project root, simply type:

`fuse build --target=iOS --run`

This will open the built project in Xcode. If you select your device from the pulldown list of available output targets and press the "run" button, the application will deploy to your device and run.

> ### Android

To export to Android, you go to the project root and type:

`fuse build --target=Android --run`

This will deploy and start the project on your connected Android device.