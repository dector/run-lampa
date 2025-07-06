# Run Lampa GitHub Action

This GitHub Action runs the [Lampa](https://github.com/dector/lampa) static analysis tool.

## Usage

To use this action in your GitHub Workflows, add the following step to your job:

```yaml
- name: Run Lampa
  uses: dector/run-lampa@v1
  with:
    # The version of Lampa to use (e.g., v0.1.0). "latest" is also supported.
    # Default is "latest".
    version: "latest"

    # Arguments to pass to the lampa command. See tool repo for instructions.
    # Optional, default is empty.
    args: "collect"

    # GitHub token for authenticated API requests to avoid rate limiting.
    # Optional, default is ${{ github.token }}.
    github-token: ${{ secrets.GITHUB_TOKEN }}
```

### Inputs

| Name           | Description                                                              | Default            | Required |
|----------------|--------------------------------------------------------------------------|--------------------|----------|
| `version`      | The version of Lampa to use (e.g., `v0.1.0`). `latest` is also supported. | `latest`           | **true** |
| `args`         | Arguments to pass to the `lampa` command.                                | `''`               | **false**|
| `github-token` | GitHub token for authenticated API requests to avoid rate limiting.      | `${{ github.token }}` | **false**|

## Example Workflow

Here is an example workflow that runs Lampa on every push to the `main` branch:

```yaml
name: Lampa Analysis

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  lampa:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Set up JDK
        uses: actions/setup-java@v4
        with:
          java-version: "17"
          distribution: "temurin"

      - name: Set up Android SDK
        uses: android-actions/setup-android@v3

      - name: Run Lampa
        uses: dector/run-lampa@v1
        with:
          args: "collect --variant debug --format html,json"

      - name: Publish report
        uses: actions/upload-artifact@v4
        with:
          name: lampa-report
          path: |
            report.lampa.json
            report.lampa.html
```

## License

The scripts and documentation in this project are released under the [MIT License](LICENSE).
