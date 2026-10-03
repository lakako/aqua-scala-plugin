# aqua-scala-plugin

[aqua](https://aquaproj.github.io/) plugin for [Scala](https://www.scala-lang.org/), supporting both **Scala 2** and **Scala 3**.

## Packages

| Package | Source | Description |
|---------|--------|-------------|
| `scala/scala3` | [scala/scala3](https://github.com/scala/scala3) | Scala 3 — native binary, no JVM required |
| `scala/scala2` | [scala/scala](https://github.com/scala/scala) | Scala 2 — JVM-based, platform independent |

## Usage

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
  # Install Scala 3 (native binary, no JVM required)
  - name: scala/scala3@3.6.4
    registry: scala

  # Install Scala 2 (JVM-based)
  # - name: scala/scala2@v2.13.16
  #   registry: scala
```

### Local development / testing

Clone this repository and use a `local` registry:

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

## Platform Support

### Scala 3 (`scala/scala3`)

Scala 3 ships **native binaries** (no JVM required) for all major platforms:

| Platform | Architecture | Download format |
|----------|-------------|-----------------|
| Linux | x86_64 (amd64) | `.tar.gz` |
| Linux | aarch64 (arm64) | `.tar.gz` |
| macOS | x86_64 (amd64) | `.tar.gz` |
| macOS | aarch64 / Apple Silicon (arm64) | `.tar.gz` |
| Windows | x86_64 (amd64) | `.zip` |

> **Note:** Scala 3 native binaries embed a bundled JVM and are significantly larger than Scala 2 JVM-based distributions.

### Scala 2 (`scala/scala2`)

Scala 2 distributions are JVM-based and **platform independent** — a single zip works on all platforms. Java (JDK 8+) must be installed separately.

| Platform | Architecture | Download format |
|----------|-------------|-----------------|
| All | All | `.zip` |

## Asset Naming Convention

### Scala 3
```
scala3-{version}-{arch}-{platform}.tar.gz   # Linux / macOS
scala3-{version}-{arch}-pc-win32.zip        # Windows
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
