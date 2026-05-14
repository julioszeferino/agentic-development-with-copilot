# Community Submission Checklist

> **Purpose**: Ship a plugin release that passes Obsidian community submission checks with minimal review churn.
> **MCP Validated**: 2026-05-02

## When to Use

- Preparing first submission to the Community Plugins directory
- Updating a plugin after review feedback
- Running a pre-flight release audit before tagging

## Implementation

```text
1) Validate repository baseline
   - README.md present and accurate
   - LICENSE present
   - manifest.json present and valid

2) Validate manifest compliance
   - id is unique and does not contain "obsidian"
   - description <= 250 chars, concise, ends with period
   - minAppVersion set appropriately
   - isDesktopOnly=true if Node.js/Electron APIs are used
   - remove fundingUrl if not accepting donations

3) Build release assets
   - Compile production main.js
   - Ensure manifest.json version is x.y.z
   - Create GitHub release tag matching manifest version
   - Upload main.js + manifest.json (+ styles.css when applicable)

4) Submit for review
   - Fork obsidianmd/obsidian-releases
   - Add plugin entry in community-plugins.json
   - Open PR: "Add plugin: <Plugin Name>"
   - Complete checklist in PR body

5) Handle review loop
   - Address bot/reviewer feedback in same PR
   - Update release artifacts for fixed version
   - Comment on PR with change summary
```

## Configuration

| Setting | Default | Description |
|---------|---------|-------------|
| Release version format | `x.y.z` | Required semantic versioning format |
| PR target repo | `obsidianmd/obsidian-releases` | Official community plugin registry source |
| Required artifacts | `main.js`, `manifest.json` | Core binaries consumed by plugin directory |

## Example Usage

```text
Pre-release gate status:
- Manifest checks: PASS
- Release assets: PASS
- community-plugins.json entry prepared: PASS
- Ready to open PR: YES
```

## See Also

- [Manifest and Compatibility](../concepts/manifest-and-compatibility.md)
- [Quick Reference](../quick-reference.md)
