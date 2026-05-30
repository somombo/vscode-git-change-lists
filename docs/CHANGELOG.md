# Changelog

All notable changes to the Smart Commit extension will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Fixed
- Fixed extension activation deadlock caused by synchronously blocking on the Git extension initialization (#1).
- Fixed potential data loss where change list file assignments were wiped on startup before the Git repository finished loading (#1).
- Fixed packaging bug that excluded runtime dependencies, preventing the extension from activating (#1).

---

## [1.0.0] - 2026-02-01

### Added
- Initial release of Smart Commit.
- Custom Named Change Lists.
- Integrated Staging Workflow.
- Commit Guard (prevent mixed commits).
- List and Tree View modes.
- Status Bar integration.
- Patch Management (Create/Apply).
- Auto-assignment of changes to active list.

#### Core Features
- **Change List Management**
  - Create custom named change lists to organize uncommitted changes
  - Rename change lists with uniqueness validation
  - Delete change lists with confirmation for non-empty lists
  - Default change list that cannot be deleted (captures unassigned changes)

- **Active List System**
  - Set any change list as "active" to receive new changes automatically
  - Only one list can be active at a time (singleton enforcement)
  - Active status persists across VS Code restarts
  - Active list displayed in status bar with quick-switch dropdown

- **File Organization**
  - Drag-and-drop files between change lists with visual feedback
  - Move files via context menu "Move to Change List"
  - Multi-select support for moving multiple files at once
  - Auto-assignment of externally staged files to active list

- **View Modes**
  - **List Mode**: Flat view of all files (optimized for quick scanning)
  - **Tree Mode**: Hierarchical view preserving project structure
  - Toggle between modes with persistence of view state
  - Expansion state remembered per-mode across sessions

- **Git Integration**
  - Real-time synchronization with Git state changes
  - Detection of external staging operations (command line, file explorer)
  - Post-commit cleanup (automatically removes committed files from lists)
  - File status mapping (Modified, Added, Deleted, Renamed, etc.)
  - Compatible with VS Code's built-in Git extension

- **Patch Management**
  - Create patches from change lists (Copy to Clipboard / Save to File)
  - Apply patches to specific change lists (From Clipboard / From File)
  - Support for untracked files in patches
  - Robust handling of non-ASCII filenames and quoted paths

- **Commit Guard Service**
  - Validates staged files before commit
  - Warns when files from multiple change lists are staged
  - Options: Unstage extra files, Commit anyway, or Cancel
  - Guarded commit command with keybinding (`Ctrl+Enter`/`Cmd+Enter`)
  - Configuration to intercept native commit command

- **User Interface**
  - Tree view in SCM panel showing change lists and files
  - Status bar integration with active list display
  - Context menus for list operations (rename, delete, stage, set active)
  - Context menus for file operations (move, open diff)
  - Toolbar buttons (create list, toggle view, refresh)
  - Command palette integration for all operations

- **Configuration System**
  - 8 configurable settings via VS Code settings UI
  - Settings for view mode, status bar, confirmations, auto-activation
  - Commit guard settings (enabled, intercept commit)
  - Auto-assign staged files setting
  - Debug logging control

- **Debug Logging**
  - Configurable logging system with multiple levels (DEBUG, INFO, WARN, ERROR, EVENT)
  - Output to "Smart Commit" channel in Output panel
  - Optional verbose DEBUG logging for troubleshooting
  - Async logging with no file I/O overhead
  - See [DEBUGGING.md](DEBUGGING.md) for details

#### Developer Features
- TypeScript codebase with strict type checking
- Event-driven architecture with proper disposables
- State persistence using workspace storage
- Schema versioning for future migrations
- Comprehensive logging for debugging
- Well-organized project structure (`/services`, `/providers`, `/commands`, `/types`, `/utils`)

### Technical Details

#### Architecture
- **Services**: `ChangeListManager`, `GitService`, `CommitGuardService`, `ConfigService`
- **Providers**: `TreeDataProvider`, `TreeDragAndDropController`
- **Commands**: 11 commands registered with proper context awareness
- **Types**: Strongly typed interfaces for `ChangeList`, `TrackedChange`, `TreeNode` variants
- **State Management**: JSON serialization with atomic writes to `workspaceState`

#### Dependencies
- Runtime: `simple-git@^3.x`, `uuid@^9.x`
- Dev: TypeScript, ESLint, VS Code types, VSCE

#### Compatibility
- **Minimum VS Code**: 1.103.0
- **Tested On**: VS Code, Cursor, Kiro, Windsurf, Trae, VSCodium, Google Antigravity
- **Git Extension**: Requires `vscode.git` extension (built-in)
- **Multi-Root Workspaces**: Prepared (isolated state per workspace folder)

### Known Limitations

- **Commit Guard**: Cannot intercept native commit button click (only keybindings work)
- **Untracked Files**: No special handling yet (appear in Default list)
- **Change List Colors**: Not yet supported (planned for v0.1.0)

### Documentation

- Comprehensive README with installation, features, and usage
- Installation guide for all supported editors
- User guide with workflows and best practices
- Complete feature documentation
- Configuration reference
- Debugging guide
- FAQ for common questions
- Contributing guide for developers
- Development setup guide
- Technical architecture documentation (OVERVIEW.md, BLUEPRINT.md)

### Notes

- This is an early release (0.0.1) with core functionality complete
- Active development continues - see [Roadmap](../README.md#roadmap)
- Feedback and contributions welcome on [GitHub](https://github.com/maxinne-dev/vscode-smart-commit)

---

## Version History

### Versioning Strategy

Smart Commit follows [Semantic Versioning](https://semver.org/):
- **Major (1.0.0)**: Breaking changes, major rewrites
- **Minor (0.x.0)**: New features, backwards-compatible
- **Patch (0.0.x)**: Bug fixes, documentation, minor improvements

### Release Cadence

- **Patch releases**: As needed for critical bugs
- **Minor releases**: Monthly to quarterly with new features
- **Major releases**: When significant breaking changes are necessary

---

## Upgrade Guide

### From Future Versions

No upgrades available yet. This is the initial release.

When upgrading in the future:
1. Check this CHANGELOG for breaking changes
2. Review new configuration options
3. Update your `settings.json` if needed
4. Test with your workflow before relying on new features

---

## Contributing to the Changelog

When contributing to Smart Commit, please update this changelog as part of your PR:

1. Add your changes under the `[Unreleased]` section
2. Categorize changes as: `Added`, `Changed`, `Deprecated`, `Removed`, `Fixed`, `Security`
3. Use clear, user-facing language (not technical jargon)
4. Link to relevant issues or PRs

Example:
```markdown
## [Unreleased]

### Added
- Support for change list colors (#42)

### Fixed
- Crash when renaming active list (#38)
```

Maintainers will move unreleased changes to versioned sections during releases.

---

## Links

- [GitHub Repository](https://github.com/maxinne-dev/vscode-smart-commit)
- [Issue Tracker](https://github.com/maxinne-dev/vscode-smart-commit/issues)
- [Releases](https://github.com/maxinne-dev/vscode-smart-commit/releases)
- [Documentation](../README.md)

[Unreleased]: https://github.com/maxinne-dev/vscode-smart-commit/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/maxinne-dev/vscode-smart-commit/releases/tag/v1.0.0
