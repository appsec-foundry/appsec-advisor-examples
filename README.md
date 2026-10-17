# appsec-advisor examples

Example outputs of [appsec-advisor](https://github.com/appsec-foundry/appsec-advisor),
a Claude Code plugin that derives security architecture and threats from a
repository's code and configuration.

## Files

The current security assessment and supporting artifacts are in
[`threat-modeler/`](threat-modeler/). Files belonging to the same assessment
share the name `security-assessment-<target>-<depth>-v<version>`. The version
suffix identifies the plugin release (`b5` is beta 5).

- `.md`, `.html`, `.pdf`: the report
- `.figure1.svg`, `.figure1b.svg`, `.figure2.svg`: architecture and risk-flow
  diagrams, also available as `-dark.svg` variants
- `.yaml`: the structured assessment, including plugin version, models, and
  invocation in its `meta` block
- `.sarif.json`: code-scanning results (SARIF 2.1)
- `.threatdragon.json`: export for Threat Dragon and ThreatAtlas
- `pentest-tasks*.yaml`: retained endpoint catalogs and pentest plans from
  earlier runs

## OWASP Juice Shop

[Juice Shop](https://owasp.org/www-project-juice-shop/) is a deliberately
insecure web shop. The assessment covers its Angular, Node.js, Express,
Socket.IO, Sequelize, SQLite, and Docker architecture.

The current assessment was generated on October 17, 2026 with appsec-advisor
v0.6.0-beta.5 (analysis v5).

- [Markdown report](threat-modeler/security-assessment-juice-shop-standard-v0.6.0b5.md)
- [HTML report](threat-modeler/security-assessment-juice-shop-standard-v0.6.0b5.html)
- [PDF report](threat-modeler/security-assessment-juice-shop-standard-v0.6.0b5.pdf)
- [Structured YAML](threat-modeler/security-assessment-juice-shop-standard-v0.6.0b5.yaml)
- [SARIF](threat-modeler/security-assessment-juice-shop-standard-v0.6.0b5.sarif.json)
- [Threat Dragon](threat-modeler/security-assessment-juice-shop-standard-v0.6.0b5.threatdragon.json)

The filename matches the assessment metadata:
`assessment_depth: standard` and the invocation
`--assessment-depth standard --hard-budget 30`. Its reasoning profile is
`sonnet-economy`, with `claude-sonnet-5-5` recorded for the orchestrator,
STRIDE analysis, triage, and merging.

The [retained Juice Shop pentest plan](threat-modeler/pentest-tasks-juice-shop-standard-v0.6.0b4.yaml)
is a separate beta 4 artifact targeting `http://localhost:3000`; it was not
generated from the current assessment.

## Assessment depths

Coverage, cost, and runtime of each depth are described under
[Assessment depth & cost control](https://github.com/appsec-foundry/appsec-advisor/blob/main/docs/threat-modeler.md#assessment-depth--cost-control).
The YAML `meta.assessment_depth` field records the depth used for an assessment.

## Running it yourself

Install the plugin as described in the
[Quick start](https://github.com/appsec-foundry/appsec-advisor#quick-start),
then run `/appsec-advisor:appsec-analyzer` in Claude Code from the repository
you want to assess. The current Juice Shop assessment records this invocation:

```text
/appsec-advisor:appsec-analyzer --assessment-depth standard --hard-budget 30
```

See `meta.invocation` in the YAML for the recorded analysis options.
