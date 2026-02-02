# M365 Daily News - Developer's Guide

A comprehensive guide for developers maintaining and extending the M365 Daily News declarative agent.

---

## Table of Contents

1. [Current Architecture](#current-architecture)
2. [Data Sources](#data-sources)
3. [How to Customize](#how-to-customize)
4. [File Structure](#file-structure)
5. [Deployment Workflow](#deployment-workflow)
6. [Common Customizations](#common-customizations)
7. [Examples](#examples)
8. [Security & Best Practices](#security--best-practices)

---

## Current Architecture

The M365 Daily News agent is a **declarative agent** built with the Microsoft 365 Agents Toolkit. It runs within Microsoft Teams/Copilot and provides AI-powered summaries of Microsoft's latest developments.

**Key Components:**
- **Frontend:** Microsoft Teams/Copilot (no custom UI)
- **Backend:** Microsoft Copilot (managed)
- **Configuration:** JSON + Markdown files
- **Deployment:** Microsoft Teams Admin Center

---

## Data Sources

### Current Sources

Your agent currently uses:

1. **Bing Web Search** (Automatic)
   - Searches public web for latest Microsoft AI news
   - Triggered automatically by Copilot

2. **System Instructions** (`instruction.txt`)
   - Guides agent behavior and responses
   - Defines personality and boundaries

3. **No internal knowledge base** (Default)
   - Can be added if needed

---

## How to Customize

### Option 1: Web Search Only (Current)

Agent automatically searches Bing for information.

```json
{
  "name": "M365 Daily News",
  "description": "Your daily source for Microsoft AI news",
  "instructions": "$[file('instruction.txt')]"
}
```

✅ **Pros:** Simple, real-time, no setup
❌ **Cons:** Public info only, potential hallucinations

---

### Option 2: Add OneDrive/SharePoint Knowledge

Connect internal company documents:

```json
{
  "instructions": "$[file('instruction.txt')]",
  "knowledgeSources": [
    {
      "type": "sharepoint",
      "connectorId": "sharepoint",
      "displayName": "Company M365 Resources"
    }
  ]
}
```

✅ **Pros:** Use internal docs, accurate company-specific info
❌ **Cons:** Requires setup, access permissions needed

---

### Option 3: Add API Plugins

Connect to external APIs for real-time data:

```json
{
  "instructions": "$[file('instruction.txt')]",
  "actions": [
    {
      "id": "microsoftGraphApi",
      "definition": {
        "openapi": "3.0.0",
        "info": {"title": "Microsoft Graph API", "version": "1.0"}
      }
    }
  ]
}
```

✅ **Pros:** Real-time data, custom integrations
❌ **Cons:** Complex setup, requires API keys

---

## File Structure

```
M365 News/
├── appPackage/
│   ├── manifest.json              ← App metadata & permissions
│   ├── declarativeAgent.json       ← Agent configuration
│   ├── instruction.txt             ← Agent behavior/instructions
│   ├── build/
│   │   ├── appPackage.local.zip    ← Ready to upload to Teams
│   │   ├── declarativeAgent.local.json
│   │   └── manifest.local.json
│   ├── color.png                  ← Colored app icon (128x128)
│   └── outline.png                ← Outline app icon (128x128)
│
├── env/
│   ├── .env.local                 ← Local environment variables
│   ├── .env.dev                   ← Dev environment variables
│   └── .env.prod                  ← Production variables
│
├── .vscode/
│   ├── launch.json                ← Debug configuration
│   └── tasks.json                 ← Build tasks
│
├── m365agents.yml                 ← Toolkit configuration
├── m365agents.local.yml           ← Local config
├── DEVELOPER_GUIDE.md             ← This file
├── README.md                       ← Project overview
└── .gitignore                     ← Git ignore rules
```

---

## Deployment Workflow

### Step-by-Step Process

#### 1. **Make Changes**
Edit the files you need to modify:
- `instruction.txt` - Change agent behavior
- `manifest.json` - Update app info/metadata
- Icons - Update `color.png` and `outline.png`

#### 2. **Save to GitHub**
```bash
git add .
git commit -m "Description of changes"
git push
```

#### 3. **Update Version**
Edit `appPackage/manifest.json`:
```json
"version": "1.0.2"  // Increment from previous
```

**Version Format:** `major.minor.patch`
- Major: Significant feature changes
- Minor: New features, improvements
- Patch: Bug fixes, small changes

#### 4. **Rebuild Application**
```
Ctrl+Shift+B → Select "Start Agent Locally"
```

This generates `appPackage/build/appPackage.local.zip`

#### 5. **Deploy to Teams**
```
Ctrl+Shift+P → Select "Teams: Publish"
```

Or manually upload zip to Teams Admin Center

#### 6. **Users Get Updates**
Users automatically see the new version in Copilot

---

## Common Customizations

| What to Change | Where | How | Impact |
|---|---|---|---|
| **Agent personality** | `instruction.txt` | Edit text directly | Changes how agent responds |
| **App name** | `manifest.json` → `name.short` | Update string | Changes visible name in Teams |
| **App icon** | `color.png`, `outline.png` | Replace files (128x128 PNG) | Changes app appearance |
| **App description** | `manifest.json` → `description` | Update strings | Changes Store listing |
| **Focus area** | `instruction.txt` | Add/remove topics | Changes what agent specializes in |
| **Language support** | `instruction.txt` | Add language instructions | Enables multilingual responses |
| **Knowledge sources** | `declarativeAgent.json` | Add knowledgeSources section | Adds document access |
| **Permissions** | `manifest.json` → `permissions` | Add/remove permissions | Changes what agent can access |

---

## Examples

### Example 1: Change Agent Focus to Copilot Pro Only

**File:** `appPackage/instruction.txt`

**Old:**
```
Your role is to keep users informed about the latest developments 
in Microsoft's AI world.
```

**New:**
```
Your role is to keep users informed EXCLUSIVELY about Copilot Pro 
updates, features, pricing, and capabilities. Do not discuss other 
Microsoft AI products.
```

**Then:**
```bash
git add instruction.txt
git commit -m "Focus agent on Copilot Pro only"
git push
# Rebuild and publish
```

---

### Example 2: Update App Display Name

**File:** `appPackage/manifest.json`

**Change:**
```json
"name": {
    "short": "M365 Daily News",
    "full": "M365 Daily News - Microsoft AI Updates & Insights"
}
```

**To:**
```json
"name": {
    "short": "Copilot Pro News",
    "full": "Copilot Pro - Latest News & Updates"
}
```

**Then:**
```bash
# Update version
# Change "version": "1.0.2" → "1.0.3"

git add appPackage/manifest.json
git commit -m "Rename to Copilot Pro News"
git push
# Rebuild and publish
```

---

### Example 3: Add Disclaimer to Responses

**File:** `appPackage/instruction.txt`

**Add at beginning:**
```
IMPORTANT DISCLAIMER: This agent provides information summaries 
from public sources and AI. Always verify critical information 
from official Microsoft documentation. Do not rely on this agent 
for security-sensitive or confidential matters.
```

---

## Security & Best Practices

### Security Checklist

- ✅ **Minimal permissions** - Only request what's needed
- ✅ **No credentials in code** - Use environment variables
- ✅ **Input validation** - Instructions validate user input
- ✅ **Authenticated access** - Requires Teams login
- ✅ **Regular updates** - Keep versions current
- ✅ **Privacy compliance** - Include privacy policy link

### Best Practices

1. **Always increment version number** before deployment
2. **Test locally** before publishing to organization
3. **Document changes** in commit messages
4. **Keep instructions clear** and unambiguous
5. **Include disclaimers** for AI-generated content
6. **Monitor usage** for any issues
7. **Gather feedback** from users
8. **Update regularly** with latest information

---

## Commands Reference

### Build Tasks

```bash
# Validate prerequisites and build
Ctrl+Shift+B → "Start Agent Locally"

# Alternative: Run directly
teamsfx provision --env local
```

### Deployment

```bash
# Publish to organization
Ctrl+Shift+P → "Teams: Publish"

# Manual upload
1. Go to Teams Admin Center
2. Apps > Manage apps
3. Upload custom app
4. Select appPackage/build/appPackage.local.zip
```

### Git Workflow

```bash
# Check status
git status

# Stage all changes
git add .

# Commit with message
git commit -m "Describe your changes"

# Push to GitHub
git push

# View history
git log --oneline
```

---

## Troubleshooting

### App Not Appearing in Copilot

1. Check app is published: Teams Admin Center > Manage apps
2. Clear Teams cache: Restart Teams
3. Check version number is incremented
4. Verify permissions in manifest.json

### Agent Not Responding Correctly

1. Review `instruction.txt` for clarity
2. Test with different prompts
3. Check for typos in instructions
4. Verify system instructions are loaded: `$[file('instruction.txt')]`

### Build Errors

1. Run "Validate prerequisites" task first
2. Check manifest.json JSON syntax
3. Ensure all required fields present
4. Check file paths are correct

---

## Support & Resources

- **Microsoft 365 Agents Toolkit:** https://aka.ms/teams-toolkit-declarative-agent
- **Teams App Schema:** https://developer.microsoft.com/json-schemas/teams/
- **Copilot Documentation:** https://learn.microsoft.com/microsoft-365-copilot/
- **GitHub Repository:** https://github.com/SBGSaurav/M365-News.git

---

## Contributing

To contribute changes:

1. Clone repository
2. Create feature branch: `git checkout -b feature/your-feature`
3. Make changes and commit
4. Push to GitHub
5. Create Pull Request

---

**Last Updated:** February 3, 2026  
**Version:** 1.0.0  
**Maintainer:** Saurav Poudel
