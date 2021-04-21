<p align="center">
<a href="https://img.shields.io/badge/License-MIT-green"><img src="https://img.shields.io/badge/License-MIT-green" alt="MIT License"></a>
</p>

### Introduction
##### What is Fluent?
It’s a Flutter package that helps you handling your project translations via code generation techniques.
##### Why Fluent?
Fluent came to get rid of the boilerplate code that the developers used to write to build translations into their apps. It wasn't coding at all more than time wasting and totally error prone process. So why not let the machine does it?!

### Installation

```yaml
dependencies:
  fluent: [latest-version]

dev_dependencies:
  fluent_generator: [latest-version]
  build_runner:
```

### Setup and Usage

---

Create a placeholder class and annotate it with `@Fluent` which takes: 
* A list of `Translatable` objects (in case you'll provide your translations manually)
* A CSV file path (in case your translations will be provided `fromCSV`)
* A list of locales for the supported languages as a required argument.
* A `fallback language` as a required argument.

**Note**: In case of loading translations from a CSV file. Please consider to declare it first in the `assets` section of your project `pubspec.yaml` file.

```dart
/// In case loading translations from a CSV file

@Fluent(
  fromCSV: 'assets/csv_test.csv',
  fallbackLanguage: 'ar',
  supportedLanguages: ['ar', 'en', 'fr']
)
class Translations {}

/// In case manually provide the traslations into [Translatable] objects
@Fluent(
  fallbackLanguage: 'ar',
  supportedLanguages: [
    'ar',
    'en',
    'fr',
  ],
  translations: [
    Translatable(
      arabic: 'مرحبا',
      english: 'Hello',
      french: 'Salut',
      fieldName: 'helloText',
    )
  ],
)
class Translations {}
```
**Note**: In case of providing both CSV file & a list of `Translatable` objects. `Fluent` is designed to overwrite the content of the `Translatable`s with the CSV file content. It will not add content from both resources.

Lastly, You need to launch `build_runner` to start translations code generation using command `flutter pub run build_runner build` for a single time or using command `flutter pub run build_runner watch` to keep tracking the changes in the file system and automatically update the translations.


### CSV file content structure
For your CSV file be parsed correctly. You need to struct its content in a unified way according to the following rules
* Starting from cell A1, add all your locales in the first cell of each column (e.g.: ar, en, fr,...etc)
* After the last locale cell directly, add additional cell with text "fieldName"
* List all your translations of a certain language in the column related to its locale. (e.g.: Arabic translations in `ar` column)
* list the unique correspondant field name to each translation in `fieldName` column. If any field name duplicated in this column that will lead to errors in code generation process.
* If you need to add extra information in your CSV file, Skip one column empty next to the right of `fieldName` column. Any type of data will be added next to the empty column will be ignored and won't be parsed in any form into code.
* Make sure that no empty columns added between locales columns or before `fieldName` column
* Make sure that `fieldName` spelling and format _(Camelcase)_ is correct.

For full CSV file example, kindly download this [CSV file example](https://drive.google.com/file/d/1UGsoUkxWiiccSOsuIkedinJanOs5OpM2/view?usp=sharing)  