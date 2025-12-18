# ddev-claude-code

## Claude Code (Native Installation)

This add-on installs Claude Code **natively** inside the DDEV container (no npm dependency). It is accessible using `ddev claude`.

When you first run, you will be prompted to authenticate interactively. Your configuration will be stored in `.ddev/claude-code/.claude.json` and `.ddev/claude-code/.claude/`, and will be persisted across restarts.

You can copy in an existing `.ddev/claude-code/.claude.json`, for example if you want to use an existing key, or allowed functions etc.

### Installation

```bash
ddev add-on get Gonzalo2683/claude-code-native
ddev restart
```

### Usage

```bash
# Start Claude Code
ddev claude

# Get help
ddev claude --help
```

### GitLab Integration

To let Claude interact with GitLab, you will need to authenticate `glab`. This can be done using:

```bash
ddev glab auth login
```

Configuration will be stored in `.ddev/.glab-cli/` and will also be persisted across restarts.

## Drupal CLAUDE.md

For Drupal, we recommend using <https://www.drupal.org/project/claude_code>. You can install by running:

```bash
ddev composer config extra.drupal-scaffold.allowed-packages --json --merge '["drupal/claude_code"]'
ddev composer require --dev drupal/claude_code
```

## Benefits of Native Installation

- **No Node.js dependency** - Self-contained binary
- **Faster startup** - No npm overhead
- **Auto-updates** - Claude Code manages its own updates
- **Smaller footprint** - Single executable
