# appsec-advisor examples

Example outputs of [appsec-advisor](https://github.com/appsec-foundry/appsec-advisor),
a Claude Code plugin that builds a threat model from a repository's code and
configuration: it derives the architecture and runs STRIDE against it.

This repository complements the plugin's own
[`examples/threat-modeler`](https://github.com/appsec-foundry/appsec-advisor/tree/dev/examples/threat-modeler)
directory with more targets and depths. The reports show what the output looks
like before you run a scan yourself.

## Files

All runs are in [`threat-modeler/`](threat-modeler/). The files of one run share
the name `threat-model-<target>-<depth>-v<version>`. The version suffix is the
plugin release (`b4` is beta 4), so runs from different releases can sit side by
side.

- `.md`, `.html`, `.pdf`: the report
- `.figure1.svg`, `.figure2.svg`: the figures used in the report
- `.yaml`: the structured model, including plugin version, models and
  invocation in its `meta` block
- `.sarif.json`: code-scanning results (SARIF 2.1)
- `.threatdragon.json`: export for Threat Dragon and ThreatAtlas
- `pentest-tasks-*.yaml`: endpoint catalog and pentest plan

Not every run has all of these. Only the thorough Juice Shop run was exported
to HTML, PDF, SARIF and Threat Dragon.

## Models

The standard runs use Claude Sonnet 4.6 as orchestrator and for the STRIDE
analysis. Triage and merging of findings run on Claude Sonnet 5. The thorough
run uses Claude Opus throughout.

## Examples

### OWASP Juice Shop

[Juice Shop](https://owasp.org/www-project-juice-shop/) is a deliberately
insecure web shop (version 20.1.1 in all runs). Stack: Angular, Node.js,
Express, Socket.IO, Sequelize, SQLite, Docker.

- [Standard run](threat-modeler/threat-model-juice-shop-standard-v0.6.0b4.md):
  6 critical, 42 high, 23 medium, 71 findings in total. Also available as
  [YAML](threat-modeler/threat-model-juice-shop-standard-v0.6.0b4.yaml).
- [Thorough run](threat-modeler/threat-model-juice-shop-thorough-v0.6.0b5.md):
  6 critical, 21 high, 32 medium, 59 findings in total. Also available as
  [YAML](threat-modeler/threat-model-juice-shop-thorough-v0.6.0b5.yaml),
  [HTML](threat-modeler/threat-model-juice-shop-thorough-v0.6.0b5.html),
  [PDF](threat-modeler/threat-model-juice-shop-thorough-v0.6.0b5.pdf),
  [SARIF](threat-modeler/threat-model-juice-shop-thorough-v0.6.0b5.sarif.json)
  and [Threat Dragon](threat-modeler/threat-model-juice-shop-thorough-v0.6.0b5.threatdragon.json).
- [Pentest tasks](threat-modeler/pentest-tasks-juice-shop-thorough-v0.6.0b4.yaml):
  endpoint catalog and pentest plan in the Strix dialect for
  `http://localhost:3000`, exported from the thorough run.

### OWASP VulnerableApp

[VulnerableApp](https://github.com/SasanLabs/VulnerableApp) is an application
for demonstrating and testing security issues. Stack: Java, Spring Boot, JSP,
PHP.

- [Standard run](threat-modeler/threat-model-owasp-vulnerableapp-v0.6.0b4.md):
  4 critical, 23 high, 21 medium, 48 findings in total. Also available as
  [YAML](threat-modeler/threat-model-owasp-vulnerableapp-v0.6.0b4.yaml).

### Insecure Large Spring App

The files `threat-model-insecure-large-spring-app-*` come from
[Insecure Large Spring App](https://github.com/matthiasrohr/insecure-large-spring-app),
a repository built to test how the process copes with many components. It
defines 42 Docker Compose services in 7 network zones.

The standard run models 21 components and 124 entry points. 14 components get a
full STRIDE analysis. The other 7 are out of scope at standard depth and are
listed in the report under "Components Not Individually Analyzed". Result: 13
critical, 40 high, 16 medium, 69 findings in total.

Files: [report](threat-modeler/threat-model-insecure-large-spring-app-v0.6.0b4.md),
[YAML](threat-modeler/threat-model-insecure-large-spring-app-v0.6.0b4.yaml),
[figure 1 (architecture)](threat-modeler/threat-model-insecure-large-spring-app-v0.6.0b4.figure1.svg),
[figure 2 (risk flow)](threat-modeler/threat-model-insecure-large-spring-app-v0.6.0b4.figure2.svg).

All counts exclude low and informational findings (reporting threshold medium).

## Assessment depths

Coverage, cost and runtime of each depth are described under
[Assessment depth & cost control](https://github.com/appsec-foundry/appsec-advisor/blob/main/docs/threat-modeler.md#assessment-depth--cost-control).

- quick: early feedback and low-risk changes. Skips abuse-case validation and
  the final model-based QA.
- standard (default): the usual choice. Full analysis, abuse-case validation
  and QA.
- thorough: for high-risk services and major releases. Deeper component
  analysis and an architecture review.

## Running it yourself

Install the plugin as described in the
[Quick start](https://github.com/appsec-foundry/appsec-advisor#quick-start),
then run `/appsec-advisor:create-threat-model` in Claude Code from the
repository you want to model. The `meta.invocation` field in each YAML file
shows which options a run used. The thorough Juice Shop run, for example, used
`--thorough --sarif --pentest-tasks --pentest-format strix --pentest-target http://localhost:3000 --threatdragon --pdf --html`.
