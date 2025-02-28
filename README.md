# TailwindCSS Color Matcher

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Vercel](https://img.shields.io/badge/Vercel-Deployed-blue)](https://color.arumi.top)

[TailwindCSS Color Matcher](https://color.arumi.top) is a tool to find the closest TailwindCSS color matches for any color you need to work with. This tool lets you identify which predefined Tailwind colors are closest to your target color using different color distance metrics.

## Introduction

When working with Tailwind CSS, you often need to choose from Tailwind's predefined color palette. This app helps you find the closest Tailwind color to any color you have, saving you from adding custom colors unnecessarily and helping you maintain design consistency.

The Color Matcher analyzes your input color and compares it against all Tailwind colors using three different comparison methods:

1. RGB distance
2. HSL distance
3. DeltaE

## Features

- Interactive color picker with visual preview
- Manual color input with multiple format support
- Three different color comparison methods:
  - RGB distance calculation
  - HSL distance with 4x hue bias
  - DeltaE perceptual color difference
- Top 10 closest matches displayed for each method
- Visual comparison between your color and the matches
- Copy-friendly format for color names and hex codes

## User Guide

### Setting Your Target Color

You have two ways to specify the color you want to match:

1. **Using the Color Picker**:

   - Click on the circular color swatch in the top left of the interface
   - A color picker dialog will appear
   - Use the main color area to select brightness and saturation
   - Use the vertical slider to adjust hue
   - The color will update in real-time as you move the picker and the slider

2. **Direct Color Input**:
   - Type a valid color code into the text input field
   - Supports multiple formats:
     - Hex: `#rgb`, `#rgba`, `#rrggbb`, or `#rrggbbaa` (e.g., `#76c6cc`)
     - RGB: `rgb(r, g, b)`
     - HSL: `hsl(h, s%, l%)`
   - The color will update in real-time as you type

### Understanding the Results

The app displays three tables of results, each using a different method to calculate color similarity:

1. **RGB Table**:

   - Shows matches based on raw RGB component differences
   - Best for finding colors that have similar RGB values
   - The percentage indicates the distance - lower percentages mean closer matches

2. **HSL Table**:

   - Shows matches based on Hue, Saturation, and Lightness
   - Applies a hue bias (4x weight) to prioritize similar hues
   - Better for finding perceptually similar colors
   - The percentage indicates the distance - lower percentages mean closer matches

3. **DeltaE Table**:
   - Shows matches based on perceptual color difference ([DeltaE](https://en.wikipedia.org/wiki/Color_difference#CIELAB_%CE%94E*))
   - Most accurate for how humans perceive color differences
   - Lower values mean closer matches

### Working with Results

Each table shows the top 10 closest matches with:

- A visual swatch showing your color (inner) and the matched color (outer)
- The Tailwind color name (e.g., "blue-500")
- The hex code value
- The distance/difference measurement

**To copy a color name or hex code**:

1. Double-click on the color name or hex code cell
2. The text will be automatically selected
3. Use Ctrl+C (or Cmd+C on Mac) to copy the selected text

### Example Workflow

1. You have a design with color "#76c6cc"
2. Enter this color using either the color picker or text input
3. Review the three results tables
4. Notice that "emerald-300", "teal-400" or "cyan-400" might be the closest Tailwind colors
5. Double-click to copy the Tailwind color name
6. Use this color in your Tailwind project instead of a custom color

## Limitations and Known Issues

- The app only compares against default Tailwind colors, not any custom colors you may have in your project
- Alpha/transparency values are not considered - all compared colors are fully opaque
- The app doesn't provide a way to customize the weight of different color components in the distance calculations

## References

- [TailwindCSS Colors](https://tailwindcss.com/docs/colors) - Default color palette used in this tool
- [RGB Color Model](https://en.wikipedia.org/wiki/RGB_color_model) - Used for RGB distance calculation
- [HSL Color Space](https://en.wikipedia.org/wiki/HSL_and_HSV) - Used for HSL distance calculation
- [DeltaE](https://en.wikipedia.org/wiki/Color_difference#CIELAB_%CE%94E*) - Perceptual color difference metric
- [Colord Library](https://github.com/omgovich/colord) - The color manipulation library used in this project

## License

This project is licensed under the MIT License.
