# Nextcloud

## Installation

### Folder in cloud

Create a new folder (with Root permission) in `/media` then change the permissions:

```
chmod 770 /media/cloud
chown -R www-data:www-data /media/cloud
```

Notice: the folder /media/cloud is the mount folder included in [Fstab](../hardware/hard-disk.md#fstab-file) file.

## Administration

### Manage Apps

#### 1. List current apps

Before installing an app, you can check to see what you already have installed.

`occ app:list`

This will provide a list of all enabled and disabled apps on your system. (the image below is cut short to save space).

&#x20;

#### 2. Installing a new app

For this article, we'll be using the Contacts app as an example. To install it, you run the following command.

`occ app:install contacts`

The app will be installed in a disabled state.

&#x20;

#### 3. Enabling an app

In order for an installed app to function, it must be enabled.

`occ app:enable contacts`

&#x20;

#### 4. Disabling an app

It may be necessary to disable apps when performing an upgrade of Nextcloud, or during other maintenance events.

`occ app:disable contacts`

&#x20;

#### 5. Removing an app

In the event that you need to remove an app entirely, it's a different type of command. Make sure to always disable an app before you remove it.

`rm -rf /public_html/apps/[APPNAME]/`
