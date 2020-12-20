# Android-Best-Practices
A list of personal best practices that I (would like to) follow while releasing the App to PlayStore! Code Snippets for software best practices like Design Patterns, Design Principles, SOLID Principles, Effective Java, etc.

## Before starting the project:
1. Write a design doc
2. Describe what you will build
3. What feature requirements are
4. How you will set things up 
5. Which functions you will need
6. What classes you will need
7. What Data Structures you will need

## User Interface & Experience
1. Use Shimmer placeholder for showing the content that is being loaded.
2. Use recycler view animation. Cards stacking one after another from below or above.
3. AB Testing using Firebase Remote Config.
4. Use Material Components as a lot of common use cases are baked into them and can also be controlled with a single line of code. For example, lets see a good EditText example with all the convenience features like clearing the text with a single action, error handling, support text, character count, text hiding for password feature, background styling, etc.
```xml
<com.google.android.material.textfield.TextInputLayout
        android:id="@+id/et_password"
        style="@style/Widget.MaterialComponents.TextInputLayout.FilledBox.Dense"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="8dp"
        android:hint="Password"
        app:counterEnabled="true"
        app:counterMaxLength="15"
        app:endIconMode="password_toggle"
        app:errorEnabled="true"
        app:helperText="Minimum 8 digits"
        app:helperTextEnabled="true"
        app:layout_constraintTop_toBottomOf="@+id/et_profession"
        app:startIconContentDescription="Password"
        app:startIconDrawable="@drawable/ic_twotone_lock_24">

        <com.google.android.material.textfield.TextInputEditText
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:inputType="textPassword"
            android:maxLines="1" />
    </com.google.android.material.textfield.TextInputLayout>
```
5. Use shared element transitions for providing better context.

