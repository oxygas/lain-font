# AGENTS.md - Lain Font Repository

## Project Overview

This repository contains a custom font configuration for use with Revenge/Bunny Discord client modifications. The font is "Moms Typewriter" (also known as the Lain font), styled for Serial Experiments Lain theming.

## Repository Structure

```
lain-font/
├── lain.json           # Font configuration file for Revenge
├── MomsTypewriter.ttf  # Font file
└── AGENTS.md           # This file
```

## Build/Lint/Test Commands

This is a static asset repository with no build system. Testing is done manually:

### Validate JSON Syntax
```bash
# Check JSON is valid
cat lain.json | python3 -m json.tool > /dev/null && echo "Valid JSON" || echo "Invalid JSON"
```

### Verify Font URL Accessibility
```bash
# Test font file is accessible via GitHub raw URL
curl -I "https://github.com/oxygas/lain-font/raw/main/MomsTypewriter.ttf?raw=1" 2>/dev/null | head -5
```

### Git Operations
```bash
# Stage and commit changes
git add lain.json MomsTypewriter.ttf
git commit -m "Description of changes"
git push
```

## Font JSON Specification

### Required Fields

The `lain.json` file must follow the Revenge font JSON spec:

```json
{
  "spec": 1,
  "name": "Font Display Name",
  "previewText": "Preview text shown in Revenge",
  "main": {
    "font-mapping": "URL to font file"
  }
}
```

### Required Font Mappings

All Revenge fonts must map these Discord system fonts:
- `ABCGintoNord-ExtraBold`
- `ggsans-Bold`
- `ggsans-BoldItalic`
- `ggsans-ExtraBold`
- `ggsans-ExtraBoldItalic`
- `ggsans-Medium`
- `ggsans-MediumItalic`
- `ggsans-Normal`
- `ggsans-NormalItalic`
- `ggsans-Semibold`
- `ggsans-SemiboldItalic`
- `NotoSans-Bold`
- `NotoSans-ExtraBold`
- `NotoSans-Medium`
- `NotoSans-Normal`
- `NotoSans-NormalItalic`
- `NotoSans-Semibold`
- `ggmono-Normal`
- `SourceCodePro-Semibold`

### Font URL Format

URLs must use the GitHub raw format with `?raw=1` suffix:
```
https://github.com/USERNAME/REPO/raw/main/FONTFILE.ttf?raw=1
```

## Code Style Guidelines

### JSON Formatting

- Use 2-space indentation
- No trailing whitespace
- Keep alphabetically sorted font mappings in `main` object
- Use consistent quote style (double quotes)

### File Naming

- Font files: Use ASCII-only characters, no special characters
  - Good: `MomsTypewriter.ttf`
  - Bad: `Mom®t___.ttf` (special characters break URLs)
- JSON files: Use lowercase with hyphens, or match font name
  - Example: `lain.json`

### URL Encoding

If font filenames contain special characters, URL-encode them:
- `®` → `%C2%AE`
- Space → `%20`

However, prefer renaming files to avoid special characters entirely.

## Error Handling

### Common Issues

1. **"name field missing" error**
   - Ensure JSON has `"name"` field at root level
   - Must be a string, not empty

2. **"spec field invalid" error**
   - `"spec"` must be integer `1`
   - Do NOT use string `"spec": "1"`

3. **Font doesn't apply**
   - Check all required font mappings are present
   - Verify font URL returns HTTP 200
   - Ensure font file is valid TTF

4. **Revenge crashes after import**
   - Clear Discord data: Settings → Apps → Discord → Clear Data
   - Reimport font after fixing JSON

### Recovery

If Revenge crashes due to corrupt font:
```bash
# Clear font data via ADB (requires root)
adb shell rm -rf /data/data/com.discord/files/fonts/*

# Or clear all Discord data via Android Settings
# Settings → Apps → Discord → Storage → Clear Data
```

## Testing in Revenge

1. Open Discord with Revenge mod
2. Go to Settings → Revenge → Fonts
3. Click "Install a font"
4. Click "Import font entries from a link"
5. Paste: `https://raw.githubusercontent.com/oxygas/lain-font/main/lain.json`
6. Click Import
7. Select the font from the list

## Reference

- [Rairof's Theme-Fonts](https://github.com/Rairof/Theme-Fonts) - Working font examples
- [Revenge Unofficial Docs](https://joamoncab.github.io/revenge-unofficial-docs/en/misc/faq) - Font installation guide

## Notes

- This repository serves a single font file
- The JSON maps one font to all Discord font styles
- Single font file = smaller download, simpler maintenance
