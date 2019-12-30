# videox_unityPlugin

------



# 1.Access VideoXSDK Unity

### 1.1 Import the VideoXSDK.unitypackage plugin

- For instructions on how to use the access, please refer to the demo file Main.cs in the package we provided.

### 1.2 Android Platform configuration

- Please use the ad type script used below to the main camera of the unity main Scene (as shown in Figure  below).

  - `BannerAd`

  - `InterstitialAd` 

  - `RewardAd`

    

   ### 1.3 iOS Platform configuration

##### Xcode Project Configuration

* Xcode version is less than 11, add VideoXSDK.framework in target-> General-> Embedded Binaries of Xcode project (as shown in Figure 1 below).

![365cfe4af51c4f27164b8db91cf376e5](../document/images/365cfe4af51c4f27164b8db91cf376e5.jpg)

* Xcode 11, add VideoXSDK.framework to *target-> General-> Frameworks, Libraries, and Embedded Content*, and select Embed as Embed & Sign

* Add a key-value pair with a value of Boolean type YES and a key of GADIsAdManagerApp to the Info.plist of the project.

  ```
  <key>GADIsAdManagerApp</key>
  <true/>
  ```

##### Unity Project Configuration

Please attach the ad type script used below to the Main Camera of the Unity main Scene (as shown in Figure  below).

![c222a0f50303dce3e85df4a7f21412d7](../document/images/c222a0f50303dce3e85df4a7f21412d7.jpg)

------

# 2. SDK Initialization

### 2.1 Initialize SDK

initialization methods:

```csharp
VideoXAds.InitializeSDK("Main Camera","appId", "pubKey")
```

- gameObjectString: unity Main Camera name under main Scene
- appId: app identification id,  please find in app page
- pubKey: encrypted key, please find in account page

### 2.2 Set SDK method callback obtain

​       //Rewarded ads

​        `VXRewardsAds.onLoadSuccessHandler += rewardAdOnLoadSuccess;`
​        `VXRewardsAds.onLoadErrorHandler   += rewardAdOnLoadError;`
​        `VXRewardsAds.onShowHandler        += rewardAdOnShowStart;`
​        `VXRewardsAds.onCloseHandler       += rewardAdOnShowFinish;`
​        `VXRewardsAds.onShowErrorHandler   += rewardAdOnShowError;`

​        // Interstitial ads

​        `VXInterstitialAds.onLoadSuccessHandler += interAdOnLoadSuccess;`
​        `VXInterstitialAds.onLoadErrorHandler += interAdOnLoadError;`
​        `VXInterstitialAds.onCloseHandler += interAdOnClose;`
​        `VXInterstitialAds.onClickedHandler += interAdOnClicked;`
​      

​        //Banner ads

​        `VXBannerAds.onLoadSuccessHandler += bannerAdOnLoadSuccess;`
​        `VXBannerAds.onLoadErrorHandler += bannerAdOnLoadError;`
​        `VXBannerAds.onClickedHandler += bannerAdOnClicked;`
​        `VXBannerAds.onCloseHandler += bannerAdOnClose;`

As shown in the code above

- VXRewardsAds.onLoadSuccessHandler      (string adUnitId): rewarded video ad loading success callback
- VXRewardsAds.onLoadErrorHandler      (string adUnitId, string error): rewarded video ad loading failure      callback
- VXRewardsAds.onShowHandler      (string adUnitId): rewarded video display starts callback
- VXRewardsAds.onCloseHandler      (string adUnitId, bool isReward): rewarded video display finish callback
- VXRewardsAds.onShowErrorHandler      (string adUnitId, string error): rewarded video display error callback
- VXInterstitialAds.onLoadSuccessHandler      (string adUnitId): interstitial ad loading success callback
- VXInterstitialAds.onLoadErrorHandler      (string adUnitId, string error): interstitial ad loading or display      failure callback
- VXInterstitialAds.onCloseHandler      (string adUnitId): user close and leave interstitial ad callback
- VXInterstitialAds.onClickedHandler      (string adUnitId): user click interstitial ad callback
- VXBannerAds.onLoadSuccessHandler      (string adUnitId): Banner ad loading success callback
- VXBannerAds.onLoadErrorHandler      (string adUnitId, string error): Banner ad loading or display failure callback
- VXBannerAds.onCloseHandler      (string adUnitId): user close and leave banner ad callback
- VXBannerAds.onClickedHandler      (string adUnitId): user click interstitial ad callback

