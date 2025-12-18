# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a DDEV add-on that installs Claude Code natively inside DDEV containers. It provides a self-contained binary installation (no npm/Node.js dependency) that persists configuration across container restarts.

## Architecture

### Installation Flow
The add-on uses DDEV's `install.yaml` to copy files into the project's `.ddev` directory:
- `web-build/Dockerfile.claude-code-native` - Docker build instructions for native Claude Code installation
- `commands/web/claude` - DDEV command wrapper for Claude Code
- `commands/web/glab` - DDEV command wrapper for GitLab CLI
- `config.claude-code-native.yaml` - Post-start hooks and environment configuration
- `claude-code/.claude.json` - Persisted Claude Code configuration

### Configuration Persistence
The `config.claude-code-native.yaml` post-start hooks create symlinks to persist Claude Code configuration:
- `~/.claude.json` → `/var/www/html/.ddev/claude-code/.claude.json`
- `~/.claude/` → `/var/www/html/.ddev/claude-code/.claude/`

This ensures configuration survives `ddev restart` by storing it in the project's `.ddev` directory (which is mounted as a volume).

### GitLab Integration
GitLab CLI (glab) is installed alongside Claude Code. Configuration is stored in `.ddev/.glab-cli/` via the `GLAB_CONFIG_DIR` environment variable set in `config.claude-code-native.yaml`.

## Development Commands

### Testing the Add-on
```bash
# Install the add-on in a DDEV project
ddev add-on get Gonzalo2683/claude-code-native
ddev restart

# Use Claude Code
ddev claude
ddev claude --help

# Configure GitLab integration
ddev glab auth login
```

### File Structure
- `install.yaml` - Defines which files are copied during `ddev get` (add-on name: `claude-code-native`)
- `web-build/Dockerfile.claude-code-native` - Docker build file for native Claude Code installation
- `web-build/claude-code-native.gitignore` - Gitignore for persisted config
- `commands/web/` - DDEV command wrappers
- `config.claude-code-native.yaml` - Post-start hooks and environment configuration
- `claude-code/` - Default configuration templates

## Key Design Decisions

1. **Native Binary Installation**: Uses `curl -fsSL https://claude.ai/install.sh | bash` instead of npm to avoid Node.js dependency
2. **PATH Management**: All scripts ensure `$HOME/.local/bin` is in PATH where the native binary is installed
3. **Configuration Persistence**: Symlinks from home directory to project `.ddev` directory enable stateful configuration
4. **GitLab CLI Bundling**: Includes glab to enable GitLab integration for Claude Code

## Important Notes

- All generated files should include `#ddev-generated` comment so they can be safely replaced by future `ddev get` commands
- The `.claude.json` file starts empty (`{}`) - Claude Code populates it on first run during authentication
- The add-on is designed to work with Drupal projects via the `drupal/claude_code` module
