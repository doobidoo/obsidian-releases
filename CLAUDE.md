# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This repository hosts the public release infrastructure for Obsidian and serves as the community directory for plugins and themes. It does NOT contain Obsidian's source code. The repository manages:

- Community plugin directory (`community-plugins.json`)
- Community theme directory (`community-css-themes.json`)
- Plugin statistics and deprecation tracking
- Desktop release metadata
- Automated validation for plugin/theme submissions

## Core Data Files

### Plugin Management
- `community-plugins.json`: Master list of all approved community plugins
- `community-plugin-stats.json`: Download and usage statistics (auto-generated)
- `community-plugin-deprecation.json`: Deprecated plugin versions
- `community-plugins-removed.json`: Historical record of removed plugins

### Theme Management
- `community-css-themes.json`: Master list of all approved themes
- `community-snippets.json`: CSS snippet directory

### Release Tracking
- `desktop-releases.json`: Obsidian desktop app release history

## JSON File Structure

### Plugin Entry Format
```json
{
  "id": "plugin-id",           // Unique identifier (lowercase, alphanumeric, dashes only)
  "name": "Plugin Name",       // Display name (no "Obsidian" or "Plugin" in name)
  "author": "Author Name",     // Author display name (no email)
  "description": "Short desc", // <250 chars, no "Obsidian" or "this plugin"
  "repo": "user/repo"          // GitHub repository (owner/repo format)
}
```

### Theme Entry Format
```json
{
  "name": "Theme Name",        // Display name (no "Obsidian" or "Theme" in name)
  "author": "Author Name",     // Author display name
  "repo": "user/repo",         // GitHub repository
  "screenshot": "path.png",    // Path to screenshot (512×288px recommended)
  "modes": ["dark", "light"],  // Supported color modes
  "publish": true              // Optional: Obsidian Publish support
}
```

## Development Commands

```bash
# Validate JSON syntax
npm run lint

# (Note: No build, test, or compilation - this is a data repository)
```

## Validation Workflows

### Plugin Validation (.github/workflows/validate-plugin-entry.yml)
Automatically validates plugin submissions on PR:
- Checks that only `community-plugins.json` is modified
- Validates JSON structure and required fields
- Verifies plugin ID format (no "obsidian", no "plugin" suffix)
- Confirms name doesn't include redundant "Obsidian" or "Plugin"
- Validates description (<250 chars, no redundant phrases)
- Checks for duplicate ID, name, or repo
- Verifies GitHub repository exists and author matches PR submitter
- Validates manifest.json in plugin repo
- Checks for GitHub release matching manifest version
- Verifies release contains required files (main.js, manifest.json)
- Checks for LICENSE file in repository

### Theme Validation (.github/workflows/validate-theme-entry.yml)
Automatically validates theme submissions on PR:
- Checks that only `community-css-themes.json` is modified
- Validates JSON structure and required fields
- Verifies name doesn't include redundant "Obsidian" or "Theme"
- Checks for duplicate name or repo
- Validates manifest.json in theme repo
- Verifies theme.css exists
- Validates screenshot (PNG/JPG, 250×100 to 1000×500px range)
- Checks for LICENSE file in repository

## Submission Workflow

### Plugin Submission
1. Author adds entry to END of `community-plugins.json`
2. Opens PR using `.github/PULL_REQUEST_TEMPLATE/plugin.md` checklist
3. Validation workflow runs automatically
4. On success: PR labeled "Ready for review", assigned to ObsidianReviewBot
5. On failure: PR labeled "Validation failed", errors posted as comment
6. PR title auto-updated to "Add plugin: {name}"

### Theme Submission
1. Author adds entry to END of `community-css-themes.json`
2. Opens PR using `.github/PULL_REQUEST_TEMPLATE/theme.md` checklist
3. Validation workflow runs automatically
4. On success: PR labeled "Ready for review"
5. On failure: PR labeled "Validation failed", errors posted as comment
6. PR title auto-updated to "Add theme: {name}"

## Common Validation Rules

### Plugin/Theme IDs and Names
- **ID**: lowercase, alphanumeric, dashes/underscores only (plugins only)
- **No "Obsidian"**: Don't include in ID, name, or description
- **No redundant words**: Don't use "Plugin" or "Theme" in names

### Repository Requirements
- Must exist and be accessible on GitHub
- Submitter must be repo owner or public org member
- Must contain valid `manifest.json` at root
- Should have issues enabled
- Should have a LICENSE file

### Plugin-Specific Requirements
- Latest GitHub release must match manifest.json version
- Release must include `main.js` and `manifest.json` as assets
- Optional: `styles.css` in release
- Description max 250 characters

### Theme-Specific Requirements
- Must include `theme.css` at root
- Screenshot must be 16:9 aspect ratio (512×288px recommended)
- Screenshot must be PNG or JPG, between 250×100 and 1000×500px
- Must specify supported modes (dark/light/both)

## Important Constraints

### Entry Modification Rules
- New entries MUST be added to the END of the JSON array
- Submitter must be the repository owner or public org member
- Cannot reuse IDs/names of removed plugins
- Validation only runs if PR adds more lines than it removes

### Manifest Validation
- Plugin/theme ID in JSON must match manifest.json ID
- Plugin/theme name in JSON must match manifest.json name
- Version in manifest.json must have a matching GitHub release tag
- authorUrl should not point to obsidian.md or the plugin/theme repo
- fundingUrl should not point to obsidian.md/pricing

## External References

- Plugin submission guide: https://docs.obsidian.md/Plugins/Releasing/Submit+your+plugin
- Theme submission guide: https://docs.obsidian.md/Themes/App+themes/Submit+your+theme
- Developer policies: https://docs.obsidian.md/Developer+policies
- Plugin guidelines: https://docs.obsidian.md/Plugins/Releasing/Plugin+guidelines
- Theme guidelines: https://docs.obsidian.md/Themes/App+themes/Theme+guidelines

## This is a Data Repository

This repository contains NO executable code - only JSON data files and GitHub Actions workflows. When making changes:
- Always edit JSON files directly (no build process)
- Add entries to the END of arrays
- Run `npm run lint` to validate JSON syntax
- Let the automated workflows handle PR validation
