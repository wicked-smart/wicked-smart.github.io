---
layout: post
title:  Code Snippets
Date: 2020-23-02 06:41  PM +05:30
---

### ToDOs

  - [X] Add permissions image before and after running the script
  - [ ] Add Explanation of how 600 and 700 is a security bug for any website !
  - [ ] Explantion of script !

# PHP Script

  **permission.php**

  > php script to configure file and folder permissions of website at one go rather than manually doing chmod's to every file extension

  I used this script while solving [CS50's Pset7](https://cdn.cs50.net/2016/x/psets/7/pset7/pset7.html). When you first download the pset's boilerplate code, the folder's permissions are mostly 700's for folders and 600's for files. This script assigns  644's to the files and 755's to the folders

Folder and file's permissions before running below script:
![permisions-before.png](/assets/code-snippets/permissions-before.png)


  ```php

  <?php
  $dir = dirname( __FILE__ );
  $extensions_to_chmod_644 = array(
  "jpg","gif","png","ico","css","md","txt","html","htm","xml","js","csv","json","htaccess");
  $extension_separator = "\|";
  $command_to_chmod_644 = 'find "' . $dir . '" -type f -iregex ".*\.\(';
  foreach( $extensions_to_chmod_644 as $ext ) {
      $command_to_chmod_644 .= $ext . $extension_separator;
  }
  $command_to_chmod_644 =
  rtrim($command_to_chmod_644,$extension_separator);
  $command_to_chmod_644 .= '\)$" -print0 | xargs -0 chmod 644'. PHP_EOL;
  shell_exec($command_to_chmod_644);

  $command_to_chmod_755 = "find '$dir' -type d -print0 | xargs -0 chmod 755" . PHP_EOL;
  shell_exec($command_to_chmod_755);

  ```
Folder and Files permissions after running the script:
![permisions-after.png](/assets/code-snippets/permissions-after.png)


