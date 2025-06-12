# Claude Template Simplification Implementation Guide

You are implementing a simplified AI Orchestration Framework that maintains power while dramatically reducing complexity. This framework leverages Claude Code's native capabilities for context management and persistence.

## Core Principles
1. **MECE**: Mutually Exclusive, Collectively Exhaustive - no overlaps, no gaps
2. **OOP**: Clear separation of concerns, encapsulation, single responsibility
3. **Context Awareness**: Use Claude Code's `/clear` and command system strategically
4. **Progressive Enhancement**: Start minimal, add only proven necessities

## Implementation Tasks

### Phase 1: Core Architecture Simplification (Foundation)

#### Task 1.1: Simplify Master Orchestrator
- [ ] **Reduce CLAUDE.md to ~100 lines**
  - [ ] Extract only essential orchestration logic
  - [ ] Remove complex decision trees
  - [ ] Implement simple confidence handling
  - [ ] Create clear delegation patterns
  - [ ] **Notes**: _____________________

#### Task 1.2: Create Lightweight Confidence System
- [ ] **Design simple confidence mechanism**
  - [ ] Add confidence field to task JSON (high/medium/low)
  - [ ] Implement 3 simple rules:
    - High (>80%): Proceed autonomously
    - Medium (50-80%): Log concerns, continue
    - Low (<50%): Flag for human review
  - [ ] Remove complex calculations
  - [ ] **Notes**: _____________________

#### Task 1.3: Streamline Task Management
- [ ] **Simplify task JSON structure**
  ```json
  {
    "id": "task-xxx",
    "title": "Clear description",
    "type": "backend|frontend|qa|docs",
    "status": "active|done|blocked|review",
    "confidence": "high|medium|low",
    "dependencies": [],
    "acceptance": [],
    "log": []
  }
  ```
  - [ ] Remove unnecessary metadata
  - [ ] Keep only resumability essentials
  - [ ] **Notes**: _____________________

### Phase 2: Specialist Consolidation (Agents)

#### Task 2.1: Create Shared Coding Ethos
- [ ] **Design shared principles document**
  - [ ] Create `.orchestrator/shared/coding-ethos.md` (~30 lines)
  - [ ] Include: TDD principles, error handling, documentation standards
  - [ ] Include: Code quality expectations, naming conventions
  - [ ] All specialists load this as base context
  - [ ] **Notes**: _____________________

#### Task 2.2: Simplify Specialist Definitions
- [ ] **Reduce each specialist to ~50 lines**
  - [ ] Backend: Focus on API, database, business logic
  - [ ] Frontend: Focus on UI, components, accessibility
  - [ ] QA: Focus on testing strategy and validation
  - [ ] Docs: Focus on clear documentation
  - [ ] Each references shared ethos for common patterns
  - [ ] **Notes**: _____________________

#### Task 2.3: Extract Common Patterns
- [ ] **Move to shared ethos file**
  - [ ] Common TDD workflow
  - [ ] Shared handoff format
  - [ ] Unified logging approach
  - [ ] Standard error handling
  - [ ] **Notes**: _____________________

#### Task 2.4: Remove Redundancy
- [ ] **Eliminate overlapping responsibilities**
  - [ ] Clear ownership matrix
  - [ ] No duplicate instructions
  - [ ] Trust base Claude knowledge
  - [ ] **Notes**: _____________________

### Phase 3: Command System Optimization

#### Task 3.1: Consolidate Commands
- [ ] **Create smart /orchestrate command**
  ```bash
  # Intelligent detection of current state
  # PRD exists? → Create tasks
  # Tasks active? → Show status and resume
  # Nothing? → Guide to start
  ```
  - [ ] Merge create/resume logic
  - [ ] Auto-detect project state
  - [ ] **Notes**: _____________________

#### Task 3.2: Simplify Status Command
- [ ] **Lightweight status without heavy parsing**
  - [ ] Simple file counts
  - [ ] Current task titles
  - [ ] Clear next actions
  - [ ] Avoid loading full JSONs
  - [ ] **Notes**: _____________________

#### Task 3.3: Context Management Integration
- [ ] **Leverage Claude Code features**
  - [ ] Use `/clear` between major tasks
  - [ ] Document when to reset context
  - [ ] Create context budget guidelines
  - [ ] **Notes**: _____________________

### Phase 4: Configuration Cleanup

#### Task 4.1: Remove Non-Functional Configs
- [ ] **Delete aspirational features**
  - [ ] Remove monitoring/metrics.yaml (420+ lines)
  - [ ] Remove event definitions
  - [ ] Remove health check patterns
  - [ ] Archive MCP config for later
  - [ ] **Notes**: _____________________

#### Task 4.2: Consolidate Essential Configs
- [ ] **Single configuration approach**
  - [ ] Merge related YAMLs
  - [ ] Use conventions over configuration
  - [ ] Provide smart defaults
  - [ ] Document only exceptions
  - [ ] **Notes**: _____________________

#### Task 4.3: Simplify Communication Protocol
- [ ] **Natural language over JSON**
  - [ ] Simple handoff format
  - [ ] Clear context preservation
  - [ ] Minimal metadata
  - [ ] Focus on clarity
  - [ ] **Notes**: _____________________

### Phase 5: PRD System Refinement

#### Task 5.1: Standardize PRD Format
- [ ] **Create strict PRD processing rules**
  - [ ] Define required sections (problem, requirements, technical)
  - [ ] Create parser for consistent interpretation
  - [ ] Orchestrator becomes "farmer" after PRD parsed
  - [ ] Predictable task generation from standard format
  - [ ] **Notes**: _____________________

