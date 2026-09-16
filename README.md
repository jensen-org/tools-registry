# tools-registry

The tool manifest catalog Jensen fetches at runtime: which formatters, linters, language
servers, and debug adapters exist, what they need, and how to install them.

## Files

- `tools.yaml` — the manifest, parsed by `serde_saphyr` into the same shape documented in
  `schema/tools.schema.json` in the `jensen-org/jensen` repo. Its `schema_version` field lets a
  client reject a future breaking change instead of misparsing it.
- `checksums.txt` — one `<sha256>  <filename>` line per published file, checked before a fetched
  file is trusted.

## Publishing a change

1. Edit `tools.yaml`.
2. Recompute its checksum and update `checksums.txt`:
   ```
   shasum -a 256 tools.yaml
   ```
3. Open a pull request against `main`. Jensen fetches straight from `main`, so merging publishes
   immediately to every client on its next catalog refresh.

There is no seed copy of this file anywhere else: a client with no network and no cache shows a
"catalog not fetched" state rather than falling back to stale bundled data.
