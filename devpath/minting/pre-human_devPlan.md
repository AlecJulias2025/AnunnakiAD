#### **Execute these phases in sequence. This is the path from concept to cosmos.**

---

### **Phase I: The Grand Unified Architecture**

This is the definitive structure of the Anunnaki AD ecosystem, reflecting your chosen options.

| Component | Technology / Implementation | Role & Justification |
| :--- | :--- | :--- |
| **On-Chain Foundation** | **Merkle Tree (Depth: 14)** + **Collection NFT** | **(1A Modified):** A focused genesis pool of 16,384 possible slots. The "upgrade" comes from a deflationary **Ritual Sacrifice** system, where multiple Pre-Human cNFTs are burned to mint one transcendent "Anunnaki" cNFT into a *separate, elite collection*, creating value without expanding the original tree. |
| **Asset Visuals**| **Server-Side Compositing Engine** | **(Custom Image Spec):** The cNFT's `image` URI will point to a server API (`/api/image/:fuid`). This API will fetch a character's foreground (transparent PNG) and a dynamic background from a cloud storage bucket (AWS S3, Cloudflare R2), composite them in real-time, and serve the final image. This provides maximum creative flexibility at the cost of centralization. |
| **Mint Experience** | **The Generative Forge** | **(2B):** A user-driven mint. The backend orchestration service handles on-demand generation, variant selection, and the minting transaction, providing an immediate, personal connection to the asset. |
| **RPG Engine** | **On-Chain Hybrid Model** | **(3B):** Core progression (`Level`, `Unspent AP`, `Primary Echo`) is anchored on-chain as `attributes` for verifiability and prestige. High-frequency data (XP, detailed stats) lives in the off-chain Chronicle Database for speed and low cost. |
| **Utility Engine** | **Economic Crafting & Public API** | **(4C/4B Hybrid):** A deep crafting system (**Ritualistic Crafting**) forms the core gameplay loop. A public-facing, ownership-verified API (**Chronicle as a Key**) provides interoperability. |
| **Economy Anchor** | **Stablecoin Integration (USDC)** | **(Stablecoin Reinforcement):** The Forge mint and high-value marketplace/crafting fees are priced in USDC. This pegs core economic activities to a stable value, insulating the economy from Solana's price volatility and creating a predictable financial foundation. |

---

### **Phase II: Step-by-Step Implementation Protocol**

Follow this tutorial precisely.

#### **Sprint 0: Project Setup & Environment**

1.  **Initialize Project:**
    ```bash
    npx create-next-app@latest anunnaki-ad-app --ts
    cd anunnaki-ad-app
    ```
2.  **Install Dependencies:**
    ```bash
    npm install @solana/web3.js @solana/spl-token @metaplex-foundation/umi @metaplex-foundation/umi-bundle-defaults @metaplex-foundation/umi-signer-wallet-adapters @metaplex-foundation/mpl-bubblegum @solana/wallet-adapter-react @solana/wallet-adapter-react-ui @solana/wallet-adapter-base @solana/wallet-adapter-wallets form-data bs58 sharp @aws-sdk/client-s3 pg
    ```    *(Note: `sharp` is for image composition, `pg` for PostgreSQL, `@aws-sdk/client-s3` for S3).*
3.  **Setup Environment Variables:** Create a file named `.env.local` in your project root.
    ```env
    # Solana Cluster & RPC
    SOLANA_RPC_URL="YOUR_HELIUS_RPC_URL_WITH_API_KEY"
    SOLANA_CLUSTER="devnet"

    # Server Wallet (Tree Authority, Fee Payer, etc.)
    SERVER_WALLET_SECRET_KEY="[...your wallet's secret key as a byte array...]"

    # On-Chain Program Addresses (Leave blank until created in Sprint 1)
    COLLECTION_MINT_ADDRESS=""
    MERKLE_TREE_ADDRESS=""
    
    # USDC Details (Devnet USDC Mint Address)
    USDC_MINT_ADDRESS="Gh9ZwEmdLJ8DscKNTkTqPbNwLNNBjuSzaG9Vp2KGtKJr"
    
    # Off-Chain Services
    POSTGRES_URL="postgres://user:password@host:port/database"
    R2_ACCOUNT_ID="YOUR_CLOUDFLARE_R2_ACCOUNT_ID"
    R2_ACCESS_KEY_ID="YOUR_R2_ACCESS_KEY"
    R2_SECRET_ACCESS_KEY="YOUR_R2_SECRET_KEY"
    R2_BUCKET_NAME="anunnaki-assets"
    ```

