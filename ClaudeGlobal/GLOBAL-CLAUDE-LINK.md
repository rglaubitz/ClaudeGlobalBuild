# Global Claude Configuration Link

## What is `claude-global-symlink`?

The `claude-global-symlink` is a symbolic link (shortcut) that points to the global `~/.claude/` directory on your system.

## Purpose

This symlink allows you to:
1. **View** the global Claude configuration from within this project
2. **Track** changes being made to the global config
3. **Reference** the structure when implementing phases
4. **Debug** configuration issues more easily

## Important Notes

- **This is the ACTUAL global directory** - changes here affect ALL Claude Code sessions
- The symlink is just a reference - deleting it won't delete the actual `~/.claude/` directory
- When implementing phases, we're modifying the REAL global config through this link

## Current Structure

```
~/.claude/ (via claude-global-symlink/)
├── agents/          # Agent definitions
├── backups/         # System backups
├── commands/        # Custom commands (Phase 2 target)
├── docker/          # Docker configurations
├── events/          # Event bus config
├── health/          # Health monitoring
├── logs/            # System logs
├── memory/          # Persistent memory files
├── projects/        # Per-project configs
├── scripts/         # Automation scripts
├── templates/       # Reusable templates
├── tests/           # Test suites
└── settings.*.json  # Permission configs
```

## Phase 2 Implementation

When we implement Phase 2, we'll be creating/modifying files in:
- `claude-global-symlink/commands/` → Creates in `~/.claude/commands/`
- This ensures the commands are available globally across all Claude sessions