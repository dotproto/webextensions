# Specification contributor guidelines

These guidelines describe the [Bikeshed](https://speced.github.io/bikeshed/)
conventions used in `index.bs`. For general information about contributing to
this repository, see [CONTRIBUTING.md](../CONTRIBUTING.md).

## Building

Install Bikeshed with `pip install bikeshed`, then build the spec with:

```sh
bikeshed --die-on=everything spec index.bs
```

The spec should build without any errors or warnings. Pull requests that change
`index.bs` should keep it that way.

## IDs

- Give every heading an explicit ID, for example
  `## Manifest keys ## {#manifest-keys}`. Never change an existing ID, because
  other documents may link to it.
- Manifest key headings use IDs of the form `key-<name>`, for example
  `{#key-content_scripts}`. Keys nested inside another key include the parent
  key's name, for example `{#key-externally_connectable-ids}`.
- When a heading is also the definition of a term, put the `<dfn>` in the
  heading and give it an explicit `dfn-` prefixed ID, so it does not collide
  with the heading's ID:

  ```
  # <dfn id="dfn-extension-origin">Extension origin</dfn> # {#extension-origin}
  ```

## Definitions

- Define a term once, with `<dfn>`, and link to it everywhere else.
- Definitions are not exported by default. Only export a definition, with
  `<dfn export>`, once its design is stable enough for other specifications to
  depend on it.
- Write algorithms as definitions with explicit, named parameters, wrapped in
  `<div algorithm>`:

  ```
  To <dfn id="dfn-inject-a-content-script">inject a content script</dfn>,
  given a {{ManifestContentScript}} |script| and a [=/document=] |document|:
  ```

  Call other algorithms by linking to their definitions and passing every
  parameter explicitly, rather than referring to their section.
- Write string literals in algorithms in the [Infra](https://infra.spec.whatwg.org/#strings)
  style, for example "`https`".

## Manifest keys

- The manifest is described by Web IDL dictionaries: `WebExtensionManifest`,
  plus one dictionary for each key whose value is an object, such as
  `ManifestContentScript`. The IDL blocks use `<xmp class="idl" noexport>`, so
  that `<` and `>` in types like `sequence<DOMString>` don't need escaping, and
  so the dictionaries are not exported.
- Each key's heading defines the corresponding dictionary member:

  ```
  ### Key <dfn dict-member for=WebExtensionManifest noexport>name</dfn> ### {#key-name}
  ```

  When adding a key, add it to the dictionary and to the list of valid keys
  too.

## Links

- Prefer autolinks to raw `<a href>` links and section links:
  - Terms: `[=extension origin=]`.
  - Manifest keys: `{{WebExtensionManifest/name}}` or
    `{{ManifestContentScript/matches}}`.
  - Use a section link, such as `[[#localization]]`, only when there is no
    definition to link to.
- When a term has several possible definitions, disambiguate it with `for`, for
  example `[=url/scheme=]`, or add an entry to the `link-defaults` block at the
  end of `index.bs`.
- To link to a term that another specification doesn't export, name the
  specification explicitly: `<l spec=html>[=opener origin at creation=]</l>`.
- Write APIs that are not specified yet, such as `runtime.sendMessage()`, as
  code rather than links, and add an `Issue:` noting that they are not
  specified.
