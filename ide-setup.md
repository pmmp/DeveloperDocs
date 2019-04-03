# IDE Setup
This tutorial is only for PHPStorm, an IDE designed for PHP development.
Using external libraries, PHPStorm can be setup to autocomplete functions and imports from PocketMine-MP.

## How to setup PHPStorm
1. Create a new Empty PHP Project (New Project -> Empty PHP Project)
2. Now you have created your new project, download the latest phar file from the [releases page](https://github.com/pmmp/PocketMine-MP/releases), and put it on a folder in your Desktop called "phar" (The folder name does not matter)
3. Back in PHPStorm, on the menu on the left of your screen, Right-Click "External Libraries" then press "Configure PHP Include Paths..."
4. Now you are where you can configure your include paths, at the top of the popup change the "PHP language level" to 7.2, as it is the PHP version PocketMine-MP uses.
5. You also need to add the phar you downloaded earlier, you can do this by clicking the "+" on the right of the popup, and select the folder on your Desktop which contains the PocketMine-MP.phar from step 2.
6. Now, click Apply then Ok and then you're done. Happy Coding!
