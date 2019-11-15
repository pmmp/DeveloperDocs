# Plugin formats

PocketMine-MP supports several plugin formats. The standard ones are described below; you can make your own by making a custom plugin loader.

## Contents
- [Standard plugin types](#standard-plugin-types)
  - [Phar](#phar)
- [Development plugin types](#development-plugin-types)
  - [Folder](#folder)
  - [Script](#script)
- [Frequently Asked Questions](#frequently-asked-questions)

## Standard plugin types
### Phar
This plugin format consists of a single [`.phar`](https://www.php.net/manual/en/intro.phar.php) which bundles all of the files necessary to make the plugin work.
This type of plugin is commonly used to distribute pre-made plugins, because they are easy for users to download and move around without needing to modify the plugin code.

#### Loading
1. Drop the `.phar` file into your `plugins` directory.
2. Restart the server. The plugin will be loaded (if compatible).

## Development plugin types
Since the standard types of plugins are usually pretty difficult to work with while developing a plugin, there are more types of plugins that developers can use, but these are **not** recommended for distribution.

### Folder
**Note:** this plugin format is **not** enabled by default. The [DevTools](https://github.com/pmmp/DevTools) plugin is required to load this type of plugin.

This plugin format is similar to a PHAR plugin in structure, but all the files are in a folder on the disk instead of inside a phar file.
This gives you easy access to the source code when you're developing a plugin. 

This plugin format is commonly used for development, because it is just as feature-complete as a PHAR plugin, and can be compiled to a PHAR plugin when you're ready to release it.

You can see the [ExamplePlugin](https://github.com/pmmp/ExamplePlugin) or any plugin source-code repository to get an idea of how this should look.

#### Loading
1. If you don't have the `DevTools` plugin, [download its phar file](https://github.com/pmmp/DevTools/releases) and put it in your `plugins` folder.
2. Move the folder containing the plugin's source code into your `plugins` folder. The plugin's folder should contain a `plugin.yml` file and a `src` folder.
3. Restart the server and the plugin will be loaded.


### Script
This plugin format works out of the box, but it's not recommended for production use, and has less features than a normal plugin would.
This format is useful if you want to quickly test some features but don't need a full-blown plugin.

See [this thread](https://forums.pocketmine.net/threads/new-plugin-scripting-format-draft.8335/) to see how a script plugin looks and works.

#### Loading
1. Drop the `.php` file into your `plugins` folder.
2. Restart the server and the plugin will be loaded.

## Frequently Asked Questions
### Does the `/reload` command reload plugin source code?
No. This is not currently possible.

### How do I load a `.zip` plugin?
PocketMine-MP does not directly support loading zip plugins, but there may be third-party plugins available which allow you to do this. However, this is not recommended.
