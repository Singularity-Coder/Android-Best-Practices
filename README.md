# Android-Best-Practices
A list of personal best practices that I (would like to) follow while releasing the App to PlayStore!

## User Interface & Experience
1. Use Shimmer placeholder for showing the content that is being loaded.
2. Use recycler view animation. Cards stacking one after another from below or above.
3. AB Testing using Firebase Remote Config.

## Cross -Device Functionality Checks
1. Follow this site if you dont have live devices to test on - [dontkillmyapp.com](https://dontkillmyapp.com/).
2. Follow this site if you want to test on live devices - [browserstack.com](https://www.browserstack.com/)

## Resource Management
1. Get rid of unused images
2. Crunch Pngs in Gradle
3. Clear unwanted imports. Select all packages -> right click -> Optmize Imports.

## Architecture
1. Use MVVM architecture for decoupling the module and making it more testable.
2. Strucutre the App packages feature/module wise rather than architecture wise. This is something even Robert C Martin also talks about in this [clean code series](https://www.youtube.com/watch?v=7EmboKQH8lM). 
3. Package common utilities used in different kinds of apps in a library and use it as a dependency. For example, you can use a Bluetooth Utils Library, API Utils Library, Network Utils Library, etc. Keeps the working packages clean. The below package structure clearly tells you that it is an E-Commerce App. The idea is that the package structure should not inform us about the architecure of the App first but rather what the App does. The "What App" before the "What Architecture" just like when you a blueprint you will know what they are building rather than what they used to build.
![alt text](https://github.com/Singularity-Coder/Android-Best-Practices/blob/main/assets/s1.png)

## Handling Crashes
1. Use ACRA or similar libraries for avoid shoing ANR dialog when the app crashes.
2. Use Timber wisely especially if you have lots of users.
3. Check for possible exceptions.
4. Check for possible null values.

## Security
1. Encrypt API Auth Tokens.

## Testing
1. Unit tests.
2. Espresso or other instrumentation tests.
3. End-to-End tests.
4. Some form of CI/CD using Firebase Test Lab or Jenkins for automated testing.
5. Try TDD - Test Driven Development. I know its hard but we must start somewhere.

## Release
1. Shrink or split the APK or use some form of App Size shrinking mechanism.
2. Use App Bundle instead of an APK.
