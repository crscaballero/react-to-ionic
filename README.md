# React web-app to Ionic mobile-app

Short tutorial/project that leads you to transform your React@17 Web-Application into an Ionic Mobile-Application

I know, React-Native exists and does that, **BUUUUT** sometimes your application is too big to transform all HTML elements into React-Native components and you might need to release a Mobile-App ASAP, if this is your case, this tutorial is for you.

Before you follow this tutorial, make sure that you can upgrade or downgrade to use react@17, if you can't this might not work for you.

To follow this tutorial you are going to need:
- [NodeJS](https://nodejs.org/en/download/)
- (optional) [Android Studio](https://developer.android.com/studio) (only if you want to run the app in an Android Device)
- (optional) [Xcode](https://developer.apple.com/xcode/) (only if you want to run the app in an iOS Device, don't forget that for this you need to use a Mac computer)
- Any Code Editor
- Your terminal

Let's start

1. Create a React app (using [create-react-app](https://create-react-app.dev/docs/getting-started)) `npm create-react-app my-test-app --template cra-template-typescript`
2. Install [Ionic CLI](https://ionicframework.com/docs/intro/cli) `npm install -g @ionic/cli`
3. Install Ionic/Capacitor packages:
    - `npm install @capacitor/app @capacitor/core @capacitor/haptics @capacitor/keyboard @capacitor/status-bar @ionic/react`
    - `npm install --D @capacitor/cli`
4. Update the react package to the compatible version
    - `npm install react@17.0.1 react-dom@17.0.1 react-scripts@5.0.0 typescript@4.1.3`
      - If you are using react version 18 (or higher), make sure update the [`src/index.tsx`](./src/index.tsx) file to make it compatatible with version 17, like this
        ```
        import React from 'react';
        import ReactDOM from 'react-dom';
        import './index.css';
        import App from './App';
        import reportWebVitals from './reportWebVitals';

        ReactDOM.render(
            <React.StrictMode>
                <App />
            </React.StrictMode>,
            document.getElementById('root')
        );
        ```
5. Create `capacitor.config.ts` & `ionic.config.json` files
    - [`capacitor.config.ts`](capacitor.config.ts)
    ```
    import { CapacitorConfig } from '@capacitor/cli';

    const config: CapacitorConfig = {
        appId: 'io.ionic.starter',
        appName: 'my-test-app',
        webDir: 'build',
        bundledWebRuntime: false
    };

    export default config;
    ```
    - [`ionic.config.ts`](ionic.config.ts)
    ```
    {
        "name": "my-test-app",
        "integrations": {
            "capacitor": {}
        },
        "type": "react"
    }
    ```
    - Don't forget update your app name in those files
    - If you need an extra config to run you mobile-app you are going to need update the `capacitor.config.ts` file, you can find the information [here](https://capacitorjs.com/docs/config)
6. Add these lines to your [`public/index.html`](./public/index.html) file
    ```
    <base href="/" />
    <meta name="color-scheme" content="light dark" />
    <meta
        name="viewport"
        content="viewport-fit=cover, width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no"
    />
    <meta name="format-detection" content="telephone=no" />
    <meta name="msapplication-tap-highlight" content="no" />

    <!-- add to homescreen for ios -->
    <meta name="apple-mobile-web-app-capable" content="yes" />
    <meta name="apple-mobile-web-app-title" content="Ionic App" />
    <meta name="apple-mobile-web-app-status-bar-style" content="black" />
    ```
6. Add new commands to [`package.json`](package.json) file
   ```
   ...
    "build:ionic": "ionic capacitor build android && ionic capacitor build ios && npm run resources",
    "build:android": "ionic capacitor build android && npm run resources",
    "build:ios": "ionic capacitor build ios && npm run resources",
    "build:sync": "npx cap sync",
    "jetify": "npx jetify && npx cap sync",
    "resources": "capacitor-resources -p android,ios",
    "emulate:ios": "ionic capacitor run ios --port=3000 --livereload",
    "emulate:android": "ionic capacitor run android --port=3000 --livereload --external",
    "emulate:android:ssl": "ionic capacitor run android --ssl",
    "release:android": "ionic capacitor build android --prod --release",
    "release:ios": "ionic capacitor build ios --prod --release"
    ...
   ```
7. (Optional) Create Icon & Splash-Screen images (if you skip this step you need to remove the command from [`package.json`](package.json))
   1. Install [Capacitor Resources](https://www.npmjs.com/package/capacitor-resources) `npm install capacitor-resources -g`
   2. Create in the root of the project a folder with the name `resources`
   3. Follow [these instructions](./resources/README.md) for create the right images
8. Build Project `npm run build:ionic` (this is going to build the project for both platforms)
    - If you have a peer dependency issue that doesn't allow you to finish the build you can try build with the command `npx cap add android` for Android and `npx cap add ios` for iOS
    - Sometimes you might need to update `gradle` for Android app, check it in your [Android Studio](https://developer.android.com/studio) IDE to do this, more info in the [the troubleshooting section](#android-app)
9.  Emulate project `npm run emulate:ios` for iOS or `npm run emulate:android` for Android, and choose the device that you want

### Helpful commands

| Command        | What it does |
| -------------- | ------------ |
| `npm run build:ionic` | Build the Ionic project for Android & iOS and copy the resources files (icon and splash-screen images) into the new built |
| `npm run build:android` | Build the Ionic project for Android and copy the resources files (icon and splash-screen images) into the new built |
| `npm run build:ios` | Build the Ionic project for iOS and copy the resources files (icon and splash-screen images) into the new built |
| `npm run build:sync` | Update the Ionic project for Android & iOS |
| `npm run jetify` | If you have updated your gradle from Android Studio and you are using Cordova, run this command to update the Capacitor config |
| `npm run resources` | Copy the resources files (icon and splash-screen images) into the new built |
| `npm run emulate:ios` | Emulate your application on livereload mode in the port `3000` |
| `npm run emulate:android` | Emulate your application on livereload mode in the port `3000`, this mode use an `ip` for the address instead of `localhost` |
| `npm run emulate:android:ssl` | Emulate yourt application on production mode using `SSL` |
| `npm run release:android` | Create a release package for production |
| `npm run release:ios` | Create a release package for production |
| `npx cap open android` | Open project on Android Studio |
| `npx cap open ios` | Open project on Xcode |
| `npx cap add android` | Alternative way to build Android project |
| `npx cap add ios` | Alternative way to build iOS project |


### Mobile (Ionic) Application Release


-   Android Build `APK` (Android application Package is the packaging format which eventually will be installed on devices):

    1. Assuming that you have tested the app, so have built it once at least, run `npm run build:sync`, `npm run resources` and `npm run release:android`
    2. Open the the folder `[root]/android` as a project using [Android Studio](https://developer.android.com/studio) (if the previous step didn't open it automatically), alternatively, you can open it by running `npx cap open android`
    3. From the top NavBar select `Build -> Build Bundle(s) / APK(s) -> Build APK(s)`
    4. Doing that will iniciate the compile process and you will see a message in the right-bottom corner telling you when it's ready and where is it the `APK`


-   Android Build `Bundle` (Publishing format used to upload to Google Play Store):

    1. Assuming that you have tested the app, so have built it once at least, run `npm run build:sync`, `npm run resources` and `npm run release:android`
    2. Open the the folder `[root]/android` as a project using [Android Studio](https://developer.android.com/studio) (if the previous step didn't open it automatically), alternatively, you can open it by running `npx cap open android`
       1. Make sure that for each release you increase the bundle version (Version Code) and App version (Version Name)
       2. You can do this in [Android Studio](https://developer.android.com/studio) `File -> Project Structure -> Modules -> Default Config`
    3. From the top NavBar select `Build -> Generate Signed Bundle or APK`
    4. Select `Android App Bundle` and click `next`
    5. Select the `key` stored to update the app, once the `key` file has been selected this should complete the remaining fields, and now click on `next` 
       - If is the first time uploading the app you must create a new key clicking on the `Create new...` button
    6. Select `release` option and click `finish`
    7. Now, Android Studio will start to build the APK Bundle, if you keep an eye in the right-bottom corner you will be able to see the a notification when is ready and where is located, which usually would be in `[root]/android/app/release/app-release.aab`
    8.  Now go to [Google Play Console](https://play.google.com/console/u/0/developers) to upload the app
    9.  Go to the `Production` section and in the `App bundles` and upload the created file in the previous step


-   iOs Build `IPA` (Used to directly install into a device):

    1. Assuming that you have tested the app, so have built it once at least, run `npm run build:sync` and `npm run resources`
    2. Open the the folder `[root]/ios` as a project using [Xcode](https://developer.apple.com/xcode/) (if the previous step didn't open it automatically), alternatively, you can open it by running `npx cap open ios`
       - (Optional) Once is open, check the "Build Warning" tab and click in the automatic fix (if is possible) when that fix is finished, select a device for testing in the top nav bar, and click on "play" to make sure that is still working 
    3. In the top bar, click on `Product -> Build` and wait till finish the process, then go to [Xcode](https://developer.apple.com/xcode/) under Project Navigator `App -> Products -> App` and there you have the installer file


-   iOs Build Distributable bundle (`Bundle` used to publish in the App Store):

    1. Assuming that you have tested the app, so have built it once at least, run `npm run build:sync` and `npm run resources`
    2. Open the the folder `[root]/ios` as a project using [Xcode](https://developer.apple.com/xcode/), you can open it by running `npx cap open ios`
       - (Optional) Once is open, check the "Build Warning" tab and click in the automatic fix (if is possible) when that fix is finished, select a device for testing in the top nav bar, and click on "play" to make sure that is still working 
    3. Now, in the TopNavBar, where says `App` (in the center), select the option `Any iOS Device` (this is for built the app for any device)
    4. Under Project Navigator, select your porject and then in the middle section select `Signing & Capabilities` and make sure you have chosen your organization's team
    5. Under Project Navigator, select your project and then in the middle section select `General` check the version and build info, those must be greater than the previous built and version (ideally should match with the used on the Android Build)
    6. In the TopNavBar, click on `Product -> Archive` and wait till finish the process (can be checked on the TopNavBar, there should be a green wheel), then in the new opened windows, select your version app (it will show you a list with previous version of the same app) and click on `Validate App` and follow the instruction until finish the process
       - Once the Validate windws is opene, write the name of your app in the field "name" the other way will stay just as "App"
       - If you have the error "error finding app store connect credentials" open your account on [App Store Connect](https://appstoreconnect.apple.com/) using the website and try again
    7. Now click on `Distribute App`, select `App Store Connect`, and then `Upload`
    8. All the rest must be updated on the [App Store Connect](https://appstoreconnect.apple.com/) website

## Troubleshooting

#### Use native mobile features

To use some native mobile features, like the Camera, Bluetooth, among others, you must install a plugin, to do that we have two options:

-   [Capacitor plugin](https://capacitorjs.com/docs/plugins):

    -   This must be our priority, either the [Official Plugins](https://capacitorjs.com/docs/apis) or the [Community Plugins](https://capacitorjs.com/docs/plugins/community), to do that you just have to find the plugin that you want and follow the documentation.

-   [Cordova plugin](https://cordova.apache.org/plugins/):

    -   We should avoid use Cordova plugins as much as we can, because most of these are deprecated, but due to Capacitor is still considered "new", most of the plugins that we might need are not available for Capacitor, so here is where we would be needing use a Cordova plugin.

    -   Use a Cordova plugin it's more complex than use Capacitor plugin, so here are the necessary steps to do that:

        1. Find the Cordova plugin on [this](https://cordova.apache.org/plugins/) list, or just google it (you can use [npm](https://www.npmjs.com/) to find it too), most of the time (if not all the time) the name of the plugin will start with the words `cordova-plugin-`, for example, the name of the [plugin to read/write](https://www.npmjs.com/package/cordova-plugin-file) files on the device is `cordova-plugin-file`

        2. To continue with the above example, to install that plugin you must use the command `npm i cordova-plugin-file` (unlike was done with previous versions `cordova plugin add cordova-plugin-file`)

        3. Then you should go and find the "bridge plugin", this it's an npm package used to call/import the plugin from your react project, so you will be calling the "bridge plugin" from your code, and that "bridge plugin" is going to call (internally) the real plugin.

            - As a "bridge plugin" we have two options, the [@awesome-cordova-plugins](https://www.npmjs.com/search?q=awesome-cordova-plugins%2F) and @ionic-native, lets **try** to use only the @awesome-cordova-plugins for consistency

            - Example of Cordova functions use (without Ionic)

            ```
            // No need import
            // to call a Cordova function you use `window`
            // and then inside of it you might be able to get the functions that you want

            window.resolveLocalFileSystemURL(file, fileEntry => {
            	...
            }
            ```

            - Example of Cordova function using the "bridge plugin" from @awesome-cordova-plugins

            ```
            import { File } from '@awesome-cordova-plugins/file';

            File.resolveLocalFilesystemUrl(path).then(fileSystem => {
            	...
            }
            ```

            For the above example, due to the lack of documentation, it was necessary to find the function (and structure) to import directly on the `node_module` folder

        4. You can find the official "bridge plugin" list [here](https://github.com/rdn87/awesome-cordova-plugins#list-plugins), I suggest find it on [npm here](https://www.npmjs.com/search?q=awesome-cordova-plugins%2F) to get the exact name and installation command.

        5. So to install the [Awesome Cordova Plugin File](https://www.npmjs.com/package/@awesome-cordova-plugins/file) we must run the command `npm i @awesome-cordova-plugins/file`

        6. At this point we have installed 2 different packages to get the plugin, now we have to pass that information to the iOS/build & android/build (mostly for the gradle) that we have installed a package that it's not part of Capacitor, to do that we have installed [`jetifier`](https://www.npmjs.com/package/jetifier) this it's a tool that will care of pass the external plugins to the built files.

        7. To use [`jetifier`](https://www.npmjs.com/package/jetifier), we need to run `npm run jetify` (our minified command), this will update our build and synchronize it with Capacitor, after this you are ready to run and test the new installed plugin in the emulator/device, see more in [the troubleshooting section](#android-app)

#### Building Android/iOs application

-   If you face the error `An error occurred while running subprocess` (which might appear for corrupts build) you will have to delete the `[root]/android` or `[root]/ios` (depending of which built gaves you the problem) **and** `[root]/build` after that try again.

#### Android App

-   The `SSL` does not work on livereload mode and doesn't allow to choose a port.

-   Livereload mode only can work with the external flag, so the Ionic App can only run under an `ip` url instead of under `localhost` (unlike iOs).

-   If you have a `Gradle` trouble building the app you have to open the `[root]/android` with [Android Studio](https://developer.android.com/studio) as a project, and in the right-bottom corner you will have a message asking to update the grade, so do it (see the next images)
![Screen Shot 2022-05-27 at 15 28 22](https://user-images.githubusercontent.com/48134692/170797390-7ce5d12b-a80f-4237-913e-94f54b260cca.png)
![Screen Shot 2022-05-27 at 15 28 42](https://user-images.githubusercontent.com/48134692/170797394-b77096d1-5d19-40ec-abfd-96535cd23a24.png)
![Screen Shot 2022-05-27 at 15 28 57](https://user-images.githubusercontent.com/48134692/170797396-cbf80345-bcaa-4c6b-93cc-dbc9218522c8.png)
and then after that **only if** you have installed a Cordova ( see [the troubleshooting : Use Native mobile features](#use-native-mobile-features) ) package run `npm run jetify` the other way it should be ready and fixed.

-   If you have a JDK version trouble while you are building the project, go to `Android Studio -> Preferences -> Build, Execution, Deploymeny -> Build Tools -> Gradle` and in the section `Gradle JDK` choose the one that [Android Studio](https://developer.android.com/studio) is asking for.

-   Some times when you want to run the app you could have an error saying that your `Manifest` file wasn't found, so you have to delete the folder `[root]/android` and then run `ionic capacitor build android`, after that you should be able to run again `ionic capacitor run android --livereload --external`.

#### iOs App

-   The `SSL` does not work on iOS.

## Final note
This tutorial doesn't mean to replace a React-Native or any other optimized mobile-app, this might not have the best performance, but is still good enough to release a mobile-app ASAP and give you extra time to create an app with the best performance possible =) 
