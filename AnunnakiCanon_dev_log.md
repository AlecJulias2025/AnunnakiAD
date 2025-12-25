# Anunnaki Canon Development Log

## 1. System SOP: Content Generation Standards

**Objective:** Maintain absolute structural consistency for `AnunnakiCanon` JSON-in-Markdown entries.

### 1.1 Entry Structure
All entries must be wrapped in a `<details>` block.
```html
<details>
<summary><code>FUID: [CLASS]-[kebab_case_name]-v[X.Y]</code></summary>

```json
{
  "FUID": "[CLASS]-[kebab_case_name]-v[X.Y]",
  "key": "nde:[class_lower]::[snake_case_name]",
  "ontological_class": "[CLASS_UPPER]",
  "title": "Title Case Descriptive Name",
  "ref_name": "Common Name",
  "content_proposition": {
    "summary": "Concise definition.",
    "properties": { ... },
    "historical_provenance": [ ... ],
    "analysis": { ... }
  },
  "relational_hooks": { ... },
  "development_log": { "version": "[X.Y]", "status": "..." },
  "metadata": { ... }
}
```
</details>
```

### 1.2 Ontology Classes & FUID Prefixes
*   **RULE**: `nde:rule::` (e.g., Divine Laws, Cosmic Principles)
*   **DEITY**: `nde:deity::` (Gods, Goddesses)
*   **CHARACTER**: `nde:character::` (Mortals, Demigods, Heroes)
*   **ENTITY**: `nde:entity::` (Monsters, Spirits, Constructs)
*   **ARTIFACT**: `nde:artifact::` (Objects of Power)
*   **EVENT**: `nde:event::` (Narrative Arcs, Historical Moments)
*   **LOCATION**: `nde:loc::` (Cities, Realms, Geographical Features)
*   **APPENDIX**: `nde:appendix::` (Deep Analytical Documents) [NEW]
*   **PATTERN**: `nde:pattern::` (Motifs, Recurring Symbolism) [PROPOSED]

### 1.3 Data Integrity Rules
1.  **FUID Consistency**: The `FUID` in the summary tag and the JSON body must match exactly.
2.  **Key Linking**: All cross-references must use the `nde:class::name` format.
3.  **No Hallucination**: Deduce details only from validated surrounding sources. Mark speculation clearly in `analysis`.
4.  **Formatting**: Valid JSON is mandatory. Escape quotes within strings.

---

## 2. Gap Analysis: v1.2 vs. Completion Needs (v2.0)

This analysis cross-references the current `AnunnakiCanon_v1-2.md` against the `canon/AegisPrime/completion_needs.md` worklist and user requirements.

### 2.1 Status of Priority 1 & 2 Items (Narrative Critical)
| Entity | Ref Name | Status in v1.2 | Action Required |
| :--- | :--- | :--- | :--- |
| `nde:deity::kingu` | Kingu | **PRESENT** | None. |
| `nde:deity::ninsun` | Ninsun | **PRESENT** | None. |
| `nde:deity::aruru` | Aruru | **PRESENT** | None. |
| `nde:character::nincubura` | Nincubura | **PRESENT** | None. |
| `nde:character::shamhat` | Shamhat | **PRESENT** | None. |
| `nde:deity::ningal` | Ningal | **PRESENT** | None. |
| `nde:entity::scorpion_beings` | Scorpion Beings | **PARTIAL** | Cut off in file ("142 lines omitted"). Needs verification/re-instantiation. |

### 2.2 Critical Content Gaps (Outstanding v2.0 Needs)
The following items are listed in `completion_needs.md` but are missing or listed as "PLANNED" in v1.2:
*   **Locations**:
    *   `nde:loc::kish` (Planned): Essential political counterweight to Uruk.
    *   `nde:loc::mount_nisir` (Planned): Essential geographical anchor for the Flood.
    *   `nde:loc::e-ana_temple` (Planned): The "White House" of Uruk.
    *   `nde:loc::ekur` (Planned): The seat of Enlil in Nippur.
*   **Artifacts**:
    *   `nde:artifact::sharur_mace`: Essential for Ninurta's narrative.
    *   `nde:artifact::pukku_and_mikku`: Missing from Gilgamesh's later narrative (Tablet XII).

### 2.3 The "Immersion Layer" Gaps (User Request)
The current locations (Uruk, Nippur, Eridu) exist as high-level summaries. To meet the requirement for "solid details" and avoid "megoliths in a void", we need:
*   **Architectural Schematics**: Floorplans for the *E-ana* and *Ekur* (gates, courtyards, shrine names).
*   **Sensory Profiles**: The smell of the mudbrick, the sound of specific rituals, the light quality in the inner sanctum.
*   **Daily Life**: How the "Main System" of the city functions around these temples.

### 2.4 Thematic Gaps (Patterns & Appendices)
*   **`nde:appendix::cosmological_models`**: Explicitly requested. Must resolve An/Ki vs. Enuma Elish.
*   **`nde:pattern::`**: New class needed to capture motifs like "The Seven", "The Sleep of the Gods", and "The Power of the Name".

---

## 3. Deep Research Queries (Prompts)

