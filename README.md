# Android-Best-Practices
A list of personal best practices that I (would like to) follow while releasing the App to PlayStore! Code Snippets for software best practices like Design Patterns, Design Principles, SOLID Principles, Effective Java, etc.

## Before starting the project:
* Write a design doc
* Describe what you will build
* What feature requirements are
* How you will set things up 
* Which functions you will need
* What classes you will need
* What Data Structures you will need

## User Interface & Experience
* Use Shimmer placeholder for showing the content that is being loaded.
* Use recycler view animation. Cards stacking one after another from below or above.
* AB Testing using Firebase Remote Config.
* Use Material Components as a lot of common use cases are baked into them and can also be controlled with a single line of code. For example, lets see a good EditText example with all the convenience features like clearing the text with a single action, error handling, support text, character count, text hiding for password feature, background styling, etc.
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
* Use shared element transitions for providing better context.

## Cross-Device Functionality Checks
* Follow this site if you dont have live devices to test on - [dontkillmyapp.com](https://dontkillmyapp.com/).
* Follow this site if you want to test on live devices - [browserstack.com](https://www.browserstack.com/)

## Caching
* Use Retrofit Caching (Somewhere on the internet it is recommended not to use Retrofit Caching).
* Use Room ORM.
* For images use Glide's diskCacheStrategy method. Refer [stackoverflow](https://stackoverflow.com/questions/46349657/difference-diskcachestrategy-in-glide-v4) for more info.
```java
Glide.with(context)
        .load(imgUrl)
        .apply(new RequestOptions().diskCacheStrategy(DiskCacheStrategy.AUTOMATIC))
        .into(imageView);
```

## Resource Management
* Get rid of unused images and assets.
* Crunch Pngs in Gradle.
* Clear unwanted imports. Select all packages -> right click -> Optmize Imports.

## Memory Management
* Try avoing static stuff. 
* Clean up stuff in onDestroy() lifecycle callback.
* Pass the right context. ActivityContext, ApplicationContext.

## Background Processing
* Use Executors instead of AsyncTask as the later is deprecated for Android versions over API 30.
* More than Executors use RxJava for handling Async operations. In Kotlin use Coroutines.
* For lifecycle aware observables use LiveData.

## Mechanisms to Control Concurrency
Read SICP book for this 
* Synchronisation
* Volatile
* Atomic 

## Architecture
* Use MVVM architecture for decoupling the module and making it more testable. It is also recommended by Google.
* Strucutre the App packages feature/module wise rather than architecture wise. This is something even Robert C Martin also talks about in this [clean code series](https://www.youtube.com/watch?v=7EmboKQH8lM). The below package structure clearly tells you that it is an E-Commerce App. The idea is that the package structure should not inform us about the architecure of the App first but rather what the App does. The "What App" before the "What Architecture" just like when you a blueprint you will know what they are building rather than what they used to build.
![alt text](https://github.com/Singularity-Coder/Android-Best-Practices/blob/main/assets/s1.png)
* Package common utilities used in different kinds of apps in a library and use it as a dependency. For example, you can use a Bluetooth Utils Library, API Utils Library, Network Utils Library, etc. Keeps the working packages clean.

## Architecture Rules
Refer [clean code series](https://www.youtube.com/watch?v=7EmboKQH8lM) by Robert C Martin.
* There are hard boundaries - views don’t spill into business rules, etc.
* There is a very linear dependency flow. Like an onion. Business rules don’t know anything abt views. Its all about dependency control.
* Small, modular and highly separated and testable components.
* Its also all about testability. How easy it is to test an object or an entity or model or test business rules, or controllers or interactors.
* Dependencies point in one direction that is towards the business rules. Like a plugin architecture. Modularity comes from this. Like the web is a plugin to the business rules.
* If something changes a lot then it should be a plugin like UI. Plugins always have source code dependencies that point in one direction.
* If something doesn’t change much then it should be plugged into. 
* The idea is to have independent testable components without the need of a dependency of other classes. I.e. I pass input it spits output.

## Handling Crashes
* Use [ACRA](https://github.com/ACRA/acra) or similar libraries for avoid showing ANR dialog when the app crashes.
* Use Timber wisely especially if you have lots of users.
* Check for possible exceptions.
* Check for possible null values.
* Check for memory leaks. Use [Leak Canary](https://square.github.io/leakcanary/) library to detect memory leaks.

## Security
* Encrypt API Auth Tokens.
* Use Version Control System like Git.
* Write good commit messages. Something I myself must follow.
* Squash unwanted commits. Makes it easy to track and revert to old project states. [squash commits](https://stackoverflow.com/questions/5189560/squash-my-last-x-commits-together-using-git).
* Handle user input for emoji and other special characters using custom EditText properties. 
* If you are targeting only english speaking users then you can limit the user input into EditTexts using the "digits" attribute to prevent users from entering unwanted characters. For example
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
* Unit tests.
* Espresso or other instrumentation tests.
* End-to-End tests.
* Some form of CI/CD using Firebase Test Lab or Jenkins for automated testing.
* Try TDD - Test Driven Development. I know its hard but we must start somewhere.

## Debugging
* Prefer breakpoints over log statements for debugging. When it fails use log statements.
* Surround with try-catch-finally blocks after you make the feature work. Else it is sometimes hard to trace out the error in a large code base.

## Debugging Techniques
* Go back to the previous working state and check. Keep removing or adding lines of code that messed that up.
* Method of exclusion. Exclude possibilities that don’t work. Then narrow down on the few or one thing that is causing the problem. 
* Divide and conquer.

## Performance
* Use Android device's Developer Options to check View drawing performance.

## Fallback Strategies
* When Image doesnt load use Glide's placeholder and error methods to load default images for those 2 states.
```java
Glide.with(context)
        .load(imgUrl)
        .apply(new RequestOptions().placeholder(R.color.colorAccent).error(R.mipmap.ic_launcher))
        .into(imageView);
```

## Release
* Shrink or split the APK or use some form of App Size shrinking mechanism.
* Use App Bundle instead of an APK.
* Follow some form of Agile Methodology like SCRUM for releasing.

## Other
* Use Glide Library's image rounding methods rather than importing a separate library for just that one purpose.
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
* Always format your code. Cmd + Option + L
* Always remove unwanted imports. Cmd + Option + O
* Check for Time and Space complexities.
* Use SVG image files rather than png or jpg formats for infinite scaling.
* Use some form of Analytics to monitor events.
* Disable button every network request until response. Disable buttons when performing a background task or making a network call to avoid multiple requests and enable them on response.
* Close soft keyboard on button presses and navigations before navigating or use Navigation Components as it handles all these minor things automatically. I have especially observed closing the soft keyboard while navigating between fragments in Navigation Components.
* Use Builder Pattern for designing readable objects.
* Avoid Try Catch Finally blocks while Testing/Building until u cracked the logic perfectly. Add Try Catch just to avoid unexpected behaviour. Dont build the feature using Try Catch. Else you will miss cases where the logic is not perfect and Try Catch does its job but you wont be able to figure out what went wrong. This happens when you are dealing with large code bases. 
* Avoid Else statements. Totally unclear. 
* Avoid passing data using shared prefs. They are not meant for direct communication. Use Intent extras in activities and bundles in Fragments as much as possible.
* Make every variable, function and class final or non inheritable, immutable, non overridable by default. If necessary then make it mutable. 
* Use SQLite or Room for caching complex data types. Dont use shared prefs or other simple primitive type storage systems by converting complex objects to String type using Gson and then storing them.
* Always extend the system than modify it. It is far more safer especially if you are dealing with large code base and you dont know the ins and outs of it.
* Dont follow DRY principle blindly. Sometimes it is better to repeat the code for better context. Especially when you are dealing with messy code bases.
* Comments should be avoided as much as possible. Comments should only be used to inform why such decisions were made in the code rather than explain the code. For example, why Glide is used instead of Picasso for image loading. The "why" part often cannot be conveyed through code unless the developer already knows the trade offs which is often not the case.
* Function or Method names should be verbs. A verb is an action word like doThis, getThat, PassThis, LaunchThat, RepeatThis, setThat, etc.
* Class names should be nouns. A noun is an identity word used to represent or describe something. Its like giving a name to an animal or a person or a thing based on what they do. For example a person who flings from one building to another and shoots webs like a spider, we call him Spider Man. Something that trims your beard is called a Trimmer. Etc. 
* Write descriptive variable and method names.
* Use REPL for quick experiments and feedback. For Java REPL use https://repl.it/languages/java10 and for Kotlin, use the inbuilt REPL. In Android Studio naivagte to the menu Tools > Kotlin > REPL.
* Use tools:visibility to make the views visible during the development phase.
* Use Byte Type for variables like age, height, etc that do not exceed the max Byte value.
* Always use integers or long whole numbers and not decimal number for calculating money as you dont want to round off money on large values. You will lose a ton of money that way.
* For representing ridiculously large numbers use BigDecimal or BigInteger classes in Java and Kotlin.
* Instead of representing large numbers this way 2000000, prefer using the scientific notation to avoid missing zeros and causing bugs. Ex: 2E6 or 2E-6 which means 2 * 10 ^ 6 or 2 * 10 ^ -6. E is an exponent. You can also use 2 * Math.pow(10, 6) to represent large numbers in a readable way. In Kotlin you can also use underscores like 2_000_000.
* Avoid nesting if statements. Instead use small functions and exit the function using the return keyword if the condition is false. This works for functions that are void or return something.
```Java
public final void welcomeGuest(final int age) {
    if (age < 18) return;
    println("Welcome!");
}
```
* Avoid switch statements in Java as they do not read well. Use the standard if-else statements.
* Always use named parameters in Kotlin functions. It makes things super clear.
* Use Facebook Stetho to monitor and debug network calls in your app. 
* Use Kotlin for Android Development. You can check this link https://kotlinlang.org/docs/reference/comparison-to-java.html for more info on why we should use Kotlin over Java. However, we must cautiously use the syntax in Kotlin as it sometimes feels like it is more optimized for writing code than reading code which is done way more than writing code. Higher-order functions like run{}, with() {}, etc, introduce 2 problems which are excessive nesting statements and the other is the usage of extra brain cells where I have to consciously understand that the inner methods and fields are being called on the wrapper. In my opinion, Java is not verbose, it's just explicit. That is what makes it clear. Inference is not always clear. Writing less code doesn't necessarily mean easier to understand or clearer in meaning. So sometimes opting for more verbose routes can make things clearer in the long term as we tend to forget things easily. Slightly irrelevant but, E=mc^2 is a small equation but it has a vast amount of insight compressed in it which can be inferred. Also, not enforcing a statement separator like ";" at the end of each statement makes the code harder to read. In my opinion, every language must have some sort of statement terminator like how English has "." period and Java has ";". These are essential for reading the content properly.

## Prefer Functional Programming Paradigm
Functional Programming in Java by Venkat Subbramaniam
* Be Declarative - immutability and declarative programming is functional. Its basically working with higher order functions or constructs instead of relying or implementing your own details.
* Promote immutability - final vars, functions and classes. (How will u build the app if you cannot update any variable. Like Haskell probably recursive functions.)
* Avoid Side effects - Immutable functions. One function does only one thing. It does not trigger other functions. Avoids sequential function calls. Avoids race conditions like which function must execute first. A side effect is that when you update one thing or change one thing another one or a bunch change. That must be avoided. Especially problematic in multithreading.
* Prefer Expressions over statements - Expressions promote immutability. Use streams. (Not clear)
* Design with higher order functions - Manage with functions, anonymous classes and functions, functional interfaces like Runnable, Callable, Comparable, Function, Predicate, Consumer, Supplier. Try Rx as well, the idea is create more abstract interfaces to accommodate change:
    1. Pass functions to functions
    2. Create functions within functions
    3. Return functions from functions

## Material Design 

## Common Android Studio Keyboard Shortcuts
Using these shortcuts is also one form of best practice as it reduces the scope of errors while doing things manually especially when refactoring.

CODE APPEARANCE
* To move methods up and down: ctrl + shift + up or down
* To move lines up and down: alt + shift + up or down
* Duplicate lines: ctrl + d
* Delete lines: ctrl + x
* Comment lines: ctrl + /
* Collapse function block: Ctrl + Numpad +
* Expand function block:  Ctrl + Numpad -
* Collapse all function blocks: Ctrl + Shift + Numpad +
* Expand all function blocks: Ctrl + Shift + Numpad -
* Format Code: Cmd + Alt + L
* See Format Settings: Cmd + Alt + Shift + L

CODE SUGGESTIONS
* Code suggestion: Ctrl + Space
* Extended Code suggestion: 2 times - Ctrl + Space
* Smart Type Suggestions: Ctrl + Shift + Space
* Context Suggestions (even for errors): Alt + Enter
* Show method signature or parameter info: Ctrl + P
* Show Type (type of a variable): Ctrl + Shift + P
* Show Documentation: Ctrl + Q
* Show Definition (shows entire method with body): Ctrl + Shift + I
* Show Error or Warning suggestion once and twice for expanded: Ctrl + F1
* Highlight usages (highlights all places we used that): Ctrl + Shift + F7
* Show usages (shows all places we used that): Ctrl + Alt+ F7
* Navigate to Source: Ctrl + B

CODE COMPLETION
* Replace code completion: press Tab after Ctrl + Space and selection
* Code completion: Ctrl + Shift + Enter
* Postfix code completion: Press . (Period)
* Statement completion like for, if, etc: Ctrl + Shift + Enter
* Wrap/Surround like surround with try/catch/finally etc: Ctrl + Alt + T
* UnWrap say try/catch block: Ctrl + Shift + Delete

SEARCH
* Search Everywhere: press Shift twice
* Search Actions: Ctrl + Shift + A
* Search Classes: Ctrl + N - To search for library implementations as well filter from "Project Files" to "All Places"
* Sequential navigation to error locations: F2
* List all methods in a file (File Structure): Ctrl + F12
* List them in tool window (File Structure): Alt + 7
* Jump to Declaration or Usage of a method: Ctrl + B
* Show Detailed usage of a method: Alt + F7
* Search for implementations of an interface: Ctrl + Alt + B
* Navigate to a super method from derived (super method is just the unimplemented method in the interface): Ctrl + U
* Explore whole hierarchy for a super method: Ctrl + Shift + H
* Show recently opened files: Ctrl + E
* Recently visited code: Ctrl + Shift + E
* Find next occurrence: Ctrl + F then Enter
* Find previous occurrence: Ctrl + F then Shift Enter
* Run Anything: Press Ctrl twice
* Jump to line number: Ctrl + G
* Find Stuff: Help > Actions > Analyze > Run Inspection by Name > Unused resources
* Find Stuff: File > Refactor > Remove Unused Resources > Preview

CODE SELECTIONS
* Sequential Multiple Selections: Alt + J
* Sequential Multiple De-Selections: Alt + Shift + J
* Select All Occurrences in the file: Ctrl + Alt + Shift + J
* Expand or shrink code selection: ctrl + w or ctrl + shift + w

REFACTORING
* Rename: Shift + F6
* Extract Variables: Ctrl + Alt + V
* Extract Method: Ctrl + Alt + M
* Refactoring Menu (Loads of other refactoring options): Ctrl + Alt + Shift + T

OTHERS
* Hide tool window: Shift + Esc
* Run: Ctrl + Shift + F10

## Common Git Commands
* Get rid of or Stash unwanted changes
* Remove commit from remote
* Create branch n switch btw branches
* Set commit author

## Common Vim keyboard Shortcuts
A good alternative to IDE for practicing whiteboarding and coding. Android in Vim will probably be painful.

## Common Mac keyboard Shortcuts
Using keyboard shortcuts is also one type of best practice as they are precise in action and most importantly quicker to use.
* Use Ctrl + Backspace to delete a word.
* Use Shift to write uppercase letters.

