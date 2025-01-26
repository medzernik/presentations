---
# try also 'default' to start simple
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/s
title: Useful tools in Rust (for Rust)
class: "text-center"
fonts:
  mono: "TX-02, JetBrainsMono Nerd Font"
# some information about the slides, markdown enabled
transition: slide-left
mdc: true
---

# Useful tools for Rust (in Rust)

David Marek Manca

---

# Types of Tools
- CLI Tools
- IDEs/Editors
- Anything helping you develop with Rust

*Not all have to be written in Rust (but I tried to find those that are)*

---

# Default Tools - rustup & cargo completions setup
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

# Default Tools - rustup & cargo completions example
Result

```zsh
󰀵 david  …/presentations/rust-conf-jan-tools   main ?   v23.6.1   14:13 
$ rustup *tab* *tab*
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
---

# Clippy
The best linter
- Easy to use
- Can be integrated into a CI system

```zsh
# in a rust project
$ cargo clippy
```
- It also has an autofix
```zsh
$ cargo clippy --fix --allow-dirty #<-- Use this when already in dirty git tree
```


---

# Clippy Levels

<style scoped>
table {
  font-size: 11px;
}
</style>
| Category | Description | Default Level |
| --- | --- | --- |
| `clippy::all` | All lints that are on by default | warn/deny |
| `clippy::correctness` | Code that is outright wrong or useless | deny |
| `clippy::suspicious` | Code that is most likely wrong or useless | warn |
| `clippy::style` | Code that should be written in a more idiomatic way | warn |
| `clippy::complexity` | Code that does something simple but in a complex way | warn |
| `clippy::perf` | Code that can be written to run faster | warn |
| `clippy::pedantic` | Lints which are rather strict or have occasional false positives | allow |
| `clippy::restriction` | Lints which prevent the use of language and library features | allow |
| `clippy::nursery` | New lints that are still under development | allow |
| `clippy::cargo` | Lints for the cargo manifest | allow |

--- 

# Nextest
A better testing tool

- Faster
- Better interface
- CI compatible

```zsh
$ brew install cargo-nextest
```


Running tests:

```zsh
------------
 Nextest run ID 1f79aa0d-4ec8-4a5c-aa83-5e8dc2f36573 with nextest profile: default
    Starting 14 tests across 3 binaries (177 tests skipped)
        reporter::tests::on_test_finished_write_status_line
        PASS [   0.014s] nextest-runner reporter::tests::on_test_finished_dont_store_final
        PASS [   0.014s] nextest-runner reporter::tests::test_write_skip_counts
        PASS [   0.018s] nextest-runner reporter::tests::on_test_finished_dont_write_status_line
        PASS [   0.019s] nextest-runner reporter::tests::on_test_finished_store_final_4
------------
     Summary [   0.021s] 4 tests run: 4 passed, 187 skipped
```


---

# Bacon
Replacement for cargo-watch
- Integrates with nextest
- Has a summary view
- Autoadjusts to screensize
```zsh
$ brew install bacon 
$ bacon || $ bacon nextest
```

<img src="https://dystroy.org/bacon/img/vi-and-bacon.png"/>

---

# cargo-install
Update installed packages
- Using `cargo install` to install packages
```zsh
$ cargo install cargo-update
```

Usage:
```zsh
 cargo install-update --all
    Polling registry 'https://index.crates.io/'......

