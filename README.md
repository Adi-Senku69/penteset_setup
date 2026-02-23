# Parrot OS Pentest Setup — Ansible

Automated setup for a Parrot OS pentest environment using Ansible roles.

## Usage

```bash
# Install Ansible
sudo apt install ansible -y

# Run the full playbook
ansible-playbook site.yml --ask-become-pass

# Run a single role only
ansible-playbook site.yml --ask-become-pass --tags neovim
ansible-playbook site.yml --ask-become-pass --tags zsh,powerlevel10k
```

## Configuration

Edit `group_vars/all.yml` to customize:
- `alacritty_config_repo` — your alacritty dotfiles repo
- `nvim_config_repo` — your neovim config repo
- `zsh_plugins` — add/remove zsh plugins (just name + repo)
- `jetbrains_nerd_font_version` — bump font version

## What Gets Installed

| Role           | What it does                                                                        |
|----------------|-------------------------------------------------------------------------------------|
| `system`       | Updates apt, installs core tools (git, fzf, ripgrep, fd, jq, zoxide, luarocks…)   |
| `rust`         | Installs rustup + cargo                                                             |
| `fonts`        | Downloads JetBrainsMono Nerd Font, rebuilds font cache                              |
| `alacritty`    | Installs alacritty (apt → cargo fallback), clones config via ngrok, sets default   |
| `zsh`          | oh-my-zsh, sets zsh as login shell, deploys .zshrc template, plugins, aliases      |
| `powerlevel10k`| Clones p10k theme into oh-my-zsh custom themes                                     |
| `neovim`       | Downloads latest Neovim tarball, symlinks to ~/.local/bin, clones config via ngrok |
| `uv`           | Installs uv Python package manager, adds shell completion                           |
| `yazi`         | Downloads pre-built binary from GitHub releases to ~/.local/bin                     |
| `zellij`       | Downloads pre-built binary from GitHub releases to ~/.local/bin, dumps default config |
| `lazygit`      | Downloads latest release binary from GitHub to ~/.local/bin                        |
| `custom_tools` | Clones pentest tools to ~/opt, installs rustscan (.deb) and feroxbuster (binary)   |

## Notes

- **aliases.zsh** — place it alongside `site.yml` and it will be copied to `~/.oh-my-zsh/custom/`
- **Cargo builds** (yazi, zellij) run after Rust is installed and will take a few minutes
- After the playbook completes, log out and back in (or `exec zsh`) to activate all PATH changes
