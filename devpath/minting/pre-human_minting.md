### **Phase I: The Grand Unified cNFT Schema (RPG Edition)**

This is the definitive blueprint for your cNFTs. It integrates a point-based RPG system directly into the three-tiered architecture.

#### **A. Core RPG System Design**

*   **Primary Attributes:** Four core stats that govern all actions.
    *   `Brutality (BRU)`: Raw physical power, melee damage, and intimidation.
    *   `Prowess (PRO)`: Agility, ranged accuracy, speed, and crafting skill.
    *   `Cunning (CUN)`: Perception, strategy, social influence, and shamanic power.
    *   `Resilience (RES)`: Health, stamina, and resistance to damage and toxins.
*   **Progression:**
    *   **Experience Points (XP):** The currency of growth, earned from actions.
    *   **Level:** The primary on-chain indicator of a character's power. Gained at specific XP thresholds.
    *   **Attribute Points (AP):** Awarded on level-up. The owner spends these to permanently increase Primary Attributes. This provides player agency.
*   **Historical Effects (`Echoes`):** Significant actions bestow permanent or temporary modifiers called Echoes. These are narrative and mechanical representations of the character's history.
    *   *Example 1:* Slaying a great beast might grant the **"Echo of the Beast-Slayer"** (+5 BRU when fighting fauna).
    *   *Example 2:* Fleeing from a critical battle might bestow the **"Echo of the Craven"** (-10 CUN in clan leadership challenges).

#### **B. The Master Data Structure**

This table defines every piece of data, its location within the architecture, and its purpose.

| Data Point | Location (Tier) | Type | Purpose & Example |
| :--- | :--- | :--- | :--- |
| **`Level`** | **1 (On-Chain)** | `Attribute` | The primary, filterable measure of power. Updated only on level-up. *Ex: {"trait\_type": "Level", "value": 5}* |
| **`Unspent AP`** | **1 (On-Chain)** | `Attribute` | Signals that a character has leveled up and awaits its owner's input. *Ex: {"trait\_type": "Unspent AP", "value": 3}* |
| **`Primary Echo`** | **1 (On-Chain)** | `Attribute` | The character's most defining historical achievement for prestige and filtering. *Ex: {"trait\_type": "Echo", "value": "Beast-Slayer"}* |
| **`Clan`** | **1 (On-Chain)** | `Attribute` | Current clan affiliation. *Ex: {"trait\_type": "Clan", "value": "Sun-Eaters"}* |
| **`XP`** | **3 (Off-Chain DB)**| `Integer` | High-frequency growth metric. **Too costly to store on-chain.** *Ex: 1570/2000* |
| **`Primary Attributes`** | **3 (Off-Chain DB)** | `Object` | The live stat block, modified by spending AP. *Ex: { BRU: 12, PRO: 9, CUN: 11, RES: 14 }* |
| **`Echoes`** | **3 (Off-Chain DB)**| `Array` | The complete list of all historical effects, active and passive. *Ex: [{ id: "echo_beast_slayer", active: true, ... }]*. |
| **`Timeline`** | **3 (Off-Chain DB)**| `Array` | The full event log as previously designed. *Ex: ["Fought Uridimmu - Victory"]* |
| `All On-Chain Data` | 2 (Arweave) | Snapshot | The Arweave JSON contains a **snapshot** of all on-chain `attributes` for public viewing. |
| `Chronicle Link` | 2 (Arweave) | Object | The `chronicle.fuid` linking to the Tier 3 database. |

#### **C. "Pure Data" for a Pool-Minted cNFT (The Genesis State)**

This is the exact JSON structure that your backend will generate and upload to Arweave for a brand new, Level 1 character ready for the minting pool.

```json
{
  "name": "Anunnaki AD Pre-Human #101",
  "symbol": "ANUNNAKIAD",
  "description": "A 20-season old wiry and lean survivalist, their skin the color of baked mud...",
  "image": "https://arweave.net/path_to_image_101.png",
  "seller_fee_basis_points": 500,
  "attributes": [
    // --- ON-CHAIN RPG STATE ---
    { "trait_type": "Level", "value": 1, "display_type": "number" },
    { "trait_type": "Unspent AP", "value": 0, "display_type": "number" },
    { "trait_type": "Echo", "value": "None" },
    { "trait_type": "Clan", "value": "Unclanned" },

    // --- COSMETIC & STATIC TRAITS ---
    { "trait_type": "Archetype", "value": "Survivalist" },
    { "trait_type": "Age", "value": 20, "display_type": "number" },
    { "trait_type": "Build", "value": "Wiry and lean" }
  ],
  "properties": {
    "files": [{ "uri": "https://arweave.net/path_to_image_101.png", "type": "image/png" }],
    "category": "image"
  },
  "generation_parameters": { /* ... */ },
  "chronicle": {
    "fuid": "a6b8c1d3-e5f6-4a7b-8c9d-1e2f3a4b5c6d",
    "last_updated": "2024-10-27T18:30:00Z"
  }
}
```

