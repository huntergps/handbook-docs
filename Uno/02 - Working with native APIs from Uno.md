# Working with native APIs from Uno

This chapter is about using the Android and iOS native APIs from Uno and how one can native functionality in a common interface and expose it as a JavaScript module.

## Using iOS APIs
```
using Uno;
using Uno.Collections;
using Fuse;
using Fuse.Scripting;
using Fuse.Reactive;
using iOS.AudioToolbox;
using iOS.Foundation;

public class SoundPlayer : NativeModule
{
	public SoundPlayer()
	{
		AddMember(new NativeFunction("Play", (NativeCallback)Play));
	}

	static object Play(Context c, object[] args)
	{
		//Sounds http://iphonedevwiki.net/index.php/AudioServices
		global::iOS.AudioToolbox.Functions.AudioServicesPlaySystemSound(1310);
		return null;
	}
}
```

## Using Android APIs
Make sure you `.unoproj` file contains the `Android` package:

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
Here is an example of how to access the system sounds using the RingtoneManager:
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

Make sure your `.unoproj`file contains the `Experimental.iOS` and `ObjC` packages:

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

## Creating your own abstraction

```
namespace MyAPI
{
	public class SystemSounds
	{
		readonly ISystemSoundsProvider _provider;

		public SystemSounds()
		{
			if defined(Android) _provider = new AndroidImpl.AndroidSystemSoundsProvider();
			else if defined(iOS) _provider = new iOSImpl.iOSSystemSoundsProvider();
		}

		public void PlayNotification()
		{
			_provider.PlayNotification();
		}

	}

	interface ISystemSoundsProvider
	{
		void PlayNotification();
	}
}

namespace MyAPI.AndroidImpl
{

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
}

namespace MyAPI.iOSImpl
{

	using global::iOS.AudioToolbox;

	extern(iOS) class iOSSystemSoundsProvider : ISystemSoundsProvider
	{
		public void PlayNotification()
		{
			Functions.AudioServicesPlaySystemSound(1310);
		}
	}
}
```

One way to do it:
```
namespace MyNamespace
{
	public class MyCrossplatformClass
	{
		public void DoSomething()
		{
			if defined(iOS)
			{
				// iOS code here
			}
			else if defined(Android)
			{
				// Android code here
			}
			else if defined(!Mobile)
			{
				// Uno code here
			}
			else if defined(CPlusPlus)
			{
				// Cplusplus code
			}
			else if defined(CIL)
			{
				// CIL/DotNet code
			}
		}
	}

}
```


Another way to do it:
```
namespace MyNamespace
{
	public class CrossplatformClass
	{
		readonly CrossplatformFeature _impl;

		public CrossplatformFeature()
		{
			if defined(Android) _impl = new AndroidImpl();
			else if defined(iOS) _impl = new iOSImpl();
			else if defined(CIL) _impl = new CILImpl();
			else throw new Exception("Platform not supported");
		}

		public void DoSomething()
		{
			_impl.DoSomething();
		}

	}

	abstract class CrossplatformFeature
	{
		public abstract void DoSomething();
	}

	extern(Android) class AndroidImpl : CrossplatformFeature
	{
		public override void DoSomething() { }
	}

	extern(iOS) class iOSImpl : CrossplatformFeature
	{
		public override void DoSomething() { }
	}

	extern(CIL) class CILImpl : CrossplatformFeature
	{
		public override void DoSomething() { }
	}

}
```
```
namespace MyNamespace
{
	public class MyClass
	{
		extern(Android) public void DoSomething() { }

		extern(iOS) public void DoSomething() { }

		extern(!Mobile) public void DoSomething() { }
	}
}
```
