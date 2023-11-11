# Setup project

## Linter

- Clippy
  - Comes by default, as it's the official Rust linter
  - If the CI env uses rustup's minimal profile, it would not include Clippy. But we can easly install it via rust component add `rustup component add clippy`
  - Running clippy for your project via `cargo clippy`
  - If we want to fail the linter check if clippy emits any warnings in the CI pipeline, we can do that with `cargo clippy -- -D warnings`
  - Fun tip - clippy has been named after the famous paperclip-shaped microsoft word assitant
  - To mute warnings `#[allow(clippy::lint_name)]` attribute or disable the noisy lint altogether for the whole project with a confing in clippy.toml or project level `#![allow(clippy::lint_name)]` directive.

## Code Coverage

- Cargo-Tarpaulin
  - Install with `cargo install cargo-tarpaulin`.
  - Command to compute code coverage without test functions included `cargo tarpaulin --ingore-tests`
  - Can also be used to upload code coverage metrics to services like (codecov.io)[Codecov.io] or Coveralls

## Reduce percieved compilation time

- Cargo-watch
  - Install with `cargo install cargo-watch`
  - This does for you the cargo commands, after you save the code, until you get to see if it was compiled, you the compilation is almost done - under the hood it does `cargo check` & `cargo run`!
  - Monitors your source code to trigger commands everytime a file changes `cargo watch -x check`. Basically this runs the `cargo check` each time a file is changed. 
  - It also support chaining `cargo watch -x check -x test -x run`
  This will first run `cargo check` -> `cargo test` -> `cargo run` - of course if every command before it, is successfull.

## Formatter

- Rustfmt
  - This is the official Rust formatter
  - Like clippy, rustfmt is included in the set of default components installed by `rustup`
  - If needed to be install, like inside a minimal configuration on CI - `rustup component add rustfmt`
  - Can format whole project with `cargo fmt`
  - Inside the CI pipeline we will add a formatting step: `cargo fmt -- --check` - will fail when a commit contains unformatted code, printing the difference to the console.
    - We can also use the git pre-push hook, to format the code before it gets pushed.
  - Can configure rustfmt via a config file `rustfmt.toml` (more info in rustfmt README)

## Basic health check endpoint

- Used to check if the application is up and ready to accept incoming request.
- We can combine this with a SaaS service like `pingdom.com`, and we can be alerted when the API goes dark
- Also good if you use a container orchestrator like `Kubernetes` or `Nomad` - the orchestrator can call the `/health_check` to detect if the API has become unresponsive and trigger a restart.
