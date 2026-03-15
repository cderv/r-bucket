# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a **custom Scoop bucket** repository that maintains Windows package manifests for R-related tools and data science applications. Scoop is a command-line installer for Windows that uses JSON manifests to describe how to install, update, and manage portable applications.

**Key characteristics:**
- Manifests are JSON files in the `bucket/` directory
- Automated updates via GitHub Actions (Excavator workflow runs every 2 hours)
- Supports multiple versions of key applications (daily, preview, release, branched)
- No admin privileges required for most installations

## Common Commands

### Version Management
```powershell
# Check for new versions of apps
.\bin\checkver.ps1

# Check specific app(s)
.\bin\checkver.ps1 quarto rstudio-daily

# Force update manifests with new versions/hashes
.\bin\checkver.ps1 -ForceUpdate

# Check for apps missing checkver configuration
.\bin\missing-checkver.ps1
```

### Manifest Formatting
```powershell
# Format all JSON manifests in bucket/
.\bin\formatjson.ps1

# Format specific manifest
.\bin\formatjson.ps1 bucket/quarto.json
```

### Validation
```powershell
# Run Pester tests (requires Pester 4.4.0+)
.\bin\test.ps1

# Check URLs in manifests are accessible
.\bin\checkurls.ps1

# Verify hash integrity
.\bin\checkhashes.ps1
```

### Pull Request Automation
```powershell
# Create PR for version updates (used by CI)
.\bin\auto-pr.ps1
```

## Architecture Overview

Understanding these "big picture" concepts will help you work effectively in this repository:

### 1. Manifest-Based Installation

Scoop manifests are JSON files that define:
- **Download URLs and hashes** for verification
- **Extraction and installation instructions**
- **Binary/executable locations** for PATH integration
- **Version-specific properties** for different architectures

Example from `bucket/quarto.json`:
```json
{
    "version": "1.8.25",
    "url": "https://github.com/quarto-dev/quarto-cli/releases/download/v1.8.25/quarto-1.8.25-win.zip",
    "hash": "3f02318ca958b13fceec53e8ec089f9bf348eef40977bb588f92dd2e5e2287db",
    "bin": "bin\\quarto.exe",
    "checkver": "github",
    "autoupdate": {
        "url": "https://github.com/quarto-dev/quarto-cli/releases/download/v$version/quarto-$version-win.zip"
    }
}
```

### 2. Multi-Version Strategy

This bucket maintains multiple versions of key applications to support different user needs:

- **Daily/Nightly builds**: Latest development versions (e.g., `rstudio-daily`, `positron-daily`)
- **Preview/Pre-release**: Beta/RC versions (e.g., `rstudio-preview`, `quarto-prerelease`)
- **Release**: Stable versions (e.g., `rstudio-release`, `quarto`)
- **Branched**: Frozen versions for compatibility (e.g., `rstudio-2024.04`)

When multiple versions provide the same binary name, users switch between them with `scoop reset <app>`. The `post_install` script often includes notes about this behavior.

### 3. Checkver/Autoupdate System

Automation is critical for maintaining currency:

**Checkver** (`checkver` property in manifest):
- Defines how to detect new versions from upstream sources
- Supports: GitHub API, SourceForge RSS, JSON APIs, regex on web pages
- Uses named regex capture groups for complex version schemes

**Autoupdate** (`autoupdate` property in manifest):
- Defines how to generate new URLs and extract hashes for new versions
- Uses variable substitution: `$version`, `$matchDate`, `$matchBuild`, etc.
- Can extract hashes from separate files or compute them from downloads

**Variable Types for Autoupdate:**

1. **Captured Variables** (from checkver regex groups):
   - Named groups: `$matchName1`, `$matchDate`, `$matchBuild` (in autoupdate)
   - Named groups: `${name1}`, `${date}`, `${build}` (in checkver.replace)
   - Unnamed groups: `$match1`, `$match2`, `$match3` (in autoupdate)
   - Unnamed groups: `${1}`, `${2}`, `${3}` (in checkver.replace)

2. **Version Variables** (auto-generated from version):
   - `$version`: `3.7.1`
   - `$majorVersion`, `$minorVersion`, `$patchVersion`, `$buildVersion`: `3`, `7`, `1`, `2`
   - `$cleanVersion`: `371` (removes dots)
   - `$underscoreVersion`: `3_7_1`
   - `$dashVersion`: `3-7-1`
   - `$matchHead`: First 2-3 digits (`3.7.1-rc.1` → `3.7.1`)
   - `$matchTail`: Rest after matchHead (`3.7.1-rc.1` → `-rc.1`)
   - `$preReleaseVersion`: After last dash (`3.7.1-rc.1` → `rc.1`)

