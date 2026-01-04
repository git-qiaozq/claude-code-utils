# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a collection repository for Claude Code-related utilities. It uses Git submodules to manage independent tool projects, each maintaining its own version control and development workflow. The repository is primarily for organizing and maintaining VS Code extensions that enhance the Claude Code experience, particularly for image pasting functionality in Windows/WSL environments.

## Repository Architecture

**Git Submodule Structure**: This repository does not contain application code itself. Instead, it aggregates multiple independent projects as Git submodules:

- **claude-code-image-paste-wsl/**: Enhanced fork with WSL path conversion, auto-save to project directory, and automatic cleanup
  - Repository: https://github.com/git-qiaozq/claude-code-image-paste-wsl
  - Main file: `extension.js` (VS Code extension entry point)
  - Build script: `build.sh` (packages extension to .vsix)

- **claude-image-paste/**: Original extension providing basic clipboard image paste functionality
  - Repository: https://github.com/aggroot/claude-image-paste
  - Main file: `extension.js`

Each submodule is a complete VS Code extension that runs in VS Code's Node.js environment without compilation.

## Common Commands

### Repository Management

```bash
# Clone repository with all submodules
git clone --recursive https://github.com/git-qiaozq/claude-code-utils.git

# Initialize submodules if cloned without --recursive
git submodule update --init --recursive

# Update all submodules to latest commits
git submodule update --remote --merge

# Update a specific submodule
cd claude-code-image-paste-wsl
git pull origin main
cd ..
git add claude-code-image-paste-wsl
git commit -m "Update submodule to latest version"
```

### Building Extensions

The submodule extensions are built independently:

```bash
# Build claude-code-image-paste-wsl (creates .vsix file)
cd claude-code-image-paste-wsl
./build.sh

# Manual build using vsce (if installed)
cd claude-code-image-paste-wsl
vsce package --allow-missing-repository
```

### Installing Extensions

```bash
# Install from VSIX in VS Code/Cursor
# Method 1: Command palette (Ctrl+Shift+P) → "Extensions: Install from VSIX..."
# Method 2: Command line
code --install-extension claude-code-image-paste-wsl/claude-code-image-paste-wsl-1.2.0.vsix
cursor --install-extension claude-code-image-paste-wsl/claude-code-image-paste-wsl-1.2.0.vsix
```

### Testing Extensions

```bash
# Develop/test an extension (launches Extension Development Host)
cd claude-code-image-paste-wsl
code .
# Press F5 in VS Code to launch development instance
```

## Key Technical Details

### Extension: claude-code-image-paste-wsl

- **Command ID**: `claude-image-paste.pasteImage`
- **Keyboard shortcut**: `Ctrl+Alt+V` (when terminal is open)
- **Platform**: Windows 10/11 with WSL2 (or native Windows)
- **Requirements**: PowerShell (built into Windows)
- **Supported image formats**: PNG, JPG, JPEG, GIF, BMP, WebP, SVG, ICO, TIFF

**Core functionality**:
- Uses PowerShell scripts for Windows clipboard access
- Automatically converts Windows paths to WSL format (e.g., `\\wsl$\Ubuntu\...` → `/home/...`)
- Saves images to project directory (configurable via `claudeImagePaste.saveDirectory`)
- Auto-cleanup: Maintains max N images in directory (default 10)
- Auto-generates timestamped filenames: `img_YYYYMMDD_HHMMSS.png`
- Automatically adds save directory to `.gitignore`
- Inserts image path with `@` prefix for Claude Code file imports

**Configuration settings** (in VS Code settings.json):
```json
{
  "claudeImagePaste.saveDirectory": ".claude-images",  // Relative to workspace root
  "claudeImagePaste.skipRenamePrompt": true,           // Skip rename dialog
  "claudeImagePaste.maxImages": 10,                     // Max images to keep
  "claudeImagePaste.filenamePrefix": "img_"            // Filename prefix
}
```

### Extension: claude-image-paste

- **Command ID**: `claude-image-paste.pasteImage`
- **Keyboard shortcut**: `Ctrl+Shift+Alt+I`
- **Platform**: Windows/WSL with PowerShell 7 and .NET Framework
- Saves images to system temp folder as `claude_paste.png`

## Submodule Development Workflow

When working on a submodule:

1. **Enter the submodule directory**: `cd claude-code-image-paste-wsl`
2. **Create a branch**: `git checkout -b feature/your-feature`
3. **Make changes**: Edit `extension.js` or other files
4. **Test**: Press F5 in VS Code to launch Extension Development Host
5. **Build**: Run `./build.sh` to create .vsix package
6. **Commit in submodule**: `git commit -am "Your changes"`
7. **Push submodule**: `git push origin feature/your-feature`
8. **Update parent repo**: `cd .. && git add claude-code-image-paste-wsl && git commit -m "Update submodule"`

## Adding New Tools

To add a new Claude Code utility as a submodule:

```bash
# Add the submodule
git submodule add <repository-url> <directory-name>

# Update README.md to document the new tool

# Commit the changes
git add .gitmodules <directory-name> README.md
git commit -m "Add <tool-name> as submodule"
```
