<p align="center">
  <img src="https://img.shields.io/badge/WHMCS-Dev_Skills-0066FF?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgdmlld0JveD0iMCAwIDI0IDI0IiBmaWxsPSJub25lIiBzdHJva2U9IiNmZmYiIHN0cm9rZS13aWR0aD0iMiI+PHBhdGggZD0iTTEyIDJMMiA3bDEwIDUgMTAtNS0xMC01eiIvPjxwYXRoIGQ9Ik0yIDE3bDEwIDUgMTAtNSIvPjxwYXRoIGQ9Ik0yIDEybDEwIDUgMTAtNSIvPjwvc3ZnPg==&logoColor=white" alt="WHMCS Dev Skills" />
</p>

<h1 align="center">🧠 WHMCS Dev Skills</h1>
<h3 align="center">AI Agent Skill for WHMCS Module Development</h3>

<p align="center">
  An <strong>industrial-grade skill file</strong> that turns any AI coding agent into a
  <strong>Senior WHMCS Developer & Architect</strong>.
</p>

<p align="center">
  <a href="https://www.gnu.org/licenses/old-licenses/gpl-2.0.en.html"><img src="https://img.shields.io/badge/License-GPL--2.0-yellow.svg?style=flat-square" alt="License: GPL-2.0" /></a>
  <a href="https://www.whmcs.com"><img src="https://img.shields.io/badge/WHMCS-8.x%20%7C%209.x-blue.svg?style=flat-square" alt="WHMCS Version" /></a>
  <a href="https://www.php.net"><img src="https://img.shields.io/badge/PHP-8.1%2B-777BB4.svg?style=flat-square&logo=php&logoColor=white" alt="PHP 8.1+" /></a>
  <a href="https://agentskills.io"><img src="https://img.shields.io/badge/Agent_Skills-Spec_Compliant-brightgreen.svg?style=flat-square" alt="Agent Skills Spec" /></a>
  <a href="https://github.com/waqasahmedwaseer"><img src="https://img.shields.io/badge/Author-waqasahmedwaseer-181717.svg?style=flat-square&logo=github" alt="Author" /></a>
</p>

<p align="center">
  <a href="#-quick-install">Quick Install</a> •
  <a href="#-what-it-covers">What It Covers</a> •
  <a href="#-ide-setup-guides">IDE Setup</a> •
  <a href="#-compatible-agents">Compatible Agents</a> •
  <a href="#-contributing">Contributing</a>
</p>

---

## ⚡ Quick Install

### One Command (Recommended)

```bash
npx ai-agent-skills install whmcs-dev-skills
```

That's it. The skill installs to the right location for your agent automatically.

#### Install for a Specific Agent

```bash
# Claude Code (default)
npx ai-agent-skills install whmcs-dev-skills

# Cursor IDE
npx ai-agent-skills install whmcs-dev-skills --agent cursor

# VS Code / GitHub Copilot
npx ai-agent-skills install whmcs-dev-skills --agent vscode

# Windsurf
npx ai-agent-skills install whmcs-dev-skills --agent windsurf

# Amp
npx ai-agent-skills install whmcs-dev-skills --agent amp

# Goose
npx ai-agent-skills install whmcs-dev-skills --agent goose

# OpenCode
npx ai-agent-skills install whmcs-dev-skills --agent opencode

# Any project (generic)
npx ai-agent-skills install whmcs-dev-skills --agent project
```

### Manual Install

```bash
# Clone the repository
git clone https://github.com/waqasahmedwaseer/whmcs-dev-skills.git

# Then copy to your agent's skills directory (see IDE Setup below)
```

---

## 📖 What Is This?

This repository contains a **`SKILL.md`** file — a comprehensive instruction set
designed for AI coding agents. When loaded, it transforms the AI into a **WHMCS
expert** that writes production-ready, secure, and maintainable code following
WHMCS 8.x / 9.x best practices.

