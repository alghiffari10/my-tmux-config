# My tmux Configuration

A minimal and productive tmux setup focused on fast navigation, Neovim workflow, and fuzzy searching with fzf.

---

## Features

- Mouse support
- Vim-style pane navigation
- Easy pane splitting
- Fast config reload
- 256-color support
- Zsh as default shell
- Fuzzy file search with preview
- Open files directly in current pane
- Directory picker with fzf

---

## Keybindings

### Pane Navigation

| Key | Action |
|---|---|
| `Ctrl+h` | Move left |
| `Ctrl+j` | Move down |
| `Ctrl+k` | Move up |
| `Ctrl+l` | Move right |

---

### Split Panes

| Key | Action |
|---|---|
| `Prefix + \|` | Split horizontally |
| `Prefix + -` | Split vertically |

---

### Reload tmux Config

| Key | Action |
|---|---|
| `Prefix + r` | Reload `.tmux.conf` |

---

### Fuzzy File Search

| Key | Action |
|---|---|
| `Prefix + s` | Open file in new tmux window |
| `Prefix + a` | Open file in current pane |

Uses:

- fzf
- bat
- Neovim

with live preview support.

---

### Directory Picker

| Key | Action |
|---|---|
| `Prefix + d` | Fuzzy search directories |

Requires:

```bash
~/.tmux-cd.sh
```

---

## Requirements

Install required packages:

### Ubuntu

```bash
sudo apt install tmux fzf bat
```

Optional but recommended:

```bash
sudo apt install ripgrep fd-find
```

---

## tmux.conf

```tmux
# Enable mouse
set -g mouse on

# Vim-style pane navigation
bind -n C-h select-pane -L
bind -n C-j select-pane -D
bind -n C-k select-pane -U
bind -n C-l select-pane -R

# Split panes
bind | split-window -h
bind - split-window -v
unbind '"'
unbind %

# Reload config
bind r source-file ~/.tmux.conf \; display-message "Config reloaded!"

# Colors
set -g default-terminal "tmux-256color"
set -ga terminal-overrides ",xterm-256color:Tc"

# Window and pane indexing from 1
set -g base-index 1
setw -g pane-base-index 1

# Status bar
set -g status-left "#[fg=green]#H"
set -g status-right "#[fg=yellow]%Y-%m-%d #[fg=cyan]%H:%M"

# Force tmux to use zsh as the default shell
set-option -g default-shell /usr/bin/zsh
set-option -g default-command /usr/bin/zsh

# Open fzf in a popup with preview
bind s run-shell "tmux split-window -v -c '#{pane_current_path}' \"fzf --preview 'bat --style=numbers --color=always {} | head -500' --bind 'enter:execute(tmux new-window -c \\\"$(dirname {})\\\" \\\"nvim {}\\\")'\""

# Open file in current pane
bind a run-shell "tmux split-window -v -c '#{pane_current_path}' \"fzf --preview 'bat --style=numbers --color=always {} | head -500' --bind 'enter:execute(tmux send-keys -t ! \\\"nvim {}\\\" C-m)+abort'\""

# fzf => pick directory => cd
bind d run-shell "~/.tmux-cd.sh"
```

---

## Credits

- tmux
- fzf
- Neovim
- bat
