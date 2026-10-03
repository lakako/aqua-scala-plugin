# aqua-scala-plugin

[aqua](https://aquaproj.github.io/) plugin for [Scala](https://www.scala-lang.org/), supporting both **Scala 2** and **Scala 3**.

Also fully compatible with [mise](https://mise.jdx.dev/)'s built-in aqua backend!

## Packages

| Package | Aliases | Source | Description |
|---------|---------|--------|-------------|
| `scala/scala3` | - | [scala/scala3](https://github.com/scala/scala3) | Scala 3 — native binary (>= 3.5.0) / JVM (< 3.5.0) |
| `scala/scala` | `scala/scala2` | [scala/scala](https://github.com/scala/scala) | Scala 2 — JVM-based, platform independent |

## Usage with aqua

### As a custom registry

Add this registry to your `aqua.yaml`:

```yaml
# aqua.yaml
registries:
  - name: scala
    type: github_content
    repo_owner: <your-org>
    repo_name: aqua-scala-plugin
    ref: main
    path: registry.yaml

packages:
  # Install Scala 3
  - name: scala/scala3@3.6.4
    registry: scala

  # Install Scala 2
  # - name: scala/scala2@v2.13.16
  #   registry: scala
```

### Local development / testing

```yaml
# aqua.yaml
registries:
  - name: local
    type: local
    path: registry.yaml

packages:
  - name: scala/scala3@3.6.4
```

Then run:

```bash
aqua install
```

## Usage with mise

`mise` natively supports aqua registries without needing the `aqua` CLI installed.

### Configuration (`mise.toml`)

```toml
[settings]
aqua.registries = [
  # Local registry file
  "file:///path/to/aqua-scala-plugin/registry.yaml"
  # Or remote repository:
  # "https://github.com/<your-org>/aqua-scala-plugin"
]

[tools]
"aqua:scala/scala3" = "3.6.4"
# Or Scala 2:
# "aqua:scala/scala2" = "2.13.16"
```

### Commands

```bash
# List available versions
mise ls-remote aqua:scala/scala3
mise ls-remote aqua:scala/scala2

# Install
mise install aqua:scala/scala3@3.6.4
mise install aqua:scala/scala2@2.13.16

# Run
mise exec aqua:scala/scala3@3.6.4 -- scala --version
mise exec aqua:scala/scala2@2.13.16 -- scala -version
```

## Platform Support

### Scala 3 (`scala/scala3`)

Starting with Scala 3.5.0+, Scala 3 ships **native binaries** (bundled JVM, no external JDK needed):

| Platform | Architecture | Download format |
|----------|-------------|-----------------|
| Linux | x86_64 (amd64) | `.tar.gz` |
| Linux | aarch64 (arm64) | `.tar.gz` |
| macOS | x86_64 (amd64) | `.tar.gz` |
| macOS | aarch64 / Apple Silicon (arm64) | `.tar.gz` |
| Windows | x86_64 (amd64) | `.zip` |

For Scala 3.3.x LTS and older (< 3.5.0), cross-platform JVM zip archives are automatically selected (requiring a local JDK 8+).

### Scala 2 (`scala/scala` / `scala/scala2`)

Scala 2 distributions are JVM-based and **platform independent** — a single zip works on all platforms. Java (JDK 8+) must be installed separately.

| Platform | Architecture | Download format |
|----------|-------------|-----------------|
| All | All | `.zip` |

## Asset Naming Convention

### Scala 3
```
scala3-{version}-{arch}-{platform}.tar.gz   # Linux / macOS (>= 3.5.0)
scala3-{version}-{arch}-pc-win32.zip        # Windows (>= 3.5.0)
scala3-{version}.zip                        # JVM fallback (< 3.5.0)
```
- `{arch}`: `x86_64` or `aarch64`
- `{platform}`: `pc-linux`, `apple-darwin`, `pc-win32`

### Scala 2
```
scala-{version}.zip
```

## Reference

- Inspired by [vfox-scala](https://github.com/mise-plugins/vfox-scala)
- [aqua documentation](https://aquaproj.github.io/)
- [aqua registry configuration](https://aquaproj.github.io/docs/reference/registry-config/)
- [Scala 3 releases](https://github.com/scala/scala3/releases)
- [Scala 2 releases](https://github.com/scala/scala/releases)