#### Task 5.2: Simplify PRD Template
- [ ] **Essential sections only**
  - [ ] Problem statement
  - [ ] Requirements (checkbox list)
  - [ ] Technical needs (agents, complexity)
  - [ ] Success criteria
  - [ ] Remove complex YAML structures
  - [ ] **Notes**: _____________________

#### Task 5.3: Improve PRD → Task Flow
- [ ] **Clearer transformation logic**
  - [ ] Simple sizing heuristics
  - [ ] Obvious task boundaries
  - [ ] Natural dependencies
  - [ ] Consistent task format from PRD structure
  - [ ] **Notes**: _____________________

### Phase 6: Documentation & Testing

#### Task 6.1: Create Minimal Documentation
- [ ] **Essential docs only**
  - [ ] Quick start guide (<1 page)
  - [ ] Command reference
  - [ ] Simple examples
  - [ ] Troubleshooting basics
  - [ ] **Notes**: _____________________

#### Task 6.2: Test Simplified Framework
- [ ] **Real project validation**
  - [ ] Test with simple project
  - [ ] Test with complex project
  - [ ] Test resumability
  - [ ] Test with context resets
  - [ ] **Notes**: _____________________

#### Task 6.3: Measure Improvements
- [ ] **Quantify simplification**
  - [ ] Line count reduction (target: 80%)
  - [ ] Token usage (target: 60% less)
  - [ ] Time to first task (target: <2 min)
  - [ ] Setup complexity (target: 5 min)
  - [ ] **Notes**: _____________________

### Phase 7: Advanced Features (Optional)

#### Task 7.1: Design Plugin System
- [ ] **For future enhancements**
  - [ ] Optional specialist modules
  - [ ] MCP integration hooks
  - [ ] Advanced monitoring (if needed)
  - [ ] Custom workflows
  - [ ] **Notes**: _____________________

#### Task 7.2: Plan MCP Integration
- [ ] **For future enhancement**
  - [ ] Research which MCP servers add value
  - [ ] Design integration without core dependency
  - [ ] Consider: filesystem, GitHub, database
  - [ ] Keep as optional enhancement
  - [ ] **Notes**: _____________________

#### Task 7.3: Create Migration Guide
- [ ] **From old to new**
  - [ ] Mapping of changed features
  - [ ] Conversion scripts
  - [ ] Deprecation notices
  - [ ] **Notes**: _____________________

## Implementation Guidelines

### Context Management Strategy
1. Use `/compact` after completing major task groups (preserves key context)
2. Load PRDs/tasks only when needed via commands
3. Keep orchestrator context minimal but continuous
4. Let specialists start fresh for each task with shared ethos

### Confidence Integration
```json
// In task completion by specialist
{
  "status": "done",
  "confidence": "high",
  "confidence_notes": "All tests passing, follows patterns",
  "concerns": []
}

// Orchestrator handles:
// high → mark complete, continue
// medium → log concerns, continue with note
// low → pause for human review
```

### Specialist Handoff Format
When specialists complete tasks, they should:
1. Update task JSON with status and confidence
2. Add confidence_notes explaining their assessment
3. List any concerns for medium/low confidence
4. Save their work and update task log

This keeps confidence assessment simple and actionable.

### MECE Architecture
- **Orchestrator**: Overall coordination ONLY
- **Specialists**: Domain expertise ONLY
- **Commands**: State detection and action ONLY
- **Tasks**: Work tracking ONLY
- **PRDs**: Requirements ONLY

### File Structure (Simplified)
```
/
├── CLAUDE.md (100 lines)
├── .claude/
│   ├── commands/ (20 lines each)
│   │   ├── orchestrate.md
│   │   └── status.md
│   └── confidence.yaml (20 lines)
├── .orchestrator/
│   ├── shared/
│   │   └── coding-ethos.md (30 lines)
│   ├── specialists/ (50 lines each)
│   │   ├── backend.md
│   │   ├── frontend.md
│   │   ├── qa.md
│   │   └── docs.md
│   ├── tasks/
│   │   ├── active/
│   │   ├── done/
│   │   └── blocked/
│   └── requirements/
│       ├── template.prd.md (30 lines)
│       └── active/
└── README.md (Updated, 100 lines)
```

## Implementation Philosophy

### Farmer vs Chef Approach
- **PRD Processing**: Orchestrator acts as "farmer" - structured, predictable
- **Before PRD**: Flexible, help user create good PRD (chef mode)
- **After PRD**: Systematic task generation and delegation (farmer mode)
- **Specialists**: Start as farmers (follow ethos) but can be chefs when solving

### Modularity Principles
- Each component independently improvable
- Changes to one specialist don't affect others
- PRD format evolution doesn't break task generation
- Command improvements don't require orchestrator changes

## Success Criteria
- [ ] Total framework < 500 lines
- [ ] Setup time < 5 minutes
- [ ] Clear separation of concerns
- [ ] Maintains resumability
- [ ] Reduces token usage by >60%
- [ ] Actually completes projects

## Notes Section
_Add implementation discoveries, decisions, and learnings here:_

### Context Management Decision
- Using `/compact` over `/clear` to preserve continuity
- Future: Consider intelligent compression strategies
- Keep implementation flexible for future enhancements

### Shared Ethos Benefits
- Reduces specialist file sizes
- Ensures consistent practices
- Single place to update common patterns
- Each specialist inherits base behavior

### PRD Standardization
- Enables "farmer" approach post-PRD
- Reduces orchestrator complexity
- Makes task generation predictable
- Allows PRD format evolution independently

_______________________________________________
_______________________________________________
_______________________________________________

---

Remember: The goal is elegant simplicity that works, not complex systems that might work. When in doubt, choose the simpler option.