#### **Sprint 1: The On-Chain Foundation**

This is a one-time script you run from your server to create the permanent on-chain assets. Create a file `scripts/createFoundation.ts`.

```typescript
// scripts/createFoundation.ts
import { createUmi } from '@metaplex-foundation/umi-bundle-defaults';
import { createSignerFromKeypair, generateSigner, keypairIdentity, percentAmount, publicKey } from '@metaplex-foundation/umi';
import { mplBubblegum, createTree, createCollectionV1 } from '@metaplex-foundation/mpl-bubblegum';
import { bs58 } from 'bs58';
import 'dotenv/config';

async function main() {
    const umi = createUmi(process.env.SOLANA_RPC_URL!).use(mplBubblegum());

    // Setup the Server Wallet as the Signer
    const serverKeypair = umi.eddsa.createKeypairFromSecretKey(
        bs58.decode(process.env.SERVER_WALLET_SECRET_KEY!)
    );
    umi.use(keypairIdentity(serverKeypair));
    console.log(`Server wallet address: ${serverKeypair.publicKey}`);

    // 1. Create the Collection NFT
    const collectionMint = generateSigner(umi);
    await createCollectionV1(umi, {
        collectionMint,
        name: 'Anunnaki AD Pre-Humans',
        uri: 'https://arweave.net/path_to_your_collection_metadata.json',
        sellerFeeBasisPoints: percentAmount(5), // 5% royalty
        updateAuthority: serverKeypair.publicKey,
    }).sendAndConfirm(umi);

    console.log('--- Collection NFT Created ---');
    console.log(`Mint Address: ${collectionMint.publicKey}`);

    // 2. Create the Merkle Tree (Depth 14 for 16,384 NFTs)
    const merkleTree = generateSigner(umi);
    await createTree(umi, {
        merkleTree,
        maxDepth: 14,
        maxBufferSize: 64,
        public: false, // Only tree authority can mint
        treeAuthority: serverKeypair.publicKey,
    }).sendAndConfirm(umi);

    console.log('--- Merkle Tree Created ---');
    console.log(`Tree Address: ${merkleTree.publicKey}`);
    console.log('--- ACTION REQUIRED ---');
    console.log('Update your .env.local file with these new mint and tree addresses!');
}

main().catch(console.error);
```
**To run this script:** `npx ts-node scripts/createFoundation.ts`. **Update your `.env.local` file with the output.**

#### **Sprint 2: The Backend Core (The Chronicle & Forge API)**

This sprint builds the "brain" of your application.

1.  **Database Schema (`scripts/createDatabase.ts`):**
    ```typescript
    import { Pool } from 'pg';
    import 'dotenv/config';

    const pool = new Pool({ connectionString: process.env.POSTGRES_URL });

    async function createSchema() {
        const client = await pool.connect();
        try {
            await client.query(`
                CREATE TABLE IF NOT EXISTS characters (
                    fuid UUID PRIMARY KEY,
                    owner_wallet VARCHAR(44),
                    asset_id VARCHAR(44) UNIQUE, -- cNFT Address
                    level INT NOT NULL DEFAULT 1,
                    unspent_ap INT NOT NULL DEFAULT 0,
                    xp INT NOT NULL DEFAULT 0,
                    base_brutality INT NOT NULL,
                    base_prowess INT NOT NULL,
                    base_cunning INT NOT NULL,
                    base_resilience INT NOT NULL,
                    primary_echo VARCHAR(100) DEFAULT 'None',
                    foreground_path VARCHAR(255) NOT NULL, -- Path in S3/R2 bucket
                    background_path VARCHAR(255) NOT NULL
                );

                -- Other tables for Echoes, Clans, Events, etc. go here
            `);
            console.log('Database schema created successfully.');
        } finally {
            client.release();
        }
    }
    createSchema().catch(console.error);
    ```

