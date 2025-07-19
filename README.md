# pulsar-ide-yaml package

Rich language support for YAML files in Pulsar via [yaml-language-server](https://github.com/redhat-developer/yaml-language-server).

## Prerequisites

None! `yaml-language-server` is bundled with this package.

## Features

* Code completion (via `autocomplete-plus`) for YAML properties and values based on the document’s [schema](https://json-schema.org/). Schemas for some common types are included; others can be configured.
* Validation (via `linter` and `linter-ui-default`).
* Document symbol resolution (via `symbols-view`) for JSON properties in the document.
* Hover (via `pulsar-hover`) for rich tooltip content based on descriptions in the document’s JSON schema.
* Format-as-you-type support (via `pulsar-code-format`). (Whole-document formatting and selected-range formatting are not yet supported by the underlying language server.)
* Outline support (via `pulsar-outline-view`) for a hierarchical representation of the YAML document.

## Configuration

### Node path

> [!TIP]
> Soon `pulsar-ide-yaml` will be able to use Pulsar’s built-in version of Node. For now, though, the built-in version is too old; you’ll have to tell the language server the path to your local version of Node.

The version of Node inherited from your shell environment will usually suffice; if Pulsar fails to find it, you may specify the absolute path to your version of `node` in the “Path To Node Binary” configuration field.

### Schemas

#### Builtin schemas

Some common schemas are included out of the box:

* `docker-compose.yml`
* GitHub Actions workflow

#### Explicit schemas

You can opt into a certain schema with a modeline comment…

```yaml
# yaml-language-server: $schema=<schemaPath>
```

…where `<schemaPath>` can be a URL, a relative path, or an absolute path.

#### Custom schemas

Other JSON schemas can be added — not through the settings UI, but via your `config.cson`. For each new JSON schema you want to add, create a new object property like so:

```coffeescript
"*":
  "pulsar-ide-yaml":
    schemas:
      "https://www.schemastore.org/circleciconfig.json": [".circleci.yml", "**/.circleci.yml"]
```

The key is a path to a schema (whether local on disk or a URL); the value is either a single glob (described relative to the project root) or an array of multiple such globs.

You can also define project-specific schemas with the help of a package like [atomic-management][] or [project-config][].

[atomic-management]: https://web.pulsar-edit.dev/packages/atomic-management
[project-config]: https://web.pulsar-edit.dev/packages/project-config
