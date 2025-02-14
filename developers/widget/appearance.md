# Appearance

You can customize the widget's appearance by setting your own custom theme or using our default `dark` or `light` mode themes. Specify the following properties in the `config` object.

### FKAppearance Interface

{% hint style="info" %}
All colors should be given in their hex value.
{% endhint %}

<table><thead><tr><th width="197">Property</th><th width="195">Type</th><th>Description</th></tr></thead><tbody><tr><td>themeColor</td><td>string | undefined</td><td>The primary theme color for the UI.</td></tr><tr><td>textColor</td><td>string | undefined</td><td>The color used for text elements.</td></tr><tr><td>backgroundColor</td><td>string | undefined</td><td>The background color for the UI.</td></tr><tr><td>highlightColor</td><td>string | undefined</td><td>The color used for highlighted or emphasized elements.</td></tr><tr><td>borderColor</td><td>string | undefined</td><td>The color used for borders in the UI.</td></tr><tr><td>logoUrl</td><td>string | null</td><td>URL for the logo to be displayed. Can be null if no logo is used.</td></tr><tr><td>dark</td><td>Colors | undefined</td><td>An object containing color configurations for dark mode.</td></tr><tr><td>roundness</td><td>number | undefined</td><td>A value determining the roundness of UI elements (e.g., button corners).</td></tr><tr><td>theme</td><td>ThemeName</td><td>The name of the theme to be used. </td></tr></tbody></table>

#### ThemeName

`ThemeName` is an Enum with three values: Dark, Light, and System. To apply a theme, choose from the following:

* For dark theme, use `ThemeName.Dark`
* For light theme, use `ThemeName.Light`
* For system theme, use `ThemeName.System`

### Colors Interface

{% hint style="info" %}
If you mention below color specifically it will overwrite the default theme of widget.
{% endhint %}

The `Colors` interface is used both in the main FKAppearance interface and in its `dark` property. It contains the following properties:

<table><thead><tr><th>Property</th><th width="190">Type</th><th>Description</th></tr></thead><tbody><tr><td>themeColor</td><td>string | undefined</td><td>Primary theme color</td></tr><tr><td>textColor</td><td>string | undefined</td><td>Main text color</td></tr><tr><td>backgroundColor</td><td>string | undefined</td><td>Background color</td></tr><tr><td>highlightColor</td><td>string | undefined</td><td>Color for highlighted elements</td></tr><tr><td>borderColor</td><td>string | undefined</td><td>Border color of widget</td></tr></tbody></table>

### Example

```typescript
  const config: FKConfig = {
    appName: "Dapp Name",
    // remaning config...
    appearance: {
      themeColor: "#2D2D2D",
      textColor: "#2D2D2D",
      backgroundColor: "#FFF",
      highlightColor: "#F0F0F0",
      borderColor: "#2D2D2D",
      dark: {
        themeColor: "#FFF", 
        textColor: "#FFF", 
        backgroundColor: "#2D2D2D", 
        highlightColor: "#2D2D2D", 
        borderColor: "#2D2D2D",
      },
      roundness: 42,
      theme: ThemeName.Light // if using theme, no need to mention custom colors
    },
    // remaining config...
  };
```
