# Updater for PHP

Updater for PHP is an update script that can be used with any PHP project to update
the files via a couple clicks.  The script only requires that it can get at a
update file that contains the current version as well as the local filesystem
location and the remote location.  So, potentially Amazon S3 could be used.

Did we mention that this update script can adapt to languages?  All you need is
the appropriate language files in js/langs and langs/php.  There are currently
three languages supported out of the box, English, Spanish, and German.  The
script will default to English if it cannot find a suitable language file based
on the brower's settings.

## Screen Shots

![Imgur](http://i.imgur.com/kF1GrPp.png)

There is a new update

![Imgur](http://i.imgur.com/bcCyLJc.png)

The code is currently up to date

![Imgur](http://i.imgur.com/BbKhpls.png)

The update has finished

![Imgur](https://i.imgur.com/enVBO6O.png)

Prompt if you want to restore backup

## Setup the Update Script

To use this update script you will need to define a YAML file that explains
what to do for the update.  This file must contain `version` and `files`.
Under `files` there is two additional items, `add` and `delete`.  The version
must be a set of integers seperated by a period (.) the leftmost number is of
greatest importance.

Both the `add` and `delete` items are sequences.  With all items in the `add`
sequence being a mapping with `local` and `remote` defined.  `local` is where
you want the downloaded file to go and `remote` is where to get it from the
update server.

The `script` section defines scripts you want to run after everything is added
and deleted.  This is useful for database updates and possibly some other cases.
However, note that there are no protection against scripts.  In other words, there
are no backups from what the script does unless you handle it yourself.  Scripts
will not be deleted after execution.

Below is an example of a YAML update file:

```
version:    1.1.17
files:
        add:
            - {local: "foo/bar/writable/foobar.txt", remote: "files/foobar.txt"}
            - {local: "scripts/writefile.php", remote: "files/writefile.php.txt"}
        delete:
            - "foo/foo.txt"
scripts:
    - "scripts/writefile.php"
```

## Setup the Updater

To set up the updater you must edit config.php.  version_url is where you put the
base url for your update files.  version_file is the version file on the remote
host with the information to update.  update_folder is the folder on the local
host that you want to serve as your base.

## Changing the URL for Different Versions

If you would like to have each version have its own update script you can make
sure you include the updaters config.php in the updates with the new location
for that version.

## Restoring Backups

When the user goes to the update screen it will check for the existance of
backup-{version} files.  If they exist then a prompt will ask if they would
like to restore a backup or check for updates.

## Current Failsafes

The following failsafes are in place:

### Writability Check

All files are checked before downloading to make sure they are writable

### File Backup

All files that are going to be added and currently exist or are going to be deleted
are added to a backup file named backup-{version}.zip in the same folder as the
updater script.

### Remote File Check

All remote files are checked to exist before they are installed. 

## Help Me Out

I would love people to fork this project and contribute.  If not that I would be interested
in who uses this product.  See my website https://pessetto.com to find my email and shoot
me a quick message if you use and like this product.

## Tools

The following tools exist for testing purposes or to simply make your life easier when
creating a update file.  They can be found in the tools folder of this repository. Some
may be accessed via a command line/terminal with PHP installed others will need to use
a web browser.

### Update Test

This script will generate two folders in the root of the repository, update-test and
backup-test-source.  If you have a test webserver setup you can then test if the
backup system works by visting backup-test.  This is good for testing updates
and restoring for backups.

To run this script you will need to make sure the repository is somewhere the web browser
can see it and then point the browser to the repsoitory's URL with appended with
`tools\update-test.php`.

## Notice of Spyc

In order to parse YAML we are using the [Spyc](https://github.com/mustangostang/spyc)
library by Vladimir Andersen.  This is licensed under the [MIT License](https://github.com/mustangostang/spyc/blob/master/COPYING).

## License

Copyright (c) 2017 Travis Pessetto

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