3. **URL Variables** (from autoupdate URL):
   - `$url`: Full URL without fragments
   - `$baseurl`: URL without filename
   - `$basename`: Filename from URL

4. **Hash Variables** (for hash.regex patterns):
   - `$md5`, `$sha1`, `$sha256`, `$sha512`: Specific hash patterns
   - `$checksum`: Any of the above hash types
   - `$base64`: BASE64-encoded checksum

Example for RStudio daily with complex versioning:
```json
"checkver": {
    "url": "https://dailies.rstudio.com/rstudio/latest/index.json",
    "jsonpath": ".products.electron.platforms['windows-xcopy'].version",
    "regex": "(?<date>[\\d.]+)(?<type>-(daily|preview))?\\+(?<build>\\d+)",
    "replace": "${date}+${build}${type}"
}
```

### 4. Automation Workflow

The Excavator workflow (`.github/workflows/schedule.yml`):
1. Runs every 2 hours via GitHub Actions
2. Executes `checkver.ps1` to detect new versions
3. Updates manifests automatically if URLs/hashes change
4. Recomputes hashes after updates to fix faulty hashes
5. Commits changes directly to the repository

### 5. Wrapper Script Pattern

Scripts in `bin/` are thin wrappers that locate and invoke Scoop's built-in utilities:

```powershell
# Example from bin/checkver.ps1
if(!$env:SCOOP_HOME) {
    $env:SCOOP_HOME = resolve-path (split-path (split-path (scoop which scoop)))
}
$checkver = "$env:SCOOP_HOME/bin/checkver.ps1"
$dir = "$psscriptroot/../bucket"
Invoke-Expression -command "& '$checkver' -dir '$dir' $($args | ForEach-Object { "$_ " })"
```

This pattern:
- Locates Scoop's installation directory dynamically
- Automatically targets the `bucket/` directory
- Allows using Scoop utilities without PATH dependencies

## Scoop Documentation Resources

### Official Resources
- **Scoop Main Repository**: https://github.com/ScoopInstaller/Scoop (indexed on Deepwiki)
- **Scoop Wiki**: https://github.com/ScoopInstaller/Scoop/wiki (gitignored copy is in .scoop-wiki/)

### Using Deepwiki for Research

Deepwiki (deepwiki.com) indexes many open-source repositories and can answer questions about their code, architecture, and behavior.

**When to use Deepwiki**:
- Understanding how Scoop features work internally (e.g., "How does checkver github work?")
- Researching manifest patterns and best practices
- Learning about upstream projects before adding them to the bucket
- Finding examples of specific manifest configurations

**Available via MCP tool**: `mcp__deepwiki__ask_question`

**Example queries**:
- "How does Scoop's autoupdate system handle GitHub releases?"
- "What is the difference between checkver.jsonpath and checkver.regex?"
- "How do I configure multi-architecture manifests?"

**Note**: Deepwiki indexes source code but NOT wiki content. For wiki documentation, always use the local `.scoop-wiki/` copy first.

### Local Wiki Clone (Recommended)

This repository includes a local clone of the Scoop wiki at `.scoop-wiki/` for fast, offline access to community documentation.

#### IMPORTANT: Wiki Update Workflow

**Before working with manifests, always update the local wiki:**

```powershell
# Step 1: Update the wiki content
cd .scoop-wiki
git pull
cd ..

# Step 2: Refresh the page listing to see what's available
ls .scoop-wiki/*.md

# Step 3: Read key wiki pages relevant to your task
# Example: Understanding manifest structure
cat .scoop-wiki/App-Manifests.md

# Example: Learning about autoupdate
cat .scoop-wiki/App-Manifest-Autoupdate.md
```

#### When to Read Wiki Pages

- **Creating new manifests**: Read `Creating-an-app-manifest.md`, `App-Manifests.md`
- **Adding autoupdate**: Read `App-Manifest-Autoupdate.md`
- **Understanding autoupdate variables**: Read `App-Manifest-Autoupdate.md` (sections on captured/version/URL/hash variables)
- **Setting up hash extraction**: Read `App-Manifest-Autoupdate.md` (hash section with extraction modes and examples)
- **Writing shortcuts property**: Read `Open-With-Icons.md` (CRITICAL path warning about never using shim paths)
- **Understanding the current junction**: Read `The-'Current'-Version-Alias.md` (why paths stay stable across updates)
- **Understanding buckets**: Read `Buckets.md`
- **Troubleshooting**: Search relevant pages with `grep -i "search term" .scoop-wiki/*.md`

## Complete Scoop Wiki Content Map

The `.scoop-wiki/` directory contains 36 pages organized by category:

### Overview (3 pages)
- `So-What.md` - Why Scoop exists and what problems it solves
- `Chocolatey-and-Winget-Comparison.md` - Comparison with alternatives
- `Cygwin-and-MSYS-Comparison.md` - Comparison with Unix-like environments

### Getting Started (4 pages)
- `Quick-Start.md` - Installation and basic usage
- `Commands.md` - Command reference
- `FAQ.md` - Frequently asked questions
- `Uninstalling-Scoop.md` - How to uninstall Scoop

### Concepts (8 pages)
- `Apps.md` - What are Scoop apps
- `Buckets.md` - Bucket system and creating custom buckets
- `App-Manifests.md` - **Manifest structure and properties (READ THIS FIRST)**
- `Creating-an-app-manifest.md` - Step-by-step manifest creation
- `App-Manifest-Autoupdate.md` - **Automation system (CRITICAL FOR UPDATES)**
- `Persistent-data.md` - Handling user data across updates
- `Pre-Post-(un)install-scripts.md` - Hooks for custom logic
- `Dependencies.md` - Managing app dependencies
- `The-'Current'-Version-Alias.md` - Version switching mechanism

### Guides (7 pages)
- `Apache-with-PHP.md` - Setting up Apache with PHP
- `Custom-PHP-configuration.md` - PHP configuration
- `Docker.md` - Using Docker with Scoop
- `Github-with-SSH-key.md` - GitHub SSH setup
- `Java.md` - Java version management
- `SSH-on-Windows.md` - SSH configuration
- `Theming-Powershell.md` - PowerShell customization

### Misc (12 pages)
- `Antivirus-false-positive.md` - Handling antivirus issues
- `Antivirus-and-Anti-Malware-Problems.md` - Antivirus troubleshooting
- `Can-I-Use-Scoop-in-Bash,-Zsh,-etc.md` - Shell compatibility
- `Example-Setup-Scripts.md` - Setup script examples
- `Scoop-Folder-Layout.md` - Directory structure
- `Open-With-Icons.md` - File associations and icons
- `PowerShell-Modules.md` - PowerShell module management
- `Switching-Ruby,-Python-and-PHP-Versions.md` - Language version management
- `Global-Installs.md` - System-wide installations
- `Using-Scoop-behind-a-proxy.md` - Proxy configuration
- `Why-PowerShell.md` - Why Scoop uses PowerShell
- `Criteria-for-including-apps-in-the-main-bucket.md` - Main bucket criteria

## Adding or Updating Manifests

When creating or updating manifests, follow this workflow:

### 1. Research the Application
- Understand the download URLs and versioning scheme
- Check if official JSON APIs exist for version detection
- Identify where hash files are published (if available)

### 2. Create/Edit the Manifest
```powershell
# Create initial manifest structure
# See .scoop-wiki/Creating-an-app-manifest.md for guidance
```

Required properties:
- `version`: Current version string
- `description`: One-line description
- `homepage`: Official website
- `license`: SPDX identifier or license URL
- `url`: Download URL(s)
- `hash`: SHA256 hash(es)
- `bin` or `shortcuts`: How to make the app accessible

### 3. Add Automation (checkver/autoupdate)
```powershell
# Test checkver pattern
.\bin\checkver.ps1 <appname>

# Test autoupdate with -ForceUpdate
.\bin\checkver.ps1 <appname> -u
```

For complex version schemes like RStudio's `2025.11.0+261-daily`:

- Use named capture groups: `(?<date>...)`, `(?<build>...)`
- Reference in autoupdate URLs: `$matchDate`, `$matchBuild`

**Hash Extraction Modes** (for `autoupdate.hash.mode`):

- `extract` (default): Extract from plain text file or webpage using regex
- `json`: Extract from JSON file using JSONPath
- `xpath`: Extract from XML file using XPath
- `rdf`: Extract from RDF file digest
- `metalink`: Extract from Metalink header or `.meta4` file
- `fosshub`: Auto-detected for FossHub URLs
- `sourceforge`: Auto-detected for SourceForge URLs
- `download`: Fallback - downloads file and computes hash locally

### 4. Validate
```powershell
# Format the JSON
.\bin\formatjson.ps1 bucket/<appname>.json

# Verify URLs are accessible
.\bin\checkurls.ps1

# Check hash correctness
.\bin\checkhashes.ps1

# Test installation
scoop install bucket/<appname>.json
```

### 5. Test Autoupdate

To test autoupdate without waiting for a new version release:

```powershell
# Method 1: Temporarily change version field to an older/different version
# Edit bucket/<appname>.json - change "version": "1.2.3" to "version": "1.2.0"

# Run checkver with -u to trigger autoupdate
.\bin\checkver.ps1 <appname> -u

# Verify url, hash, extract_dir are updated correctly
.\bin\formatjson.ps1 <appname>
.\bin\checkhashes.ps1 <appname>

# Test installation works
scoop install <path-to-manifest>

# Method 2: Force update even when no new version
.\bin\checkver.ps1 <appname> -ForceUpdate
```