2.  **The Image Compositing Proxy (`pages/api/v1/image/[fuid].ts`):**
    ```typescript
    // pages/api/v1/image/[fuid].ts
    import type { NextApiRequest, NextApiResponse } from 'next';
    import sharp from 'sharp';
    import { S3Client, GetObjectCommand } from '@aws-sdk/client-s3';
    // Logic to fetch foreground_path and background_path from DB using fuid
    // ...

    export default async function handler(req: NextApiRequest, res: NextApiResponse) {
        // Fetch paths from DB... (Example paths)
        const foregroundPath = 'characters/alpha.png'; 
        const backgroundPath = 'backgrounds/caves.png';

        // Fetch images from Cloudflare R2 / AWS S3
        const s3 = new S3Client({ /* S3/R2 config */ });
        const background = await s3.send(new GetObjectCommand({ Bucket: process.env.R2_BUCKET_NAME, Key: backgroundPath }));
        const foreground = await s3.send(new GetObjectCommand({ Bucket: process.env.R2_BUCKET_NAME, Key: foregroundPath }));

        const bgBuffer = Buffer.from(await background.Body.transformToByteArray());
        const fgBuffer = Buffer.from(await foreground.Body.transformToByteArray());
        
        // Composite the images
        const compositeImage = await sharp(bgBuffer)
            .composite([{ input: fgBuffer, gravity: 'center' }])
            .toFormat('png')
            .toBuffer();

        res.setHeader('Content-Type', 'image/png');
        res.setHeader('Cache-Control', 'public, s-maxage=604800, stale-while-revalidate=86400'); // Cache for 1 week
        res.status(200).send(compositeImage);
    }
    ```

3.  **The Generative Forge API (`pages/api/v1/forge/mint.ts`):** This is a complex endpoint that orchestrates the mint.
    ```typescript
    // pages/api/v1/forge/mint.ts
    import { NextApiRequest, NextApiResponse } from 'next';
    import { createUmi } from '@metaplex-foundation/umi-bundle-defaults';
    import { mplBubblegum, mintToCollectionV1, createUpdateAsAuthorityInstruction } from '@metaplex-foundation/mpl-bubblegum';
    import { keypairIdentity, publicKey, some, Umi } from '@metaplex-foundation/umi';
    import { Transaction, SystemProgram, PublicKey, LAMPORTS_PER_SOL } from '@solana/web3.js';
    import { getOrCreateAssociatedTokenAccount, createTransferInstruction, TOKEN_PROGRAM_ID } from '@solana/spl-token';
    // ... logic to connect to DB, generate images, upload to R2, upload metadata to Arweave ...
    
    export default async function handler(req: NextApiRequest, res: NextApiResponse) {
        // 1. Get user choices and public key from request body
        const { userPublicKey, choices, variantSelection } = req.body;

        // 2. [Server Logic] Generate images and metadata based on user choices
        // ... returns arweaveMetadataUri, fuid, characterData ...

        // 3. [Server Logic] Store initial character data in your PostgreSQL DB
        
        // 4. Construct the transaction
        const umi = createUmi(process.env.SOLANA_RPC_URL!); // Assume Umi setup with server keypair
        
        // Mint to a temporary address first (can be burned later or used in game)
        const leafOwner = umi.identity.publicKey;
        
        const mintInstruction = mintToCollectionV1(umi, {
            leafOwner,
            merkleTree: publicKey(process.env.MERKLE_TREE_ADDRESS!),
            collectionMint: publicKey(process.env.COLLECTION_MINT_ADDRESS!),
            metadata: { /* full metadata spec */ }
        }).getInstructions()[0];
        
        // Add USDC Payment Instruction
        const mintPriceUSDC = 10 * 1_000_000; // $10 USDC (6 decimals)
        const usdcMint = new PublicKey(process.env.USDC_MINT_ADDRESS!);
        // ... getOrCreateAssociatedTokenAccount for user and treasury ...
        const usdcTransferInstruction = createTransferInstruction(/* ... */);

        // 5. Build transaction and return to client for signing
        const { blockhash } = await umi.rpc.getLatestBlockhash();
        const transaction = new Transaction({ recentBlockhash: blockhash, feePayer: new PublicKey(userPublicKey) });
        transaction.add(usdcTransferInstruction, mintInstruction);

        const serializedTx = transaction.serialize({ requireAllSignatures: false });
        res.status(200).json({ transaction: Buffer.from(serializedTx).toString('base64') });
    }
    ```

