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
      - If you are using react version 18 (or higher), make sure update the src/index.tsx file to make it compatatible with version 17, like this
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
5. Create `capacitor.config.ts` & `ionic.config.json` file
    - Update your app name in those files
    - If you need an extra config tu run you mobile-app you are going to need update the `capacitor.config.ts` file, you can find the information [here](https://capacitorjs.com/docs/config)
6. Add these lines to your `public/index.html` file
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
6. Add new commands to `package.json` file
7. Build Project `npm run build:ionic`
    - Sometimes you might need to update `grandle` for Android app, check in you [Android Studio](https://developer.android.com/studio) IDE to do this
8. Emulate project `npm run emulate:ios` for iOS or `npm run emulate:android` for Android, and choose the device

#### Final note, this tutorial doesn't mean to replace a React-Native or any other optimized app, it might not have the best performance, but still good enough to release a mobile-app ASAP and give you extra time to create an app with the best performance possible =) 
