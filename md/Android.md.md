# Android

## Android Studio windows scope selector?
In android studio, there are multiple scope selector, "Project, Android, Project File, ..etc" 

They all refer to the same folder, but showing a different lens/view to developer for specific jobs. 

If you need to code on kotlin, better using Android View. If you need to check the file structure, better using Project View. 

## Which file is blueprint for OS in Android APP?
The system will read the `AndroidManifest.xml` first, it's the root of app's package, without this file, the OS has no idea of what app's contains, and how to run. 

1. name of the app
2. which activities exist
3. the hardware component permissions
4. result of users's screen tap

It's like your APP identity card. Configure how Android launch your app. 

## Why Android development needs gradle?
gradle is a build automation tool. It's job is to take your dependencies + source code + configuration, to produce a runnable artifact. 

Same principle/steps while running android application. 

## Which folder is the Global Entry Point?
`AssetManagerApplication.kt`

## Which folder contains all the layout setting?
`res/layout`

## Which folder decide app's logic?
`kotlin+java/com.example.assetmanager`

## Why package name com.example?
It's purely a path on your disk. A decade ago, Java designer need a way to create a unique package to separate from people coordinating. They use the domain name they own, and reverse it. 

The name itself is a placeholder domain, when you push your real app on Google Play Store, you should change it to your domain you actually control. 

## Why XML file become canvas in res directory?
ALL Android XML is text. Android Studio just gives you smart previews for that XML file. 

The right-side canvas is just a visualization of the XML code.

Those XML need to work with Kotlin in order to present UI automation. XML defines the static skeleton. Kotlin handles dynamic, behavioral, and data logic. 

## Three layer in App Architecture?
1. UI layer
2. Domain layer
3. Data layer

In most examples, in kotlin + Java section,
UI Layer           →    presentation/
Domain Layer       →    domain/
Data Layer         →    data/

The core of Android App dev, only 3 things
1. shows stuff to user (UI part)
2. Decides what to do with input (Business Rules)
3. Get/saves data (network, database, files)

## What's MVVM architecture?
Google itself recommends MVVM(Model-View-ViewModel) as the official android architecture. It's the standard pattern for organizing a single screen. 

Those are 3 roles/layers MVVM
1. 'ViewModel' means exposes those data streams which are relevant to the View. (fetching dat, ...etc)
2. 'Model' means the abstraction of data sources
3. 'View' means to inform the ViewModel about the user's action

In the "presentation" folder, you see each feature folder will end with "View" or "ViewModel", those folders are grouped by features. 

Every screen        = built using the MVVM pattern
MVVM pattern         = View + ViewModel + UiState (+ helpers as needed)
The pattern itself  = lives nowhere, applied everywhere

## What is 'di' and 'utils' folder used for?
You can deprioritize those two folders in default structure
di/ — Dependency Injection (Hilt)
utils/ — Generic Helpers

## Kotlin compare to Java?
Kotlin runs on the JVM, the same runtimes as Java, the Kotlin source code (kt code) still compiled down to JVM .class file, so they both identical in physical level. 

But Kotlin adds a few thing java doesn't have. 

## What's Android Desugaring?
Core library desugaring is an Android build process that enables developers to use modern Java APIs (such as java.time, java.util.stream, and java.util.Optional) on older Android devices that do not natively support them. 

## Difference between kt file and xml file?
`.kt` is a programming language. (little differ from java)

`.xml` is markup language, just a static blueprint. 

## How 3 files work on one section in android?
In MVVM architecture, there're 2-3 fils to handle:
1. rendering visuals
2. holding the current data state
3. handling business logic

The files
1. The View (IndexFragment.kt), pure display which is connectes to the XML layout, display when data in and out. 
2. The ViewModel(IndexViewModel.kt), the bridge between you presenting UI, and backend database
3. The State (AssetUiState.kt),defining the boundaries and rules of screen

## What's Android DOM
the android document object model(DOM), it's an API use, to parse XML by loading entire environment into RAM, and create a tree structure. 

## How search list show ALL-STOCK popup?

SQLite (asset_manager.db)
         ↓  [DAO reads rows]
StockEntity / FundEntity / etc.
         ↓  [Repository converts to domain models]
Stock / Fund / Bond / Metal / Deposit / CustomAsset
         ↓  [UseCase aggregates all 6 types]
AssetHoldings (one combined object)
         ↓  [ViewModel holds in StateFlow]
IndexViewModel._holdings
         ↓  [Fragment observes/reads]
IndexFragment ← popup shows the list



IndexViewModel.kt
       ↓ calls
GetAssetsWithRealTimePriceUseCase.kt   ← executeFromCache()
       ↓ calls
AssetRepositoryImpl.kt                 ← getStockHoldings()
       ↓ calls
LocalAssetDataSource.kt                ← getStocks()
       ↓ calls
AssetDao.kt                            ← @Query SELECT * FROM stocks
       ↓ hits
SQLite (asset_manager.db)

## 