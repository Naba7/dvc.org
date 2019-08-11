# cache

Contains a helper command to set the <abbr>cache directory</abbr> location:
[dir](/doc/commands-reference/cache/dir).

## Synopsis

```usage
usage: dvc cache [-h] [-q] [-v] {dir} ...

positional arguments:
  COMMAND
    dir          Configure cache directory location.
```

## Description

After DVC initialization, a hidden directory `.dvc/` is created with the
[DVC internal files](/doc/user-guide/dvc-files-and-directories), including the
default `cache` directory.

The DVC cache is where your data files, models, etc (anything you want to
version with DVC) are actually stored. The corresponding files you see in the
workspace simply link to the ones in cache. (See `dvc config cache`, `type`
config option, for more information on file links on different platforms.)

> For more cache-related configuration options refer to `dvc config cache`.

## Options

- `-h`, `--help` - prints the usage/help message, and exit.

- `-q`, `--quiet` - do not write anything to standard output. Exit with 0 if no
  problems arise, otherwise 1.

- `-v`, `--verbose` - displays detailed tracing information.
