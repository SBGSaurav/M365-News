# M365 Daily News Agent

A Microsoft Copilot declarative agent that provides curated news and insights about Microsoft's AI ecosystem.

## Overview

**M365 Daily News** is an AI-powered agent that keeps users informed about:
- Latest Microsoft AI announcements and releases
- Upcoming Microsoft AI capabilities and roadmap items  
- Practical AI tools and features available today
- Real-world use cases and actionable insights

## Features

✅ **Public News Focus** - Curated information from public Microsoft sources  
✅ **Real-time Updates** - Bing web search for latest announcements  
✅ **Professional Tone** - Clear, accessible explanations for all skill levels  
✅ **Actionable Insights** - Step-by-step guidance and use cases  
✅ **Microsoft-Only** - Focus exclusively on Microsoft's AI ecosystem  

## Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) (versions 18, 20, or 22)
- [Microsoft 365 Account](https://docs.microsoft.com/microsoftteams/platform/toolkit/accounts)
- [Microsoft 365 Agents Toolkit VS Code Extension](https://aka.ms/teams-toolkit) v5.0.0+
- [Microsoft 365 Copilot License](https://learn.microsoft.com/microsoft-365-copilot/extensibility/prerequisites)

### Installation

1. Clone this repository
2. Open in VS Code
3. Install Microsoft 365 Agents Toolkit extension
4. Sign in with your Microsoft 365 account

### Local Testing

1. Press `Ctrl+Shift+B`
2. Select "Start Agent Locally"
3. Choose "Preview Local in Copilot (Edge)" or "(Chrome)"
4. Find "M365 Daily News" in Copilot
5. Start asking questions!

### Deployment

1. Update version in `appPackage/manifest.json`
2. Press `Ctrl+Shift+P` → "Teams: Publish"
3. Agent will be published to your organization

## Project Structure

```
M365 News/
├── appPackage/
│   ├── manifest.json              # App metadata
│   ├── declarativeAgent.json       # Agent configuration
│   ├── instruction.txt             # Agent behavior & personality
│   ├── build/
│   │   └── appPackage.local.zip    # Ready to upload
│   ├── color.png                  # App icon (colored)
│   └── outline.png                # App icon (outline)
├── DEVELOPER_GUIDE.md             # Comprehensive developer documentation
├── README.md                      # This file
└── .gitignore                     # Git ignore rules
```

## Development

See [DEVELOPER_GUIDE.md](DEVELOPER_GUIDE.md) for:
- Architecture overview
- Customization options
- Deployment workflow
- Examples and best practices
- Troubleshooting guide

## Key Files

| File | Purpose |
|------|----------|
| `appPackage/manifest.json` | App metadata, permissions, icons |
| `appPackage/declarativeAgent.json` | Agent configuration |
| `appPackage/instruction.txt` | Agent personality and behavior |
| `DEVELOPER_GUIDE.md` | Complete developer documentation |

## Configuration

The agent is configured to:
- ✅ Search public Microsoft news via Bing
- ✅ Provide summaries of latest AI developments
- ✅ Highlight upcoming Microsoft AI features
- ✅ Suggest actionable AI tools for today
- ✅ Maintain focus on Microsoft AI ecosystem only

**Note:** This agent intentionally uses public sources only. No internal resources or confidential data.

## Security & Compliance

- **Minimal Permissions** - Only `identity` and `messageTeamMembers`
- **Authenticated Access** - Requires Microsoft 365 login
- **Public Sources Only** - No internal data access
- **Disclaimer Included** - Recommends verification of critical information

## Support

- **Documentation:** [DEVELOPER_GUIDE.md](DEVELOPER_GUIDE.md)
- **Teams Toolkit:** https://aka.ms/teams-toolkit-declarative-agent
- **Microsoft 365 Copilot:** https://learn.microsoft.com/microsoft-365-copilot/

## Contributing

1. Clone the repository
2. Create a feature branch
3. Make changes and test locally
4. Commit with clear messages
5. Push and create pull request

## License

Copyright © 2026 SBG PTY LTD. All rights reserved.

## Version History

**v1.1.0** (2026-02-03)
- Initial stable release
- Cleaned production code
- Comprehensive documentation
- Public news focus confirmed

---

**Maintained by:** [SBG PTY LTD](https://www.sbgptyltd.com.au)  
**Repository:** https://github.com/SBGSaurav/M365-News  
**Last Updated:** February 3, 2026
