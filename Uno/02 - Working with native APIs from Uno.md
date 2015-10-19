# Working with native APIs from Uno

This chapter is about using the Android and iOS native APIs from Uno.

## Using Android APIs

Make sure the `.unoproj` file contains the `Android` package:

```
{
"RootNamespace":"",
	"Packages": [
		... other packages ...
		,"Android"
	],
	"Includes": [ "*" ]
}
```

You can then start using the Android APIs in your Uno code.
Here is an example of how to access the system sounds using the `RingtoneManager`:
```
using global::Android.android.media;
using global::Android.android.app;

extern(Android) class AndroidSystemSoundsProvider : ISystemSoundsProvider
{

	public void PlayNotification()
	{
		var uri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION);
		var ringtone = RingtoneManager.getRingtone(Activity.GetAppActivity(), uri);
		ringtone.play();
	}
}
```

## Using iOS APIs

Make sure the `.unoproj`file contains the `Experimental.iOS` and `ObjC` packages:

```
{
"RootNamespace":"",
	"Packages": [
		... other packages ...
		"Experimental.iOS",
		"ObjC"
	],
	"Includes": [ "*" ]
}
```

Now we can implement the previous Android sound example for iOS as well:
```
using global::iOS.AudioToolbox;

extern(iOS) class iOSSystemSoundsProvider : ISystemSoundsProvider
{
	public void PlayNotification()
	{
		Functions.AudioServicesPlaySystemSound(1310);
	}
}
```

## Creating your own abstractions

Fuse provides a couple neat ways to wrap target specific code so as to make it available as a unified API on all platforms.

* Take a look at the [uxl-handbook](https://www.fusetools.com/developers/guides/uxl-handbook) for detailed documentation of target specific compilation.
* For a tutorial on creating your own JavaScript module with Uno, take a look [here](https://www.fusetools.com/community/guides/fusejs/nativemodules).

In the following example, we use the `extern` and `if defined` syntax, to make a `NativeModule` which chooses the correct implementation based on which platform we build for:

```
using Uno;
using Fuse.Scripting;
using Fuse.Reactive;

using Android.android.media;
using Android.android.app;
using iOS.AudioToolbox;

public class SystemSounds : NativeModule
{
	public SystemSounds()
	{
		AddMember(new NativeFunction("playNotification", (NativeCallback)PlayNotification));
	}

	object PlayNotification(Context c, object[] args)
	{
		if defined(Android)
		{
			var uri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION);
			var ringtone = RingtoneManager.getRingtone(Activity.GetAppActivity(), uri);
			ringtone.play();
		}
		else if defined (iOS)
		{
			Functions.AudioServicesPlaySystemSound(1310);
		}
		return null;
	}
}
```

We can now call our native function from JavaScript like so:

```
<App Theme="Basic">
	<Panel>
		<SystemSounds ux:Global="SystemSounds"/>
		<JavaScript>
			var SystemSounds = require('SystemSounds');

			function playSound(){
				SystemSounds.playNotification();
			}

			module.exports = { playSound: playSound };
		</JavaScript>
		<Button Clicked="{playSound}" Text="Click me!" Alignment="Center" />
	</Panel>
</App>
```