**When to use which method:**
- **Method 1 (version trick)**: Use when you want to verify the full autoupdate flow - version detection, URL generation, and hash extraction all working together. This simulates what happens when a real new version is released.
- **Method 2 (-ForceUpdate)**: Use when you just need to recompute hashes for the current version (e.g., when creating a new manifest with empty hash field).

## Notes

### Manifest Conventions
- **Prefer portable/installer-less versions** to avoid admin privileges
- **Hash format**: SHA256 by default (64 hex chars), prefix with `md5:`, `sha1:`, or `sha512:` for other algorithms
- **Hash sources**: Extract from separate files when possible, compute from downloads as fallback
- **Branch naming**: Use descriptive names (e.g., `add-beads`, not `feature-1`)

### Critical Manifest Gotchas

#### File Associations and Shortcuts

**CRITICAL**: When writing the `shortcuts` property in manifests, ALWAYS reference the `current/` path, NEVER the shim path:

```json
// CORRECT - Uses current junction
"shortcuts": [["apps\\mpv\\current\\mpv.exe", "MPV Media Player"]]

// WRONG - Uses shim path (creates nameless blobs with broken icons)
"shortcuts": [["shims\\mpv.exe", "MPV Media Player"]]
```

Using shim paths in shortcuts causes:

- Nameless applications in "Open With" menu
- Ugly "unknown file" icons for all associated files
- Impossible to distinguish between multiple shim-based shortcuts
- Sometimes shims don't close after launching the program

See `.scoop-wiki/Open-With-Icons.md` for details.

#### The 'current' Junction

Manifests should reference `current/` in paths (`shortcuts`, `env_set`, `post_install` scripts) because it's a directory junction that stays stable across updates. When you write paths like:

- `apps\\myapp\\current\\myapp.exe` (stays valid across all versions)
- `apps\\myapp\\1.2.3\\myapp.exe` (breaks when updating to 1.2.4)

The `current` junction automatically points to the active version. This is why shortcuts and environment variables survive updates. See `.scoop-wiki/The-'Current'-Version-Alias.md`.

#### PowerShell Modules

When creating PowerShell module manifests, the `psmodule.name` property MUST match the `.psd1` manifest filename for PowerShell to automatically detect the module:

```json
"psmodule": {
    "name": "MyPSModule"  // Must match MyPSModule.psd1 in the extracted directory
}
```

See `.scoop-wiki/PowerShell-Modules.md` for details.

### Architecture-Specific Manifests
When apps have different binaries for different architectures:
```json
"architecture": {
    "64bit": {
        "url": "https://example.com/app-x64.zip",
        "hash": "..."
    },
    "32bit": {
        "url": "https://example.com/app-x86.zip",
        "hash": "..."
    }
}
```

### Special Cases
- **R-devel**: Uses `md5:` hash prefix (required by CRAN)
- **Multi-part downloads**: Use URL arrays and hash arrays in same order
- **Nested archives** (.tar.gz): Scoop handles automatically
- **InnoSetup installers**: Set `"innosetup": true` to extract without running

### GitHub Automatic Hash Extraction

When using `"checkver": "github"` with GitHub release download URLs, Scoop automatically extracts hashes from the GitHub API without needing hash configuration in autoupdate:

```json
{
    "checkver": "github",
    "autoupdate": {
        "url": "https://github.com/owner/repo/releases/download/v$version/app-$version.zip"
        // NO hash block needed - Scoop automatically fetches from GitHub API
    }
}
```

**How it works**:
- Scoop detects GitHub release URLs and sets `hashmode: 'github'` automatically
- Uses GitHub API to extract hash from `assets[].digest` in release JSON
- Works for both single and multi-architecture manifests
- **DO NOT** add `hash.url` or `hash.mode` for GitHub releases - it's redundant

**When you DO need hash configuration**:
- Non-GitHub URLs (SourceForge, custom servers, etc.)
- When checksums are in separate files (checksums.txt, SHA256SUMS, etc.)
- When using custom hash extraction methods

See examples in `bucket/air.json` and `bucket/ark.json` for GitHub-hosted apps.

## Quick Reference: Key Files

- `bucket/*.json` - Application manifests
- `bin/*.ps1` - Development and automation tools
- `.github/workflows/schedule.yml` - Excavator automation
- `.scoop-wiki/*.md` - Complete Scoop documentation (UPDATE FIRST!)
- `schema.json` - Scoop manifest validation schema (in main Scoop repo)
