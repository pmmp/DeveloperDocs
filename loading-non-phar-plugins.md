# Loading non-phar plugins

During development you don't want to have to recompile a PHAR file every time you make a little change to your code. Therefore, there are two additional ways that you can load plugins for easy code access.

## Contents
- [Script plugins](#script-plugins)
- [Folder plugins](#folder-plugins)
- [Frequently Asked Questions](#frequently-asked-questions)

## Script plugins
This plugin format works out of the box, but it's not recommended for production use, and has less features than a normal plugin would.
This format is useful if you want to quickly test some features but don't need a full-blown plugin.

See [this thread](https://forums.pocketmine.net/threads/new-plugin-scripting-format-draft.8335/) to see how a script plugin looks and works.

### Loading
1. Drop the `.php` file into your `plugins` folder.
2. Restart the server and the plugin will be loaded.

## Folder plugins
This plugin format is similar to a PHAR plugin in structure, but all the files are in a folder on the disk instead of inside a phar file.
This gives you easy access to the source code when you're developing a plugin. 

This plugin format is most commonly used because it is just as feature-complete as a PHAR plugin, and can be converted to a PHAR plugin when you're ready to release it.

### Loading
1. If you don't have the `DevTools` plugin, [download its phar file](https://github.com/pmmp/PocketMine-DevTools/releases) and put it in your `plugins` folder.
2. Move the folder containing the plugin's source code into your `plugins` folder. The plugin's folder should contain a `plugin.yml` file and a `src` folder.
3. Restart the server and the plugin will be loaded.

## Frequently Asked Questions
### Does the `/reload` command reload plugin source code?
No. This is not currently possible.

### How do I load a `.zip` plugin?
PocketMine-MP does not directly support loading zip plugins, but there may be third-party plugins available which allow you to do this. However, this is not recommended.
