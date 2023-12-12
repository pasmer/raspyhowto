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