Use the following prompts in research engines (Gemini/Perplexity) to generate strictly formatted JSON data for canon expansion.

### QUERY A: Architectural & Sensory Reconstruction (The Immersion Layer)
**Goal**: Flesh out `nde:loc::kish`, `nde:loc::e-ana_temple`, and `nde:loc::ekur`.

> **Prompt:**
> "Act as a specialist in Sumerian architecture and urban archaeology. Conduct a deep reconstruction of **[INSERT: The E-ana Precinct in Uruk OR The Ekur Complex in Nippur]**.
> Focus on:
> 1.  **Architectural Layout**: Identify specific named gates, courtyards (e.g., Ubshu-ukkinna), and sub-shrines.
> 2.  **Sensory Environment**: Describe the materials (lapis, cedar, whitewash), the smells (specific incenses, offerings), and the sounds (liturgical hymns, city noise).
> 3.  **Ritual Function**: How did the priesthood interact with this space daily?
>
> **RETURN FORMAT:** Provide a single JSON object adhering to this schema:
> ```json
> {
>   "spatial_layout": {
>     "gates": [{"name": "string", "description": "string"}],
>     "courtyards": [{"name": "string", "function": "string"}],
>     "shrines": [{"name": "string", "deity": "string"}]
>   },
>   "sensory_profile": {
>     "visuals": ["string"],
>     "olfactory": ["string"],
>     "auditory": ["string"]
>   },
>   "operational_context": "string (How the location functions within the 'Main System')"
> }
> ```"

### QUERY B: The "Pattern" Extraction (Motifs)
**Goal**: Populate the new `PATTERN` class.

> **Prompt:**
> "Analyze the corpus of Mesopotamian mythology through the lens of symbolic patterns. Identify and decode the recurring motif of **[INSERT: The Number Seven OR The Sleep of the Gods OR The Power of the Name]**.
> Focus on:
> 1.  **Occurrences**: List every significant instance (e.g., 7 gates, 7 sages, 7 days of flood).
> 2.  **Symbolic Logic**: What 'rule' does this pattern represent in the Anunnaki 'Main System'?
>
> **RETURN FORMAT:**
> ```json
> {
>   "FUID": "PATTERN-[name]-v1.0",
>   "key": "nde:pattern::[snake_case_name]",
>   "ontological_class": "PATTERN",
>   "content_proposition": {
>     "definition": "string",
>     "occurrences": [{"source": "string", "context": "string"}],
>     "systemic_function": "string (The 'law' behind the symbol)"
>   }
> }
> ```"

### QUERY C: nde:appendix::cosmological_models (Priority Task)
**Goal**: Solve the contradictions between Sumerian and Babylonian creation myths.

> **Prompt:**
> "Perform a rigorous comparative theological analysis of the two primary Mesopotamian creation models:
> 1.  **The Sumerian Model**: The separation of An (Heaven) and Ki (Earth) by Enlil.
> 2.  **The Babylonian Model**: The *Enūma Eliš*, where Marduk constructs the cosmos from Tiamat's corpse.
>
> **Analysis Points:**
> *   **Contradictions**: Identify where the physical and metaphysical mechanics clash.
> *   **Political Theology**: Analyze how the shift from Enlil (Separator) to Marduk (Architect) reflects the shift from Nippur to Babylon.
> *   **Theological Implications**: How does the nature of 'Creation' change? (Separation vs. Construction/Dismemberment).
>
> **RETURN FORMAT:**
> ```json
> {
>   "FUID": "APPENDIX-cosmological_models-v1.0",
>   "key": "nde:appendix::cosmological_models",
>   "ontological_class": "APPENDIX",
>   "title": "Analysis of Contradictions: An/Ki Separation vs. Enūma Eliš",
>   "content_proposition": {
>     "sumerian_model": {
>       "mechanism": "Separation (Binary Fission)",
>       "primary_agent": "Enlil",
>       "philosophical_basis": "Creation is the definition of boundaries between pre-existing wholes."
>     },
>     "babylonian_model": {
>       "mechanism": "Construction via Necromancy/Dismemberment",
>       "primary_agent": "Marduk",
>       "philosophical_basis": "Creation is the violent imposition of order upon chaotic matter."
>     },
>     "synthesis_analysis": {
>       "political_shift": "string (Detailed analysis of the Nippur-Babylon power transfer)",
>       "theological_evolution": "string (Detailed analysis of the shift from passive to active creation)",
>       "contradiction_resolution": "string (How the canon reconciles these views, if at all)"
>     }
>   }
> }
> ```"

### QUERY D: Missing Artifacts & Entities
**Goal**: Complete the item/entity catalog.

> **Prompt:**
> "Research the following specific Mesopotamian mythological entities for canon instantiation:
> 1.  **Sharur**: The sentient mace of Ninurta.
> 2.  **Pukku and Mikku**: The drum and drumstick of Gilgamesh (Tablet XII).
> 3.  **The Net of Marduk**: Used in the Enuma Elish.
>
> **RETURN FORMAT:** For each, provide a JSON object matching the `ARTIFACT` or `ENTITY` schema defined in the SOP."
