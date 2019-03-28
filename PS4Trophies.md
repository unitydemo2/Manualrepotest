#PS4 Trophies

Unity supports __PS4 Trophies__ via the __NPToolkit2__ plug-in, which is in the [PS4 plug-ins ownCloud folder](https://oc.unity3d.com/index.php/s/6diVG9Aaf0c0m7j). Use __Trophies__ to reward your game's player for reaching certain milestones or completing challenges in your game. Refer to the [Trophy System Overview]([https://ps4.scedev.net/resources/documents/SDK/latest/Trophy_System-Overview/__document_toc.html) and [Trophy Pack Users' Guide](https://ps4.scedev.net/resources/documents/Misc/current/Trophy_Pack_File_Utility-Users_Guide/__document_toc.html) Sony Developer pages for more information.

# How to set up Trophies in Unity

To set up __Trophies__ in Unity, follow the steps below:

* Import the following folders, along with all sub-folders and files from the __NPToolkit2__ plug-in into your project: 
    * Editor
    * Plug-ins
    * SonyAssemblies

![Importing PS4 Plug-ins via Unity Packages](../uploads/Main/ps4Trophies_image_1.png)

* Open __PlayerSettings__ (menu: __Edit__ > __Project__ __Settings__ > __Player__) and go to __Publishing Settings__ > __PlayStation Network__. Set up the __Trophy Pack__ (*trophy.trp* file), __NP Title ID__ (*nptitle.dat* file) and the __NP Title Secret__ using the values and *trophy.trp* files for your project. You must obtain the NP title ID and NP secret values from Sony before adding them to your project. Refer to the [PSN Service Setup Guide]([https://ps4.scedev.net/resources/documents/SDK/latest/PSN_Service_Setup-Guide/0003.html](https://ps4.scedev.net/resources/documents/SDK/latest/PSN_Service_Setup-Guide/0003.html)) Sony Developer documentation for more information. You can also find a guide to setting up your own Trophy Pack files on the [Sony Developer Documentation website](https://ps4.scedev.net/resources/documents/Misc/current/Trophy_Pack_File_Utility-Users_Guide/0001.html#__document_toc_00000000).
* If you are not using online features in your game (such as chat, leaderboards or online play), set the __NP Age Rating__ to 0. This stops the Playstation API from initializing unused features, but still allows trophy functionality.

![](../uploads/Main/ps4Trophies_image_2.png)

The Playstation Network settings section of the PlayerSettings Inspector window.

Refer to the [Sony Developer Trophy documentation](https://ps4.scedev.net/resources/documents/SDK/latest/Trophy_System-Overview/__document_toc.html) for more information on Trophies for PS4.

### NPToolkit2 Example files

The NPToolkit2 plug-in contains several example files to be used as a reference when setting up Trophies for your game in Unity.

The NPToolkit2 plug-in includes example versions of the *nptitle.dat* and *trophy.trp* files (in the *Assets/Editor* folder). The __SetPlatformOptionsPS4.cs__ class contains example __NP Title Secret__ and __Content ID__ values.

__Note__: The example __Np Title Secret __and __Content ID __values are not suitable for publishing as part of a completed project. Replace the values and files with your own as soon as you can before publishing your project. 

## Trophy support example script

The following is an example script that initializes the NPToolkit2 plug-in and adds trophy functionality to your game.

```
using UnityEngine;

using System.Collections;

using UnityEngine.PS4;

using System;

public class TrophyManager : MonoBehaviour 

{

//The active current user logged

	public PS4Input.LoggedInUser loggedInUser;

	// NPToolkit2 and Trophy initialization

	void Start () 

	{

		//For this example we use Aync Handling of the NPToolkit2 operations

		Sony.NP.Main.OnAsyncEvent += Main_OnAsyncEvent;

		Sony.NP.InitToolkit init = new Sony.NP.InitToolkit();

		init.contentRestrictions.DefaultAgeRestriction = 0;

		//You can add several age restrictions by region

		Sony.NP.AgeRestriction[] ageRestrictions = new Sony.NP.AgeRestriction[1];

		ageRestrictions[0] = new Sony.NP.AgeRestriction(10, new Sony.NP.Core.CountryCode("us"));

		init.contentRestrictions.AgeRestrictions = ageRestrictions;

		// you can set affinity to other cores this way: Sony.NP.Affinity.Core2 | Sony.NP.Affinity.Core4;

		init.threadSettings.affinity = Sony.NP.Affinity.AllCores; 

		//For this example we use the first user

		loggedInUser = PS4Input.PadRefreshUsersDetails(0); 

		try

		{

			//Initialize the NPToolkit2

			Sony.NP.Main.Initialize(init);

			//Register the Trophy Pack

			RegisterTrophyPack();

		}

		catch (Sony.NP.NpToolkitException e)

		{

			Debug.Log("Error initializing the NPToolkit2 : " + e.ExtendedMessage);

		}	

	}

	public void RegisterTrophyPack()

	{

		try

		{

			Sony.NP.Trophies.RegisterTrophyPackRequest request = new Sony.NP.Trophies.RegisterTrophyPackRequest();

			request.UserId = loggedInUser.userId;

			Sony.NP.Core.EmptyResponse response = new Sony.NP.Core.EmptyResponse();

			// Make the async call which returns the Request Id 

			int requestId = Sony.NP.Trophies.RegisterTrophyPack(request, response);

			Debug.Log("RegisterTrophyPack Async : Request Id = " + requestId);

		}

		catch (Sony.NP.NpToolkitException e)

		{

			Debug.Log("Exception : " + e.ExtendedMessage);

		}

	}

	public void UnlockTrophy(int nextTrophyId)

	{

		try

		{

			Sony.NP.Trophies.UnlockTrophyRequest request = new Sony.NP.Trophies.UnlockTrophyRequest();

			request.TrophyId = nextTrophyId;

			request.UserId = loggedInUser.userId;

			Sony.NP.Core.EmptyResponse response = new Sony.NP.Core.EmptyResponse();

			// Make the async call which returns the Request Id 

			int requestId = Sony.NP.Trophies.UnlockTrophy(request, response);

			Debug.Log("GetUnlockedTrophies Async : Request Id = " + requestId);

		}

		catch (Sony.NP.NpToolkitException e)

		{

			Debug.Log("Exception : " + e.ExtendedMessage);

		}

	}

	// NOTE : This is called on the "Sony NP" thread and is independent of the Behaviour update.

	// This thread is created by the SonyNP.dll when NpToolkit2 is initialised.

	private void Main_OnAsyncEvent(Sony.NP.NpCallbackEvent callbackEvent)

	{

		//Print some useful info on the current event: 

		Debug.Log("Event: Service = (" + callbackEvent.Service + ") : API Called = (" + callbackEvent.ApiCalled + ") : Request Id = (" + callbackEvent.NpRequestId + ") : Calling User Id = (" + callbackEvent.UserId + ")");

		HandleAsyncEvent(callbackEvent);

	}

	//Here we manage the response of the previous request

	private void HandleAsyncEvent(Sony.NP.NpCallbackEvent callbackEvent)

	{

		try

		{

			if (callbackEvent.Response != null)

			{

				//We got an error response 

				if (callbackEvent.Response.ReturnCodeValue < 0)

				{

					Debug.LogError("Response : " + callbackEvent.Response.ConvertReturnCodeToString(callbackEvent.ApiCalled));

				}

				else

				{

					//The callback of the event is a trophyUnlock event

					if(callbackEvent.ApiCalled == Sony.NP.FunctionTypes.trophyUnlock)

					{

						Debug.Log("Trophy Unlock : " + callbackEvent.Response.ConvertReturnCodeToString(callbackEvent.ApiCalled));

					}

				}

			}

		}

		catch ( Sony.NP.NpToolkitException e)

		{

			Debug.Log("Main_OnAsyncEvent Exception = " + e.ExtendedMessage);

		}

	}

}

```

# Trophy support for PS4 SDK 4.0

Use the original NPToolkit plug-in to add trophies to your game when using PS4 SDK 4.0 or older.

__Note__: From the PS4 SDK 4.5 onwards, it is not possible to submit games using an older SDK.

The following is an example script that initializes the NPToolkit plug-in and adds trophy support to your game. This example assumes that you are not using any online functionality.

```

using UnityEngine;

using System.Collections;

using UnityEngine.PS4;

using System;

public class TrophyManager : MonoBehaviour

{

   bool m_NpReady = false; // Is the NP plugin initialised and ready for use.

   void Start ()

   {

      //Register basic Trophy events:

      Sony.NP.Trophies.OnAwardedTrophy += OnAwardedTrophyEvent;

      Sony.NP.Trophies.OnAwardTrophyFailed += OnAwardTrophyFailedEvent;

      Sony.NP.Trophies.OnAlreadyAwardedTrophy += OnAlreadyAwardedTrophyEvent;

      // Register a callback for completion of NP initialization.

      Sony.NP.Main.OnNPInitialized += OnNPInitialized;

      // Initialize the NPToolkit

      Sony.NP.Main.Initialize(0);

      // Register the trophy pack.

      Sony.NP.Trophies.RegisterTrophyPack();

   }

   //Update the Main NPToolkit

   void Update ()

   {

      Sony.NP.Main.Update();

   }

   //Award Trophy using the index of the trophy

   void UnlockTrophy(int trophyIndex)

   {

      if(Sony.NP.Trophies.TrophiesAreAvailable && m_NpReady)

      {

         Sony.NP.Trophies.AwardTrophy(trophyIndex);

      }

   }

   //Event fired when a trophy is sucessfully awarded

   void OnAwardedTrophyEvent(Sony.NP.Messages.PluginMessage msg)

   {

        Debug.Log("OnAwardedTrophy Event: " + msg.type);

   }

   //Event fired when it wasn't possible to award a trophy

   void OnAwardTrophyFailedEvent(Sony.NP.Messages.PluginMessage msg)

   {

        Debug.Log("OnAwardTrophyFailed Event: " + msg.type);

   }

   //Event fired when a trophy is already awarded

   void OnAlreadyAwardedTrophyEvent(Sony.NP.Messages.PluginMessage msg)

   {

        Debug.Log("OnAlreadyAwardedTrophy Event: " + msg.type);

   }

   //NP plugin initialised and ready for use.

   void OnNPInitialized(Sony.NP.Messages.PluginMessage msg)

   {

        m_NpReady = true;

   }

}
```