## Cross-Device Functionality Checks
1. Follow this site if you dont have live devices to test on - [dontkillmyapp.com](https://dontkillmyapp.com/).
2. Follow this site if you want to test on live devices - [browserstack.com](https://www.browserstack.com/)

## Caching
1. Use Retrofit Caching (Somewhere on the internet it is recommended not to use Retrofit Caching).
2. Use Room ORM.
3. For images use Glide's diskCacheStrategy method. Refer [stackoverflow](https://stackoverflow.com/questions/46349657/difference-diskcachestrategy-in-glide-v4) for more info.
```java
Glide.with(context)
        .load(imgUrl)
        .apply(new RequestOptions().diskCacheStrategy(DiskCacheStrategy.AUTOMATIC))
        .into(imageView);
```

## Resource Management
1. Get rid of unused images and assets.
2. Crunch Pngs in Gradle.
3. Clear unwanted imports. Select all packages -> right click -> Optmize Imports.

## Handling Memory Leaks
1. Try avoing static stuff. 
2. Clean up stuff in onDestroy() lifecycle callback.
3. Pass the right context. ActivityContext, ApplicationContext.

## Background Processing
1. Use Executors instead of AsyncTask as the later is deprecated for Android versions over API 30.
2. More than Executors use RxJava for handling Async operations. In Kotlin use Coroutines.
3. For lifecycle aware observables use LiveData.

## Mechanisms to Control Concurrency
Read SICP book for this 
1. Synchronisation
2. Volatile
3. Atomic 

## Architecture
1. Use MVVM architecture for decoupling the module and making it more testable. It is also recommended by Google.
2. Strucutre the App packages feature/module wise rather than architecture wise. This is something even Robert C Martin also talks about in this [clean code series](https://www.youtube.com/watch?v=7EmboKQH8lM). The below package structure clearly tells you that it is an E-Commerce App. The idea is that the package structure should not inform us about the architecure of the App first but rather what the App does. The "What App" before the "What Architecture" just like when you a blueprint you will know what they are building rather than what they used to build.
![alt text](https://github.com/Singularity-Coder/Android-Best-Practices/blob/main/assets/s1.png)
3. Package common utilities used in different kinds of apps in a library and use it as a dependency. For example, you can use a Bluetooth Utils Library, API Utils Library, Network Utils Library, etc. Keeps the working packages clean.

## Architecture Rules
Refer [clean code series](https://www.youtube.com/watch?v=7EmboKQH8lM) by Robert C Martin.
1. There are hard boundaries - views don’t spill into business rules, etc.
2. There is a very linear dependency flow. Like an onion. Business rules don’t know anything abt views. Its all about dependency control.
3. Small, modular and highly separated and testable components.
4. Its also all about testability. How easy it is to test an object or an entity or model or test business rules, or controllers or interactors.
5. Dependencies point in one direction that is towards the business rules. Like a plugin architecture. Modularity comes from this. Like the web is a plugin to the business rules.
6. If something changes a lot then it should be a plugin like UI. Plugins always have source code dependencies that point in one direction.
7. If something doesn’t change much then it should be plugged into. 
8. The idea is to have independent testable components without the need of a dependency of other classes. I.e. I pass input it spits output.

## Handling Crashes
1. Use [ACRA](https://github.com/ACRA/acra) or similar libraries for avoid showing ANR dialog when the app crashes.
2. Use Timber wisely especially if you have lots of users.
3. Check for possible exceptions.
4. Check for possible null values.
5. Check for memory leaks. Use [Leak Canary](https://square.github.io/leakcanary/) library to detect memory leaks.

## Security
1. Encrypt API Auth Tokens.
2. Use Version Control System like Git.
3. Write good commit messages. Something I myself must follow.
4. Squash unwanted commits. Makes it easy to track and revert to old project states. [squash commits](https://stackoverflow.com/questions/5189560/squash-my-last-x-commits-together-using-git).
5. Handle user input for emoji and other special characters using custom EditText properties. 
6. If you are targeting only english speaking users then you can limit the user input into EditTexts using the "digits" attribute to prevent users from entering unwanted characters. For example
```xml
<com.singularitycoder.instashop.helper.CustomEditText
    android:id="@+id/et_signup_email"
    style="@style/StyleEditText"
    android:layout_width="match_parent"
    android:layout_height="50dp"
    android:layout_marginStart="15dp"
    android:layout_marginTop="5dp"
    android:layout_marginEnd="15dp"
    android:digits="qwertyuiopasdfghjklzxcvbnm 1234567890 QWERTYUIOPASDFGHJKLZXCVBNM .@-_"
    android:drawablePadding="10dp"
    android:fontFamily="@font/roboto_regular"
    android:hint="Type Email"
    android:inputType="textEmailAddress"
    android:maxLength="60"
    android:maxLines="1"
    android:nestedScrollingEnabled="true"
    android:scrollHorizontally="true"
    android:singleLine="true"
    android:textColor="@color/colorBlack"
    android:textSize="16sp"
    app:layout_constraintEnd_toEndOf="parent"
    app:layout_constraintStart_toStartOf="parent"
    app:layout_constraintTop_toBottomOf="@+id/tv_signup_email" />
```

## Testing
1. Unit tests.
2. Espresso or other instrumentation tests.
3. End-to-End tests.
4. Some form of CI/CD using Firebase Test Lab or Jenkins for automated testing.
5. Try TDD - Test Driven Development. I know its hard but we must start somewhere.

## Debugging
1. Prefer breakpoints over log statements for debugging. When it fails use log statements.
2. Surround with try-catch-finally blocks after you make the feature work. Else it is sometimes hard to trace out the error in a large code base.

## Debugging Techniques
1. Go back to the previous working state and check. Keep removing or adding lines of code that messed that up.
2. Method of exclusion. Exclude possibilities that don’t work. Then narrow down on the few or one thing that is causing the problem. 
3. Divide and conquer.

## Performance
1. Use Android device's Developer Options to check View drawing performance.

## Fallback Strategies
1. When Image doesnt load use Glide's placeholder and error methods to load default images for those 2 states.
```java
Glide.with(context)
        .load(imgUrl)
        .apply(new RequestOptions().placeholder(R.color.colorAccent).error(R.mipmap.ic_launcher))
        .into(imageView);
```

## Release
1. Shrink or split the APK or use some form of App Size shrinking mechanism.
2. Use App Bundle instead of an APK.
3. Follow some form of Agile Methodology like SCRUM for releasing.

## Other
1. Use Glide Library's image rounding methods rather than importing a separate library for just that one purpose.
```java
Glide.with(context)
        .load(imgUrl)
        .apply(new RequestOptions().circleCrop())
        .into(imageView);
```
or if you want iOS style rounded squares
```java
Glide.with(context)
        .load(imgUrl)
        .apply(new RequestOptions().transform(new CenterCrop(), new RoundedCorners(60)))
        .into(imageView);
``` 
2. Always format your code. Cmd + Option + L
3. Always remove unwanted imports. Cmd + Option + O
4. Check for Time and Space complexities.
5. Use SVG image files rather than png or jpg formats for infinite scaling.
6. Use some form of Analytics to monitor events.
7. Disable button every network request until response. Disable buttons when performing a background task or making a network call to avoid multiple requests and enable them on response.
8. Close soft keyboard on button presses and navigations before navigating or use Navigation Components as it handles all these minor things automatically. I have especially observed closing the soft keyboard while navigating between fragments in Navigation Components.
9. Use Builder Pattern for designing readable objects.
10. Avoid Try Catch Finally blocks while Testing/Building until u cracked the logic perfectly. Add Try Catch just to avoid unexpected behaviour. Dont build the feature using Try Catch. Else you will miss cases where the logic is not perfect and Try Catch does its job but you wont be able to figure out what went wrong. This happens when you are dealing with large code bases. 
11. Avoid Else statements. Totally unclear. 
12. Avoid passing data using shared prefs. They are not meant for direct communication. Use Intent extras in activities and bundles in Fragments as much as possible.
13. Make every variable, function and class final or non inheritable, immutable, non overridable by default. If necessary then make it mutable. 
15. Use SQLite or Room for caching complex data types. Dont use shared prefs or other simple primitive type storage systems by converting complex objects to String type using Gson and then storing them.
16. Always extend the system than modify it. It is far more safer especially if you are dealing with large code base and you dont know the ins and outs of it.
17. Dont follow DRY principle blindly. Sometimes it is better to repeat the code for better context. Especially when you are dealing with messy code bases.
18. Comments should be avoided as much as possible. Comments should only be used to inform why such decisions were made in the code rather than explain the code. For example, why Glide is used instead of Picasso for image loading. The "why" part often cannot be conveyed through code unless the developer already knows the trade offs which is often not the case.

## Prefer Functional Programming Paradigm
Functional Programming in Java by Venkat Subbramaniam
1. Be Declarative - immutability and declarative programming is functional. Its basically working with higher order functions or constructs instead of relying or implementing your own details.
2. Promote immutability - final vars, functions and classes. (How will u build the app if you cannot update any variable. Like Haskell probably recursive functions.)
3. Avoid Side effects - Immutable functions. One function does only one thing. It does not trigger other functions. Avoids sequential function calls. Avoids race conditions like which function must execute first. A side effect is that when you update one thing or change one thing another one or a bunch change. That must be avoided. Especially problematic in multithreading.
4. Prefer Expressions over statements - Expressions promote immutability. Use streams. (Not clear)
5. Design with higher order functions - Manage with functions, anonymous classes and functions, functional interfaces like Runnable, Callable, Comparable, Function, Predicate, Consumer, Supplier. Try Rx as well, the idea is create more abstract interfaces to accommodate change:
    1. Pass functions to functions
    2. Create functions within functions
    3. Return functions from functions

