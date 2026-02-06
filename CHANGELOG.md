# CHANGELOG

All notable changes to Thinking Review Expert will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [6.0.0] - 2025-01-06

### Major Changes

- **Restructured to code-review-expert format** for Claude Code compatibility
- **Context7 & DeepWiki**: Changed from embedded to MCP references only
- **3 Thinking Tools**: Sequential, Tractatus, and Debug remain fully embedded
- **Stop-Slop Integration**: Auto-applied to all validation outputs
- **Beautiful-Mermaid Integration**: Auto-generates diagrams during thinking
- **NPM Package**: Created package.json for publishing

### Added

- Auto-diagram generation workflow
- Stop-Slop writing principles as auto-applied enhancement
- Beautiful-Mermaid styling with 15 themes
- Priority-based tool selection (Embedded → MCP)
- Comprehensive GitHub documentation
- Architecture documentation
- Contributing guide
- MIT License

### Changed

- skill.md: Reduced from 512 to 308 lines (40% token savings)
- Context7/DeepWiki: Now MCP references, not embedded
- Tool priority: Embedded tools first, MCP as fallback
- Documentation structure: Follows code-review-expert format

### Fixed

- Tool selection priority logic
- Reference loading system
- Lazy loading for 85-90% token savings

## [5.0.0] - 2025-01-05

### Major Changes

- **Embedded Tools**: All 5 tools embedded in skill
- **7-Circle Sacred Thinking**: Complete integration
- **Stop-Slop**: Added writing principles
- **Beautiful-Mermaid**: Added diagram styling

### Added

- Embedded Context7 implementation
- Embedded DeepWiki implementation
- Embedded Sequential Thinking
- Embedded Tractatus Thinking
- Embedded Debug Thinking
- Stop-Slop principles reference
- Beautiful-Mermaid styling guide

## [4.0.0] - 2025-01-04

### Major Changes

- **7-Circle Sacred Thinking Integration**: Complete cognitive flow
- **T→S→D Rotation**: Alternating tool rotation pattern
- **Quality Gates**: All 7 circles must pass
- **Auto-Validation**: Automatic validation triggers

### Added

- 7-circle compliance checking
- Tool rotation validation
- Quality gate system
- Per-circle analysis
- Enhanced reporting

## [3.0.0] - 2025-01-03

### Added

- Three-stage review workflow
- Severity classification (P0-P3)
- Reference checklist system
- Enhancement opportunities tracking
- Next-steps confirmation

### Changed

- Restructured validation stages
- Improved output formatting
- Added inline comment support

## [2.0.0] - 2025-01-02

### Added

- Logical structure analysis
- Depth & quality scanning
- Tool rotation validation
- Reference checklist system

### Changed

- Complete skill restructure
- Modular reference loading
- Progressive disclosure design

## [1.0.0] - 2025-01-01

### Added

- Initial release
- Basic thinking review functionality
- Sequential thinking validation
- Tractatus thinking validation
- Debug thinking validation

---

## [Unreleased]

### Planned

- [ ] Additional validation checklists
- [ ] More mermaid diagram templates
- [ ] Enhanced error recovery
- [ ] Performance optimizations
- [ ] Additional language support

---

## Version Classification

- **Major (X.0.0)**: Breaking changes, major features, architectural changes
- **Minor (x.X.0)**: New features, enhancements, backward compatible
- **Patch (x.x.X)**: Bug fixes, minor improvements, backward compatible

---

**Current Version**: 6.0.0  
**Release Date**: 2025-01-06  
**Maintainer**: Alot1z