Package         Installed  Latest    Needs update
bacon           v3.7.0     v3.9.0    Yes
cargo-update    v16.0.0    v16.1.0   Yes
cargo-asm       v0.1.16    v0.1.16   No
cargo-expand    v1.0.100   v1.0.100  No
cargo-outdated  v0.16.0    v0.16.0   No
termscp         v0.16.1    v0.16.1   No
```

---

# cargo-expand
A macro expander
- Very useful to see what some macros expand to
- Doesn't give completely final code like HIR would
```zsh
$ cargo install cargo-expand
```
Usage:
```zsh
$ cargo expand
```
```rust {1,2,3,4,5,8}{lines:true,startLine:1}
#![feature(prelude_import)]
#[prelude_import]
use std::prelude::rust_2021::*;
#[macro_use]
extern crate std;
fn main() {
    {
        ::std::io::_print(format_args!("Hello, world!\n"));
    };
}
```
---

# cargo-outdated
Update major versions automatically
- Updating major versions is very annoying
- Maybe there is an automated way but I don't really know of one
```zsh
$ cargo install cargo-outdated
```
Usage:
```zsh
$ cargo outdated
Name             Project  Compat  Latest   Kind         Platform
----             -------  ------  ------   ----         --------
clap             2.20.0   2.20.5  2.26.0   Normal       ---
clap->bitflags   0.7.0    ---     0.9.1    Normal       ---
clap->libc       0.2.18   0.2.29  Removed  Normal       ---
clap->term_size  0.2.1    0.2.3   0.3.0    Normal       ---
clap->vec_map    0.6.0    ---     0.8.0    Normal       ---
num_cpus         1.6.0    ---     1.6.2    Development  ---
num_cpus->libc   0.2.18   0.2.29  0.2.29   Normal       ---
pkg-config       0.3.8    0.3.9   0.3.9    Build        ---
term             0.4.5    ---     0.4.6    Normal       ---
term_size->libc  0.2.18   0.2.29  0.2.29   Normal       cfg(not(target_os = "windows"))

```

---

# IDEs - RustRover
A JetBrains ecosystem IDE
- A fairly new IDE
- Sometimes unstable and bloated
- Free for personal use (new with JetBrains!)
- Has tools for web development (ideal for Tauri)

<img  src="https://www.jetbrains.com/rust/img/overview/Manage_your_project_and_project_dependencies-1.png"/>

--- 

# IDEs - Helix
A modal text editor
- Written in Rust
- Uses the Kakoune keyboard system
- Has great performance and featureset
- No plugins (yet)
- Uses LSP for all code insight
- Much easier to learn than Vim
- Gets frequently updated
- Very fast search, integrated git gutter, initial debugger support, **multicursor support** (!), treesitter
- No integrated AI tooling

Installation:
```zsh
$ brew install helix
```
---

# IDEs - Helix
LSP Support

Check the support:
```zsh
 hx --health rust

Configured language servers:
  ✓ rust-analyzer: /Users/david/.cargo/bin/rust-analyzer
Configured debug adapter: lldb-dap
Binary for debug adapter: /opt/homebrew/opt/llvm/bin/lldb-dap
Configured formatter: None
Tree-sitter parser: None
Highlight queries: ✘
Textobject queries: ✘
Indent queries: ✘
```

--- 

# IDEs - Helix

  <img src="https://miro.medium.com/v2/resize:fit:1400/1*ReH_8VajxDqiz4iEtoj4xA.png"/>

---

# IDEs - Zed
An "AI" editor
- Written in Rust
- Started out as a collaboration editor to make a proper livecoding system
- Jumped on the AI hype train
- Has a phenomenal VIM Mode 
- Basic Git integration and extension support
- Advanced collab tools - Voice chat, screen sharing, project sharing (Discord-like)
- Free to use ($10 monthly token credit for AI features)
- Source code on GitHub - available for MacOS, Linux and soon Windows (can be built from `master`)
- Will charge for some collaboration tooling and charges for AI usage exceeding $10 token value
- Very similair to Helix, but has a GUI, uses VIM motions (optionally), has better Git support and has AI tooling
- [https://zed.dev](https://zed.dev)

---

# IDEs - Zed

<div h="100%">
  <SlidevVideo v-click autoplay h="100%">
    <source 
    src="https://customer-snccc0j9v3kfzkif.cloudflarestream.com/cad3f9d2a307574344e41c199677609e/downloads/default.mp4" type="video/mp4" />
  </SlidevVideo>
</div>

---

# Terminal tools - starship
A better command prompt

- Written in Rust
- Integrates even into PowerShell
- Git integration, shows time, Rust/other language integration, etc
- **Requires Powerline fonts**

Installation:
```zsh
$ brew install starship

# ~/.zshrc
eval "$(starship init zsh)"
```

Usage:
```zsh
󰀵 david  …/showcase   master ?   v1.84.0   15:25

```
---

# Terminal tools - zellij
A better tmux
- Try without installing: ``$ bash <(curl -L https://zellij.dev/launch)``

<img h="82%" src="https://zellij.dev/img/tutorial-1-command-panes.png"/>

---

# Terminal tools - bottom
A better top? (lol)

```zsh
$ brew install bottom #binary is `btm`
```

<img h="80%" src="https://github.com/ClementTsang/bottom/raw/main/assets/demo.gif" />
