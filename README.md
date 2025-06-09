# Flutter Design Extension

A powerful Flutter package that provides a comprehensive design system implementation with support for theming, localization, and reusable components. Built to help you create consistent, beautiful, and maintainable Flutter applications.

## Features

- 🎨 **Design Tokens**: Centralized design tokens for colors, typography, and icons
- 🌍 **Built-in Localization**: Easy-to-use localization system with JSON support
- 🎯 **Theme Extensions**: Custom theme extensions for consistent styling
- 📱 **Responsive Components**: Pre-built components optimized for different screen sizes
- 🎭 **Dark Mode Support**: Seamless light and dark mode switching
- 🏢 **Multi-brand Support**: Easy brand switching with custom color schemes
- 📐 **Custom Scaffold**: Responsive scaffold implementation with common features
- 🔧 **Utility Extensions**: Helpful context extensions for easier development

## Installation

Add this to your package's `pubspec.yaml` file:

```yaml
dependencies:
  flutter_design_extension: ^0.0.1
```

## Usage

### Basic Setup

Wrap your app with `FlutterDesignApp` to enable the design system:

```dart
import 'package:flutter_design_extension/flutter_design_extension.dart';

void main() {
  runApp(
    FlutterDesignApp(
      // Optional: Specify custom brand colors
      brand: MyCustomBrand(),
      // Optional: Configure languages
      languages: [
        Localize(Locale('en', 'US'), 'English'),
        Localize(Locale('es', 'ES'), 'Español'),
      ],
      // Required: Your MaterialApp builder
      materialApp: (localeCallback, delegates, locales, locale, theme) {
        return MaterialApp(
          localeResolutionCallback: localeCallback,
          localizationsDelegates: delegates,
          supportedLocales: locales,
          locale: locale,
          theme: theme,
          home: MyHomePage(),
        );
      },
    ),
  );
}
```

### Using Design Tokens

Access design tokens through the theme extension:

```dart
import 'package:flutter_design_extension/flutter_design_extension.dart';

class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // Access colors
    final colors = context.designTokens.colors;

    // Access typography
    final typography = context.designTokens.typography;

    return Container(
      color: colors.brand.main,
      child: Text(
        'Hello',
        style: typography.heading1,
      ),
    );
  }
}
```

### Custom Scaffold Usage

Use the provided scaffold for consistent layouts:

```dart
import 'package:flutter_design_extension/flutter_design_extension.dart';

class MyScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return CustomScaffold(
      appBar: AppBar(title: Text('My Screen')),
      body: DeviceScreen(
        mobile: (_) => MobileLayout(),
        tablet: (_) => TabletLayout(),
        desktop: (_) => DesktopLayout(),
      ),
    );
  }
}
```

### Localization

Add your translation files in the specified language path:

```
assets/
  languages/
    en_US.json
    es_ES.json
```

Access translations in your code:

```dart
import 'package:flutter_design_extension/flutter_design_extension.dart';

Text('welcome'.i18n()); // Automatically translates based on current locale
```

## Architecture

The package follows a structured architecture:

```
lib/
  ├── src/
  │   ├── components/        # Reusable UI components
  │   │   ├── colors/       # Color definitions
  │   │   ├── typography/   # Typography styles
  │   │   └── icons/       # Icon assets
  │   ├── localisation/     # Localization support
  │   └── utils/           # Utility functions
  └── flutter_design_extension.dart
```

## Dependencies

- flutter_localizations: Flutter localization support
- intl: ^0.20.2 - Internationalization utilities
- localization: ^2.1.1 - JSON-based localization
- provider: ^6.1.5 - State management

## Requirements

- Dart SDK: ">=2.18.4 <3.0.0"
- Flutter: ">=1.17.0"

## Contributing

We welcome contributions! If you'd like to contribute:

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## **SETUP STEP**

1. Create a App Brand `extends Brand`

```
class AppBrand extends Brand {
@override
ColorTokens getColorTokens(bool isDarkMode) {
  return ColorTokens(
    brand: ColorBrand(
      main: isDarkMode ? const Color(0xFFEBEAEE) : const Color(0xFF1D1C1C),
      dark: isDarkMode ? const Color(0xFFEBEAEE) : const Color(0xFF003366),
      secondary: isDarkMode ? Colors.white24 : Colors.black26,
      background:
          isDarkMode ? const Color(0xFF1D1C1C) : const Color(0xFFEBEAEE),
    ),
    interaction: ColorInteraction(
      main: isDarkMode ? const Color(0xFFEBEAEE) : const Color(0xFF1D1C1C),
      hover: isDarkMode
          ? Colors.white24.withOpacity(0.5)
          : const Color(0xFF003366).withOpacity(0.5),
      pressed: isDarkMode ? Colors.white24 : const Color(0xFF003366),
    ),
    neutral: isDarkMode ? ColorNeutralDark() : ColorNeutralLight(),
    messaging: isDarkMode ? ColorMessagingDark() : ColorMessagingLight(),
  );
}
}
```

2.  Start `FlutterDesignApp` Widget inside RunApp

```
runApp(FlutterDesignApp(
          brand: AppBrand(),
          materialApp: (
            localeResolutionCallback,
            localizationsDelegates,
            supportedLocales,
            locale,
            theme,
          ) {
            return MaterialApp(
              localeResolutionCallback: localeResolutionCallback,
              localizationsDelegates: localizationsDelegates,
              supportedLocales: supportedLocales,
              locale: locale,
              theme: theme,
              builder: (context, child) => const YourAppHomeScreen(),
            );
          },
        ),
        );
```

3. Don't forget to add `en_US.json` file inside `assets/languages`

## `DesignTokensThemeExtension`

it supports the

```
 final TextDirection textDirection;
 final ColorTokens colors;
 final ElevationTokens elevations;
 final SpacingTokens spacings;
 final OpacityTokens opacities;
 final BorderRadiusTokens borderRadiuses;
 final TextStyleTokens textStyles;
 final IconTokens icons;
```

1. It is used by using `context.theme` inside build method

## `AppDesignProvider`

          It is used to find application design system and toggle dark, and light themes. It is also used for toggle languages.

1. use `context.appDesignForAction` for action
2. use `context.appDesign` for design
3. use `toggleTheme()` to toggle your dark and light theme
4. use `setThemeLanguage(Localize lang)` to update your app language
5. use `updateBrand(Brand brand)` to update your brand
6. use `isDarkMode` to get your current theme mode
7. use `supportedLocales` to get your app supported locales (languages)
8. use `textDirection` to get your current text direction
9. use `lang` to get your current app language
10. use `brand` to get your current app brand