#### **Sprint 3: The Frontend Client (User Interface)**

This builds the React components to interact with your API and the user's wallet.

```tsx
// components/ForgeComponent.tsx
import { useWallet, useConnection } from '@solana/wallet-adapter-react';
import { Transaction } from '@solana/web3.js';
import { useState } from 'react';

export default function ForgeComponent() {
    const { publicKey, signTransaction } = useWallet();
    const { connection } = useConnection();

    const handleForgeMint = async () => {
        if (!publicKey || !signTransaction) return;

        // 1. Get user choices and variant selection from UI state
        
        // 2. Call the backend API to get the unsigned transaction
        const response = await fetch('/api/v1/forge/mint', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ userPublicKey: publicKey.toBase58(), /* choices */ }),
        });
        const { transaction: base64Transaction } = await response.json();

        // 3. User signs the transaction
        const transaction = Transaction.from(Buffer.from(base64Transaction, 'base64'));
        const signedTransaction = await signTransaction(transaction);

        // 4. Send to the blockchain
        const signature = await connection.sendRawTransaction(signedTransaction.serialize());

        // 5. Await confirmation and update UI
        await connection.confirmTransaction(signature, 'confirmed');
        // You might want a follow-up API call to transfer the NFT from the temp address to the user
    };

    // ... return JSX for the Forge UI ...
    return <button onClick={handleForgeMint}>Forge and Mint</button>;
}
```

#### **Sprint 4: The Economic Engine (Ritual Sacrifice)**

This defines the advanced `burn-and-mint` logic for crafting.

1.  **Frontend (`RitualChamber.tsx`):**
    *   UI allows user to select 3 Pre-Human cNFTs they own.
    *   On "Begin Ritual" click, it calls a backend endpoint `/api/v1/ritual/prepare`.
2.  **Backend (`/api/v1/ritual/prepare.ts`):**
    *   Verifies ownership of all 3 selected cNFTs.
    *   Constructs a transaction with **three Bubblegum `burn` instructions**.
    *   Returns this unsigned transaction to the client.
3.  **Frontend:**
    *   User signs the burn transaction.
    *   After the transaction is confirmed, the client takes the `signature` and calls a second endpoint: `/api/v1/ritual/complete`.
4.  **Backend (`/api/v1/ritual/complete.ts`):**
    *   **Verifies the burn:** Fetches the transaction using the `signature` from the client and parses the transaction logs to confirm the three specified cNFTs were indeed burned. This is a critical security step.
    *   **Mints the Reward:** If verification passes, it generates the metadata for the new, powerful "Anunnaki" cNFT (from a different collection/tree).
    *   It then mints this Anunnaki cNFT *directly* to the user's wallet.
    *   Returns a success message with the details of the newly created asset.

This two-step `prepare/complete` flow is essential for ensuring that the user irrevocably commits their resources (by burning them) *before* the server grants them the powerful new asset.

---

This is the architectural and developmental framework derived from your strategic decisions. It provides a focused genesis collection with immense depth through crafting, a highly engaging and personal user onboarding experience, and a robust, hybrid RPG engine built for the future. The system is designed. You may now begin construction.