**Note: **

​    If your project integrates the `AppsFlyer` SDK, you need to:

* Find the `AppsFlyerTrackerCallbacks.cs` file included in AppsFlyer and implement the following code in the callback method `didReceiveConversionData` in the file:

   `pu`blic void didReceiveConversionData(string conversionData){VXAds.SetAppsFlyerConversionData(conversionData);

  `
      }`

# 3. Ad Functions

#### 3.1 Rewarded video ads

##### 3.1.1 Request to load an ad

   `VXRewardsAds.LoadRewardAd("Main Camera", adUnitId);`

##### 3.1.2 Determine if the ad is loaded successfully

   `bool value = VXRewardsAds.IsAdReady("Main Camera", adUnitId);`

##### 3.1.3 Display ad

   `VXRewardsAds.PlayRewardAd("Main Camera",adUnitId);`

* `gameObjectString`: Unity Main Camera name under main Scene
* `adUnitId`: is the ad placement id, please find in app page

##### Note:

- For better user experience, it is strongly recommended to call `VXRewardsAds.LoadRewardAd("Main Camera", adUnitId)` first, to prepare for video caching before each impression of the video ads, to avoid loading wait time during display.
- Forbid to call  VXRewardsAds.PlayRewardAd method in rewardAdOnLoadSuccess delegate
- Forbid to call  VXRewardsAds.LoadRewardAd method in rewardAdOnLoadError delegate 

#### 3.2 Interstitial ad

**3.2.1 Request to load an ad**

   `VXInterstitialAds.LoadAd("Main Camera", adUnitId);`

**3.2.2 Determine if the ad is loaded successfully**

​     `bool value = VXInterstitialAds.IsAdReady("Main Camera", adUnitId);`

**3.2.3 Display ad**

   `VXInterstitialAds.ShowAd("Main Camera", adUnitId);`

- gameObjectString: the Main Camera name under unity main Scene
- adUnitId: is the ad placement id, please find in app page

#### 3.3 Banner ad

**3.3.1 Load and display ads**

   `VXBannerAds.ShowAd("Main Camera", adUnitId, BannerAd.POSTION_BOTTOM);`

**3.3.2 Close an ad**

   `VXBannerAds.DestroyAd("Main Camera", adUnitId);`

- gameObjectString: the Main Camera name under unity main Scene
- adUnitId: is the ad placement id, please find in app page
- position: is the position of the ad, there are two options--BannerAd.POSTION*BOTTOM      and BannerAd.POSTION*TOP, at the bottom and the top of the screen respectively



**Android Note**

- Android packaging needs to pay attention to the "Use Proguard File" and "Custom Gradle Template" options in the Publishing Settings under the Android tab of PlayerSettings, and select Proguard for Release in Manify.

* If there is a crash(not found class) on phones with Android 5.0 or lower edition, please  follow the instructions below:
  - Export the Android project  when packaging Android apk, create a new class [MainApp] in the directory       [UnityPlayerActivity] 
  - Open MainApp to make this class inherit from [MultiDexApplication], for example: public       class MainApp extends MultiDexApplication { }
  - Open [AndroidManifest.xml] and add a name attribute to the Application node, for example<application android:name=".MainApp" android:theme="@style/UnityThemeSelector" ....` ，the value of this attribute is the above newly created directory 

 



