# tfx local testing

This work represents the process to upgrade TFX and its full dependency chain to Python 3.12.4, matching Pritam's build system (Fedora 42).

## Directory layout

```
tfx-pritam/
├── .devcontainer/
├── .python-version
├── pyproject.toml
├── main.py
└── libs/ ← git submodules
    ├── tensorflow-metadata/
    ├── ml-metadata/
    ├── tfx-bsl/
    └── tfx/
    └── etc
```

## Setup

### 1. Create an attempt branch

Branch names encode what the attempt targets:

```
attempt/<component>-v<version>-py<python>
attempt/<component>-<source-branch>
```

Examples:
- `attempt/tfx-bsl-v1.17.0-py3.12.4`
- `attempt/ml-metadata-pritam-branch`

```bash
git checkout -b attempt/tfx-bsl-v1.17.0-py3.12.4
```

### 2. Add the relevant submodule

```bash
git submodule add -b <branch> https://github.com/<lead>/tfx-bsl libs/tfx-bsl
git submodule update --init --recursive
```

> I am pretty sure that you can also check out specific versions of submodules (CONFIRM).

### 3. Activate the dependency in pyproject.toml

Uncomment the relevant lines in both `[project].dependencies` and
`[tool.uv.sources]`:

```toml
[project]
dependencies = [
    "tfx-bsl",
]

[tool.uv.sources]
tfx-bsl = { path = "libs/tfx-bsl", editable = true }
```

Then run the following command in the container IDE environment:

```bash
uv sync
```

### 4. Open in devcontainer

The project is mounted at `/workspace` inside the container. Submodules in
`libs/` are part of the repo, so they come along automatically.

## Python version

Pinned to **3.12.4** via `.python-version`. The Dockerfile reads this file
at build time so the interpreter is baked into the image.

## Dev environment

| Tool | Role |
|------|------|
| Fedora 42 | Base image (matches Pritam's build system) |
| uv 0.10.9 | Python version + virtualenv + dependency management |
| PyCharm | IDE backend via JetBrains devcontainer extension |
