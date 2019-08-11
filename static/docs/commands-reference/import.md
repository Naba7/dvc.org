# import

> **Note!** This command has been repurposed after its original release. The
> previous version is still available as the `dvc import-url` command.

Download or copy file or directory from another DVC repository (on a git server
such as Github) into the <abbr>workspace</abbr>, and track changes in the remote
data source with DVC. Creates a DVC-file.

> See also `dvc get` which corresponds to the first step this command performs
> (just download the data).

## Synopsis

```usage
usage: dvc import [-h] [-q | -v] [-o [OUT]] [--rev [REV]] url path

positional arguments:
  url         URL of Git repository with DVC project to download from.
  path        Path to data within DVC repository.
```

## Description

DVC provides an easy way to reuse datasets, intermediate results, ML models, or
other files and directories tracked in another DVC repository into the present
<abbr>workspace</abbr>. The `dvc import` command downloads such a <abbr>data
artifact</abbr> in a way that it is tracked with DVC, so it can be updated when
the external data source changes.

The `url` argument specifies the external DVC project's Git repository URL (both
HTTP and SSH protocols supported, e.g. `[user@]server:project.git`), while
`path` is used to specify the path to the data to be downloaded within the repo.

> See `dvc import-url` to download and tack data from other supported URLs.

After running this command successfully, the data found in the `url` `path` is
created in the current working directory with its original file name e.g.
`data.txt`. An import stage (DVC-file) is then created (similar to having used
`dvc run` to generate the same output) extending the full file or directory name
of the imported data e.g. `data.txt.dvc`.

DVC supports [DVC-files](/doc/user-guide/dvc-file-format) which refer to data in
an external DVC repository (hosted on a Git server). In such a DVC-file, the
`deps` section specifies the `repo` URL and data `path`, and the `outs` section
contains the corresponding local path in the workspace. It records enough data
from the external file or directory to enable DVC to efficiently check it to
determine whether the local copy is out of date.

To actually [track the data](https://dvc.org/doc/get-started/add-files),
`git add` (and `git commit`) the import stage (DVC-file).

Note that import stages are considered always "locked" - meaning that if you run
`dvc repro`, they won't be updated. Use `dvc update` on them to update the
downloaded data artifact from the external DVC repo.

## Options

- `-o`, `--out` - specify a location in the workspace to place the imported data
  in, as a path to the desired directory. The default value (when this option
  isn't used) is the current working directory (`.`).

- `--rev` - specific Git revision of the DVC repository to import the data from.
  `HEAD` by default.

- `-h`, `--help` - prints the usage/help message, and exit.

- `-q`, `--quiet` - do not write anything to standard output. Exit with 0 if no
  problems arise, otherwise 1.

- `-v`, `--verbose` - displays detailed tracing information.

## Examples

An obvious case for this command is to import a dataset from an external DVC
repo, such as our
[get started example repo](https://github.com/iterative/example-get-started).

```dvc
$ dvc import git@github.com:iterative/example-get-started data/data.xml
Importing 'data/data.xml (git@github.com:iterative/example-get-started)' -> 'data.xml'
...
Saving information to 'data.xml.dvc'.
...
```

In contrast with `dvc get`, this command doesn't just download the data file,
but it also creates an import stage (DVC-file) to keep track of this <abbr>data
artifact</abbr> as a special `repo`
[external dependency](/doc/user-guide/external-dependencies). Check
`data.xml.dvc`:

```yaml
md5: 7de90e7de7b432ad972095bc1f2ec0f8
wdir: .
deps:
  - path: data/data.xml
    repo:
      url: git@github.com:iterative/example-get-started
      rev_lock: 6c73875a5f5b522f90b5afa9ab12585f64327ca7
outs:
  - md5: a304afb96060aad90176268345e10355
    path: data.xml
    cache: true
    metric: false
    persist: false
```

Several of the values above are pulled from the original stage file
`model.pkl.dvc` in the external DVC repo. `url` and `rev_lock` fields are used
to specify the origin and version of the dependency.

<!-- ## Example: Dataset registry -->
