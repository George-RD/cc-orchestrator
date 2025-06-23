# Overview

CC Orchestrator is a minimal AI orchestration framework that enables Claude Code to manage complex software development projects through intelligent task delegation. It solves the problem of coordinating multiple AI specialists working on different aspects of a project while maintaining code quality through Test-Driven Development (TDD) enforcement. The framework is designed for developers who want to leverage AI for parallel development work without losing control over quality and architecture.

# Core Features

## Mermaid-Driven Orchestration
- **What it does**: Defines all orchestration logic using mermaid diagrams
- **Why it's important**: Makes the framework's behavior visual and easily modifiable
- **How it works**: Mermaid state machines control task flow, assignment logic, and verification processes

## Native Task Tool Integration
- **What it does**: Uses Claude Code's built-in Task tool for specialist delegation
- **Why it's important**: Leverages existing capabilities without reinventing the wheel
- **How it works**: Orchestrator spawns specialists via Task tool with focused prompts

## Headless Mode Operations
- **What it does**: Performs context-free analysis and summaries
- **Why it's important**: Keeps orchestrator context clean while getting structured data
- **How it works**: Uses `claude -p --output-format json` for conflict detection, status reports

## TDD Enforcement System
- **What it does**: Ensures all code follows test-first development
- **Why it's important**: Maintains code quality and prevents specialists from gaming tests
- **How it works**: Monitors commits, verifies tests fail first, detects test modifications

## JSON-Based State Persistence
- **What it does**: Tracks all task states in JSON files
- **Why it's important**: Enables session recovery and progress tracking
- **How it works**: Tasks move through directories representing states (todo/in_progress/review/completed)

# User Experience

## User Personas
- **Solo Developer**: Uses framework to parallelize their own work across multiple aspects
- **Team Lead**: Orchestrates AI specialists to handle routine development tasks
- **Open Source Maintainer**: Manages contributions and ensures quality standards

## Key User Flows
1. **Project Setup**: Run `setup-project.sh` to initialize framework in any project
2. **Task Creation**: Define tasks manually or generate from PRD
3. **Orchestration**: Run `/orchestrate` and let framework manage everything
4. **Progress Monitoring**: Check task JSONs or use status commands

## UI/UX Considerations
- Command-line interface for all interactions
- Visual mermaid diagrams for understanding flow
- Clear task states visible in directory structure
- Minimal configuration required

# Technical Architecture

## System Components
```
Framework Core:
- Mermaid diagrams defining behavior
- Command implementations
- Headless mode specifications

User Project:
- CLAUDE-project.md (orchestrator)
- Task JSON files
- Specialist definitions
```

## Data Models
```json
// Task Structure
{
  "id": 1,
  "title": "string",
  "type": "backend|frontend|qa|docs|devops|security",
  "status": "todo|in_progress|blocked|review|completed",
  "files": ["paths affected"],
  "dependencies": [task_ids],
  "tdd_verified": boolean,
  "log": [{"timestamp": "ISO", "event": "string"}]
}
```

## APIs and Integrations
- Claude Code CLI (native integration)
- Git for version control
- File system for state management
- Headless mode for analysis

## Infrastructure Requirements
- Claude Code CLI installed
- Git repository
- Unix-like file system
- Bash shell for setup script

# Development Roadmap

## MVP Requirements
1. **Core Orchestration Loop**
   - Read task states
   - Assign work to specialists
   - Monitor progress
   - Review completed work

2. **Basic TDD Verification**
   - Detect test creation
   - Verify tests fail first
   - Flag test modifications

3. **Minimal Specialist Set**
   - Backend specialist
   - Frontend specialist
   - QA specialist
   - Documentation specialist

4. **Setup Script**
   - Copy framework files
   - Initialize task directories
   - Create example tasks

## Future Enhancements
1. **Advanced Conflict Detection**
   - Dependency graph visualization
   - Automatic task ordering
   - Resource allocation optimization

2. **Enhanced TDD Features**
   - Coverage tracking
   - Test quality metrics
   - Automated test generation

3. **Additional Specialists**
   - DevOps specialist
   - Security specialist
   - Performance specialist

4. **Framework Extensions**
   - Plugin system for custom specialists
   - Integration with CI/CD
   - Multi-project orchestration

# Logical Dependency Chain

1. **Foundation Layer**
   - Mermaid diagram parser/executor
   - Basic task JSON structure
   - Directory-based state management

2. **Orchestration Core**
   - Task state machine
   - Specialist spawning via Task tool
   - Progress monitoring

3. **Quality Layer**
   - TDD verification system
   - Test modification detection
   - Review process

4. **Usability Layer**
   - Setup script
   - Status commands
   - Example project

# Risks and Mitigations

## Technical Challenges
- **Risk**: Specialists modifying tests to pass
- **Mitigation**: Git diff analysis via headless mode

- **Risk**: Parallel task conflicts
- **Mitigation**: File-based conflict detection before assignment

- **Risk**: Lost context between sessions
- **Mitigation**: JSON state files for full recovery

## MVP Scoping
- **Risk**: Over-engineering the framework
- **Mitigation**: <300 line target, mermaid-driven simplicity

- **Risk**: Complex setup process
- **Mitigation**: Single bash script, <1 minute setup

## Resource Constraints
- **Risk**: Orchestrator context overflow
- **Mitigation**: Headless mode for analysis tasks

- **Risk**: Slow specialist responses
- **Mitigation**: Parallel assignment, async monitoring

# Appendix

## Design Principles
- MECE (Mutually Exclusive, Collectively Exhaustive) task breakdown
- Confidence-based delegation (high/medium/low)
- Fail-fast with clear guidance
- Visual-first with mermaid diagrams

## Success Metrics
- Framework under 300 lines total
- Setup time under 1 minute
- Zero configuration required
- Full TDD compliance
- Session recovery capability