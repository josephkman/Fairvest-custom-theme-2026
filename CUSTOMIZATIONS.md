# Fairvest Theme Customizations

This document tracks all customizations made to the Ghost Source theme for the Fairvest project.

## Overview

Fairvest is a knowledge hub/blog for Bouthillier Capital (B-CAP), built on Ghost's Source theme with custom landing page modifications.

## Modified Files

### 1. `home.hbs`
**Purpose**: Custom corporate landing page for Fairvest  
**Changes**:
- Custom hero section with "Investment Intelligence by Bouthillier Capital" branding
- About section with three principles (Macro-Economic Context, Financial Analysis, Dynamic Approach)
- Featured posts integration using theme's built-in component
- About Bouthillier Capital footer section with link to https://bcap.ca
- Footer suppression on home page only

### 2. `default.hbs`
**Purpose**: Main template wrapper  
**Changes**:
- Added reference to `assets/css/fairvest-custom.css` (line ~21)

### 3. `assets/css/fairvest-custom.css`
**Purpose**: All custom styling for Fairvest landing page  
**Changes**:
- All custom CSS for the landing page
- Uses `.fairvest-landing` prefix to avoid conflicts with base theme
- Color scheme harmonized with B-CAP branding (dark blue-teal gradient: #0f2027 → #2c5364)
- Responsive design for mobile devices
- Footer hiding rules for home page

## Custom CSS Naming Convention

All custom styles use the `.fairvest-landing` prefix to prevent conflicts with the base Source theme. This makes it easy to identify custom code and prevents breaking changes during theme updates.

## Color Scheme

- **Primary dark gradient**: `linear-gradient(135deg, rgba(15, 32, 39, 0.95) 0%, rgba(32, 58, 67, 0.95) 50%, rgba(44, 83, 100, 0.95) 100%)`
- **Accent colors**: White buttons with dark text, white outline buttons on dark backgrounds
- **Text colors**: White on dark backgrounds, `#0f2027` and `#4a5568` on light backgrounds

## Files That Should NOT Be Modified

The following files are part of the base Source theme and should remain unchanged to facilitate easy updates:

- `index.hbs` - Blog post listing page
- `post.hbs` - Individual post template
- `page.hbs` - Page template
- `tag.hbs` - Tag archive template
- `author.hbs` - Author archive template
- All files in `/partials/` except where noted
- All files in `/assets/js/`
- All files in `/assets/fonts/`
- Core CSS in `/assets/css/screen.css`

## How to Update the Theme When Ghost Updates Source

When Ghost releases updates to the Source theme, follow this process to merge updates while preserving customizations:

### Step 1: Add Upstream Remote (One-time setup)

```bash
# Add the official Ghost Source theme repository as upstream
git remote add upstream https://github.com/TryGhost/Source.git

# Verify remotes
git remote -v
```

### Step 2: Fetch Latest Updates

```bash
# Fetch the latest changes from the official Source theme
git fetch upstream

# View what changed
git log --oneline HEAD..upstream/main
```

### Step 3: Create a Backup Branch

```bash
# Create a backup of your current state before merging
git checkout -b backup-before-update-$(date +%Y%m%d)
git checkout main
```

### Step 4: Merge Updates

```bash
# Merge the upstream changes into your main branch
git merge upstream/main
```

### Step 5: Resolve Conflicts

If conflicts occur, they will most likely be in:

- `home.hbs` - **Keep your version** (it's completely custom)
- `default.hbs` - **Manually merge**: Keep the line that references `fairvest-custom.css`, accept other upstream changes
- `assets/css/fairvest-custom.css` - **Keep your version** (it's custom)

For each conflict:

```bash
# Edit the conflicted files
# Look for conflict markers: <<<<<<<, =======, >>>>>>>

# For home.hbs - keep your custom version:
git checkout --ours home.hbs

# For default.hbs - manually edit to preserve the fairvest-custom.css reference
# Open in editor and resolve conflicts

# Mark as resolved
git add home.hbs default.hbs

# Continue merge
git merge --continue
```

### Step 6: Test Locally

```bash
# Install dependencies (in case any changed)
npm install

# Build theme
npm run zip

# Test the theme in a local Ghost instance
```

### Step 7: Deploy

```bash
# If everything works, push to GitHub
git push origin main

# The GitHub Action will automatically deploy to your Ghost instance
```

## Alternative: Manual Update Process

If the merge process is too complex, you can manually update:

1. Download the latest Source theme from Ghost
2. Extract to a new folder
3. Copy these files from your current Fairvest theme to the new theme:
   - `home.hbs`
   - `assets/css/fairvest-custom.css`
   - `CUSTOMIZATIONS.md` (this file)
4. Edit `default.hbs` in the new theme to add the reference to `fairvest-custom.css`
5. Test and deploy

## GitHub Repository

**Repository**: https://github.com/josephkman/Fairvest-custom-theme-2026

**Deployment**: Automatic via GitHub Actions on push to `main` branch

## Secrets Configuration

The following GitHub secrets are configured for automatic deployment:

- `GHOST_ADMIN_API_URL` - Your Ghost admin API URL
- `GHOST_ADMIN_API_KEY` - Your Ghost admin API key

## Development Workflow

```bash
# Make changes to home.hbs or assets/css/fairvest-custom.css
git add home.hbs assets/css/fairvest-custom.css
git commit -m "Description of changes

Co-Authored-By: Warp <agent@warp.dev>"
git push

# GitHub Action automatically deploys to Ghost
```

## Support

For questions about:
- **Base Source theme**: https://github.com/TryGhost/Source
- **Ghost themes**: https://ghost.org/docs/themes/
- **Fairvest customizations**: Refer to this document or repository commit history
