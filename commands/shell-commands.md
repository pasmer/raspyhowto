# Shell commands

### Create Symlink for File <a href="#ftoc-heading-4" id="ftoc-heading-4"></a>

To create a symbolic link to a file, open a terminal window and enter the command below:

```
ln -s [target] [symlink]
```

The command consists of the following elements:

* The **`-s`** option instructs **`ln`** to create a symlink. With no options specified, the command creates a hard link.
* **\[target]** is the file the link references.
* **\[symlink]** is the location to save the link. If this element is omitted, the command places the symlink in the current working directory.

The example below creates a symbolic link **(link-file.txt)** that points to the target file (**target-file.txt**) located in the _test_ directory.

```
ln -s test/target-file.txt link-file.txt
```



### Copy all files and folder from Source to Target

You can copy the content of a folder `/source` to another existing folder `/dest` with the command

```
cp -a /source/. /dest/
```

The `-a` option is an improved recursive option, that preserve all file attributes, and also preserve symlinks.

The `.` at end of the source path is a specific `cp` syntax that allow to copy all files and folders, included hidden ones.



### Find a file on terminal

The `find` command lets you efficiently search for files, folders, and character and block devices.

Search in the current folder and sub-folder, example:

```
find . -type f -iname "*.txt"
```

Below is the basic syntax of the `find` command:

```bash
find /path/ -type f -name file-to-search
```

Where,

* `/path` is the path where file is expected to be found. This is the starting point to search files. The path can also be`/`or `.` which represent root and current directory, respectively.
* `-type` represents the file descriptors. They can be any of the below:

`f` – **Regular file** such as text files, images and hidden files.

`d` – **Directory**. These are the folders under consideration.

`l` – **Symbolic link**. Symbolic links point to files and are similar to shortcuts.

`c` – **Character devices**. Files that are used to access character devices are called character device files. Drivers communicate with character devices by sending and receiving single characters (bytes, octets).  Examples include     keyboards, sound cards and mouse.

`b` – **Block devices**. Files that are used to access block devices are called block device files. Drivers communicate with block devices by sending and receiving entire blocks of data. Examples include USB, CD-ROM

* `-name` is the name of the file type that you want to search.

LInk:

{% embed url="https://www.freecodecamp.org/news/how-to-search-for-files-from-the-linux-command-line/" %}



### HD Speed Test

Launch the shell prompt or use ssh to access a distant server. To gauge server throughput (write speed), use the dd command:

```
sudo dd if=/dev/zero of=/tmp/test1.img bs=1G count=1 oflag=dsync
```

