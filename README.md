# aqua-scala-plugin

Unified [aqua](https://aquaproj.github.io/) plugin for [Scala](https://www.scala-lang.org/), seamlessly supporting both **Scala 2** and **Scala 3** as a single tool!

Also fully compatible with [mise](https://mise.jdx.dev/)'s built-in aqua backend.

## Unified Package: `scala/scala`

Both Scala 2 and Scala 3 are managed under **a single package name** (`scala/scala`):
- Specifying a **3.x** version installs Scala 3 (native binary for >= 3.5.0; JVM for < 3.5.0)
- Specifying a **2.x** version installs Scala 2 (JVM cross-platform archive)

Aliases supported: `scala/scala3`, `scala/scala2`.

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
  - name: scala/scala@3.6.4
    registry: scala

  # Or install Scala 2 (same tool, different version!)
  # - name: scala/scala@2.13.16
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
  - name: scala/scala@3.6.4
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
# Scala 3
"aqua:scala/scala" = "3.6.4"

# Or Scala 2 (just change the version number!)
# "aqua:scala/scala" = "2.13.16"
```

### Commands

```bash
# Install Scala 3
mise install aqua:scala/scala@3.6.4
mise exec aqua:scala/scala@3.6.4 -- scala --version

# Install Scala 2
mise install aqua:scala/scala@2.13.16
mise exec aqua:scala/scala@2.13.16 -- scala -version
```

## How It Works

Under the hood, aqua's `version_constraint` and `version_overrides` mechanism dynamically switches the release source, archive format, and platform assets based on the requested version:

| Version | Distribution Source | Asset Format | JDK Requirement |
|---------|---------------------|--------------|-----------------|
| `>= 3.5.0` | GitHub Releases (`scala/scala3`) | Native binary (`.tar.gz` / `.zip`) | **None** (bundled JVM) |
| `>= 3.0.0` & `< 3.5.0` (3.3 LTS) | GitHub Releases (`scala/scala3`) | Cross-platform `.zip` | JDK 8+ required |
| `< 3.0.0` (Scala 2.13, 2.12, ...) | GitHub Releases (`scala/scala`) | Cross-platform `.zip` | JDK 8+ required |

## Reference

- Inspired by [vfox-scala](https://github.com/mise-plugins/vfox-scala)
- [aqua documentation](https://aquaproj.github.io/)
- [aqua registry configuration](https://aquaproj.github.io/docs/reference/registry-config/)
- [Scala 3 releases](https://github.com/scala/scala3/releases)
- [Scala 2 releases](https://github.com/scala/scala/releases)
