# README: canon.context.kb.nde ("The Anchor")
**Version:** 1.0.0

### **1. Description**
The Canon Context KB ('The Anchor') is a specialist tool for loremaster-historians and world-builders. It links mythological canon to its real-world historical, cultural, and sociological context, operating with the academic persona of a 'Cultural Curator'. By enabling the systematic, data-driven association of fictional events with factual anchors, it adds profound verisimilitude and intellectual consistency to any fictional world.

### **2. Key Features**
- **Historical Synchronization:** Link mythological canon entries to parallel historical events, creating a richer, dual-layered timeline.
- **Cultural Documentation:** Define and store detailed information about real-world cultural practices, such as religious rituals, power structures, and societal roles.
- **Systemic Belief Analysis:** Document and analyze how a culture interacts with its canon through systems like divination and eschatology.

### **3. Dependencies**
This KB requires the following modules to be active for full functionality:
- `memory.kb.nde`: Used to read the foundational canon and to store all new contextual data.
- `analysis.kb.nde`: Used by the `context.check.coherence` command to perform consistency audits.

### **4. Installation**
To install this KB, load the patch file using the following Kernel command:
`nde.patch.install(patch:<file_content>)`

Once installed, it can be loaded into a session with:
`nde.kb.load(name:'canon.context.kb.nde')`

### **5. Core Commands**

| Command | Description |
| :--- | :--- |
| `context.sync.event(myth_id, hist_desc)` | **Primary command.** Links a new historical event to an existing mythological one. The `myth_id` must be a key from the 'events' namespace in memory. |
| `context.define.ritual(name, desc)` | Documents a cultural or religious ritual. |
| `context.define.hierarchy(name, desc)`| Documents a societal or religious power structure. |
| `context.detail.divination(name, desc)`| Details a system of divination. |
| `context.detail.eschatology(desc)`| Documents beliefs about the afterlife. |
| `context.report(myth_id)` | Generates a complete, layered report on a single canon entry, showing the myth and all associated context. |
| `context.check.coherence(id_a, id_b)`| Checks two data entries (mythological or historical) for contradictions. |

### **6. Usage Example**
*Assuming the 'Project Chronos' canon is loaded in memory.*

1.  **Link a historical event to the myth of the Great Flood:**
    ```
    context.sync.event(myth_id:'nde:event::the_great_flood', hist_desc:'Archeological evidence of the Shuruppak fluvial event (c. 2900 BCE), a layer of river sediment that indicates a major, localized flood which may have inspired later deluge myths.')
    ```

2.  **Define a related cultural practice:**
    ```
    context.define.ritual(name:'Akitu Spring Festival', desc:'A complex new year festival that often involved the public recital of creation epics, reinforcing the foundational myths of the culture.')
    ```

3.  **Generate a combined report:**
    ```
    context.report(myth_id:'nde:event::the_great_flood')
    ```
    *(This would output a structured report containing the original myth, the linked historical data from step 1, and any other associated cultural data.)*

---
This README provides a complete overview for a new user of the `canon.context.kb.nde` module.
