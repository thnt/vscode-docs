---
Order: 12
Area: editor
TOCTitle: Emmet
ContentId: baf4717c-ea52-486e-9ea3-7bf1c4134dad
PageTitle: Emmet in Visual Studio Code
DateApproved: 8/14/2017
MetaDescription: Using Emmet abbreviations inside VS Code. 
---
# Emmet in Visual Studio Code

Visual Studio Code supports most of the the [Emmet Actions](https://docs.emmet.io/actions/) including expanding of [Emmet abbreviations and snippets](https://docs.emmet.io/cheat-sheet/).

In the July 2017 (v1.15) release of VS Code, we introduced Emmet 2.0, which enabled features like Emmet in the suggestions/auto-completion list, and multi-cursor support.
Read more on the why's and how's of this major overhaul in [Emmet 2.0 blog post](https://code.visualstudio.com/blogs/2017/08/07/emmet-2.0).

## How to expand Emmet abbreviations and snippets

Emmet abbreviation and snippet expansions are enabled by default in `html`, `haml`, `jade`, `slim`, `jsx`, `xml`, `xsl`, `css`, `scss`, `sass`, `less` and `stylus` files. As well as any language that inherits from any of the above like `handlebars` and `php`.

Just start typing an Emmet abbreviation and you will see the same show up in the suggestion list. Keep the suggestions docs expanded as you type, and you will see a preview of the expansion of the abbreviation you are typing.
If you are in a stylesheet file, then the expanded abbreviation shows up in the suggestion list sorted among the other css suggestions.

![Emmet in suggestion/auto-completion list](images/emmet/emmet.gif)

If you have disabled `editor.quickSuggestions`, then you wont see the suggestions show up as you type. You can still trigger it manually by pressing `kb(editor.action.triggerSuggest)` and see the preview.

If you don't want to use suggestions at all, then use the command `Emmet: Expand Abbreviation` to expand your abbreviations. You can bind any keyboard shortcut to the command id `editor.emmet.action.expandAbbreviation` as well.

If you want to use the `kbstyle(Tab)` key for expanding the Emmet abbreviations, add the [setting](/docs/getstarted/settings.md)  `emmet.triggerExpansionOnTab` and set it to `true`. We use this setting to provide the appropriate fallback to provide indentation when there is no abbreviation to expand.

## Emmet abbreviations in other file types

To enable the Emmet abbreviation expansion in file types where it is not available by default, use the `emmet.includeLanguages` setting. Make sure to use language ids for both sides of the mapping.

For example:
```json
"emmet.includeLanguages": {
    "javascript": "javascriptreact",
    "vue-html": "html",
    "plaintext": "jade"
}
```

> Note: If you used `emmet.syntaxProfiles` previously to map new file types, from VS Code 1.15 onwards you should use the setting `emmet.includeLanguages` instead. `emmet.syntaxProfiles` is meant for [customizing the final output](https://docs.emmet.io/customization/syntax-profiles) only.

## Using custom Emmet snippets

Custom Emmet snippets need to be defined in a json file named `snippets.json`. The `emmet.extensionsPath` setting should have the path to the directory containing this file.

Below is an example for the contents of this `snippets.json` file.

```json
{
    "html": {
        "snippets": {
            "ull": "ul>li",
            "ran": "{This is random text not markup or abbreviation}"
        }
    },
    "css": {
        "snippets": {
            "cb": "color: black"
        }
    }
}
```

Keep in mind that there are some restrictions on the snippet name and value.

### HTML Emmet snippets

Values for HTML Emmet snippets should be a valid abbreviation. For example, if you want an unordered list with a list item, your snippet should be `ul>li` and not `<ul><li></li></ul>`.

If you want to have just text and not markup in your snippet then use the `{}` notation as shown in the example above.

HTML custom snippets are applicable to all other markup flavors like `haml` or `jade`. When snippet value is an abbreviation and not actual HTML, the appropriate transformations can be applied to get the right output as per the language type.

### CSS Emmet snippets

Values for CSS Emmet snippets should either be a property value or the complete property value pair.

Name the snippet such that it contains the letters from the snippet value in the order that they appear in the latter, so that the fuzzy matching algorithm of the suggestion list can make the right match. If you don't use similar letters, the **Emmet: Expand Abbreviation** command will still work, but the snippets won't show up as expected in a filtered suggestion list.

For example, don't use `bc` or `darkc` as the name for `color: black`. Use `cb` or `cob` instead.

CSS custom snippets are applicable to all other stylesheet flavors like `scss`, `less` or `sass`. Therefore, don't include a trailing `;` at the end of the snippet value. Emmet will add it as needed based on the whether the language requires it.

> Note: After making changes to the `snippets.json` file, remember to reload VS Code for it to take effect.

## Emmet configuration

Below are Emmet [settings](/docs/getstarted/settings.md) that you can use to customize your Emmet experience in VS Code.

* `emmet.includeLanguages`

    Use this setting to add mapping between the language of your choice and one of the Emmet supported languages to enable Emmet in the former using the syntax of the latter.
    Make sure to use language ids for both sides of the mapping.

    For example:
    ```json
    "emmet.includeLanguages": {
        "javascript": "javascriptreact",
        "vue-html": "html",
        "plaintext": "jade"
    }
    ```

* `emmet.excludeLanguages`

    If there is a language where you do not want to see Emmet expansions, add it in this setting which takes an array of language id strings.

* `emmet.syntaxProfiles`

    See [Emmet Customization of output profile](https://docs.emmet.io/customization/syntax-profiles/#create-your-own-profile) to learn how you can customize the output of your HTML abbreviations.

    For example:
    ```json
    "emmet.syntaxProfiles": {
        "html": {
            "attr_quotes": "single"
        },
        "jsx": {
            "self_closing_tag": true
        }
    }
    ```

* `emmet.variables`

    Customize variables used by Emmet snippets.

    For example:
    ```json
    "emmet.variables": {
        "lang": "de",
        "charset": "UTF-16"
    }
    ```

* `emmet.showExpandedAbbreviation`

    Controls the Emmet suggestions that show up in the suggestion/completion list.

    Setting Value | Description
    ----------- | -------
    `never` | Never show Emmet abbreviations in the suggestion list for any language.
    `inMarkupAndStylesheetFilesOnly` | Show Emmet suggestions only for languages that are purely markup and stylesheet based ('html', 'pug', 'slim', 'haml', 'xml', 'xsl', 'css', 'scss', 'sass', 'less', 'stylus').
    `always` | Show Emmet suggestions in all Emmet supported modes as well as the languages that have a mapping in the `emmet.includeLanguages` setting.

    **Note:** In the `always` mode, the new Emmet implementation is not context aware. For example, if you are editing a JavaScript React file, you will get Emmet suggestions not only when writing markup but also while writing JavaScript.

* `emmet.showAbbreviationSuggestions`

    Shows possible emmet abbreviations as suggestions. Its `true` by default. 

    For example, when you type `li`, you get suggestions for all emmet snippets starting with `li` like `link`, `link:css` , `link:favicon` etc.
    This is helpful in learning Emmet snippets that you never knew existed unless you knew the [Emmet cheatsheet](https://docs.emmet.io/cheat-sheet/) by heart.
    
    Not applicable in stylesheets or when emmet.showExpandedAbbreviation is set to "never".
   

* `emmet.extensionsPath`

   Provide the location of the directory that houses the `snippets.json` file which in turn has your custom snippets.

* `emmet.triggerExpansionOnTab`

    Set this to true to enable expanding Emmet abbreviations with `kbstyle(Tab)` key. We use this setting to provide the appropriate fallback to provide indentation when there is no abbreviation to expand.

    
## Emmet preferences

Prior to VS Code 1.15, you could use the `emmet.preferences` setting to customize Emmet as documented in [Emmet Preferences](https://docs.emmet.io/customization/preferences/). Emmet 2.0's modular nature does not support these Emmet Preferences in the same way as it did before. 

In the upcoming August 2017 release of VS Code (1.16), we are working on getting back support for the below preferences. 
- css.propertyEnd
- css.valueSeparator
- sass.propertyEnd
- sass.valueSeparator
- stylus.propertyEnd
- stylus.valueSeparator
- css.unitAliases
- css.intUnit
- css.floatUnit
- filter.commentAfter
- filter.commentBefore
- filter.commentTrigger
- bem.elementSeparator
- bem.modifierSeparator


If you happen to use any of the other preferences and want them to be supported in the new Emmet, please add a comment in the [issue #32496](https://github.com/Microsoft/vscode/issues/32496) 

## Known issues in Emmet 2.0

Below are some of the upstream issues with Emmet 2.0 that we are working on fixing. Any help in these areas is appreciated.

- Support to customize formatters for stylesheets like `css.propertyEnd`, `css.valueSeparator`, `css.UnitAliases`, `css.intUnit` and `css.floatUnit`. [Issue: emmetio/expand-abbreviation#8](https://github.com/emmetio/expand-abbreviation/issues/8)
- Use of `@-` to get numbering in descending order in repeaters is not supported. [Issue: emmetio/html-transform#2](https://github.com/emmetio/html-transform/issues/2) 
- Html snippets ending with `+` like `select+`, `ul+` etc from the [Emmet cheatsheet](https://docs.emmet.io/cheat-sheet/) are not supported. [Issue: emmetio/html-matcher#1](https://github.com/emmetio/html-matcher/issues/1)


