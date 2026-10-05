# Contribute to helm-charts

Contributions can fix behavior, improve documentation, or add focused tests.

## Before you start

Read [the support guide](SUPPORT.md) for questions and issue routing.
Search existing issues and pull requests. Discuss larger API, architecture, or dependency changes before implementation.

Work from `main` and target that branch in your pull request.
Keep each change focused. Avoid unrelated formatting and dependency updates.

## Prepare a checkout

Install Helm and a Kubernetes version supported by the chart. A cluster is only needed for runtime verification.

Run the commands below from the repository root unless a command names another directory.
On Windows, use `gradlew.bat` in place of `./gradlew` for Gradle commands.

## Repository layout

- `charts/enderdash-agent/`: chart templates, values, and metadata.

## Verify your change

```bash
helm lint charts/enderdash-agent
helm template enderdash-agent charts/enderdash-agent > /tmp/enderdash-chart.yaml
helm template enderdash-agent charts/enderdash-agent --set rbac.mode=readonly > /tmp/enderdash-chart-readonly.yaml
```

Inspect both default and read-only RBAC output. Cover changed persistence, Secret references, resource settings, and image overrides. For runtime changes, use a disposable cluster and verify upgrades as well as fresh installs. Increase the chart version for chart changes. Change `appVersion` only when the packaged application version changes. Do not put an agent key in values files.

Run the relevant checks before review. State the command and result in the pull request.
If a check cannot run, explain the missing dependency or service. Do not claim it passed.
Keep generated artifacts consistent with their source and review their diff.

## Style and documentation

Follow the existing code conventions and repository formatter. Keep commit hooks enabled.
Add focused tests for changed logic when practical. Avoid tests that only assert source strings.
Update documentation when commands, APIs, configuration, or expected behavior change.
Keep examples small and reproducible. Preserve exact identifiers, commands, and error messages.

## Open a pull request

Explain the problem and resulting behavior. Link related issues without a placeholder issue number.
Include Helm lint/template results and upgrade evidence when required. Explain chart version, values, and RBAC changes.
Include commands and results. State any runtime checks that remain necessary.
Respond to review with a correction or concrete evidence.

Use Conventional Commits: `type(scope): description`, for example `docs(contributing): explain local validation`.
Use a meaningful scope, or omit it. Keep the subject concise and imperative.
Add a body when the reason or compatibility impact is not obvious.

For vulnerabilities, follow [the security reporting instructions](SECURITY.md).
Remove credentials and private data from examples, logs, and screenshots.
