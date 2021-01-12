---
description: Install LTO Network full node plus supporting tools on Windows 10.
---

# Windows

## LTO Network full node

The LTO full node is comprised of a set of [Docker](https://www.docker.com/) containers. For development, use _docker compose_ as orchestration tool.

### Docker Desktop

{% hint style="success" %}
1. Download [Docker Desktop for Windows](https://hub.docker.com/editions/community/docker-ce-desktop-windows).
2. Double-click `Docker Desktop for Windows Installer.exe` to run the installer.
3. Docker does not start automatically. To start it, search for `Docker` and select _Docker Desktop for Windows_ in the search results.
{% endhint %}

For more detailed instructions please read the [Installation guide in the Docker documentation](https://docs.docker.com/docker-for-windows/install/).

_Docker Desktop includes docker compose, so it's not needed to install that separately._

### LTO full node

{% hint style="success" %}
1. Download the [docker composer configuration file](https://raw.githubusercontent.com/legalthings/lto-deepdive/master/docker/dev/docker-compose.yml) for LTO full node.
2. In PowerShell \(or another terminal\) run `docker-composer up`.
{% endhint %}

## Live contracts tester

The live contract tester \(`lctest`\) is build on [Behat](http://behat.org/en/latest/) and runs on [PHP](https://php.net/). It requires PHP 7+ with the  _mongodb_ and _yaml_ PECL extension.

### PHP CLI

{% hint style="success" %}
1. Install the [Visual C++ Redistributable for Visual Studio 2015](http://www.microsoft.com/en-us/download/details.aspx?id=48145).
2. [Download PHP for Windows](https://windows.php.net/download/). Recommended is the 64-bit _Non-thread-safe_ version.
3. Expand the zip file into the path `C:\PHP7`.
4. Configure PHP to run correctly on your system:
   1. In the `C:\PHP7` folder, rename the file `php.ini-development` to `php.ini`.
   2. Edit the `php.ini` file in a text editor \(e.g. Notepad++, Atom, or Sublime Text\).
   3. Change the following settings in the file and save the file:
      1. Uncomment the line that reads `; extension_dir = "ext"` \(remove the `;` so the line is just `extension_dir = "ext"`\).
      2. In the section where there are a bunch of `extension=` lines, uncomment the following lines:
         1. `extension=php_curl.dll`
         2. `extension=php_openssl.dll`
         3. `extension=php_sodium.dll`
5. Add `C:\PHP7` to your Windows system path:
   1. Open the System Control Panel.
   2. Click 'Advanced System Settings'.
   3. Click the 'Environment Variables...' button.
   4. Click on the `Path` row under 'System variables', and click 'Edit...'
   5. Click 'New' and add the row `C:\PHP7`.
   6. Click OK, then OK, then OK, and close out of the System Control Panel.
6. Open PowerShell \(or another terminal emulator\), and type in `php -v` to verify PHP is working.
{% endhint %}

### PECL extensions for PHP

#### MongoDB

{% hint style="success" %}
1. Visit [https://windows.php.net/downloads/pecl/releases/mongodb/](https://windows.php.net/downloads/pecl/releases/mongodb/) and choose the latest stable version \(not alpha, beta or RC\).
2. Download the version that matches your PHP installation. For PHP 7.3 64-bit Non-thread-safe, choose the version that ends with `7.3-nts-vc15-x64.zip`.
3. Extract the zip file into path `C:\PHP7\ext`.
4. Configure PHP to run correctly on your system and add the following line: `extension=php_mongodb.dll`.
5. In PowerShell \(or another terminal emulator\) type `php --re mongodb` to verify the extension is installed correctly.
{% endhint %}

#### Yaml

{% hint style="success" %}
1. Visit [https://windows.php.net/downloads/pecl/releases/yaml/](https://windows.php.net/downloads/pecl/releases/yaml/) and choose the latest stable version \(not alpha, beta or RC\).
2. Download the version that matches your PHP installation. For PHP 7.3 64-bit Non-thread-safe, choose the version that ends with `7.3-nts-vc15-x64.zip`.
3. Extract the zip file into path `C:\PHP7\ext`.
4. Configure PHP to run correctly on your system and add the following line: `extension=php_yaml.dll`.
5. In PowerShell \(or another terminal emulator\) type `php --re yaml` to verify the extension is installed correctly.
{% endhint %}

### lctest.phar

{% hint style="success" %}
1. Download [lctest.phar](https://github.com/legalthings/livecontracts-tester/raw/master/lctest.phar) from the LTO _livecontracs-tester_ repository.
2. Run `php lctest.phar` to verify it's working correctly.
{% endhint %}



