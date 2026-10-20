# lisa-pack-mem0

Lisa Mem0 provider pack — external memory backend and MCP server manifest.

The pack owns the generic `memory.add` / `memory.search` / `memory.list`
aliases and routes them to Mem0, so any Lisa operation that reads or writes
long-term memory works against Mem0 without changing.

## What it provides

| Layer | Path | Role |
| --- | --- | --- |
| MCP manifest | `mcp_servers.yaml` | Declares the Mem0 backend, its result maps, and the tool policy. |
| Result maps | `result_maps.py` | Normalize Mem0 responses to canonical Lisa `MemoryRecord` JSON. |

### Result maps

- **`mem0_add_result_map`** — normalizes the add response to a confirmation
  string.
- **`mem0_search_result_map`** — maps the Mem0 `score` field to `salience`.
- **`mem0_list_result_map`** — unwraps the paginated envelope into a flat
  JSON array of records.

### Tool policy

The manifest denies `delete_all_memories`, `delete_entitites`, and
`delete_memory`, so a model cannot erase the user's memory through the pack.

## Configuration

| Variable | Meaning |
| --- | --- |
| `MEM0_API_KEY` | Bearer token for the Mem0 MCP server. |

Select the pack as the memory owner via session context:

```yaml
providers:
  capabilities:
    memory: lisa-pack-mem0
```

## Installation

```bash
pip install lisa-pack-mem0
```

## Development

```bash
uv run ruff format --check lisa_packs/mem0
uv run ruff check lisa_packs/mem0
uv run mypy lisa_packs/mem0
uv run pytest -m "not live"
```

Run the checks from a sibling framework checkout so the shared
`[tool.ruff]` and `[tool.mypy]` settings apply; `pytest` runs from the pack
root so `pythonpath = ["."]` makes `lisa_packs.mem0` importable.

## License

MIT.
