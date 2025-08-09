Title: feat(gpt5): add model_verbosity config and wire text.verbosity for GPT-5 via Responses API

Summary
- Adds a new config option `model_verbosity` (values: `low`, `medium`, `high`).
- Wires `text.verbosity` into the OpenAI Responses API request when the selected model family is GPT‑5.
- Updates docs and adds tests for serialization/omission behavior.

Motivation
- GPT‑5 introduces a verbosity control to steer output length/detail without prompt surgery.
- Exposing this as a first-class config knob keeps prompts stable and makes the behavior explicit and repeatable.

Scope of changes
- Config types: introduce `Verbosity` enum and optional `model_verbosity` in `ConfigToml` and `Config`.
- Request wiring: add optional `text` field with `verbosity` to `ResponsesApiRequest` and set it only when using GPT‑5.
- Docs: document `model_verbosity` in `config.md` and add a short note to `README.md`.
- Tests: verify that `text.verbosity` serializes when set, and is omitted when not set.

Usage
```toml
model = "gpt-5"
model_verbosity = "low"   # or "medium" (default) / "high"
```

Notes
- The `text` field is omitted for non‑GPT‑5 model families.
- Chat Completions providers are unaffected.

Validation
- Ran `cargo fmt` and `cargo test --all-features` locally — all tests pass.
- Could not run `just fmt` / `just fix` in this environment (no `just`), but repository formatting is clean; CI should verify.
