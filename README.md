# Internationalizing Flutter apps

## Setting up an internation­alized app: the Flutter_localizations package

To use `flutter_localizations`, add the package as a dependency to your `pubspec.yaml` file, as well as the `intl` package:
```
flutter pub add flutter_localizations --sdk=flutter
flutter pub add intl:any
```
This creates a `pubspec.yaml` file with the following entries:
```
dependencies:
  flutter:
    sdk: flutter
  flutter_localizations:
    sdk: flutter
  intl: any
```
Then import the `flutter_localizations` library and specify `localizationsDelegates` and `supportedLocales` for your `MaterialApp` or `CupertinoApp`:
```
import 'package:flutter_localizations/flutter_localizations.dart';
```
```
return const MaterialApp(
  title: 'Localizations Sample App',
  localizationsDelegates: [
    GlobalMaterialLocalizations.delegate,
    GlobalWidgetsLocalizations.delegate,
    GlobalCupertinoLocalizations.delegate,
  ],
  supportedLocales: [
    Locale('en'), // English
    Locale('ru'), // Russian
  ],
  home: MyHomePage(),
);
```
## Adding your own localized messages

Open the `pubspec.yaml` file and enable the generate flag. This flag is found in the `flutter` section in the pubspec file.
```
# The following section is specific to Flutter.
flutter:
  generate: true # Add this line
```
Add a new yaml file to the root directory of the Flutter project. Name this file `l10n.yaml` and include following content:
```
arb-dir: lib/src/core/localization/translations
template-arb-file: intl_en.arb
output-localization-file: app_localizations.dart
```
This file configures the localization tool. In this example, you’ve done the following:

- Put the App Resource Bundle (.arb) input files in `${FLUTTER_PROJECT}/lib/src/core/localization/translations`. The `.arb` provide localization resources for your app.
- Set the English template as `intl_en.arb`.
- Told Flutter to generate localizations in the `app_localizations.dart` file.

In `${FLUTTER_PROJECT}/lib/src/core/localization/translations`, add the `intl_en.arb` template file. For example:
```
{
    "hello": "Hello",
    "@hello": {
        "description": "The conventional newborn programmer greeting"
    }
}
```
Add another bundle file called `intl_ru.arb` in the same directory. In this file, add the Russian translation of the same message.
```
{
    "hello": "Привет"
}
```
Now, run `flutter pub get` or `flutter run` and codegen takes place automatically. You should find generated files in `${FLUTTER_PROJECT}/.dart_tool/flutter_gen/gen_l10n`. Alternatively, you can also run `flutter gen-l10n` to generate the same files without running the app.

Add the import statement on `app_localizations.dart` and `AppLocalizations.delegate` in your call to the constructor for `MaterialApp`:
```
import 'package:flutter_gen/gen_l10n/app_localizations.dart';
```
```
return const MaterialApp(
  title: 'Localizations Sample App',
  localizationsDelegates: [
    AppLocalizations.delegate, // Add this line
    GlobalMaterialLocalizations.delegate,
    GlobalWidgetsLocalizations.delegate,
    GlobalCupertinoLocalizations.delegate,
  ],
  supportedLocales: [
    Locale('en'), // English
    Locale('ru'), // Russian
  ],
  home: MyHomePage(),
);
```
The AppLocalizations class also provides auto-generated localizationsDelegates and supportedLocales lists. You can use these instead of providing them manually.
```
const MaterialApp(
  title: 'Localizations Sample App',
  localizationsDelegates: AppLocalizations.localizationsDelegates,
  supportedLocales: AppLocalizations.supportedLocales,
);
```
Once the Material app has started, you can use AppLocalizations anywhere in your app:
```
appBar: AppBar(
  // The [AppBar] title text should update its message
  // according to the system locale of the target platform.
  // Switching between English and Russian locales should
  // cause this text to update.
  title: Text(AppLocalizations.of(context)!.hello),
),
```
> ❗ **Note** The Material app has to actually be started to initialize `AppLocalizations`. If the app hasn’t yet started, `AppLocalizations.of(context)!.hello` causes a null exception.

This code generates a `Text` widget that displays `"Hello"` if the target device’s locale is set to English, and `"Привет"` if the target device’s locale is set to Russian. In the `arb` files, the key of each entry is used as the method name of the getter, while the value of that entry contains the localized message.

To localize your device app description, pass the localized string to `MaterialApp.onGenerateTitle`:
```
return MaterialApp(
  onGenerateTitle: (context) => AppLocalizations.of(context)!.hello,
```

## Maintaining arb files with Flutter Intl extension

This VS Code extension will help to create a binding between translations from `.arb` files and Flutter app. It generates boilerplate code for official Dart [Intl](https://pub.dev/packages/intl) library and adds auto-complete for keys in Dart code.

This plugin is also available for [IntelliJ / Android Studio](https://plugins.jetbrains.com/plugin/13666-flutter-intl).

[Install from the Visual Studio Code Marketplace](https://marketplace.visualstudio.com/items?itemName=localizely.flutter-intl) or by [searching within VS Code](https://code.visualstudio.com/docs/editor/extension-marketplace#_search-for-an-extension).

Open the `pubspec.yaml` file and include the following Flutter Intl configuration:
```
flutter_intl:
  enabled: true
  class_name: GeneratedLocalization
  main_locale: en
  arb_dir: lib/src/core/localization/translations
  output_dir: lib/src/core/localization/generated
```
> ❗ **Note** For `arb_dir` specify already existing path to `.arb` files, defined at the stage of creating of `l10n.yaml` file.

## Editor actions

`Extract to ARB files`

Extract string to `.arb` files using [code actions](https://code.visualstudio.com/docs/editor/refactoring#_code-actions-quick-fixes-and-refactorings). The dialog will ask you to enter the string key, which will be extracted to `.arb` files together with the selected content.

> ❗ **Note** After extracting the string to `.arb` file, you should run `flutter pub get` or `flutter run` again.

## Links

[Internationalizing Flutter apps](https://docs.flutter.dev/ui/accessibility-and-internationalization/internationalization)

[Flutter Intl](https://marketplace.visualstudio.com/items?itemName=localizely.flutter-intl)
