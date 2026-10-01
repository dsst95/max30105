# Contributing

Thanks for helping improve this MAX30105 driver. Issues, bug reports, documentation improvements, and pull requests are welcome.

## Set up your development environment

### Requirements

- Git, to clone the repository and create a branch.
- Rust stable with Cargo. This crate uses the Rust 2024 edition, which requires Rust 1.85 or newer. The current stable toolchain is recommended.
- The `rustfmt` and `clippy` Rust components, for formatting and lint checks.

Install Rust using [rustup](https://rustup.rs/). After installing it, install the stable toolchain and the components used by this project:

```sh
rustup toolchain install stable --component rustfmt --component clippy
rustup default stable
```

Check that the tools are available:

```sh
rustc --version
cargo --version
cargo fmt --version
cargo clippy --version
```

No board-specific toolchain, cross-compilation target, or MAX30105 hardware is required to build and run the host-side checks. The driver is `no_std` outside tests and implements an async `embedded-hal` I2C interface; hardware testing is only needed to validate behavior on a real device.

### Clone and build

Clone the repository and enter its directory:

```sh
git clone https://github.com/dsst95/max30105.git
cd max30105
```

Build the crate and run its tests once to confirm the environment is ready:

```sh
cargo build
cargo test
```

Cargo downloads dependencies on the first build. The build output is stored in `target/`.

## Before making a change

- For substantial changes or new features, open an issue first to discuss the approach.
- Keep changes focused, and describe any hardware assumptions or behavior that may affect users of the driver.
- Follow the existing Rust style and avoid adding dependencies unless they are needed.

Create a topic branch before editing:

```sh
git switch -c describe-your-change
```

The main implementation is in `src/lib.rs`; sensor configuration types and register definitions live in `src/configuration.rs` and `src/register/`. Keep register values and behavior consistent with the MAX30105 datasheet, and prefer tests for observable behavior such as I2C transactions.

## Checks and tests

Run these checks from the repository root before opening a pull request. They match the checks listed in the pull request template and CI:

```sh
cargo fmt --all -- --check
cargo build
cargo clippy --all-targets --all-features -- -D warnings
cargo test
```

Use `cargo fmt --all` to apply formatting if the formatting check reports changes. Tests run on the host; the existing driver tests use a mock I2C implementation, so they do not need physical hardware. If a change requires a real sensor or board to verify, note whether you performed that test and describe the setup. If you could not perform it, say so and report the host-side checks you did run.

## Pull requests

- Explain the problem and the change, and link any related issue.
- Include or update tests for behavior changes where practical.
- Update documentation when public behavior or configuration changes.
- Report the commands you ran and their results; call out any checks you could not run.
- State any relevant hardware, wiring, or measurement assumptions, and whether hardware validation was performed.
- Keep unrelated formatting or refactoring out of the change.

The pull request template includes a checklist to help summarize validation and documentation updates.

Please follow the [Code of Conduct](CODE_OF_CONDUCT.md) in all project spaces.
