# Create Tasks from PRD
# Usage: /tasks-create

Simple PRD-to-task conversion using logical mapping:

## Logic-Based Task Creation

### Step 1: Identify Active PRD
- Check `/.orchestrator/requirements/active/` for PRD files
- If multiple PRDs, use most recent or prompt user

### Step 2: Extract Key Sections
**Map PRD sections to tasks:**
- **High Merit Items** → High priority tasks
- **Medium Merit Items** → Medium priority tasks  
- **Recommendations** → Implementation tasks
- **Technical Requirements** → Specialist-specific tasks

### Step 3: Simple Agent Assignment
**Agent mapping rules:**
- File/directory changes → `docs`
- Code/logic implementation → `backend` 
- Testing/validation → `qa`
- Infrastructure → `devops`

### Step 4: Basic Task Structure
**Create task JSON with:**
```json
{
  "id": "task-XXX",
  "title": "[Clear description from PRD]",
  "type": "[agent type]", 
  "status": "active",
  "confidence": "[high/medium/low based on complexity]",
  "acceptance": ["[Bullet points from PRD section]"],
  "dependencies": ["[Based on logical order]"]
}
```

### Step 5: Auto-Registry Update
- Update `registry.json` with new task counts
- Place task files in `/.orchestrator/tasks/todo/` directory

## Simplicity Rules
1. **No bash scripting** - Pure logical mapping
2. **Direct PRD mapping** - Section → Task conversion
3. **Simple agent rules** - Content type → Specialist
4. **Basic dependencies** - Sequential for same agent, parallel for different

**Result**: 5-10 tasks generated automatically from any PRD analysis.