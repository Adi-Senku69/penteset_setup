# Parrot OS Pentest Setup — Ansible

Automated setup for a Parrot OS pentest environment using Ansible roles.

## Usage

```bash
# Install Ansible
sudo apt install ansible -y

# List all the tools that are going to be installed
ansible-playbook site.yml --list-tags

# Run the full playbook
ansible-playbook site.yml --ask-become-pass

# Run a single role only
ansible-playbook site.yml --ask-become-pass --tags neovim
ansible-playbook site.yml --ask-become-pass --tags zsh,powerlevel10k

# Skip installation of certian modules
ansible-playbook site.yml --ask-become-pass --skip-tags neovim
ansible-playbook site.yml --ask-become-pass --skip-tags neovim,zsh --tags zellij
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
| `tmux`         | Installs tmux (apt), deploys .tmux.conf, clones TPM + catppuccin v2.1.3, installs all plugins headlessly |
| `lazygit`      | Downloads latest release binary from GitHub to ~/.local/bin                        |
| `custom_tools` | Clones pentest tools to ~/opt; installs Go runtime, RustScan, feroxbuster, subfinder, tealdeer, ligolo-ng (proxy + agent), chisel (Linux + Windows), mimikatz, Rubeus, kerbrute (Linux + Windows), dnscat2 (server + client built from source), curlie, posting, RustHound-CE, and udpx |

# Custom Pentest Tools

A collection of custom penetration testing tools included in this environment.

---

## Cloned Tools

| Tool | Category | Description |
|------|----------|-------------|
| **FinalRecon** | Reconnaissance | All-in-one web reconnaissance tool for gathering information about a target |
| **LaZagne** | Credential Harvesting | Retrieves stored passwords from local applications |
| **PCredz** | Credential Harvesting | Extracts credentials from network captures (pcap files or live interfaces) |
| **PEASS-ng** | Privilege Escalation | Privilege escalation awesome scripts suite (LinPEAS/WinPEAS) for local enumeration |
| **enum4linux-ng** | Enumeration | SMB/Samba enumeration tool — next-generation rewrite of enum4linux |
| **firefox_decrypt** | Credential Harvesting | Extracts credentials stored in Mozilla Firefox profiles |
| **impacket** | Exploitation / Lateral Movement | Python library for working with network protocols; includes tools like psexec, secretsdump, etc. |
| **odat** | Exploitation | Oracle Database Attacking Tool for testing Oracle DB instances |
| **username-anarchy** | Reconnaissance | Generates username permutations from a target's name for use in attacks |
| **KeyTabExtract** | Exploitation | Extracts hashes from keytab files |
| **Linikatz** | Exploitation | Linux version of mimikatz for credential extraction on Linux |
| **PKINITtools** | Exploitation | Tools for pass-the-certificate attacks via PKINIT |
| **Pywhisker** | Exploitation | Python tool for Shadow Credentials attacks via msDS-KeyCredentialLink |
| **smtp-enum** | Enumeration | SMTP user enumeration tool for discovering valid usernames via VRFY, EXPN, and RCPT TO commands |
| **Subbrute** | Reconnaissance | Fast subdomain brute-forcing tool using open resolvers |
| **Certipy** | Exploitation | Tool for enumerating and abusing Active Directory Certificate Services |
| **Invoke-TheHash** | Exploitation | PowerShell pass-the-hash attack tools using .NET invoke |
| **KRBRelayX** | Exploitation | Kerberos relaying and unconstrained delegation abuse toolkit |
| **dnscat2** | C2 / Tunneling | DNS-based C2 and tunneling tool; server (Ruby) and client (C) built from source |
| **dnscat2-powershell** | C2 / Tunneling | PowerShell client for dnscat2; connects Windows targets back to a dnscat2 server |
| **bloodyAD** | Exploitation | Active Directory privilege escalation tool for abusing AD misconfigurations |
| **targetedKerberoast** | Exploitation / Kerberos | Kerberoast attack tool that targets specific accounts by setting SPNs via DACL abuse |

## Binary Tools

| Tool | Category | Description |
|------|----------|-------------|
| **RustScan** | Reconnaissance | Fast port scanner; pipes results into nmap |
| **feroxbuster** | Reconnaissance | Fast content discovery / directory brute-forcing tool |
| **subfinder** | Reconnaissance | Passive subdomain enumeration tool |
| **tealdeer** | Utility | Fast tldr client for simplified man pages (installed via cargo) |
| **ligolo-ng proxy** | Tunneling / Pivoting | Attacker-side proxy for ligolo-ng tunneling |
| **ligolo-ng agent** | Tunneling / Pivoting | Target-side agent for ligolo-ng tunneling (also copied to ~/opt) |
| **chisel** | Tunneling / Pivoting | Fast TCP/UDP tunnel over HTTP; both Linux and Windows binaries decompressed to ~/opt/chisel/ |
| **mimikatz** | Credential Harvesting | Post-exploitation tool for extracting plaintext passwords, hashes, and Kerberos tickets from Windows |
| **Rubeus** | Exploitation / Kerberos | C# toolset for raw Kerberos interaction and abuse |
| **kerbrute** | Exploitation / Kerberos | Fast Kerberos brute-forcing and user enumeration tool |
| **curlie** | Utility | curl frontend with httpie-like syntax highlighting and formatting |
| **posting** | Utility | TUI HTTP client for testing and debugging APIs |
| **RustHound-CE** | Reconnaissance / AD | BloodHound CE data collector written in Rust for Active Directory enumeration |
| **Go** | Runtime | Go language runtime; required by several tools and added to PATH |
| **udpx** | Reconnaissance | Fast UDP port scanner; installed via `go install` |



## Notes

- **aliases.zsh** — place it alongside `site.yml` and it will be copied to `~/.oh-my-zsh/custom/`
- **Cargo builds** (yazi, zellij) run after Rust is installed and will take a few minutes
- After the playbook completes, log out and back in (or `exec zsh`) to activate all PATH changes
