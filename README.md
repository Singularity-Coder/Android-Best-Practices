![alt text](https://github.com/Singularity-Coder/Android-Best-Practices/blob/main/assets/banner_android_best_practices.png)
# Android-Best-Practices
A list of personal best practices that I (would like to) follow while releasing the App to PlayStore! Code Snippets for software best practices like Design Patterns, Design Principles, SOLID Principles, Effective Java, etc.


## Before starting the project
* Write a design doc
* Describe what you will build
* What feature requirements are
* How you will set things up 
* Which functions you will need
* What classes you will need
* What Data Structures you will need


## Before starting the project (More)
* Add R8 optimisation to Gradle. Optimizes Java bytecode while translating Kotlin. [Link](https://www.youtube.com/watch?v=lTo03M2HzFY) 
```Groovy 
android {
    buildTypes {
        release {
            minifyEnabled true  // R8 optimisation
        }
    }
}
```


## When you are stuck
* Where, When, What, Why, How 
    * is it initialised.
    * is it being called.
    * is the data coming from.
* Start from the view. How the data is set to the view.
* Refactor the code to the pattern you understand.


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
* Use Kotlin for Android Development. You can check this link https://kotlinlang.org/docs/reference/comparison-to-java.html for more info on why we should use Kotlin over Java. However, we must cautiously use the syntax in Kotlin as it sometimes feels like it is more optimized for writing code than reading code which is done way more than writing code. Scope functions like run{}, with(){}, etc, introduce 2 problems which are excessive nesting statements and the other is the usage of extra brain cells where I have to consciously understand that the inner methods and fields are being called on the wrapper. In my opinion, Java is not verbose, it's just explicit. That is what makes it clear. Inference is not always clear. Writing less code doesn't necessarily mean easier to understand or clearer in meaning. So sometimes opting for more verbose routes can make things clearer in the long term as we tend to forget things easily. Slightly irrelevant but, E=mc^2 is a small equation but it has a vast amount of insight compressed in it which can be inferred. Also, not enforcing a statement separator like ";" at the end of each statement makes the code harder to read. In my opinion, every language must have some sort of statement terminator like how English has "." period and Java has ";". These are essential for reading the content properly.
* If you want to know the syntax well, use a text editor instead of an IDE and use the terminal to build and install the project.
* Before releasing the App to Playstore it is good to take care of compile time warnings as well. They could not only increase build speeds but also take care of potential bugs. However the IDE is not always right about things like code style, readability, and sometimes even correctness. We should not blindly consider every yellow highlight in our code base as a warning as Android Studio seems to warn about things that are not warnings to begin with. For example, the unnecessary safe call in Kotlin or the short-circuiting of a cast as a similar condition was already checked before. It is always good to err on the side of caution and keep things as explicit and human readable as possible. Sometimes I feel the suggestions of Android Studio are aimed towards a robot than a human. So it is important to use good sense when we take care of warnings. However it is true that these warnings can be noisy when you read the code but I feel that is a minor trade-off for possible safety. I am not sure of the performance implications here though.
```Kotlin
data class Person(val biodata: String = "xxxxxxxx")
val person = Person()
val myBio = person?.biodata // Safe call doesn't hurt but I am not sure of the performance implication

if (person is Person && (person as Person).biodata.equals("xxxxxxxx")) {    // Redundant cast as it was already confirmed true in the previous condition but still it is clear to read in my opinion. This is a simple example used for demo but makes sense in complex code bases.
}

person?.let { _ -> 
    // Even if we don't do anything with the lambda value, the use of an _ is just bad in my opinion and unfortunately Android Studio suggests that. It just doesnt read well. Once again this is a simple example used for demo but makes sense in complex code bases.
}
```
* When we refactor the code, it is often good to take suggestions from Android Studio and linting tools like sonarlint plugin for Android Studio. And that should be it. They are suggestions and not necessarily hard and fast rules. They are sometimes very subjective so blindly following them will not always lead to human readable code. The optimisations somtimes lose context and feels like the code is meant to be read by robots and not humans. So again use good sense here as well. 
* When you  are not dealing with primitives in Java or default parameter values in Kotlin, for booleans we should use `x != true` rather than `x == false`. That way the condition never fails even if `x` is `null`.
* A faster way to build a feature is by preparing static dummy data. This is so obvious but sometimes when you are dealing with APIs and external data sources, you get caught up in that and develop a sense of dependency on the need for an external data source to build the feature and delay the process. So approximately prepare dummy data, create the views and finish the feature first. Then operate on real data.
* Android Studio seems to break a long line of code into sensible multi-line code blocks. This is great but it is not always good. Sure it makes the code readable and even reduces mistakes in some cases but not all code blocks need that much attention all the time. It is good to get a sense of what a method does in a single glance. That is only possible if the method is short and fits the standard laptop screen size. Small methods is something even Robert C Martin emphasizes however he also says that making the reader scroll horizontally to read the code is rude to the reader. So it is a bit contradictory. But think of Toast messages. We don't need to know every parameter in the Toast message all the time. We know the only important thing in a Toast is the message text. One line is more than enough for that even if it "crosses the max code length per line" in Android Studio. To change that, go to **Settings -> Editor -> Code Style -> Hard wrap at -> xxx columns**.
* Prefer `lateinit` variables over `nullable` variables in Kotlin.
* Try to avoid creating too many variables as they allocate memory and can be a potential memory waste. Consequently, try using `lazy initialisation` to only allocate memory on first use.
* Always set explicit types for every field. It is good for Java interoperability.


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


## Useful Facts
* Java primitives are also called as value types and object types are called as reference types. 


## Math of Computers
* Ideas
* Logic
* Counting
* Probability
* Numerical Bases
* Gauss's Trick
* Sets
* Kadane's Algorithm


## Nature of Computers
* Architecture
* Compilers
* Memory Hierarchy


## Nature of Code
* Linguistics
* Variables
* Paradigms


## Nature of Data
* Abstract Data Types
* Common Abstractions
* Structures


## Code Smells (Smells of bad code)
#### Bloaters
* Long Method
* Large Class
* Primitive Obsession
* Long Parameter List
* Data Clumps
#### Object-Orientation Abusers
* Switch Statements
* Temporary Field
* Refused Bequest
* Alternative Classes with Different Interfaces
#### Change Preventers
* Divergent Change
* Shotgun Surgery
* Parallel Inheritance Hierarchies
#### Dispensables
* Comments
* Duplicate Code
* Lazy Class
* Data Class
* Dead Code
* Speculative Generality
#### Couplers
* Feature Envy
* Inappropriate Intimacy
* Message Chains
* Middle Man
* Incomplete Library Class
#### Other Smells


## Refactoring Techniques in Android
#### Composing Methods
* Extract Method 
* Inline Method
* Extract Variable
* Inline Temp
* Replace Temp with Query
* Split Temporary Variable
* Remove Assignments to Parameters
* Replace Method with Method Object
* Substitute Algorithm
#### Moving Features between Objects
* Move Method
* Move Field
* Extract Class
* Inline Class
* Hide Delegate
* Remove Middle Man
* Introduce Foreign Method
* Introduce Local Extension
#### Organizing Data
* Self Encapsulate Field
* Replace Data Value with Object
* Change Value to Reference
* Change Reference to Value
* Replace Array with Object
* Duplicate Observed Data
* Change Unidirectional Association to Bidirectional
* Change Bidirectional Associatino to Unidirectional
* Replace Magic Number with Symbolic Constant
* Encapsulate Field
* Encapsulate Collection
* Replace Type Code with Class
* Replace Type Code with Subclass
* Replace Type Code with State/Strategy
* Replace Subclass with Fields
#### Simplifying Conditional Expressions
* Decompose Conditional
* Consolidate Conditional Expression
* Consolidate Duplicate Conditional Fragments
* Remove Control Flag
* Replace Nested Conditional with Guard Clauses
* Replace Conditional with Polymorphism
* Introduce Null Object
* Introduce Assertion
#### Simplifying Method Calls
* Rename Method
* Add Parameter
* Remove Parameter
* Separate Query from Modifier
* Parameterize Method
* Replace Parameter with Explicit Methods
* Preserve Whole Object
* Replace Parameter with Method Call
* Introduce Parameter Object
* Remove Setting Method
* Hide Method
* Replace Constructor with Factory Method
* Replace Error Code with Exception
* Replace Exception with Test
#### Dealing with Generalisation
* Pull Up Field
* Pull Up Method
* Pull Up Constructor Body
* Push Down Method
* Push Down Field
* Extract Subclass
* Extract Superclass
* Extract Interface
* Collapse Hierarchy
* Form Template Method
* Replace Inheritance with Delegation
* Replace Delegation with Inheritance


## Design Principles
#### Loose coupling
#### Prefer Composition over Inheritance
#### Program to an Interface, not an implementation
#### Encapsulate what varies
#### Encapsulation
#### Abstraction
#### Polymorphism
#### Inheritance


## Design Patterns & their use cases
#### Creational Patterns
* Abstract Factory 
* Builder [Kotlin Link](https://github.com/Singularity-Coder/Android-Examples/tree/master/kotlin/KotlinBuilderPattern1)
* Factory Method 
* Object Pool
* Prototype 
* Singleton 
#### Structural Patterns
* Adapter 
* Bridge 
* Composite 
* Decorator 
* Facade 
* Flyweight 
* Private Class Data
* Proxy 
#### Behavioural Patterns
* Chain of Responsibility 
* Command 
* Interpreter 
* Iterator 
* Mediator 
* Memento 
* Null Object
* Observer 
* State 
* Strategy 
* Template method
* Visitor 


## Android Development Anti-Patterns and their problems
* The Blob
* Continuous Obsolescence
* Lava Flow
* Ambiguous Viewpoint
* Functional Decomposition
* Poltergeists
* Boat Anchor
* Golden Hammer
* Dead End
* Spaghetti Code
* Input Kludge
* Walking through a Minefield
* Cut-And-Paste Programming
* Mushroom Management


## Android Architecture Anti-Patterns
* Autogenerated Stovepipe
* Stovepipe Enterprise
* Jumble
* Stovepipe System
* Cover Your Assets
* Vendor Lock-In
* Wold Ticket
* Architecture By Implication
* Warm Bodies
* Design By Committee
* Swiss Army Knife
* Reinvent The Wheel
* The Grand Old Duke of York


## Project Management Anti-Patterns
* Blowhard Jamboree
* Analysis Paralysis
* Viewgraph Engineering
* Death By Planning
* Fear of Success
* Corncob
* Intellectual Violence
* Irrational Management
* Smoke and Mirrors
* Project Mismanagement
* Throw It over the Wall
* Fire Drill
* The Feud
* E-mail is Dangerous


## SOLID Principles and their use cases
#### Sigle Responsibility
#### Open/Closed
#### Liskov Substitution
#### Interface Segregation
#### Dependency Injection


## Analyzing Time & Space Complexity
* Counting Time
* The Big-O Notation
* Exponentials
* Counting Memory


## Algorithms
* Sorting
* Searching
* Graphs
* Operations Research


## Strategies for Algorithmic Problems
* Iteration
* Recursion
* Brute Force
* Backtracking
* Heuristics
* Divide & Conquer
* Dynamic Programming
* Branch & Bound


## Databases in Android
* Relational Database (SQL): SQLite & Room ORM
* Non-Relational Database (NoSQL): Realm
* Distributed
* Geographical
* Serialization Formats


## Programming Paradigms in Android
#### Reactive Programming 
#### Functional Programming
#### Object-oriented Programming
#### Declarative Programming
#### Imperative Programming
#### Procedural Programming
#### Logic Programming
#### Modular Programming
#### Structured Programming
#### Generic Programming
#### Event-driven Programming
#### Aspect-oriented Programming
#### Prototype based Programming
#### Data-driven Programming
#### Function-level Programming
#### Array Programming
#### Dataflow Programming
#### Role-oriented Programming
#### Agent-oriented Programming
#### Stack oriented Programming


## Architecture Patterns in Android
* MVVM Architecture (Model View ViewModel) [Link](https://github.com/Singularity-Coder/Blog/tree/master/java/MVVMArchitecture)
* MVP Architecture (Model View Presenter) [Link](https://github.com/Singularity-Coder/Blog/tree/master/java/MVPArchitecture)
* MVC Architecture (Model View Controller) [Link](https://github.com/Singularity-Coder/Blog/tree/master/java/MVCArchitecture)
* VIPER Architecture (View Interactor Presenter Entity Router) [Link](https://github.com/Singularity-Coder/Blog/tree/master/java/VIPERArchi1)
* MVI Architecture (Model View Intent)
* Clean Architecture


## About UML (Unified Modeling Language)
#### Basic Principles & Background
* Models, Views, and Diagrams
* Information Systems & IT Systems
* History of UML: Methods & Notations
* Requirement Specification
* UML 2.0
#### Modeling Business Systems
* Business Processes & Business Systems
* One Model-Two Views
* External View
* The Elements of a View
* Use Case Diagrams
* Constructing Use Case Diagrams
* Activity Diagrams
* Constructing Activity Diagrams
* Sequence Diagrams
* Constructing Sequence Diagrams
* High-Level Sequence Diagrams
* Sequence Diagrams for Scenarios of Business Use Cases
* Internal View
* Package Diagram
* Constructing Package Diagrams
* Class Diagram
* Constructing Class Diagrams
* Activity Diagram
#### Modeling IT Systems
* External View
* The User View or "I don't care how it works, as long as it works."
* The Elements of a View
* Use Case Diagram
* Query Events and Mutation Events
* Use Case Sequence Diagram
* Constructing the External View
* Structural View
* Objects and Classes
* Generalization, Specialization, and Inheritance
* Static and Dynamic Business Rules
* Elements of the View
* Class Diagram
* Constructing Class Diagrams
* The Behavioral View
* The Life of an Object
* The Elements of the View
* Statechart Diagram
* Constructing Statechart Diagrams
* Interaction View
* Seeing What Happens Inside the IT System
* Elements of the View
* Communication Diagram
* Sequence Diagram
* Constructing Communication Diagrams
* Constructing Sequence Diagrams
#### Modeling System Integration
* Terminology of System Integration
* Messages in UML
* One Model-Two Views
* Process View
* The Business System Model as Foundation
* Elements of the View
* Activity Diagrams
* Sequence Diagrams
* Constructing Diagrams in the Process View
* The Static View
* Elements of the View
* Class Diagram
* Constructing Class Diagrams
* Transforming Data from the IT System to the Message "passenger list"
* Transformation of UML Messages into Various Standard Formats


## Material Design 


## Android Studio
* View Kotlin Bytecode (View Java Code): Search Everywhere: press Shift twice -> Kotlin Bytecode -> Decompile
* To compare something in another class or maybe even from outside the project, copy the text from the other source and select the code in your project you want to compare with. **Right click -> Compare with Clipboard**.


## Faster Gradle Builds
* Use terminal in Android Studio to perform the build.
```
./gradlew installDebug
```
* In debug mode `minifyEnabled false` and `shrinkResources false`.
```Groovy
debug {
    minifyEnabled false 
    shrinkResources false
}
```


## Common Android Studio Keyboard Shortcuts
Using these shortcuts is also one form of best practice as it reduces the scope of errors while doing things manually especially when refactoring.  

**Refactoring**
* Rename: Shift + F6
* Extract Variables: Ctrl + Alt + V
* Extract Method: Ctrl + Alt + M
* Refactoring Menu (Loads of other refactoring options): Ctrl + Alt + Shift + T
* To move methods up and down: ctrl + shift + up or down
* To move lines up and down: alt + shift + up or down
* Duplicate lines: ctrl + d
* Delete lines: ctrl + x
* Comment lines: ctrl + /
* Format Code: Cmd + Alt + L
* See Format Settings: Cmd + Alt + Shift + L

**Code Appearance**
* Collapse function block: Ctrl + Numpad +
* Expand function block:  Ctrl + Numpad -
* Collapse all function blocks: Ctrl + Shift + Numpad +
* Expand all function blocks: Ctrl + Shift + Numpad -

**Code Suggestions**
* Code suggestion: Ctrl + Space
* Extended Code suggestion, 2 times: Ctrl + Space
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

**Code Completion**
* Replace code completion: press Tab after Ctrl + Space and selection
* Code completion: Ctrl + Shift + Enter
* Postfix code completion: Press . (Period)
* Statement completion like for, if, etc: Ctrl + Shift + Enter
* Wrap/Surround like surround with try/catch/finally etc: Ctrl + Alt + T
* UnWrap say try/catch block: Ctrl + Shift + Delete

**Search**
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

**Code Selections**
* Sequential Multiple Selections: Alt + J
* Sequential Multiple De-Selections: Alt + Shift + J
* Select All Occurrences in the file: Ctrl + Alt + Shift + J
* Expand or shrink code selection: ctrl + w or ctrl + shift + w

**Others**
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


## Useful Plugins
* [Protocol Buffer Editor](https://plugins.jetbrains.com/plugin/14004-protocol-buffer-editor)
* [JSON to Kotlin Class](https://plugins.jetbrains.com/plugin/9960-json-to-kotlin-class-jsontokotlinclass-)
* [Inline blame for Git](https://plugins.jetbrains.com/plugin/7499-gittoolbox)


## Online Tools
* [JSON to Kotlin Class](https://www.json2kotlin.com/)
* [JSON to POJO Class](http://www.jsonschema2pojo.org/)


## Common Vocabulary
* **Data:** The quantities, characters or symbols on which operations are performed by a computer, which may be stored and transmitted in the form of electrical signals and recorded on magnetic, optical or mechanical recording media.
* **Expression:** A function with return value or a variable. They are things that have values. You assign expressions to variables.
* **Statement:** Code with an assignment operator. They are things that have effects. You write statements to assign things to other things.
* **Pascal Case:** KotlinCodeSnippets  
* **Camel Case:** kotlinCodeSnippets  
* **Snake Case:** kotlin_code_snippets 
* **Main Function:** Entry point in our app.  
* **Compiler:** Translates high level language to low level language that machine can understand. Also checks for syntax errors.  
* **Run Kotlin Program:** Converts Kotlin code to Java byte code to run it on JVM. JVM further converts the Java byte code into machine code that <a href=""></a> platform like Mac or Windows can understand.  
* **Statically Typed Programming Language:** Type of a variable is known at compile-time. So once variable is declared with a type, it cannot ever be assigned to a variable of different type and doing so will result in a type error at compile-time. So you CANNOT build the app without fixing the bugs. Fast & Efficient. Ex: Kotlin, Java, C, C++, etc.  
* **Dynamically Typed Programming Language:** Type of a variable is known at run-time. Variables are bound to objects at run-time. You can assign a variable with a different an Integer type even if it was initially assigned String type. So you CAN build the app without fixing the bugs. Slow & less optimized but has its own advantages like dynamic dispatch, late binding, down-casting, reflection. Ex: JavaScript, Objective-C, PHP, Python, Ruby, Lisp, etc.  
* **Type Checking:** The process of verifying and enforcing the constraints of types.  
* **Static check:** Type check happening at compile-time.  
* **Dynamic check:** Type check happening at run-time.  
* **Strongly-typed language:** Variables are bound to specific data types, and will result in type errors if types do not match up as expected in the expression - regardless of when type checking occurs. Ex: Java, Python, etc.  
* **Weakly-typed language:** Variables are not bound to a specific data type; they still have a type, but type safety constraints are lower compared to strongly-typed languages. Ex: PHP, C, C++, etc.  
* **Top-Level function:** A function that is not enclosed in a class.  
* **Double Type:** Double-precision floating-point. More exact than Float numbers.
* **Number System:** A way of representing numbers. The value of any digit in a number can be known if we know:
    1. The Digit 
    2. Its position in the number
    3. The base/radix of the number system
* **Radix/Base:** Available numbers in a number system (or) numbers used in a system (or) number of digits used in a positional number system. Ex: base-10 numbering system or decimal numbers (0,1,2,3,4,5,6,7,8, and 9) is most common. So the radix/base for Decimal numbers is 10. Computers deal with base-2 or binary which only deals with numbers 0 and 1. So for binary numbers the radix/base is 2.
* **Types of base or Types of Number Systems:** 
    1. Base-2 (Binary): Uses only 0, 1
    2. Base-8 (Octal): Uses only 0, 1, 2, 3, 4, 5, 6, 7
    3. Base-10 (Decimal): Uses only 0, 1, 2, 3, 4, 5, 6, 7, 8, 9
    4. Base-16 (Hexadecimal): Uses only 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, A, B, C, D, E, F
* **Signed Integer:** Whole range of integers negative, zero and positive integers. Ex: -∞...-3, -2, -1, 0, 1, 2, 3...∞
* **Unsigned Integer:** Non-negative integers including 0. Ex: 0, 1, 2, 3...∞
* **ASCII:** American Standard Code for Information Interchange.
* **ASCII Characters:** Start from 0 to 127. Consist of commonly used characters like upper and lower case english letters, numbers, etc. Each character is exactly 8 bits or 1 byte.
* **Extended ASCII Characters:** Start from 128 to 256. Extended set of ASCII characters that include famous currency symbols, etc.
* **Unicode Characters:** Emojis, Math symbols, Spoken Language characters, etc.
* **ISO:** International Standards Organization.
* **1 bit:** A bit is a binary digit. It can hold either a 0 value or 1 value.
* **1 byte:** 8 bits form a byte. It can store a single ASCII character like 'C'. Since each bit can hold either a 0 or 1, the character 'C' will have 01000011 values where each bit out of the 8 holds a 0 or 1.
* **1 kilobyte (KB):** 1024 bytes form a kilobyte.
* **1 megabyte (MB):** 1024 kilobytes form a megabyte.
* **1 gigabyte (GB):** 1024 megabytes form a gigabyte.
* **1 terabyte (TB):** 1024 gigabytes form a terabyte.
* **1 petabyte (PB):** 1024 terabytes form a petabyte.
* **1 exabyte (EB):** 1024 petabytes form a exabyte.
* **1 zettabyte (ZB):** 1024 exabytes form a zettabyte.
* **1 yottabyte (YB):** 1024 zettabytes form a yottabyte.
* **1 Mbps:** Lowercase 'b' means 1 megabits per second. Broadband connection terminology.
* **1 MBps:** Uppercase 'B' means 1 megabytes per second. Broadband connection terminology.
* **Operator:** A symbol that tells the compiler or interpreter to perform specific mathematical, relational or logical operation and produce a result. Ex: +, -, &&, ||, etc.
* **Operand:** The element(s) we are operating on.
* **Unary Operator:** Operate on a single operand or element. Ex: a++
* **Binary Operator:** Operate on 2 operands or elements. Ex: a + b
* **Ternary Operator:** Operate on 3 operands or elements. Ex: a ? b : c
* **Infix Operator:** Operator is inbetween 2 operands. Like the + in a + b
* **Prefix Operator:** Operator to the left of an operand. Like ++a
* **Postfix Operator:** Operator to the right of an operand. Like a++
* **Operator Overloading or ad hoc polymorphism:** A specific case of polymorphism, where different operators have different implementations depending on their arguments. Ex: + can behave like -, etc.
* **Module:** A module encapsulates complex functionality away from user, provides a well defined interface for a user and it should have a plug-and-play setup.
* **Abstract Data Type:** A set of rules on how something should behave and operate. Ex: A linked list can implement the stack abstract data type (ADT).
* **Data Structure:** A particular way of organizing data. Its a concrete implementation of an Abstract Data Type.
* **Abstract in Programming:** Set of rules that must be followed. Like an interface. No implementation. Like an idea that is not tangible until implemented.
* **Client/Server Model:** Client makes a request. Server responds to that request.
* **Proxy:** Software that makes a request on behalf of Client.
* **Application Programming Interface (API):** Mediator software that allows 2 apps to communicate. (OR) A set of publicly exposed methods that allow access to the data layer.
* **Software Development Kit (SDK):** Collection of APIs. Ex: To build Android Apps, you need an Android SDK.
* **Cache:** A quicker way of accessing something. Ex: Kitchen Pantry is a cache for the grocery store. Programs currently running on a computer are cache for all the big files saved in the hard drive. Files saved on hard drive of a computer is a cache for all the things saved on the cloud.
* **Compilation:** Converting human readable code like Java, Kotlin, Python into machine readable code.
* **Open Device:** MacBook Pro. General purpose. Its the system.
* **Embedded Device:** iPod. Specific purpose. Its a subset of the system.
* **Deeply Embedded Device:** Nest thermostat. Super specific purpose. Its a subset of another subset.
* **Middleware:** Software that acts as a bridge between an operating system or database and applications, especially on a network.
* **Serialization:** Converting object to byte stream.
* **Deserialization:** Converting byte stream to object.
* **Parse:** Process of findng the meaning of a sentence by breaking it into parts that are syntactically and grammatically correct.
* **Traversal:** Visiting each element in a data structure exactly once.
* **Fibonacci Numbers:** Each number is the sum of the 2 preceding ones, starting from 0 and 1. Ex: 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144... where 8 is a sum of 5 and 3, its preceding 2 numbers.
* **Palindrome:** A word, number, phrase, or other sequence of characters which reads the same backward as forward, such as madam or racecar. There are also numeric palindromes, including date/time stamps using short digits 11/11/11 11:11 and long digits 02/02/2020. Refer Wikipedia.
* **Factorial:** A function that multiplies a number by every number below it. Ex: 5!= 5 * 4 * 3 * 2 * 1 = 120. The function is used, among other things, to find the number of ways “n” objects can be arranged.
* **Armstrong Number:** A number that is equal to the sum of cubes of its digits. Ex: 0, 1, 153, 370, 371, 407, etc. If we take 153 -> 1^3 = 1, 5^3 = 125, 3^3 = 27. So 1 + 125 + 27 = 153. 
* **Natural Numbers (N):** Also called as Positive Integers, Counting Numbers. 1, 2, 3...∞
* **Whole Numbers (W) or Non-Negative Integers (Z Star):** Set of Natural Numbers, plus Zero. 0, 1, 2, 3...∞
* **Integers (Z):** Set of Negative numbers and Whole Numbers. -∞...-3, -2, -1, 0, 1, 2, 3...∞   
* **Rational Numbers (Q):** Can be expressed as ratio of two integers. All the fractions where the top and bottom numbers are integers. The denominator cannot be 0, but the numerator can be. 1/2, 3/4, 7/2, ⁻4/3, 4/1.
* **Unit Fractions:** If numerator of a fraction is one. 1/2, 1/3, 1/4, 1/5.
* **Decimal Fractions:** Decimal numbers (such as 0.3,  0.32, -2.7) can be called decimal fractions, because they can be written in a fractional form (e.g., 3/10, 32/100, -27/10).
* **Irrational Numbers (Q' or Q Prime):**
* **Real Numbers (R):** 
* **Complex Numbers (C):**
* **Prime Numbers:**
* **Composite Numbers:**
* **Dynamic Memory Allocation:**
* **Preprocessor:**
* **Matrix:**
* **2D Matrix:**
* **3D Matrix:**
* **Numeric Characters:** 
* **Alphanumeric Characters:** Subsets of ASCII characters. Depends on the implementation and requirements. Some allow all ASCII characters while others use a subset of them. Generally Alphabets, Numbers and few Special characters. 0 to 9, A to Z(both uppercase and lowercase), @ # * & etc.
* **Enumeration:** List things one by one. To count. A synonym to the verb "list". It emphasizes the fact that things are being specifically identified and listed one at a time. In mathematics and computer science, enumeration is a complete, ordered listing of all the items in a collection. Used to refer to a listing of all of the elements of a set. 
* **Protobuf:** Protocol buffers are Google's language-neutral(any programming language), platform-neutral(any OS like Mac or Windows), extensible mechanism for serializing structured data.
* **Encoding:** Converting or Transforming (information or an instruction) into another form.
* **Decoding:** Converting or Translating (coded or encoded info) into an understandable form.
* **Cipher:** Secret or disguised way of writing; a code.
* **Callback:** You tell it to do something and it will let you know when its done.
* **DSL:** Domain-Specific Language.
* **Main Memory:** Random Access Memory (RAM). 
* **Data Structure:** A way to arrange data in main memory (RAM) for efficient usage. Ex: Arrays, LinkedLists, Stacks, etc.
* **Algorithm:** Sequence of steps that solves a problem.
* **Database:** Collections of data stored in permanent storage that can be added, fetched, updated and deleted.
* **Data Warehousing:** 
* **Big Data:** 
* **Binary to Decimal Conversion:** Binary numeral system: 1 is ON and 0 is OFF. 0s don't matter. 1s only matter so add the 2^placeValue values. So 1001011 is 75 in binary. Read from right to left.
```
Binary Number:             1     0     0     1     0     1     1
Place Value:               6     5     4     3     2     1     0
2 power Place Value:       2^6   2^5   2^4   2^3   2^2   2^1   2^0
Solve the above:           64    32    16    8     4     2     1

Add the 1 bit values:      64 + 8 + 2 + 1 = 75
```
* **Hexadecimal to Decimal Conversion:**
* **Octal to Decimal Conversion:**
* **Test Driven Development (TDD):** Write the test first before building the actual feature.


## Temporary Solutions
* If you are using "Tabs" instead of "Windows" in Mac OS Big Sur, then Android Studio pop-ups and windows streach and get stuck. Open Terminal and use this command: [Link](https://stackoverflow.com/questions/64827350/android-studio-4-1-1-macos-full-screen-error)
```
defaults write com.google.android.studio AppleWindowTabbingMode manual
```


## References
1. https://sourcemaking.com/
2. [D8, R8 and enums - Kotlin Vocabulary](https://www.youtube.com/watch?v=lTo03M2HzFY)
3. [Clean Code - Uncle Bob / Lesson 1](https://www.youtube.com/watch?v=7EmboKQH8lM)
4. https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book.html
5. https://kotlinlang.org/docs/home.html
6. https://www.amazon.in/Functional-Programming-Java-Venkat-Subramaniam/dp/1937785467
7. https://android.jlelse.eu/magic-lies-here-statically-typed-vs-dynamically-typed-languages-d151c7f95e2b
8. https://byjus.com/maths/number-system
9. http://www.asciitable.com
10. https://kb.iu.edu/d/ackw
11. https://www.eso.org/~ndelmott/ascii.html
12. [Programming Words You Should Know](https://www.youtube.com/watch?v=es4DYTFdqlg)
13. [10 Useful Technical Terms](https://www.youtube.com/watch?v=HtEzuAqWmoE)
14. https://www.sciencedirect.com/topics/computer-science/alphanumeric-character
15. [How to Kotlin - from the lead Kotlin language designer (Google I/O '18)](https://www.youtube.com/watch?v=6P20npkvcb8)
16. [KotlinConf 2018 - Creating Internal DSLs in Kotlin by Venkat Subramaniam](https://www.youtube.com/watch?v=JzTeAM8N1-o)
17. [Computer Basics 4: Decoding a Binary Number](https://www.youtube.com/watch?v=xXLj5MbrI44&list=PLWKjhJtqVAbmfoj2Th9fvxhHIeqFO7wOy&index=4)

