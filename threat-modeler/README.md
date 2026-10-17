# Security assessment example reports

This directory contains the generated reports and supporting artifacts. See the
[repository README](../README.md) for an overview of the examples, assessment
depths, artifact formats, and instructions for running the threat modeler.

Files belonging to the same assessment share a common
`security-assessment-<target>-<depth>-v<version>` slug. The current Juice Shop
assessment is available as [Markdown](security-assessment-juice-shop-standard-v0.6.0b5.md),
[HTML](security-assessment-juice-shop-standard-v0.6.0b5.html),
[PDF](security-assessment-juice-shop-standard-v0.6.0b5.pdf),
[YAML](security-assessment-juice-shop-standard-v0.6.0b5.yaml),
[SARIF](security-assessment-juice-shop-standard-v0.6.0b5.sarif.json), and
[Threat Dragon](security-assessment-juice-shop-standard-v0.6.0b5.threatdragon.json).

The filename matches this assessment's metadata:
`assessment_depth: standard` and `--assessment-depth standard --hard-budget 30`.
The retained `pentest-tasks` files are separate artifacts from earlier runs.
