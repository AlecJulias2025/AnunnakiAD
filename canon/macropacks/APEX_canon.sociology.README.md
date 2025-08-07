# Knowledge Base: canon.sociology.kb.nde

## 1. Description

The Sociology KB is a specialist module for the NDE designed for world-builders and narrative designers who need to establish and maintain complex social systems. It provides a structured, academic interface for defining the laws, power structures, and covenants that govern the inhabitants of a fictional world. Its primary purpose is to transform abstract societal concepts into a hard, auditable canon.

## 2. Core Purpose & Philosophy

This KB acts as an intelligent front-end for more foundational modules like `worldbuilding.kb.nde` and `analysis.kb.nde`. Instead of just storing facts, it provides purpose-built commands to ensure sociological data is added to the session memory in a structured, consistent, and machine-readable format. This allows for powerful, high-level consistency checks.

## 3. Key Features & Commands

-   **`soc.define.governing_body(...)`**: Canonizes a ruling body like a council or senate.
-   **`soc.define.social_caste(...)`**: Canonizes a distinct social class with defined duties and rights.
-   **`soc.define.pact(...)`**: Canonizes a formal agreement or covenant between two parties.
-   **`soc.define.hierarchy(...)`**: Canonizes a formal, ordered power structure (e.g., military ranks, underworld levels).
-   **`proc.sociopolitical_coherence(target)`**: The keystone procedure. Audits a piece of text against the *entire* established social canon, reporting any contradictions (e.g., a character performing an act reserved for a higher caste).

## 4. Requirements & Installation

### Dependencies
This KB **requires** the following modules to be active to function correctly:
-   `worldbuilding.kb.nde`
-   `analysis.kb.nde`
-   (And by extension, their dependencies: `memory.kb.nde`, etc.)

### Conflicts
This KB is a primary user interface and **will not run** alongside the following KBs:
-   `synergy.kb.nde`
-   `composition.kb.nde`

### Installation
1. Ensure the Kernel is running.
2. If the Patcher KB just generated the file, use the command: `nde.patch.install(patch: last_output)`
3. To load the KB for use, you must load it and its dependencies:
   `nde.kb.load(name:['memory.kb.nde', 'analysis.kb.nde', 'worldbuilding.kb.nde', 'canon.sociology.kb.nde'])`

## 5. Example Usage

# Define the 'Igigi' as a social caste
soc.define.social_caste(
  name:'Igigi', 
  description:'A group of lesser gods tasked with manual labor for the Anunnaki before humanity's creation.',
  status:'Subordinate',
  duties:['Digging canals', 'Maintaining shrines'],
  rights:['Divine sustenance', 'Residence in the lower heavens']
)

# Audit a story chapter for contradictions against the new canon
proc.sociopolitical_coherence(target:'Chapter_3_draft.txt')
