# DVC Shell Autocomplete

Enjoy working with DVC faster and with fewer typos!

Command completion is usually requested by pressing the `tab` key on your shell,
it will then present the possible options that could follow that command call.
For example:

```dvc
$ dvc r # Press [tab] key
 -- dvc commands --
remote  -- Manage remote storage configuration.
remove  -- Remove outputs of DVC-file.
repro   -- Check for changes and reproduce DVC-file and dependencies.
root    -- Relative path to project's directory.
run     -- Generate a stage file from a command and execute the command.
```

Depending on what you typed on the command line so far, it completes:

- Available DVC commands
- Options that are available for a particular command
- File names that make sense in a given context, such as using them as a target
  for some commands
- Arguments for selected options. For example, `dvc repro` completes with stage
  files to reproduce

Depending upon your preference and the availability of both Bash and Zsh on your
system, follow the steps given below to Configure Bash and/or Zsh.

If you are new to working with shell or uncertain about your active shell, print
`$0` to check your active shell. For example:

```dvc
$ echo $0

  /bin/bash
```

In this case, follow the steps to configure Bash as it is your active shell.

## Configure Bash

First, make sure Bash completion support is installed:

- On a current Linux OS (in a non-minimal installation), bash completion should
  be available;
- On a Mac, install with `brew install bash-completion`.

The DVC specific completion script is located in this path of our main
repository:
[dvc/scripts/completion/dvc.bash](https://github.com/iterative/dvc/blob/master/scripts/completion/dvc.bash)

You will need to place this script into `/etc/bash_completion.d/` (or
`/usr/local/etc/bash_completion.d/` on a Mac):

For example:

```dvc
$ sudo wget \
    -O /etc/bash_completion.d/dvc \
    https://raw.githubusercontent.com/iterative/dvc/master/scripts/completion/dvc.bash
```

On a Mac, add the following to your `~/.bash_profile`:

```bash
if [ -f $(brew --prefix)/etc/bash_completion ]; then
    . $(brew --prefix)/etc/bash_completion
fi
```

You can `source` your `~/.bash_profile` or launch a new terminal to utilize
completion.

<details>

### Click to expand if it doesn't work on Debian/Ubuntu

As mentioned above, it should work out of the box. But if it does not, try these
steps:

- Make sure that the package `bash-completion` is installed:

  ```dvc
  $ sudo apt install --reinstall bash-completion
  ```

- Make sure that it is enabled. Edit `~/.bashrc` and make sure that these lines
  are there:

  ```bash
  # enable bash completion in interactive shells
  if ! shopt -oq posix; then
    if [ -f /usr/share/bash-completion/bash_completion ]; then
      . /usr/share/bash-completion/bash_completion
    elif [ -f /etc/bash_completion ]; then
      . /etc/bash_completion
    fi
  fi
  ```

- Exit from the shell and open a new one, or just reload `~/.bashrc`:

  ```dvc
  $ source ~/.bashrc
  ```

For more details see: https://linuxhandbook.com/enable-tab-completion/

</details>

## Configure Zsh

The DVC specific completion script is located in this path of our main
repository:
[dvc/scripts/completion/dvc.zsh](https://github.com/iterative/dvc/blob/master/scripts/completion/dvc.zsh)

Place the completion script in a directory included in `$fpath`, the file name
should be `_dvc`.

For example:

```dvc
$ sudo wget \
    -O /usr/share/zsh/site-functions/_dvc \
    https://raw.githubusercontent.com/iterative/dvc/master/scripts/completion/dvc.zsh
```

Make sure `compinit` is loaded or do it by adding in `~/.zshrc`:

```dvc
$ autoload -Uz compinit && compinit -i
```

Then reload your shell:

```dvc
$ exec $SHELL -l
```

This step is optional but will make look much nicer by adding more colors to it.
Add the following to your `~/.zshrc`:

```bash
# Case insensitive match
zstyle ':completion:*' matcher-list 'm:{a-zA-Z}={A-Za-z}' 'r:|[._-]=* r:|=*' 'l:|=* r:|=*'

# Group matches and describe.
zstyle ':completion:*:*:*:*:*' menu select
zstyle ':completion:*:matches' group 'yes'
zstyle ':completion:*:options' description 'yes'
zstyle ':completion:*:options' auto-description '%d'
zstyle ':completion:*:corrections' format ' %F{green}-- %d (errors: %e) --%f'
zstyle ':completion:*:descriptions' format ' %F{yellow}-- %d --%f'
zstyle ':completion:*:messages' format ' %F{purple} -- %d --%f'
zstyle ':completion:*:warnings' format ' %F{red}-- no matches found --%f'
zstyle ':completion:*:default' list-prompt '%S%M matches%s'
zstyle ':completion:*' format ' %F{yellow}-- %d --%f'
zstyle ':completion:*' group-name ''
zstyle ':completion:*' verbose yes
```
