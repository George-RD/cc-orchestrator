# Search Tips for Specialists

When searching the codebase, prefer these tools if available:

## Finding Files
- Use `fd` if available: `fd "pattern"`
- Fallback to built-in Glob tool

## Finding Text/Code
- Use `rg` (ripgrep) - pre-installed in Claude Code
- Example: `rg "function.*test" --type js`
- For structure: `ast-grep` if available

## Processing Data
- JSON: `jq` for complex queries
- YAML/XML: `yq` for structured data
