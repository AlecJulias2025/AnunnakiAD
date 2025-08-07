Directive acknowledged. As the systems architect, I will provide a functional analysis detailing the primary use-cases for the four installed specialist modules and their architecturally valid combinations. This report is based on their declared capabilities and dependencies.

// REPORT: MODULE UTILITY ANALYSIS //

---
#### **[I] Individual Module Capabilities**

**1. `canon.mechanics.kb.nde` (The Mechanic)**
*Purpose: To transform static lore into interactive systems.*
*   **Use Case 1:** Deconstructing a mythological event into a quest line for a game by using `proc.story_to_template`.
*   **Use Case 2:** Defining the specific powers and spheres of influence for a deity (`domains.map`) to create a balanced "cleric/paladin" class system.
*   **Use Case 3:** Turning a world's creation myth into a playable, branching interactive fiction.
*   **Use Case 4:** Operationalizing abstract canonical rules (e.g., 'The Tablet of Destinies') into concrete mechanics for a TTRPG.

**2. `canon.context.kb.nde` (The Anchor)**
*Purpose: To ground established canon in simulated history and culture for verisimilitude.*
*   **Use Case 1:** Linking the rise of a specific god in your pantheon to the political ascendancy of their patron city (`context.sync.event`).
*   **Use Case 2:** Creating authentic-feeling religious rituals and social hierarchies (`define.ritual`, `define.hierarchy`) that are logically derived from your lore.
*   **Use Case 3:** Generating a complete "lore bible" entry (`context.report`) that explains not just *what* a myth is, but *why* the culture tells it.
*   **Use Case 4:** Checking a new piece of lore against your established historical timeline to ensure it's coherent.

**3. `canon.sociology.kb.nde` (The Sociologist)**
*Purpose: To define and audit the intricate social and political systems of the world.*
*   **Use Case 1:** Formally canonizing the structure of a ruling council, from its membership to its decision-making process (`soc.define.governing_body`).
*   **Use Case 2:** Detailing the rigid caste system of your world, defining the rights, duties, and status of each group (`soc.define.social_caste`).
*   **Use Case 3:** Auditing a draft chapter of your story to automatically find moments where characters act in ways that contradict the defined social or political rules (`proc.sociopolitical_coherence`).
*   **Use Case 4:** Establishing formal treaties and pacts between factions (`soc.define.pact`) to create a clear and consistent political landscape.

**4. `storyteller.kb.nde` (The Chronicler)**
*Purpose: To narrate the established canon, turning factual entries into compelling prose and dialogue.*
*   **Use Case 1:** Automatically generating an in-game lore book that recounts the 'Great Flood' epic (`storyteller.tell`).
*   **Use Case 2:** Creating a dramatic script (`storyteller.dramatize`) for a key moment in history, ensuring the characters' dialogue aligns with the event's known outcome.
*   **Use Case 3:** Quickly producing evocative, lore-accurate story content based on your factual timeline without having to write it from scratch.
*   **Use Case 4:** Ensuring 100% canonical purity in all narrative outputs, as it is architecturally forbidden from inventing new lore.

---
#### **[II] Compatible Combination Synergies**

*Architectural Note: `canon.mechanics.kb.nde` cannot be used simultaneously with `canon.context.kb.nde` or `canon.sociology.kb.nde` due to conflicting core dependencies. All other combinations listed are valid.*

**1. The "World Anvil" Suite: `Sociology` + `Context`**
*Purpose: To build a deeply realistic and internally consistent society.*
*   **Use Case:** First, use the `Sociology` KB to define the formal structure of a priesthood. Then, switch to the `Context` KB to link that priesthood's rise to power to a specific schism or historical event you define. This creates a fully-realized social institution with both a defined structure and a historical origin.

**2. The "Living History" Suite: `Storyteller` + `Context`**
*Purpose: To generate both the academic analysis and the in-universe narrative of your lore.*
*   **Use Case:** Use `Context` to generate a report on the political and agricultural reasons behind a "Dying God" myth. Immediately after, use `Storyteller` to write the myth itself—a tragic, poetic epic about the god's fate. This provides both the "real" story and the "story they tell."

**3. The "Political Drama" Suite: `Storyteller` + `Sociology`**
*Purpose: To ensure the narratives you write are constrained by the political systems you've designed.*
*   **Use Case:** Use `Sociology` to formally define a pact between two warring factions. Then, use `Storyteller`'s `dramatize` command to write the scene of the tense negotiations, guaranteeing the dialogue and outcome are consistent with the terms you established.

Analysis complete. These modules provide a comprehensive toolset for moving from high-level canon to detailed, functional, and narratively rich worldbuilding.

NDE Patcher v3.5 Ready. Awaiting build directive.
