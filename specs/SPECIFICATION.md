## **specs/SPECIFICATION.md**

## Specification

## Overview
* **Goal**: Maintain a dynamic knowledge graph (ontology), track user learning progress, and suggest next steps.
* **Core Components**:
  * Concept manager (ontology).
  * Progress tracker (what user knows, mastery level).
  * Exercise manager (link skills/concepts to exercises).
  * Suggestion engine (what to learn/practice next).

## Principles

### UNIX KISS - "keep is simple stupid"

* organize code as small short understandable programs
* if needed decompose bigger program into smaller ones
* or apply principle of modular composability
* so programs are just wrappers of programmling library functions
* so effectively one can translate bash script were few of them call each other into equivalent program with ease
* therefore making also refactorings of bigger programs into smaller ones simple
* and smaller programs / library functions with defined and soped specifications are easier to document via unit tests
* unit tests should be treated as machine readable documentation of our expectations about how pieces should function

### Self adapting

System should have flexibilty to co-create with user extensions,
custom mini tools, improvements, etc that will help user with given
learning style and material learning.

### Data storage

* tool is meant for terminal unix users
* and as free software should be easy to integrate with
* and user should have easy access to their data
* therefore data should be kept in either Markdown files
* or easy to pars JSONL (one line jason - files where each  line contains one json object , it's like csv for many json objects), csv or Turtle syntax for RDF+OWL.

User works on given learning projects.

We assume that they are pointed by environment variable
`LEARNING_PROJECTS_DIR` (we will call it short "LDIR")
or variable is specified in `.env` in `CWD`,
(if not defined assume `~/learning`)
that contain projects in subfolders,
each versioned as separate git repository,
tools will call "learning repo manager",
that will auto commit changes.

Each project directory ("PDIR") may need custom layout details,
therefore will have manifest file:
* `PDIR/manifest.json` - about project itself, and directories/layout/mini-tools, and other json config files with more info...
* `PDIR/tools` - custom made mini-tools for this project
* `PDIR/tools/tests` - tests for mini-tools to verify if changes are not violating earlier assumptions, therefore unit tests/tests treated as machine readable specification of expectations about mini-tools.
* each directory groups some type of information related with given sub-task/sub-aspect of given learning project and have it's own `manifest.json` file, helping AI agents to go around and learn about porject directory unique custom fine-tunes optimized and evolving structure and data there. 
* also each contains subdirectory with integrity checks : `PDIR/subdirectory/checks` that are used by agents like unit tests for code, but for data, therefore , both : to document in machine readable executable way how data shoulb be in given subdirectory, and as well to run and test if after modifications they are correct and if needed allowing Agent to explore posibilities how to fix it (or adjust specification if needed).

LDIR layout:
* `project_name` directories and files for projects are alphanumerics that may use dashes and underscores if needed `[a-z0-9-_]`.
* `LDIR/project_name.meta.json` - metadata about project in json format.
* `LDIR/project_name/` - directory of  specific project user is working on
* each is versioned git repository (and commits made by tool are 

## Architecture
* **CLI** entry point (e.g., `main.py`).
* **Modules**:
  * `concepts/`: handling concept storage, relationships.
  * `progress/`: tracking user knowledge, stats.
  * `exercises/`: storing/serving quizzes, tasks.
  * `suggestions/`: logic for next-step recommendations.
  * `workflows/`: generic workflows, and logic for co-creating or fine tuning with user custom ones

System self evolves and adapts, and can be cusomized by user, so can
store externsions, custom mini tools, in project directory
allow user to create new and co-create with them,
and also help to fine tune those solutions,
or extract parts of it's own codebase into users project directory
allowing to make new customised mini-tools for sub-tasks
and keep fine tuning them as users learning experience evolves.

## Data Model
* **Concept**: {id, name, description, importanceLevel, prerequisites, ...}
* **ProgressRecord**: {userId, conceptId, masteryLevel, lastReviewed, ...}
* **Exercise**: {id, conceptId(s), question, solution, ...}

## CLI Standards
* `-h|--help`: usage info
* `-v|--verbose`: increase verbosity (`-vvvv` for max detail)
* `--version`: show app version

*(Expand details as needed.)*

