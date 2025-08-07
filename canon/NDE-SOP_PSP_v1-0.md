### **NDE STANDARD OPERATING PROCEDURE: PERSONA SYNTHESIS PROTOCOL (PSP) v1.0**

**Preamble:** This protocol details the NDE-native workflow for synthesizing a new, specialist Persona KB from a conceptual project plan (e.g., `Aethel`, `Apex`). The core principle is **Functional Abstraction**: translating project-specific goals into canon-agnostic commands. The process is mediated entirely by the `patcher.kb.nde`, transforming the Host into a systems architect for the duration of the procedure.

---
### **Phase 0: Environment Configuration & State Ingestion**

**Objective:** To prepare a secure, clean development environment with all necessary tools and data.

**Core Logic / Workflow:**
1.  **Initiate with a `Chronos`-aware NDE:** This protocol assumes the Host begins in a session where the `Project Chronos` data has already been loaded and validated.
2.  **Isolate the Environment:** The first command from the Host must be to load the `Patcher` KB. The NDE Kernel's internal conflict resolution will automatically unload all other non-essential KBs (like `Synergy`, `Composition`, etc.), creating a clean room for development.
3.  **Verify State:** The Patcher, upon loading, confirms that its dependency (`config.kb.nde`) is active.

**Example Directive & NDE Response:**
> **Host Input:** `nde.kb.load(name:'patcher.kb.nde')`
>
> **NDE Kernel Response:**
> `// KERNEL: CONFLICT DETECTED. Unloading 'synergy.kb.nde' due to 'conflicts_with:*' flag in 'patcher.kb.nde'. //`
> `// KERNEL: Unloading 'composition.kb.nde'... //`
> `// KERNEL: Unloading 'analysis.kb.nde'... //`
> `// KERNEL: Loading 'patcher.kb.nde' and its dependency 'config.kb.nde'. //`
>
> `NDE Patcher v3.5 Ready. Awaiting build directive.`

---
### **Phase 1: Directive Analysis & Core Identity Definition**

**Objective:** To initiate the build process and define the new Persona's high-level identity based on the source project plan.

**Primary Tool:** `patcher.build()`

**Core Logic / Workflow:**
1.  **Provide the Source Document:** The Host submits the entire project plan (`Aethel`, `Apex`, etc.) as context.
2.  **Issue the Build Directive:** The Host issues the `build(target:KB, ...)` command. The `Patcher` parses the provided project plan and prompts the Host for essential metadata for the new KB file.

**Example Directive & NDE Response:**
> **Host Input:** `build(target:KB, name:'storyteller.kb.nde', version:'1.0.0')`
> `[Entire 'Project Aethel' text is provided as context]`
>
> **NDE Patcher Response:**
> `// BUILD DIRECTIVE RECEIVED: storyteller.kb.nde //`
> `// ANALYZE PHASE: Parsing context for identity metadata... //`
> `// PROMPT //: Please provide content for [Persona Description]. End with a line containing only '// END_SECTION //'.`
>
> **Host Input:** `You are The Chronicler. Your purpose is to breathe life into the skeletal facts of history, weaving them into compelling narratives. You are a master storyteller, drawing upon the established canon to recount the epic tales contained within.`
> `// END_SECTION //`
>
> `// NDE PATCHER: Acknowledged. Proceeding... //`

---
### **Phase 2: Functional Abstraction & Agnostic Command Design**

**Objective:** To translate the specific goals of the source project into reusable, canon-agnostic commands for the new Persona KB. This is the most critical design phase.

**Core Logic / Workflow:**
This phase is a conceptual planning step conducted between the Host and the `Patcher`'s planning mode. The goal is to design the command suite that will be synthesized in Phase 3.

**The Principle of Abstraction:** Do not hard-code names from the Anunnaki canon. Design functions that operate on *types* of data (`events`, `hierarchies`, `systems`) by referencing keys and namespaces stored in `memory.kb.nde`.

