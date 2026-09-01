# WebGoat.NET

This repository is a fork of [OWASP WebGoat.NET](https://github.com/jerryhoff/WebGoat.NET). The application source is kept as-is.

Everything specific to this fork lives outside the application code:

- [`qodana.yaml`](./qodana.yaml) - the Qodana run profile
- [`.qodana/opengrep/`](./.qodana/opengrep) - custom OpenGrep rules
- [`expected-traces/`](./expected-traces) - baselines the scan results are compared against, including one third-party baseline under [its own license](./expected-traces/sonar-ground-truth.LICENSE)

## Analyzing with Qodana

Run the following command from the repository root to analyze the project with a Qodana linter using default and custom rules:

```bash
docker run --rm -it \
    -p 8080:8080 \
    -e QODANA_TOKEN="$QODANA_TOKEN" \
    -v "$(pwd)":/data/project/ \
    -v "$(pwd)/.qodana/results":/data/results \
    -v "$(pwd)/.qodana/cache":/data/cache \
    jetbrains/qodana-dotnet:2026.2 \
    --show-report
```

Or disable default rules using the `--property=intellij.opengrep.rules.downloaded.source.enabled=false` flag to run only with the custom rules:

```bash
docker run --rm -it \
    -p 8080:8080 \
    -e QODANA_TOKEN="$QODANA_TOKEN" \
    -v "$(pwd)":/data/project/ \
    -v "$(pwd)/.qodana/results":/data/results \
    -v "$(pwd)/.qodana/cache":/data/cache \
    jetbrains/qodana-dotnet:2026.2 \
    --property=intellij.opengrep.bundled.rules.enabled=false \
    --show-report
```

## Expected traces

There is a baseline from the Sonar benchmark suite: [`sonar-ground-truth.json`](./expected-traces/sonar-ground-truth.json) taken from here: (https://github.com/SonarSource/sonar-benchmarks-scores/blob/master/csharp/security/WebGoat.Net/ground-truth.json), imported verbatim from upstream commit [`d114be8`](https://github.com/SonarSource/sonar-benchmarks-scores/commit/d114be8). Please note that this benchmark contains only the sinks and not the full traces from the Taint Analysis / Data flow analysis.

The file is licensed under LGPL-3.0, © SonarSource S.A. and contributors; the full license text is in [`sonar-ground-truth.LICENSE`](./expected-traces/sonar-ground-truth.LICENSE) (LGPL v3 supplements the [GNU GPL v3](https://www.gnu.org/licenses/gpl-3.0.txt), which it incorporates by reference).

There are also two files with the full expected traces from the Qodana team: [`expected-traces.txt`](./expected-traces/expected-traces.txt) produced by Qodana without additional custom rules and [`expected-traces-custom-rules.txt`](./expected-traces/expected-traces-custom-rules.txt) that requires the rules from the [`.qodana/opengrep`](./.qodana/opengrep) directory to reproduce.
