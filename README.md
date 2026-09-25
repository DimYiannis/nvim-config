# nvim-config

My [LazyVim](https://www.lazyvim.org) setup for Neovim.

## What's in it

| File | Purpose |
| --- | --- |
| `lua/config/options.lua` | 4-space indent, spaces instead of tabs |
| `lua/plugins/colorscheme.lua` | Gruvbox colorscheme |
| `lua/plugins/explorer.lua` | File explorer (`<leader>e`) shows hidden and gitignored files |
| `lua/plugins/python.lua` | Disables pyright; Python uses Ruff from the LazyVim Python extra |
| `lua/plugins/42_header.lua` | 42 school header (`<F1>` or `:Stdheader`, auto-updates on save) |
| `lazyvim.json` | Enabled LazyVim extras: json, markdown, python, toml, cmake |
| `lazy-lock.json` | Pinned plugin versions |
| `cheatsheet.html` | Keybinding cheatsheet; open it in a browser |

## Requirements

- Neovim **>= 0.11.2**
- Git **>= 2.19**
- A C compiler (for treesitter parsers)
- `curl`, `unzip`, `ripgrep`, `fd`, `fzf` (search and pickers)
- Node.js + npm (Mason installs the JSON and Markdown tools with npm)
- Python 3 with `venv` (Mason installs Ruff and the CMake tools with pip)
- A [Nerd Font](https://www.nerdfonts.com), set as your terminal font (needed for icons)
- `lazygit` (optional, for `<leader>gg`)

### macOS

```sh
xcode-select --install
brew install neovim git curl ripgrep fd fzf lazygit node python
brew install --cask font-jetbrains-mono-nerd-font
```

### Debian / Ubuntu

The `neovim` package from apt is usually too old, so install the official release instead:

```sh
sudo apt update
sudo apt install git build-essential curl unzip ripgrep fd-find fzf nodejs npm python3 python3-venv

# Neovim (x86_64; use nvim-linux-arm64 on ARM)
curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim-linux-x86_64.tar.gz
sudo rm -rf /opt/nvim-linux-x86_64
sudo tar -C /opt -xzf nvim-linux-x86_64.tar.gz
echo 'export PATH="$PATH:/opt/nvim-linux-x86_64/bin"' >> ~/.bashrc   # or ~/.zshrc

# Debian names the fd binary "fdfind"
mkdir -p ~/.local/bin && ln -sf "$(which fdfind)" ~/.local/bin/fd
```

### Arch

```sh
sudo pacman -S neovim git base-devel curl unzip ripgrep fd fzf lazygit nodejs npm python ttf-jetbrains-mono-nerd
```

## Install

1. Back up any existing Neovim config and data:

   ```sh
   mv ~/.config/nvim ~/.config/nvim.bak 2>/dev/null
   mv ~/.local/share/nvim ~/.local/share/nvim.bak 2>/dev/null
   mv ~/.local/state/nvim ~/.local/state/nvim.bak 2>/dev/null
   mv ~/.cache/nvim ~/.cache/nvim.bak 2>/dev/null
   ```

2. Clone this repo:

   ```sh
   git clone https://github.com/DimYiannis/nvim-config.git ~/.config/nvim
   ```

   On Windows, clone it to `%LOCALAPPDATA%\nvim` instead.

3. Start Neovim. lazy.nvim bootstraps itself and installs every plugin:

   ```sh
   nvim
   ```

4. Install the exact plugin versions pinned in `lazy-lock.json`:

   ```
   :Lazy restore
   ```

5. Restart Neovim and open any file. Mason then installs the language servers, formatters and `tree-sitter-cli`, and treesitter compiles its parsers. This runs in the background and takes a few minutes the first time. Check progress with `:Mason`.

6. Check that everything works:

   ```
   :checkhealth
   ```

7. On a new machine, update `user` and `mail` in `lua/plugins/42_header.lua` if you need different values there.

## Keeping machines in sync

After changing the config on any machine:

```sh
cd ~/.config/nvim
git add -A
git commit -m "describe the change"
git push
```

On the other machines:

```sh
cd ~/.config/nvim
git pull
```

Then run `:Lazy restore` in Neovim so plugins match the updated `lazy-lock.json`.

`:Lazy update` updates plugins and rewrites `lazy-lock.json`. Commit the new lock file so every machine gets the same versions.
