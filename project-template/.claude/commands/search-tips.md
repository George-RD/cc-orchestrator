# Search Tips for Specialists

When using the devcontainer, these tools are pre-installed for efficient searching:

## Finding Files
- **fd**: Fast file finder
  - `fd "pattern"` - Find files by name
  - `fd -e js` - Find all JavaScript files
  - `fd -H` - Include hidden files

## Finding Text/Code
- **rg (ripgrep)**: Fast text search
  - `rg "pattern"` - Search for text
  - `rg "function.*test" --type js` - Search in specific file types
  - `rg -i "todo"` - Case-insensitive search
  
- **ast-grep**: Structural code search
  - `ast-grep --pattern 'function $NAME($_) { $$$ }'` - Find functions
  - `ast-grep --pattern 'class $_ extends React.Component'` - Find React classes

## Interactive Selection
- **fzf**: Fuzzy finder for interactive use
  - `fd . | fzf` - Interactively select a file
  - `rg --files | fzf` - Select from all files

## Processing Data
- **jq**: JSON processor
  - `cat package.json | jq '.dependencies'` - Extract dependencies
  - `jq '.tasks[] | select(.status=="todo")' tasks.json` - Filter tasks

- **yq**: YAML/XML processor
  - `yq '.services' docker-compose.yml` - Extract services
  - `yq -p xml '.root.element' file.xml` - Parse XML

## Common Patterns
```bash
# Find all test files
fd -e test.js -e spec.js

# Search for TODOs in code
rg "TODO|FIXME|NOTE" --type-add 'code:*.{js,ts,jsx,tsx,py,java,c,cpp}'

# Find all imports of a module
rg "import.*from ['\"](react|@react)" --type js

# Interactive file search and edit
fd . | fzf | xargs code
```

## Notes
- These tools are available in the devcontainer
- Always prefer `rg` over `grep` for speed
- Use `fd` over `find` for simplicity