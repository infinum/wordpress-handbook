> Localization describes the subsequent process of translating an internationalized theme/plugin. Localization is often abbreviated as l10n (because there are 10 letters between the l and the n.)

This is a handy up-to-date how-to for generating WordPress Theme/Plugin translation files with WP-CLI. The following workflow is covered: creating a POT (Portable Object Template) file, copying it to a PO (Portable Object) file where the `msgstr` sections are translated or updating pre-existing PO file and finally building the MO (Machine Object) file which is used by `gettext` functions to output translations to the user.

### Available commands:

- [wp i18n make-pot](https://github.com/wp-cli/i18n-command#wp-i18n-make-pot)
- [wp i18n make-json](https://github.com/wp-cli/i18n-command#wp-i18n-make-json)
- [wp i18n make-mo](https://github.com/wp-cli/i18n-command#wp-i18n-make-mo)
- [wp i18n update-po](https://github.com/wp-cli/i18n-command#wp-i18n-update-po) - ^2.4.0 WP CLI i18n-command [package](https://github.com/wp-cli/i18n-command) is required.

### Useful Notes
- The	following files and folders are always excluded: `node_modules, .git, .svn, .CVS, .hg, vendor, Gruntfile.js, webpack.config.js, *.min.js`.
- By default, the "Text Domain" header of the plugin or theme is used. If the header is not specified you'll need to add an argument to the CLI command with text-domain it should search for: `[--domain=<domain>]`.
- when naming translation files use the [WP Locales](https://make.wordpress.org/polyglots/teams/) naming convention. Example for creating files for French language translation: fr_FR.pot, fr_FR.po, fr_FR.mo.
- `[--debug]` global WP CLI option, outputs info during runtime.
- Xdebug PHP extension with `develop` mode feature turned on can cause issues when running WP CLI I18n commands on the entire theme or plugin folder. Easiest way to get around this is to turn it off for the current session by prepending `XDEBUG_MODE=debug` to the command: `XDEBUG_MODE=debug wp i18n make-po <source> [<destination>]`.
- The POT and PO files can easily be interchangeably renamed to change file types without issues.

The Eightshift theme boilerplate folder structure for language files is used for examples: `src/I18n/languages/`.

## WP CLI Commands

### Create .pot file

_Command_: `wp i18n make-po <source> [<destination>]`

_Example:_
```
wp i18n make-pot . src/I18n/languages/fr_FR.pot --exclude=public,assets
```

This will scan through the entire current directory and output a fr_FR.pot file in the `src/I18n/languages` folder.

### Create .po file

Copy the .pot file in the same directory and change the extension to .po.

_Example:_
```
cp src/I18n/languages/fr_FR.pot src/I18n/languages/fr_FR.po
```

Translate `msgstr` sections.

### Update .po file(s)

_You may need to manually update wp i18n package (^2.4.0) for this to work: `wp package install git@github.com:wp-cli/i18n-command.git`._

If there is a pre-existing `.po` file you can update it with new entries while keeping the existing translations.

_Command_: `wp i18n update-po <source> [<destination>]`

_Example - Single:_
```
wp i18n update-po src/I18n/languages/fr_FR.pot src/I18n/languages/fr_FR.po
```

_Example - Batch:_
```
wp i18n update-po src/I18n/languages/*.pot src/I18n/languages/
```

### Create .mo file(s)

Last thing to do is to create the binary .mo file which is used by WordPress to output translations for the theme / plugin.

_Command_: `wp i18n make-mo <source> [<destination>]`

_Example - Single:_
```
wp i18n make-mo src/I18n/languages/fr_FR.po
```

_Example - Batch:_
```bash
for file in `find "src/I18n/languages/" -name "*.po"` ; do msgfmt -o ${file/.po/.mo} $file ; done
```

### Official docs:
[Localization](https://developer.wordpress.org/plugins/internationalization/localization/)\
[wp i18n command](https://developer.wordpress.org/cli/commands/i18n/) -
[GitHub](https://github.com/wp-cli/i18n-command)
