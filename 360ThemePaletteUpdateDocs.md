# Project Overview

As part of an ongoing effort to consolidate the code base, and centralize the way we do styling, we are tackling this project to respond to several problems.

- Many components use extensive in-line styling, leading to repeated styles and a frustrating experience updating styling.
- There is a lack of standardization in styling, with many components partially using CSS, and partially in-line styles, and different ways of doing colors.
- There has been a desire to implement a dark mode theme, which is not feasible with the way we currently implement colors.
- Colors were not centralized, with some in ```src/_vars.scss``` and others in ```src/theme.ts``` used in different places.
- The recent brand refresh necessitated updating certain colors and components, which gave us a good opportunity to rethink/refactor the way we do styling throughout the project.

#### For these reasons we decided to tackle a couple of related projects:

- Create palettes of all the gordon colors in ```src/theme.ts``` for light and dark modes using the ```extendTheme``` API (replacing the old ```createTheme``` API)
- Replace the old ```ThemeProvider``` with the ```Experimental_CssVarsProvider``` in ```src/components/providers```
- Go through all components and convert in-line style to CSS where possible to centralize and clean up styling
- Modify CSS to reverence the new theme CSS variables provided by the CssVarsProvider, replacing the old SASS variables defined within _vars.scss

### Changes

#### Theme variables

```src/theme.ts``` now defines two palettes within a theme object using the ```extendTheme``` API, light and dark.  This theme is then passed to the ```Experimental_CssVarsProvider``` in src/components/providers, which makes the themes accessible throughout the project via React context.

The theme is defined in the following object structure:
```
colorSchemes: {
  light: {
    palette: {
      ...
    },
  },
  dark: {
    palette: {
      ...
    },
  },
},
```

Which is accessible using the CSS variable ```--mui-palette``` which will take on the value of light or dark palette depending on which is set(see [Setting the Theme](#themesetting) below).

Within each palette there are a variety of colors, the names of which come from the Palette interface defined in mui/material/styles.  By default this includes the colors ```primary```, ```secondary```, ```error```, ```warning```, ```info```, and ```success```, and each of these can have ```main```, ```dark```, and ```light```, shades, as well as shade colors 50-900 and accent colors A100-A700.  We also extend the default palette interface within theme.ts to add a neutral color, which can contain the same shades as the primary color.

These palettes can also be used to override the default MUI colors for certain MUI components that don't reference the colors defined above, like cards, switches, etc.  (Some MUI components reference the colors described above by default).  For example, the color ```--mui-palette-background-paper``` is used by MUI cards.

These colors can then be referenced using CSS variables, i.e. ```var(--mui-palette-primary-main)``` or ```var(--mui-palette-neutral-400)``` from within CSS class in .scss files.

#### Setting the Theme {#themesetting}

For testing we made a component that defines a button to switch pallete modes (light, dark) with the ```useColorScheme()``` hook.  This sets the scheme globally, changing the color scheme of ```--mui``` throughout the project.


