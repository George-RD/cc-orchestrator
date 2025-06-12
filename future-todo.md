# Future TODO Items

This document tracks items discovered during the Claude Template Simplification refactoring that should be addressed in future iterations but are not in scope for the current Phase 1-7 tasks.

## Items Discovered During Refactoring

### Line Count Analysis Insights
- **Discovered**: Original framework was 2,634+ lines, reduced to 308 lines (88% reduction)
- **Impact**: Dramatic improvement in maintainability and comprehension
- **Future**: Consider automated line count monitoring for template growth

### Specialist Redundancy Patterns
- **Discovered**: Heavy overlap in TDD workflow, handoff formats across specialists
- **Impact**: 30-line shared ethos eliminated 200+ lines of duplication
- **Future**: Expand shared patterns library for other common elements

### PRD Template Over-Engineering
- **Discovered**: Original 171-line PRD template had extensive YAML configs mostly unused
- **Impact**: 22-line simplified version covers 90% of real use cases
- **Future**: Create optional "advanced PRD" for complex enterprise projects

### Advanced Commands Discovery
- **Discovered**: .claude/commands/ directory contains 345 lines of advanced features
  - architecture.md (76 lines), debug.md (42 lines), optimize.md (75 lines)
  - review.md (48 lines), feature.md (18 lines)
- **Impact**: These add sophistication but weren't part of core simplification
- **Future**: Consider which advanced commands provide real value vs complexity

### Archival Strategy Success
- **Discovered**: Archiving (vs deleting) complex configs preserved options
- **Impact**: 1,091+ lines of aspirational features safely preserved in .orchestrator/archive/
- **Future**: Create restoration guide for selectively re-enabling archived features

### Configuration Cleanup Insights
- **Discovered**: Most complex configs were never used in practice
- **Impact**: Single 35-line confidence.yaml handles 95% of configuration needs
- **Future**: Monitor which archived configs users actually want to restore

### Template Structure Improvements
- **Consider template versioning system**: For managing different complexity levels of templates
- **Create template validation tool**: To ensure new templates follow MECE principles
- **Add template migration utilities**: For upgrading existing projects to new template structure

### Documentation Enhancements
- **Interactive tutorial system**: Step-by-step guided setup for new users
- **Video documentation**: Visual guides for complex concepts like confidence-based autonomy
- **Community examples repository**: Real-world examples from different domains

### Advanced Features (Post-Phase 7)
- **Claude Code integration improvements**: Better integration with Claude Code's native features
- **MCP server recommendations**: Curated list of valuable MCP servers for different use cases
- **Plugin marketplace**: Ecosystem for sharing specialist agents and workflows

### Performance Optimizations
- **Token usage analytics**: Detailed tracking of token consumption patterns
- **Smart context compression**: Intelligent compression beyond Claude Code's `/compact`
- **Caching strategies**: For frequently accessed templates and patterns

### Testing & Validation
- **Automated template testing**: CI/CD for template quality validation
- **Benchmark suite**: Standardized tests for measuring template effectiveness
- **User experience metrics**: Tracking actual user success rates and satisfaction

### Community & Ecosystem
- **Contribution guidelines**: Clear process for community improvements
- **Specialist agent marketplace**: Sharing custom specialist definitions
- **Template showcase**: Gallery of successful projects using the template

### File Structure Cleanup Needed
- **Discovered**: Redundant task folders (completed/ and done/), irrelevant docs/adr/ examples
- **Impact**: Confusing structure with leftover complex template artifacts  
- **Future**: Systematic cleanup of template cruft, standardize folder naming

### Core vs Advanced Feature Balance
- **Discovered**: Framework can be 574 lines (core) or 867 lines (with advanced commands)
- **Impact**: Clear distinction between essential and enhancement features
- **Future**: Define "framework tiers" - minimal, standard, advanced

### Documentation Effectiveness
- **Discovered**: 43-line README more effective than 324-line complex documentation
- **Impact**: Users prefer concise, actionable guides over comprehensive references
- **Future**: Apply "README-first" philosophy to all documentation

### Git Worktree Architecture Innovation
- **Discovered**: Parallel specialist development possible with git worktrees
- **Impact**: Revolutionary distributed development with session recovery
- **Future**: Explore integration with CI/CD pipelines, automated testing in worktrees

### Context Management Simplification
- **Discovered**: Manual `/compact` and `/clear` logic unnecessary with auto-compact
- **Impact**: Removed 20+ lines of context management complexity
- **Future**: Trust Claude Code's built-in optimization over manual intervention

### Archival Organization Strategy
- **Discovered**: Need systematic approach to managing archived vs active features
- **Impact**: Current archive preserves options but needs governance
- **Future**: Create feature lifecycle management (active → deprecated → archived → removed)

### Integration Opportunities
- **IDE extensions**: VS Code/JetBrains plugins for template management
- **GitHub Actions**: Automated template updates and validation
- **Monitoring integrations**: Hooks for external monitoring systems
- **Feature restoration utility**: Tool to selectively restore archived features

### Research Areas
- **Confidence calibration**: Machine learning approaches to improve confidence scoring
- **Agent coordination patterns**: New patterns for multi-agent collaboration
- **Context optimization**: Advanced techniques for managing large codebases

## Prioritization Notes

### High Impact, Low Effort
- Template validation tool
- Community examples repository
- Contribution guidelines

### High Impact, High Effort
- Interactive tutorial system
- MCP server recommendations
- Automated template testing

### Research Items (Long-term)
- Confidence calibration
- Agent coordination patterns
- Context optimization

## Review Schedule

This document should be reviewed after:
- [ ] Phase 3 completion (Command System Optimization)
- [ ] Phase 6 completion (Documentation & Testing)
- [ ] First real-world project validation
- [ ] Community feedback collection

## Notes

Items added here during refactoring should include:
- **Context**: Why this was discovered
- **Impact**: Potential benefit if implemented
- **Effort**: Rough estimate of implementation complexity
- **Dependencies**: What needs to be done first