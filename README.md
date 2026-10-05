# appsec-advisor examples

Example outputs from [appsec-advisor](https://github.com/appsec-foundry/appsec-advisor),
a Claude Code plugin for code-derived threat modeling: it reads the code and
configuration in a repository, builds an architecture model, and runs STRIDE
against it.

This repository is the companion to the plugin's own
[`examples/threat-modeler`](https://github.com/appsec-foundry/appsec-advisor/tree/dev/examples/threat-modeler)
directory and carries additional targets, depths and historical runs. Use the
reports to see the report structure, depth levels and artifact formats before
running a scan of your own.

## What's here

All runs live in [`threat-modeler/`](threat-modeler/). Each run produces a set
of files that share a common slug `threat-model-<target>-<depth>-v<version>`:

| Report formats | Data and integration formats |
|----------------|------------------------------|
| `.md` — human-readable report | `.yaml` — structured model |
| `.html` — browser-readable report | `.sarif.json` — SARIF v2.1 code-scanning results |
| `.pdf` — printable report with cover and TOC | `.threatdragon.json` — Threat Dragon and ThreatAtlas export |
| `.figure1.svg`, `.figure2.svg` — figures shown in the report | `pentest-tasks-*.yaml` — endpoint catalog and pentest plan |

Optional outputs are linked for each run below when they were generated.

The `-v<version>` suffix names the plugin release a run belongs to (`b4` =
beta 4), so outputs from different releases stay side by side and comparable.
Each YAML file records the exact plugin version, models and invocation in its
`meta` block.

## Examples

**[OWASP Juice Shop](https://owasp.org/www-project-juice-shop/)** — deliberately
insecure web shop, v20.1.1 in all runs:

- **Tech stack:** Angular, Node.js, Express, Socket.IO, Sequelize, SQLite, Docker.
- **[Standard](threat-modeler/threat-model-juice-shop-standard-v0.6.0b4.md)** —
  Model: Claude Sonnet 4.6, triage and merge on Claude Sonnet 5 · 🔴 6 Critical ·
  🟠 42 High · 🟡 23 Medium · 71 total, reporting threshold medium.
  Artifacts: [YAML](threat-modeler/threat-model-juice-shop-standard-v0.6.0b4.yaml).
- **[Thorough](threat-modeler/threat-model-juice-shop-thorough-v0.6.0b4.md)** —
  Model: Claude Opus · 🔴 6 Critical · 🟠 24 High · 🟡 36 Medium · 66 total.
  Artifacts: [YAML](threat-modeler/threat-model-juice-shop-thorough-v0.6.0b4.yaml) ·
  [HTML](threat-modeler/threat-model-juice-shop-thorough-v0.6.0b4.html) ·
  [PDF](threat-modeler/threat-model-juice-shop-thorough-v0.6.0b4.pdf) ·
  [SARIF](threat-modeler/threat-model-juice-shop-thorough-v0.6.0b4.sarif.json) ·
  [Threat Dragon](threat-modeler/threat-model-juice-shop-thorough-v0.6.0b4.threatdragon.json).
- **[Pentest tasks](threat-modeler/pentest-tasks-juice-shop-thorough-v0.6.0b4.yaml)** —
  endpoint catalog and pentest plan in the Strix dialect for
  `http://localhost:3000`, exported from the
  [thorough run](threat-modeler/threat-model-juice-shop-thorough-v0.6.0b4.md).

**[OWASP VulnerableApp](https://github.com/SasanLabs/VulnerableApp)** —
vulnerable application for demonstrating and testing security issues:

- **Tech stack:** Java, Spring Boot, JSP, PHP.
- **[Standard](threat-modeler/threat-model-owasp-vulnerableapp-v0.6.0b4.md)** —
  Model: Claude Sonnet 4.6, triage and merge on Claude Sonnet 5 ·
  🔴 4 Critical · 🟠 23 High · 🟡 21 Medium · 48 total, reporting threshold medium.
  Artifacts: [YAML](threat-modeler/threat-model-owasp-vulnerableapp-v0.6.0b4.yaml).

## Assessment depths

For component coverage criteria, cost, and runtime guidance, see
[Assessment depth & cost control](https://github.com/appsec-foundry/appsec-advisor/blob/main/docs/threat-modeler.md#assessment-depth--cost-control).

- **quick** — early feedback and low-risk changes; reduced analysis that skips
  abuse-case validation and final model-based QA.
- **standard** *(default)* — normal threat models and security reviews; full
  analysis, abuse-case validation, and QA.
- **thorough** — high-risk services and major releases; deeper component
  analysis and architecture review.

## Large-component-count test fixture

Files matching `threat-model-insecure-large-spring-app-*` were generated from
the custom-built
[Insecure Large Spring App](https://github.com/matthiasrohr/insecure-large-spring-app)
repository. The fixture was built to test how the threat-modeling process
handles applications with a large number of components.

The repository defines 42 Docker Compose services across 7 network zones. The
standard run (Claude Sonnet 4.6, triage and merge on Claude Sonnet 5)
represents the system as 21 logical components, identifies 124 entry points and
performs full STRIDE analysis on 14 components. The other 7 are listed under
"Components Not Individually Analyzed" because they are out of scope at
standard depth. 🔴 13 Critical · 🟠 40 High · 🟡 16 Medium · 69 total,
reporting threshold medium.

Artifacts: [report](threat-modeler/threat-model-insecure-large-spring-app-v0.6.0b4.md) ·
[YAML](threat-modeler/threat-model-insecure-large-spring-app-v0.6.0b4.yaml) ·
[figure 1: architecture](threat-modeler/threat-model-insecure-large-spring-app-v0.6.0b4.figure1.svg) ·
[figure 2: risk flow](threat-modeler/threat-model-insecure-large-spring-app-v0.6.0b4.figure2.svg).

## Run it yourself

Install the plugin as described in the appsec-advisor
[Quick start](https://github.com/appsec-foundry/appsec-advisor#quick-start),
then run `/appsec-advisor:create-threat-model` from Claude Code in the
repository you want to model. The `meta.invocation` field of each YAML file
shows the options a run used; the thorough Juice Shop run, for example, used
`--thorough --sarif --pentest-tasks --pentest-format strix --pentest-target http://localhost:3000 --threatdragon --pdf --html`.
