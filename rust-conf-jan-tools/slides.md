---
# try also 'default' to start simple
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/s
title: Useful tools in Rust (for Rust)
class: "text-center"
fonts:
  mono: "JetBrainsMono Nerd Font"
# some information about the slides, markdown enabled

mdc: true
---

# Useful tools for Rust (in Rust)

David Marek Manca

---

# Types of Tools
- IDEs/Editors
- Tools helping to program Rust
- Anything helping you develop Rust
- Not all have to be written in Rust (but I tried to find those that are)

---

# Default Tools - rustup completions
Basics first!

- This gives you the tutorial
```zsh
$ rustup completions 
```

short tutorial: 
```zsh
$ mkdir ~/.zfunc
$ rustup completions zsh > ~/.zfunc/_rustup
$ rustup completions zsh cargo > ~/.zfunc/_cargo
```

```zsh
# ~/.zshrc
fpath+=~/.zfunc
autoload -Uz compinit
compinit
```

---

# Default Tools - rustup completions
Result

```zsh
󰀵 david  …/presentations/rust-conf-jan-tools   main ?   v23.6.1   14:13 
 rustup
check           -- Check for updates to Rust toolchains and rustup
completions     -- Generate tab-completion scripts for your shell
component       -- Modify a toolchain's installed components
default         -- Set the default toolchain
doc             -- Open the documentation for the current toolchain
dump-testament  -- Dump information about the build
help            -- Print this message or the help of the given subcommand(s)
install         -- Update Rust toolchains
man             -- View the man page for a given command
override        -- Modify toolchain overrides for directories
run             -- Run a command with an environment configured for a given toolchain
self            -- Modify the rustup installation
set             -- Alter rustup settings
show            -- Show the active and installed toolchains or profiles
target          -- Modify a toolchain's supported targets
toolchain       -- Modify or query the installed toolchains
uninstall       -- Uninstall Rust toolchains
update          -- Update Rust toolchains and rustup
which           -- Display which binary will be run for a given command
```