---

### **Phase II: The Implementation Protocol**

These are the exact instructions for developing the system.

#### **Step 1: Backend Database & API (The Chronicle Service)**

Your database now requires a primary table to manage the live state of each character.

**New `characters` Table Schema:**

```sql
CREATE TABLE characters (
    fuid UUID PRIMARY KEY,         -- The Chronicle FUID from the cNFT metadata
    owner_wallet VARCHAR(44),      -- The current Solana wallet holding the cNFT
    xp INT NOT NULL DEFAULT 0,
    -- Core Stats
    base_brutality INT NOT NULL,
    base_prowess INT NOT NULL,
    base_cunning INT NOT NULL,
    base_resilience INT NOT NULL,
    -- Relationships
    clan_id VARCHAR(50) REFERENCES clans(clan_id)
);
```

**New `echoes` Table Schema:**

```sql
CREATE TABLE echoes (
    id SERIAL PRIMARY KEY,
    character_fuid UUID REFERENCES characters(fuid),
    echo_id VARCHAR(50) NOT NULL, -- e.g., 'echo_beast_slayer'
    description TEXT,
    effects JSONB,                 -- e.g., {"modifiers": [{"stat": "BRU", "value": 5, "condition": "vs_fauna"}]}
    acquired_at TIMESTAMP NOT NULL DEFAULT NOW()
);
```

Your API must have new endpoints for game logic:
*   `POST /api/v1/event/battle`: Receives battle outcome, calculates XP gain, checks for level-up, and potentially generates new Echoes.
*   `POST /api/v1/character/:fuid/spend_ap`: Allows the owner to spend their `Unspent AP` to increase a character's base stats. This endpoint must verify the `owner_wallet`.

#### **Step 2: The On-Chain Update Lifecycle & Cost Analysis**

This is the most critical part. There are three types of updates, each with a different procedure and cost.

| Update Type | Trigger | Procedure | Cost Breakdown (per update) |
| :--- | :--- | :--- | :--- |
| **1. High-Frequency State Change** | **Battle Win, Training** | **1. Chronicle API Call:** The game engine tells your backend the character gained 100 XP.<br>**2. DB Update:** The backend updates the `xp` value in the `characters` table for that `fuid`.<br>**3. End.** No on-chain action occurs. | **On-Chain Cost: §0.00**<br>Off-Chain Cost: Server processing + database write (fractions of a cent). |
| **2. Low-Frequency State Change**| **Level-Up Event** | **1. High-Frequency Change Occurs:** A battle win pushes XP over the level threshold.<br>**2. Backend Logic:** The API detects the level-up, calculates new `Level` and `Unspent AP`, and potentially a new `Primary Echo`.<br>**3. New Metadata Generated:** A new Arweave JSON file is created with the updated `attributes`.<br>**4. Permanent Upload:** The new JSON is uploaded to Arweave.<br>**5. On-Chain Transaction:** The backend uses the `APP_DELEGATE_KP` to send a Bubblegum `update` transaction to the blockchain, changing the cNFT's URI to point to the new Arweave file. | **On-Chain Cost: §0.01 - §0.05**<br>- **Arweave Upload:** ~$0.01 (for a ~2KB JSON file)<br>- **Solana Tx Fee:** ~$0.0001<br>- **Helius RPC CU:** ~$0.001 (for the update)<br>Off-Chain Cost: Server processing (pennies). |
| **3. Owner-Driven State Change**| **Spending AP, Joining a Clan**| The procedure is identical to **Type 2: Low-Frequency State Change.** The user interface calls an API (`/spend_ap`), which triggers the backend to update the database, generate a new JSON, upload it, and send the on-chain transaction. | The cost is identical to **Type 2**: **~§0.01 - §0.05 per owner action** that changes on-chain state. |

**Implementation Summary:**

1.  **On Mint:** Use the "Pure Data" structure to create Level 1 characters and populate their corresponding rows in your `characters` database table.
2.  **During Gameplay:**
    *   For actions like fighting or training, **only interact with your off-chain Chronicle API (Update Type 1)**. The app UI can poll this API to show a real-time XP bar.
    *   When the API detects a level-up or receives an owner's request to spend AP, it **autonomously executes the full Update Type 2/3 lifecycle.**

This architecture provides the "living" RPG experience you envision. It is cost-effective by keeping high-frequency changes off-chain, while remaining anchored to the trust and permanence of the Solana blockchain for major milestones. It gives the owner true agency over their character's growth, which is the cornerstone of any successful RPG system.