The skill follows the **[Agent Skills Specification](https://agentskills.io/specification)** —
the universal open standard for AI agent skills — making it compatible with
every major AI coding agent.

---

## ✨ What It Covers

| Area | Details |
|------|---------|
| 🔌 **Addon Modules** | Config, activate, deactivate, upgrade, admin & client area output |
| 🖥️ **Provisioning Modules** | Create, suspend, unsuspend, terminate, renew, change package, usage updates |
| 🌐 **Domain Registrars** | Register, transfer, renew, sync, nameservers, WHOIS, DNS, EPP codes |
| 💳 **Payment Gateways** | Third-party, merchant, tokenised gateways, callbacks, refunds |
| 🪝 **Action Hooks** | All hook categories with 25+ quick-reference hook points |
| 🔗 **API Integration** | Internal API (`localAPI`) + External API (`GuzzleHTTP`) patterns |
| 🗄️ **Database Operations** | Laravel Capsule ORM patterns, schema creation, migrations |
| 🎨 **Templating & UI** | Smarty v4, native Bootstrap classes, admin & client templates |
| 🔒 **Security** | 13-point checklist: CSRF, binding, encryption, scrubbing, logging |
| 🐛 **Error Handling** | `logModuleCall`, `logActivity`, error return patterns |
| ⚠️ **Common Pitfalls** | 16+ documented anti-patterns with fixes |
| 📁 **Project Structures** | Templates for all 4 module types |
| 📝 **Code Snippets** | Production-ready snippets for security guards, hooks, language files |

### Key Highlights

- 🔒 **Security-first** — Forces Capsule ORM, CSRF protection, credential scrubbing
- 🆕 **WHMCS 9.x ready** — Covers Smarty v4, Illuminate v9, PHP 8.2+ breaking changes
- 📋 **Production-ready code** — Every template is battle-tested, not hello-world
- ⚠️ **Anti-pattern protection** — 16+ real-world failures developers actually make
- 📁 **Proper structure** — Directory templates matching official WHMCS samples

---

## 🤖 Compatible Agents

This skill works with **every major AI coding agent** that supports the
[Agent Skills specification](https://agentskills.io):

| Agent | Skills Directory | Install Flag |
|-------|-----------------|--------------|
| **Claude Code** | `~/.claude/skills/` | `--agent claude` (default) |
| **GitHub Copilot** | `.github/skills/` | `--agent vscode` |
| **Cursor** | `.cursor/skills/` | `--agent cursor` |
| **Windsurf** | `.windsurf/skills/` | `--agent windsurf` |
| **Amp** | `~/.amp/skills/` | `--agent amp` |
| **Goose** | `~/.config/goose/skills/` | `--agent goose` |
| **OpenCode** | `~/.opencode/skills/` | `--agent opencode` |
| **Any Project** | `.skills/` | `--agent project` |

---

## 🛠️ IDE Setup Guides

### Claude Code

```bash
# Option 1: npx (recommended)
npx ai-agent-skills install whmcs-dev-skills

# Option 2: Manual
mkdir -p ~/.claude/skills/whmcs-dev-skills
cp SKILL.md ~/.claude/skills/whmcs-dev-skills/
```

The skill is automatically available in all Claude Code sessions.

---

### GitHub Copilot (VS Code)

GitHub Copilot uses `.github/copilot-instructions.md` for repository-level
instructions, and `.github/skills/` for skill files.

```bash
# Option 1: npx
npx ai-agent-skills install whmcs-dev-skills --agent vscode

# Option 2: Manual — As a skill file
mkdir -p .github/skills/whmcs-dev-skills
cp SKILL.md .github/skills/whmcs-dev-skills/

# Option 3: Manual — As copilot instructions
# Copy the contents of SKILL.md into .github/copilot-instructions.md
cp SKILL.md .github/copilot-instructions.md
```

> **Tip**: After adding the file, open VS Code, and Copilot will automatically
> apply these instructions to all suggestions in the repository.

---

### Cursor IDE

Cursor supports skills in `.cursor/skills/` (project-level) or
`~/.cursor/skills/` (global).

```bash
# Option 1: npx
npx ai-agent-skills install whmcs-dev-skills --agent cursor

# Option 2: Manual — Project-level (recommended)
mkdir -p .cursor/skills/whmcs-dev-skills
cp SKILL.md .cursor/skills/whmcs-dev-skills/

# Option 3: Manual — Global (applies to all projects)
mkdir -p ~/.cursor/skills/whmcs-dev-skills
cp SKILL.md ~/.cursor/skills/whmcs-dev-skills/

# Option 4: As a Cursor Rule (.mdc format)
mkdir -p .cursor/rules
cp SKILL.md .cursor/rules/whmcs-dev-skills.mdc
```

> **Tip**: You can also import directly via `Cursor Settings → Rules,
> Commands → Add Rule → Remote Rule (GitHub)` and point it to this repo.

---

### Windsurf (Codeium)

Windsurf uses `.windsurfrules` for project rules and `global_rules.md` for
global rules.

```bash
# Option 1: npx
npx ai-agent-skills install whmcs-dev-skills --agent windsurf

# Option 2: Manual — Project-level
mkdir -p .windsurf/rules
cp SKILL.md .windsurf/rules/whmcs-dev-skills.md

# Option 3: Manual — As project rules
cp SKILL.md .windsurfrules

# Option 4: Manual — Global rules
# Append to: ~/.codeium/windsurf/memories/global_rules.md
cat SKILL.md >> ~/.codeium/windsurf/memories/global_rules.md
```

---

### Gemini (Google AI)

For Gemini-powered agents and Google's AI tools:

```bash
# Project-level skill
mkdir -p .gemini/skills/whmcs-dev-skills
cp SKILL.md .gemini/skills/whmcs-dev-skills/

# Or as agent instructions
mkdir -p .agent/skills
cp SKILL.md .agent/skills/whmcs-dev-skills.md
```

---

### Amp

```bash
# Option 1: npx
npx ai-agent-skills install whmcs-dev-skills --agent amp

# Option 2: Manual
mkdir -p ~/.amp/skills/whmcs-dev-skills
cp SKILL.md ~/.amp/skills/whmcs-dev-skills/
```

---

### Any Other Agent

For any agent that supports the [Agent Skills spec](https://agentskills.io):

```bash
# Project-level (works with any agent)
mkdir -p .skills/whmcs-dev-skills
cp SKILL.md .skills/whmcs-dev-skills/
```

Or simply place the `SKILL.md` file in whatever directory your agent reads
skill/instruction files from.

---

## 📁 Repository Structure

```
whmcs-dev-skills/
├── SKILL.md        # 📘 The comprehensive 1,600-line AI agent skill file
├── README.md       # 📖 This file — documentation & install guide
├── LICENSE         # ⚖️  GPL-2.0 License
└── .gitignore      # 🙈 Git ignore rules
```

---

## 🔬 Research-Backed

This skill file was built from **deep research** of:

- ✅ [WHMCS Official Developer Documentation](https://developers.whmcs.com/) — Every module type page
- ✅ [WHMCS GitHub Sample Modules](https://github.com/WHMCS) — All 5 official samples
- ✅ WHMCS 8.x → 9.x breaking changes — Smarty v4, Illuminate v9, PHP 8.2+
- ✅ Common developer issues from [WHMCS Community Forums](https://whmcs.community/)
- ✅ Security best practices and anti-patterns from real-world deployments
- ✅ [Agent Skills Specification](https://agentskills.io/specification) — Universal skill format

---

## ❓ FAQ

<details>
<summary><strong>What AI agents does this work with?</strong></summary>

This skill follows the [Agent Skills Specification](https://agentskills.io),
which is supported by Claude Code, GitHub Copilot, Cursor, Windsurf, Amp,
Goose, OpenCode, and any agent that reads `SKILL.md` files.
</details>

<details>
<summary><strong>Do I need Node.js to install?</strong></summary>

Only if you use the `npx` install method. You can also manually copy the
`SKILL.md` file to the appropriate directory for your agent.
</details>

<details>
<summary><strong>Does this modify my WHMCS installation?</strong></summary>

No! This skill file only provides instructions to your AI coding agent. It
doesn't touch your WHMCS installation, database, or configuration in any way.
</details>

<details>
<summary><strong>Is this compatible with WHMCS 9.x?</strong></summary>

Yes! The skill includes specific guidance for WHMCS 9.x breaking changes
including Smarty v4, Illuminate v9, and PHP 8.2+ requirements.
</details>

<details>
<summary><strong>Can I use this for commercial module development?</strong></summary>

Absolutely. The skill is GPL-2.0 licensed. Use it for personal, commercial, or
open-source module development.
</details>

<details>
<summary><strong>How is this different from reading the WHMCS docs?</strong></summary>

This skill is specifically formatted for AI agents — it includes operational
boundaries (ALWAYS/NEVER rules), anti-pattern protection, security checklists,
and production-ready code templates that documentation alone doesn't provide.
The AI uses this as a continuous reference while coding.
</details>

---

## 🤝 Contributing

Contributions are welcome! Whether it's fixing a typo, adding a new hook
point, or documenting another anti-pattern:

1. **Fork** this repository
2. **Create** a feature branch: `git checkout -b feature/awesome-improvement`
3. **Make** your changes to `SKILL.md`
4. **Commit**: `git commit -m "feat: add awesome improvement"`
5. **Push**: `git push origin feature/awesome-improvement`
6. **Open** a Pull Request

### Contribution Ideas

- 📝 Add more WHMCS 9.x breaking changes as they're discovered
- 🪝 Document additional hook points with examples
- 🔒 Add more security best practices
- 🐛 Document common bugs and their fixes
- 🌍 Add non-English language file examples

---

## ⭐ Star History

If this skill saved you time building WHMCS modules, please give it a ⭐!

---

## 📄 License

This project is licensed under the [GNU GPL v2.0](LICENSE).

---

## 👤 Author

<table>
  <tr>
    <td>
      <strong>Waqas Ahmed Waseer</strong><br/>
      <a href="https://waqasahmedwaseer.com">🌐 waqasahmedwaseer.com</a><br/>
      <a href="https://github.com/waqasahmedwaseer">🐙 @waqasahmedwaseer</a>
    </td>
  </tr>
</table>

---

<p align="center">
  Built with ❤️ for the WHMCS developer community
  <br/>
  <sub>If this helped you, consider giving it a ⭐ on GitHub!</sub>
</p>
