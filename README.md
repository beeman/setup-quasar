# setup-quasar

GitHub Action for installing the Agave Solana CLI and Quasar CLI on GitHub-hosted Linux runners.

## Usage

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@8e8c483db84b4bee98b60c0593521ed34d9990e8 # v6

      - name: Setup Quasar
        id: setup-quasar
        uses: beeman/setup-quasar@v1

      - name: Verify tools
        run: |
          cargo-build-sbf --version
          quasar --help
          solana --version
```

## Inputs

| Name | Description | Default |
| --- | --- | --- |
| `agave-version` | Agave release version to install. | `3.1.13` |
| `install-rust` | Whether to install Rust before building Quasar. | `true` |
| `quasar-ref` | Git ref to resolve Quasar from when `quasar-sha` is not set. | `master` |
| `quasar-repo` | Git repository to install Quasar from. | `https://github.com/blueshift-gg/quasar.git` |
| `quasar-sha` | Exact Quasar commit SHA to install. | `""` |
| `rust-toolchain` | Rust toolchain used to build Quasar. | `stable` |

## Outputs

| Name | Description |
| --- | --- |
| `agave-version` | Agave version installed by the action. |
| `quasar-sha` | Exact Quasar commit SHA installed by the action. |

## Quasar Revision Selection

- If `quasar-sha` is set, the action installs that exact commit.
- If `quasar-sha` is empty, the action resolves `quasar-ref` from `quasar-repo`.
- The default Quasar source is `https://github.com/blueshift-gg/quasar.git` at `master`.

## Runner Requirements

- `awk`, `bash`, `curl`, `git`, and `sha256sum` must be available on the runner.
- `ubuntu-latest` is the expected environment.

## Notes

- Rust is installed by default before Quasar is built. Set `install-rust: "false"` if your runner already has the required toolchain.
- The action adds both the Agave CLI and Quasar binary locations to `PATH` for later steps.
