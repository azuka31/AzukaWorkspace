## FZF Development Setup Documentation

This document provides a comprehensive guide to setting up and customizing `fzf` for Zsh, integrating powerful tools like `fd`, `bat`, and `eza` to enhance the file search and selection process within the command-line environment.

### Prerequisites

Before you begin, ensure you have the following tools installed:
- **fzf**: The core fuzzy finder utility.
- **fd**: A simple, fast, and user-friendly alternative to `find`.
- **bat**: A `cat` clone with syntax highlighting and Git integration.
- **eza**: For directory tree visualizations.

You can install these tools using your package manager. For example, on macOS with Homebrew:

```sh
brew install fzf fd bat
```

For `eza`, follow the installation instructions on its repository page.

### Configuration

Add the following configurations to your `.zshrc` file to enable `fzf` functionalities.

#### Basic Setup

Initialize `fzf` for Zsh:

```sh
eval "$(fzf --zsh)"
```

This command sets up key bindings and autocompletion.

#### Advanced Key Bindings and Options

Set up `fzf` with previews using `fd`, `bat`, and `eza`:

```sh
# Show file or directory previews
show_file_or_dir_preview="if [ -d {} ]; then eza --tree --color=always {} | head -200; else bat -n --color=always --line-range :500 {}; fi"

export FZF_CTRL_T_OPTS="--preview '$show_file_or_dir_preview'"
export FZF_ALT_C_OPTS="--preview 'eza --tree --color=always {} | head -200'"
```

These settings configure `fzf` to show previews when searching files or directories. Directories are previewed with `eza` and files with `bat`.

#### Custom Command-Specific Options

Customize `fzf` behavior for specific commands using the `_fzf_comprun` function:

```sh
_fzf_comprun() {
  local command=$1
  shift

  case "$command" in
    cd)           fzf --preview 'eza --tree --color=always {} | head -200' "$@" ;;
    export|unset) fzf --preview "eval 'echo ${}'"         "$@" ;;
    ssh)          fzf --preview 'dig {}'                   "$@" ;;
    *)            fzf --preview "$show_file_or_dir_preview" "$@" ;;
  esac
}
```

This function allows you to define custom previews and options depending on the command being run.

### Theming fzf

Customize the appearance of `fzf` to fit your terminal's color scheme:

```sh
# fzf color theme configuration
fg="#CBE0F0"
bg="#011628"
bg_highlight="#143652"
purple="#B388FF"
blue="#06BCE4"
cyan="#2CF9ED"

export FZF_DEFAULT_OPTS="--color=fg:${fg},bg:${bg},hl:${purple},fg+:${fg},bg+:${bg_highlight},hl+:${purple},info:${blue},prompt:${cyan},pointer:${cyan},marker:${cyan},spinner:${cyan},header:${cyan}"
```

These settings adjust the foreground, background, and highlight colors of `fzf` based on your preferences.

With these configurations, `fzf` is not only set up for effective file and directory searching but also tailored for various command-specific enhancements. This setup ensures a more productive and visually pleasing command-line environment.

### Bat Configuration

To see your available themes interactively, you can use following code:
```sh
bat --list-themes | fzf --preview="bat --theme={} --color=always ~/.zshrc"
```


Enhance file viewing with `bat`:

```sh
# Set the theme for bat
export BAT_THEME="tokyonight_night"
```

`bat` is configured to use the `tokyonight_night` theme, which provides a pleasing and easy-to-read syntax highlighting in the terminal.

### Eza Configuration

Replace the traditional `ls` with `eza` for an enriched directory listing:

```sh
# Customize ls to use eza for an enhanced listing
alias ls="eza --long --git --no-filesize --icons=always --no-time --no-user --no-permissions"
```

This alias configures `eza` to provide detailed directory listings with visual enhancements such as icons, while hiding less essential information like file size, timestamps, user details, and permissions for a cleaner view.




