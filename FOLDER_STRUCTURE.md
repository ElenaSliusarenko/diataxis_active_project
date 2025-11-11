# Folder Structure Reference

Visual guide to the repository organization.

## Complete Structure

```
Diataxis_Templates/
│
├── README.md                      # Main documentation entry point
├── CONTRIBUTING.md                # How to contribute docs
├── STYLE_GUIDE.md                 # Writing and formatting standards
├── QUICK_REFERENCE.md             # Fast lookup guide
├── FOLDER_STRUCTURE.md            # This file
│
├── templates/                     # Blank templates for each type
│   ├── tutorial-template.md
│   ├── how-to-template.md
│   ├── reference-template.md
│   └── explanation-template.md
│
├── tutorials/                     # Learning-oriented (📖)
│   ├── README.md                  # Tutorials index
│   ├── getting-started/           # Beginner tutorials
│   │   ├── 01-setup-development-environment.md
│   │   └── 02-your-first-feature.md
│   └── advanced/                  # Advanced tutorials
│       └── building-microservices-tutorial.md
│
├── how-to/                        # Problem-oriented (🔧)
│   ├── README.md                  # How-to guides index
│   ├── deployment/                # Deployment guides
│   │   ├── deploy-to-production.md
│   │   └── rollback-deployment.md
│   ├── troubleshooting/           # Troubleshooting guides
│   │   └── debug-performance-issues.md
│   └── integration/               # Integration guides
│       ├── integrate-third-party-api.md
│       └── publish-events.md
│
├── reference/                     # Information-oriented (📋)
│   ├── README.md                  # Reference index
│   ├── api/                       # API documentation
│   │   ├── rest-endpoints.md
│   │   └── authentication.md
│   ├── configuration/             # Configuration reference
│   │   ├── environment-variables.md
│   │   └── message-queue.md
│   └── architecture/              # Architecture specs
│       └── system-components.md
│
├── explanation/                   # Understanding-oriented (💡)
│   ├── README.md                  # Explanations index
│   ├── concepts/                  # Conceptual explanations
│   │   ├── event-driven-architecture.md
│   │   └── data-consistency.md
│   └── decisions/                 # Design decisions
│       └── why-we-chose-microservices.md
│
└── Mobile_Nutrition_App/          # 📦 Active project documentation
    ├── README.md                  # Project overview
    ├── GETTING_STARTED.md         # Quick onboarding guide
    ├── PROJECT_OVERVIEW.md        # Executive overview
    ├── ROADMAP.md                 # Milestones and delivery plan
    ├── docs/                      # Global docs (security, protocols, decisions)
    ├── features/                  # Feature-specific docs (e.g., authentication)
    ├── tutorials/                 # Tutorials (if any)
    ├── how-to/                    # How-to guides
    ├── reference/                 # Reference docs
    ├── explanation/               # Explanations
    └── runbooks/                  # Operational runbooks
```

## Folder Naming Conventions

- **Lowercase**: All folder names use lowercase
- **Kebab-case**: Multi-word names use hyphens: `getting-started/`
- **Category folders may be plural**: e.g., `concepts/`, `decisions/`
- **Descriptive**: Names should indicate content type

## When to Create New Folders

### Create subfolder when:
- You have 3+ related documents
- Content forms logical grouping
- Aids navigation and discovery

### Don't create subfolder when:
- Only 1-2 documents
- Content doesn't form clear category
- Would create too deep nesting (3+ levels)

## File Naming Conventions

- **Kebab-case**: `setup-development-environment.md`
- **Descriptive**: Name should indicate content
- **No dates**: Don't include dates in filename
- **No versions**: Don't include version numbers

### Sequential Numbering

For ordered tutorials:
```
01-first-step.md
02-second-step.md
03-third-step.md
```

## README Files

Every major folder should have a `README.md`:

**Purpose**:
- Index of available documents
- Brief description of each
- Difficulty level or category
- Estimated reading/completion time

**Template**:
```markdown
# [Folder Name]

Brief description of this section.

## Available [Documents]

### [Subcategory]
- [Title](path/to/doc.md) - Description
```

## Cross-References

### Recommended Links

**From Tutorials**:
- Link to related how-to guides
- Link to reference for details
- Link to next tutorial

**From How-To Guides**:
- Link to related tutorials for learning
- Link to reference for specifications
- Link to troubleshooting if needed

**From Reference**:
- Link to how-to guides for usage examples
- Link to explanations for concepts

**From Explanations**:
- Link to all other types for practical application
- Link to related explanations

## Adding New Documentation

1. **Determine type** (Tutorial/How-To/Reference/Explanation)
2. **Choose location** within appropriate folder
3. **Create subfolder** if needed (3+ related docs)
4. **Copy template** from `/templates/`
5. **Name file** using conventions above
6. **Update README** in parent folder
7. **Add cross-references** to related docs

## Migration Path

When repository grows:

### From Simple to Organized
```
Before:
how-to/
  ├── guide1.md
  ├── guide2.md
  ├── guide3.md
  ├── guide4.md

After:
how-to/
  ├── deployment/
  │   ├── guide1.md
  │   └── guide2.md
  └── integration/
      ├── guide3.md
      └── guide4.md
```

### Keep Flat When Possible
Don't over-organize. If you have:
- < 10 documents: Keep flat
- 10-20 documents: Consider 2-3 categories
- 20+ documents: Organize into subcategories

---

**Last Updated**: 2024  
**Maintained By**: Documentation Team
