### **Instructional Protocol for Meticulous KB Synthesis**

To engage the NDE Patcher in a detailed, collaborative build process where each KB section is designed and approved sequentially, please follow this four-step procedure.

**Step 1: The Initial Build Directive**

Your first message must contain three components within a single submission:

1.  **The `build` Command:** The standard Patcher command with all known initial metadata (e.g., `target`, `name`, `version`).
2.  **The Unstructured Brief:** A block of text detailing the high-level objectives, project rationale, key features, target concepts, or any other guiding principles for the new module. This is your "design document."
3.  **The Meta-Instruction:** A concluding sentence that explicitly tells me to use this specific, controlled workflow.

**Example Template:**

```
build(target:KB, name:'your_new_kb.nde', version:'1.0.0')

[Your detailed unstructured brief goes here. Explain the project's goals, who it's for, what it should do, and any key concepts it should embody.]

Meta-Instruction: Patcher, engage the meticulous design protocol. You are to architect this KB section by section. For each section, you must first output your architectural reasoning and 'out-loud thinking' before synthesizing the code. I will approve each section individually before you proceed to the next.
```

---
**Step 2: Confirm the High-Level Plan**

In response to your directive, I will first analyze your brief and generate a high-level `// BUILD PLAN: CONFIRMATION REQUIRED //`. This plan will outline the inferred metadata (Dependencies, Hooks, etc.) and the list of sections to be built.

You must review this plan and confirm it by replying with **`Confirm`**, **`Approved`**, or a similar affirmative. This ensures we agree on the overall structure before architectural work begins.

---
**Step 3: The Iterative Design-Approve Cycle**

Once the plan is confirmed, we will enter the primary work cycle. For each section of the KB (e.g., Section I, Section II):

1.  **My Output:** I will provide two distinct blocks of text:
    *   A detailed **`[DESIGN LOG & ARCHITECTURAL RATIONALE]`**, explaining my design choices, the internal logic, and the reasoning behind the "what, why, and how" for that specific section.
    *   The corresponding **`[SYNTHESIZED CODE: SECTION X]`**, containing the clean, formatted code for that section.
2.  **Your Action:** You must review both my reasoning and the resulting code. If it meets your requirements, reply with a simple **`Approve`** or **`Approved`**.

I will not begin work on the next section until I receive your explicit approval for the previous one.

---
**Step 4: Finalization and Serialization**

After you approve the final section, I will automatically:
1.  Announce that the build process is complete.
2.  Enter **Phase 4: Serialize**.
3.  Output the entire, collated `.nde` file in a single code block.
4.  Provide the `nde.patch.install(...)` command for you to execute.
5.  Reset to my idle state, ready for a new build directive.
