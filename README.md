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
flutter:
  generate: true
```
Add a new yaml file to the root directory of the Flutter project. Name this file `l10n.yaml` and include following content:
```
arb-dir: lib/src/core/localization/translations
template-arb-file: intl_en.arb
output-localization-file: app_localizations.dart
```
This file configures the localization tool. In this example, you’ve done the following:

- Put the App Resource Bundle (.arb) input files in `${FLUTTER_PROJECT}/lib/src/core/localization/translations`. The `.arb` provide localization resources for your app.
- Set the English template as `app_en.arb`.
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
