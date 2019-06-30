## Δ
  A syntax-highlighter for git.

  _(This project is at a very early stage, but please feel free to open issues.)_

<img width=500px style="border: 1px solid black"
    src="https://user-images.githubusercontent.com/52205/60282969-230d2c80-98d6-11e9-8656-09073c8a0985.png"
    alt="image" />

## Usage
```
delta 0.1.0
A syntax-highlighter for git. Use 'delta | less -R' as core.pager in .gitconfig

USAGE:
    delta [FLAGS] [OPTIONS]

FLAGS:
        --compare-themes    Compare available syntax highlighting themes. To use this option, supply git diff output to
                            delta on standard input. For example: `git show --color=always | delta --compare-themes`.
        --dark              Use colors appropriate for a dark terminal background.  For more control, see --theme,
                            --plus-color, and --minus-color.
    -h, --help              Prints help information
        --light             Use colors appropriate for a light terminal background. For more control, see --theme,
                            --plus-color, and --minus-color.
        --list-languages    List supported languages and associated file extensions.
        --list-themes       List available syntax highlighting themes.
    -V, --version           Prints version information

OPTIONS:
        --minus-color <minus_color>    The background color (RGB hex) to use for removed lines.
        --plus-color <plus_color>      The background color (RGB hex) to use for added lines.
        --theme <theme>                The syntax highlighting theme to use.
    -w, --width <width>                The width (in characters) of the diff highlighting. By default, the highlighting
                                       extends to the last character on each line.
```

## Installation

1. **Install the Rust development environment:**<br>
    See https://www.rust-lang.org/tools/install.

2. **Clone this repo**<br>

3. **Build the executable:**<br>
    ```sh
    cd delta
    cargo build
    ```
    This creates an executable inside the repo at `target/debug/delta`. Make sure this executable is found on your shell
    `$PATH`.

    For example, if `~/bin` is in your `$PATH`, then you could use a symlink:
    ```
    cd ~/bin
    ln -s /path/to/delta/target/debug/delta delta
    ```

    Alternatively, you can ignore `$PATH` and use
    `/path/to/delta/target/debug/delta | less -R` in the next step.

4. **Configure git to use delta:**<br>
    (Note that delta defaults to a light theme, so if you're using a dark terminal background, you'll want to use `--dark` or `--theme=<theme-name>`.)

    Edit your `~/.gitconfig`:
    ```
    [core]
        pager = delta | less -R
    ```
    Alternatively, run this command:
    ```
    git config --global core.pager 'delta | less -R'
    ```

    You can pass arguments to delta in your `.gitconfig`. An example is
    ```
    [core]
        pager = delta --width=100 --plus-color="#012800" --minus-color="#340001" --theme="base16-ocean.dark" | less -R
    ```
    Please include the `=` characters; I'm not sure why yet, but they're necessary when writing a delta command line in `.gitconfig`!

All git commands that display diff output should now display syntax-highlighted output. For example:
  - `git diff`
  - `git show`
  - `git log -p`
  - `git stash show -p`

## 24 bit color

  delta works best if your terminal application supports 24 bit colors. See https://gist.github.com/XVilka/8346728. For example, on macos, iTerm2 works but Terminal.app does not.

  If you're using tmux, it's worth checking that 24 bit color is  working correctly. For example, run a color test script like [this  one](https://gist.githubusercontent.com/lifepillar/09a44b8cf0f9397465614e622979107f/raw/24-bit-color.sh),  or the others listed at https://gist.github.com/XVilka/8346728. If  you do not see smooth color gradients, see the discussion at  [tmux#696](https://github.com/tmux/tmux/issues/696). The short  version is you need something like this in your `~/.tmux.conf`:
  ```
  set -ga terminal-overrides ",xterm-256color:Tc"
  ```
  and you may then  need to quit tmux completely for it to take effect.

## Credit
  https://github.com/trishume/syntect<br>
  https://github.com/sharkdp/bat