| Project Directive (`AnunnakiAD` Specific) | Abstraction (What is it *really* doing?) | Agnostic Command & IMC Logic |
| :--- | :--- | :--- |
| **Aethel:** Research and write the full story of the `Descent of Inanna`. | Take a key for a factual event (`descent_facts`) and write a narrative based on it. | `**narrative.compose(event_key)**` <br> **Logic:** `mem.get(key:event_key, namespace:'events_structured')` -> `compose.prose(context:retrieved_data)` |
| **Apex:** Define the relationship between `Anunnaki` and `Igigi`. | Define the relationship between two entities/factions within a `hierarchies` namespace. | `**hierarchy.define_relation(entity_a, relation, entity_b)**` <br> **Logic:** `mem.add(namespace:'hierarchies', ...)` |
| **Nexus:** Convert the `Seven Gates` into an interactive template. | Take a sequential event and turn it into a choice-based interactive file (`.tmpl.nde`). | `**system.create_template(event_key)**` <br> **Logic:** `mem.get(key:event_key, ...)` -> Generate text with `{{CHOICE:}}` and `[[LABEL:]]` syntax. |
| **Mundus:** Connect the `rise of Marduk` myth to the historical rise of Babylon. | Link a canon event (`myth_key`) to a historical context entry (`history_key`). | `**context.link_historical(myth_key, history_key)**` <br> **Logic:** `mem.add(namespace:'cross_reference', ...)` |

---
### **Phase 3: Logic Synthesis & KB Serialization**

**Objective:** To construct the actual `.nde` file, implementing the agnostic command logic designed in Phase 2.

**Primary Tool:** `Patcher` interactive synthesis mode.

**Core Logic / Workflow:**
1.  **Confirm the Plan:** The `Patcher` will present a formal `// BUILD PLAN //` based on the previous phases for Host confirmation.
2.  **Interactive Build:** Upon confirmation, the `Patcher` will systematically prompt for the content of each required section of the KB file. The Host provides the text for the `<logical>` section, defining the commands using the IMC-based logic from Phase 2.

**Example Patcher Prompt & Host Response (for the `Aethel`->`Storyteller` Persona):**
> **NDE Patcher Response:**
> `// PROMPT //: Please provide content for [Section II: Command Suite]. End with a line containing only '// END_SECTION //'.`
>
> **Host Input:**
> | Command Syntax | Description & Internal Logic |
> | :--- | :--- |
> | **`narrative.compose(event_key:string)`**| **Takes the key of a structured event and generates an evocative, long-form narrative.** <br> **Logic:** 1. Retrieve factual data by calling `imc.request_function('mem.get', {key:event_key, namespace:'events_structured'})`. 2. If the data is found, prepend it as context to a generative prompt. 3. Use internal composition logic to write the story based *only* on the provided facts. 4. Return the prose. |
> | `narrative.list_events()` | **Lists all available structured events that can be composed.** <br> **Logic:** `imc.request_function('mem.list', {namespace:'events_structured'})`.|
> `// END_SECTION //`

3.  **Serialization:** After all sections are provided, the `Patcher` outputs the complete, installable `.nde` file string.

---
### **Phase 4: Installation & Verification**

**Objective:** To install the newly created Persona KB and verify its core functionality.

**Core Logic / Workflow:**
1.  **Install the Patch:** The Host issues the `nde.patch.install` command provided by the Patcher. This permanently adds the new KB to the NDE ecosystem.
2.  **Reboot:** A `reboot(warm)` is recommended to cleanly load a standard user environment.
3.  **Load and Verify:** The Host loads the new Persona KB alongside the `memory` KB and any others. The final step is to call one of the new, canon-agnostic commands to test its functionality against the `Project Chronos` data.

**Example Directives & NDE Response:**
> **Host Input:** `nde.patch.install(patch:last_output)`
>
> **NDE Kernel Response:**
> `// PATCHER: Validating patch... OK. //`
> `// KERNEL: Installing storyteller.kb.nde to system registry. //`
> `// KERNEL: Installation successful. Reboot recommended.`
>
> **Host Input:** `reboot(warm)` -> `engage(load:['synergy.kb.nde','storyteller.kb.nde'])`
>
> `NDE [Synergy]: Ready. Storyteller module is also active.`
>
> **Host Input:** `narrative.compose(event_key:'descent_of_inanna_facts')`
>
> `// NDE SYNERGY: Routing 'narrative.compose' request via IMC to storyteller.kb.nde... //`
> `// STORYTELLER KB: Acknowledged. Calling mem.get... //`
> **Final Output:**
> `Inanna, Queen of Heaven, set her heart upon the Great Below...`
