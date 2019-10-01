# TextMate customization

Notes and configuration for bending TextMate to your will.

## Themes

### Markdown Headings and Raw

TextMate (or one of its bundles) sets up some styles for Markdown headings and raw text under Bundle Editor → Themes → Settings. I don't like most of these defaults, so I disable them. I do, however, like giving the top two levels of heading and separators a background color. The theme is the right place to do that so that; borrow the appropriate scope from the settings. For posterity, they look like:

```
markup.heading.1, markup.heading.1 entity.name.section.markdown
```

The second part is important because it overrides the default markdown text color that would apply outside the heading.

### Behave theme (Dark)

A version of the dark Behave TextMate theme personalized primarily for Markdown. Modifications:

- Background-highlighted level 1, 2 headings
- Warmer list text
- Cooler raw/pre text
- Bare hyperlinks colored like normal Markdown links
- Brighter (white) caret/cursor

### Dawn theme (Light)

My preferred light theme. I believe it's part of the standard install/bundles.

TODO: create a customized version with background-highlighted level 1 and 2 headings and save it here.

### Theme editor

An exceptional in-browser theme editor for TextMate 2:

https://tmtheme-editor.herokuapp.com

Scopes:

- `⌘⌃⇧P` shows you the scope of the text under the cursor.
- `⌘⌃⇧C` copies the current scope.

### Installing a theme

The easiest thing I've found is to open a `.tmTheme` file from the Finder and let TextMate install it. You'll be prompted for a bundle to add it to; choose `Themes`.

If you're iterating on developing a theme with the online theme editor and already have a theme with the same name installed, TextMate will just make a copy of the theme that won't be visible in the theme list. Before installing, delete the existing theme from `~/Library/Application\ Support/TextMate/Bundles/Themes.tmBundle/Themes/`. You'll also need to quit TextMate and reopen, then select your theme to see any changes.

## Customization

### Markdown lists

I've modified the `New Item` (`⌘↩`) and `New Subitem` (`⌘⇧↩`) actions in the Markdown bundle (under Menu Actions → Lists) to always move the cursor to the end of the line before breaking the list. I have never wanted to insert a new list item by breaking the current one. Instead, I always want to start a new list item after this one. The critical change:

```rb
#index = ENV['TM_LINE_INDEX'].to_i()
# I almost always just want a new line, not to break the current line.
index = ENV['TM_CURRENT_LINE'].length
```

### Hyperlink Helper → Open Current URL

This Menu Action (shortcut `enter`) opens hyperlinks under the caret. I like to link to local files and use this shortcut to open them quickly, but TextMate doesn't expand variables or `~`, making it annoying to "jump" up to the home directory. I've changed the action to the following in order to get it to replace a `~` at the beginning of the hyperlink with the `HOME` environment variable value.

```sh
file=$(cat)
exec open "${file/#\~/$HOME}"
```