# Quick notes from Google IO
Quick summary from every session I watched.
I try to cover just the new stuff not the recapitulation parts of every session
and I cover mostly technical bits, not so much the "philosophical" parts

[All sessions from Google I/O](https://www.youtube.com/playlist?list=PLOU2XLYxmsIKC8eODk_RNCWv3fBcLvMMy)

[All android sessions](https://www.youtube.com/playlist?list=PLWz5rJ2EKKc-odHd6XEaf7ykfsosYyCKp)

#### [What’s new in android?](https://www.youtube.com/watch?v=1N9KveJ-FU8&index=1&list=PLWz5rJ2EKKc-odHd6XEaf7ykfsosYyCKp)
Just the basic summary of everything new in android

#### [What’s new in firebase?](https://www.youtube.com/watch?v=m7a26ymUu2U&list=PLOU2XLYxmsIKC8eODk_RNCWv3fBcLvMMy)
- New view for firebase analytics TODO

#### [New Release & Device Targeting Tools](https://www.youtube.com/watch?v=peCWuCSIv7U)
- New device selection screen. More filters, more devices everything is just awesome
- Google can handle signing and manage your keys for you now
- TODO

#### [Architecture Components - Introduction](https://www.youtube.com/watch?v=FrteWKKVyzI&list=PLOU2XLYxmsIKC8eODk_RNCWv3fBcLvMMy)
Opening session for other three Architecture sessions. Basic overview of new first party components and reasoning behind them.
- `Lifecycle` components can now be notified from `LifecycleOwner` and perform their lifecycle sensitive actions
- `LiveData<T>` lifecycle aware observable
- `ViewModel` component that survives configuration change
- `Room` sqlite library


#### [Architecture Components - Solving the Lifecycle Problem](https://www.youtube.com/watch?v=bEKNi1JOrNs)
- `Lifecycle` as first class citizen, answers the question what is the current state to it's observers
- `LifecycleObserver` component that can observe `Lifecycle` states using annotations
- `LifecycleOwner` interface that can return `Lifecycle`,
currently implemented by `LifecycleActivity` and `LifecycleFragment` but should be implemented by standard versions from suport lib in the future
- `ProcessLifecycleOwner` composite lifecycle of all activities
- `LiveData<T>` lifecycle aware observable with automatic subscription management.
when you call `LiveData<T>.observe` you pass in `LifecycleOwner`. It also gives it's
subscribers last available value on re-subscription
- Nice quick example how to implement your own `LiveData<Location>` with
`LocationManger` inside.
- `MutableLiveData<>` TODO research but it's more like a subject, you can
set data on it from the outside
- In depth explanation how fragments handle `LiveData<T>` so you can be sure
 there will be no Fragment transaction exception (state loss)
 - They admit it is RxJava because it promotes reactive programing but
 it's not because its lifecycle-aware and it's easier
 - `LiveDataReactiveStreams` they provide `fromPublisher` and `toPublisher`
 to translate `LiveData` and `RxJava` back and forth
 - `LiveData` is holder not a stream. New observer will receive
 last value of `LiveData`
 - `LiveData` is only on main thread
 - `ViewModel` associated with fragment/activity and it's retained across
 configChanges. It is destroyed only when activity is destroyed
 - `ViewModel` don access views, activity context and don hold resources (drawables etc)
 - Inner fragment communication via `ViewModel`. You have one `ViewModel`
 for all fragments and you get it by `ViewModelProviders.of(getActivity())` so
 you receive activity's `ViewModel` not fragment's.

#### [Architecture Components - Persistence and Offline](https://www.youtube.com/watch?v=MfHsPGQ6bgE)
- `@Query` annotation containing whole query in it's pure form
- You create the whole interface `@Dao` with annotated methods similar to retrofit
- `@Dao` maps queries to `@Entity` annotated classes
- `@Database` annotated abstract class extends `RoomDatabase`
- Build database with builder similar to retrofit
- Room provides `@Insert`, `@Delete`, `@Update` annotations too
- Multiple bind parameters
- Room compile time checks for many things and that's awesome
- You can just create simple pojo for mapping custom return types from queries
when you select just specific things in in SELECT statement and not the whole `@Entity`
- `TypeAdapters` to help Room convert data form sql to java and vice versa
you then need to add `@TypeConverters` annotation
- TypeAdapters doesn't work very well for nested objects in `@Entity` so you they
provide `@Embedded` annotation for that. You need to yous `prefix` attribute
on `@Embedded` annotation if you want more than one embedded objects of the same type
in one entity
- You can return `LiveData` or RxJava2 `Flowable` from queries to get fresh data
every time something updates
- Pojos For Relations = in room you cannot have entity in entity to represent joint sql tables
entities contains just foreign keys like real table. If you want to query
data and select rows from different tables you need to create new pojo to represent
the results, so no lazy loading stuff
- You define foreignKeys as attribute in `@Entity` annotation and the same for indexes
- For testing purpouses you can create `inMemoryDatabaseBuilder`
- Daos are just interfaces so it's easy to mock when you need to test
dependent components like `ViewModel`
- Migrations: you can `.addMigrations()` in you database builder
- Room provides testing migration `android.arch.presistence.room.testing:1.0`

#### [Whats New in Android Development Tools](https://www.youtube.com/watch?v=Hx_rwS1NTiI)
- Android Studio 3.0
###### Editor
- Parameter hints
- Kotlin support
- Bitecode view -> decompile button and you can see javacode generate by kotlin
- All lint check works on kotlin now
- `ConstraintLayout` 1.1 with barriers and everything is now supported now in
layout editor
- New sample resource type contains lorem ipsum texts and other stuff
- You can for example bind text from your resource json file to tools: attributes
to show example editor data from your real data
- You can create sample data for `RecyclerView` that looks like the real app
- Support for O downloadable fonts and new Icons directly from editor
- Device explorer for browsing files
- Support for Instant Apps and refactoring your existing app to modules
- You can pull activities to it's own modules
- Improved APK analyzer, you can look at the bitecode now
- APK debuging - demo didn't work
- Profiler for CPU, Memory and network like before but more powerful now.
- Network profile - now looks looks like stetho in chrome dev tool and supports
all networking libs you need
- CPU profiler - start collecting samples and then timeline shows you more.
- Memory profiler - start collecting alocations and then you can see heap dump
with all the objects in it with preview of bitmap etc and where the objects came from
###### Build
- Google maven repo: `http://maven.google.com` with all support libs etc
- Incremental tasks now support everything except annotation processors
- Gradle have build cache now
- More Java 8 features support
- Better dependency management `flavorSelection`
- Instant App explanation of features and feature dependencies
###### Test
- Google play store added to emulator
- OpenGL ES 3.0 in emulator
- Proxy support in emulator
- Emulator rotary input (wear)
- Improved Layout Inspector
###### Optimize
- WebP support, just click on any png file and convert




#### [Whats New in Android Support Library](https://www.youtube.com/watch?v=V6-roIeNUY0)
- Move support to minimal API Level 14 so Gingerbread and Honeycomb are dead
- Many deprecated old methods related to old API and method limit dropped significantly
- Google maven repository
- Fonts are new resource type (fonts folder) and declare font to use on TextView in xml
- Downloadable fonts, entire Google fonts catalogue
- Everything you need to download some font you can declare in xml
- Font picker in android studio under font -> "more fonts" and you can select
anything from google fonts directly from AS and it will handle all the import
xml declarations and everything for you
- Emoji compatibility lib
- TextView autosizing
- Physic-based animations Velocity not duration. Spring and Fling Animations for now
- VectorDrawable now supports fill-rule from svg back to API 14+
- AnimatedVector, DrawableCompat Path morphing & interpolation
- Backport for pathInterpolator
- Wearable support lib integrated to main lib so many improvements on wear ui elements
- Leanback lib some new prefabricated fragments for commonly used screens
- `PreferenceDataStore` allows customization of preference storage mechanism in
Preference and Preference manager. So you can now set new "Backend" for preference fragment/activity.
- `executePendingTransaction()` and `commitNow`, and similar transaction calls
are no longer allowed during FragmentManager state changes
- `FrameMetricsAggregator`
- `ActionBarDrawerToggle` finally it is simple to disable toggle animation

#### [Background Check and Other Insights into the Android Operating System Framework](https://www.youtube.com/watch?v=hbLAzwhBjFE&list=PLOU2XLYxmsIKC8eODk_RNCWv3fBcLvMMy)
TODO

#### [Whats Possible with Cloud Functions for Firebase](https://www.youtube.com/watch?v=G-MBeEW92v4)
No new material just a nice complex example of cloud function TODO

#### [Introduction to Kotlin](https://www.youtube.com/watch?v=X1RVYt2QKQE&list=PLWz5rJ2EKKc-odHd6XEaf7ykfsosYyCKp)
- Data classes are awesome, they generate
- Strong type infference
- Equals functionality
- Interop with java
- String interpolation
- Default params
- Naming params
- "I love when people crap"
- When statement
- Extension functions on `BigDecimal` example
- Infix extension functions for functions with one param
- Extension property on Int to create `BigDecimal` literal
- Overloading operators
- Null safety
- High order functions
- When last parameter is an function you can have it outside of parameter brackets and it looks like currying or some DSL
- Extension high order functions on generic collections like `map`, `filter` or `sort`
- Data class destruction
- Algebraic data type using sealed classes
- Smart cast example in when statement
- Sequences, lazy evaluation `asSequence()`
- Kotlin can now compile to javascript
- Kotlin native in preview version
- Coroutines for writing asynchronous code the way you write synchronous code


