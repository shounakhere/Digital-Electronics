# GenAI Prompts & Engineering Transcripts
## Digital Electronics Laboratory (ESC392)
**Institute of Engineering & Management (IEM), Kolkata**  
**Student Name:** SUBHRAJIT GHOSH | **Roll No.:** 491 | **Section:** G | **Semester:** 3rd Semester B.Tech.

---

## Overview

This document archives the complete collection of prompt engineering workflows, system instructions, verification prompts, and human-in-the-loop critical analyses utilized across the **ESC392 Digital Electronics Laboratory** assignments. 

The lab combines Generative AI reasoning with EDA simulation tools (**Logisim Evolution**, **CircuitVerse**) to design, verify, and implement combinational and sequential digital circuits.

---

## Assignment 1: AI-Assisted Boolean Expression Simplifier

### Objective
Simplify arbitrary Boolean algebraic expressions into minimal Sum-of-Products (SOP) and Product-of-Sums (POS) forms using tabular minimization, Karnaugh Mapping (K-Map), and verify via Verilog testbenches.

### Prompt Specification
```text
Role: You are an expert Digital Electronics EDA and Logic Synthesis Engineer.
Task: Minimize the following 4-variable Boolean expression into canonical and minimal SOP and POS representations:
      F(A, B, C, D) = \Sigma m(0, 2, 5, 7, 8, 10, 13, 15)

Deliverables:
1. Provide the complete 16-row Truth Table.
2. Group the minterms using Quine-McCluskey (Tabular Method) and Karnaugh Mapping (K-Map).
3. Identify all Prime Implicants (PI) and Essential Prime Implicants (EPI).
4. State the minimal Boolean equation using standard AND/OR/NOT gates and universal NAND gates.
5. Provide a synthesizable Verilog module and testbench to verify functional equivalence.
```

### AI Output Summary
- **Truth Table:** Output is logic 1 for minterms 0, 2, 5, 7, 8, 10, 13, 15.
- **K-Map Grouping:**
  - Group 1: Corners $(m_0, m_2, m_8, m_{10}) \implies \bar{B}\bar{D}$
  - Group 2: Center $(m_5, m_7, m_{13}, m_{15}) \implies BD$
- **Minimal SOP Equation:**
  $$F(A, B, C, D) = \bar{B}\bar{D} + BD = (B \odot D)$$
- **Hardware Realization:** Implemented using standard gates and converted to 4 two-input NAND gates.

### Critical Engineering Reflection
- **GenAI Strength:** Rapid identification of symmetrical groupings in K-Maps.
- **Human Correction:** Ensured hazard-free transitions by checking static-1 hazards and verified fan-in constraints on universal NAND realizations.

---

## Assignment 2: Natural Language to Logic Circuit Synthesis

### Objective
Translate complex real-world behavioral specifications expressed in natural language into rigorous Boolean logic equations, truth tables, and gate-level schematics.

### Prompt Specification
```text
Role: Digital System Architect.
Task: Design an automated smart industrial chemical plant safety interlocking system based on the following natural language specifications:
- The system monitors four binary sensors: Temperature High (T), Pressure High (P), Coolant Flow Low (C), and Operator Manual Override (M).
- Emergency Alarm (A) must activate IF and ONLY IF:
  1. Both Temperature and Pressure are High simultaneously, UNLESS Coolant Flow is Normal (C=0) AND Manual Override is Active (M=1).
  2. Coolant Flow is Low (C=1) AND either Temperature or Pressure is High.
  3. Operator engages Emergency Override regardless of other conditions.

Deliverables:
1. Formalize truth table for inputs T, P, C, M and output A.
2. Derive minimized Boolean equation using Boolean algebra theorems.
3. Generate schematic netlist for Logisim / CircuitVerse simulation.
```

### AI Output Summary
- **Canonical SOP:** Reduced to $A = M + C(T + P) + TP\bar{C}$.
- **Hardware Optimization:** Factored using distributive laws to minimize total gate count and propagation delay:
  $$A = M + C(T + P) + TP$$

### Critical Engineering Reflection
- **GenAI Strength:** Accurate parsing of conditional clauses ("UNLESS", "EITHER... OR").
- **Human Correction:** Disambiguated overlapping priority rules between emergency override and sensor fault flags.

---

## Assignment 3: Traffic Light Controller Finite State Machine (FSM)

### Objective
Design, model, and simulate a Moore Finite State Machine (FSM) for a 4-way crossroad traffic intersection with pedestrian crossing capability.

### Prompt Specification
```text
Role: Senior VLSI and Embedded Systems Engineer.
Task: Synthesize a synchronous 4-phase Moore Finite State Machine for an intersection Traffic Light Controller (TLC):
- Lanes: North-South (NS) and East-West (EW).
- States:
  - S0: NS Green, EW Red (Duration: 30s)
  - S1: NS Yellow, EW Red (Duration: 5s)
  - S2: NS Red, EW Green (Duration: 30s)
  - S3: NS Red, EW Yellow (Duration: 5s)
- Pedestrian Request Input (P): When P=1, transitions yellow states early to service pedestrian crossing cycle.

Deliverables:
1. Complete State Transition Table (Present State, Inputs, Next State, Flip-Flop Excitations).
2. D Flip-Flop excitation equations ($D_1, D_0$) derived via K-Maps.
3. Output logic equations for $\{G_{NS}, Y_{NS}, R_{NS}, G_{EW}, Y_{EW}, R_{EW}\}$.
4. Complete Logisim schematic wiring diagram.
```

### AI Output Summary
- **State Assignment:** $S_0=00, S_1=01, S_2=10, S_3=11$.
- **Excitation Logic:**
  - $D_1 = Q_1 \oplus Q_0$
  - $D_0 = \bar{Q}_0$
- **Output Logic:** Decoded directly from state bits with active-high LED drivers.

### Critical Engineering Reflection
- **GenAI Strength:** Correct excitation mapping for standard Gray and binary counters.
- **Human Correction:** Added synchronous reset and unused state recovery circuitry ($Q_1 Q_0 \notin \{00, 01, 10, 11\}$ in expanded 3-bit architectures) to prevent deadlocks.

---

## Assignment 4: Fault Detection in Digital Circuits

### Objective
Implement classical fault detection algorithms (D-Algorithm, Path Sensitization) to detect stuck-at-0 (s-a-0) and stuck-at-1 (s-a-1) faults in combinational networks.

### Prompt Specification
```text
Role: DFT (Design for Testability) and Fault Diagnosis Engineer.
Task: Perform single stuck-at fault analysis on the logic circuit implementing $Y = (A \cdot B) + (C \cdot \bar{D})$:
1. Identify all fault locations (internal net branches and primary I/O).
2. Select a target fault: Internal net W1 (output of AND gate A*B) stuck-at-0 ($W_1 \text{ s-a-0}$).
3. Use the D-Algorithm / Path Sensitization to find the complete test vector(s):
   - Justify the fault site ($D = 1$).
   - Sensitize the path to the primary output through the OR gate.
4. Construct a Fault Dictionary / Test Vector Matrix covering all single stuck-at faults.
```

### AI Output Summary
- **Target Fault:** $W_1 \text{ s-a-0}$.
- **Fault Activation:** Set $A=1, B=1 \implies W_1 = 1$, generating difference $D$.
- **Path Propagation:** To propagate $D$ through the OR gate, second input $W_2 = C\bar{D}$ must be set to 0.
- **Test Vector:** $ABCD = 1100, 1101, 1111$.

### Critical Engineering Reflection
- **GenAI Strength:** Step-by-step logic sensitization logic propagation.
- **Human Correction:** Eliminated redundant vectors using minimal set-covering algorithms.

---

## Assignment 5: Digital Clock Finite State Machine

### Objective
Design and simulate a digital 24-hour clock timebase system utilizing cascaded Mod-60 counters (seconds, minutes) and a Mod-24 counter (hours) with 7-segment display decoders.

### Prompt Specification
```text
Role: Synchronous Sequential Circuit Designer.
Task: Design the complete digital architecture for a 24-hour digital clock:
- Stages:
  1. 1 Hz clock generator / prescaler from a 60 Hz AC line or 32.768 kHz crystal oscillator.
  2. Mod-60 Seconds Counter (BCD Mod-10 cascaded with Mod-6).
  3. Mod-60 Minutes Counter with carry enable from Seconds stage.
  4. Mod-24 Hours Counter (counting 00 to 23) with synchronous auto-reset on reaching 24.
- Interface with BCD-to-7-segment decoders (IC 7447 / 7448).

Deliverables:
1. Block diagram architecture showing carry propagation.
2. State transition logic and asynchronous vs. synchronous reset comparison.
3. Logisim Evolution implementation schematic.
```

### AI Output Summary
- **Cascaded Architecture:** Each Mod-60 stage uses a 4-bit 74LS90 / D-FF decade counter (units) and a 3-bit Mod-6 counter (tens).
- **Reset Logic:** Detects state 6 ($0110_2$) on the tens counter via an AND gate to trigger carry-out and synchronous reset.

### Critical Engineering Reflection
- **GenAI Strength:** Hierarchical clock-divider breakdown.
- **Human Correction:** Addressed glitching on asynchronous reset lines by converting reset condition to synchronous clock-enable gates.

---

## Assignment 6: Application Mapping of Sequential Building Blocks

### Objective
Study real-world application mapping of three fundamental sequential building blocks: Flip-Flops, Mod-60 Counters, and Shift Registers.

### Prompt Specification
```text
Role: Senior Hardware Engineer & Computer Architect.
Task: Analyze three sequential building blocks and demonstrate their practical engineering applications:
1. D Flip-Flop as a 1-Bit Memory Cell in CPU register files.
2. Mod-60 Counter in digital clock timebases and frequency dividers.
3. 4-Bit Universal Shift Register in Serial-to-Parallel (UART) communication systems.

Deliverables:
- Detailed operational principle and architectural justification.
- Complete truth tables and excitation equations.
- Logisim circuit schematics and simulation waveforms.
```

### ChatGPT Session Link
- [ChatGPT Verification Transcript (Assignment 6)](https://chatgpt.com/share/6ae4a267-d7dc-83ec-b41a-197daf12c7ec)

### Key Circuit Artifacts
- `circuits/a6_flipflop_circuit.jpg`: 1-bit memory cell with MUX feedback loop.
- `circuits/a6_mod60_circuit.jpg`: Mod-60 counter with dual 7-segment displays.
- `circuits/a6_shift_register_circuit.jpg`: 4-bit universal shift register with mode selection.

---

## Assignment 7: Mini-Automation System (Parking Barrier Controller)

### Objective
Design and simulate an automated parking barrier control system using Moore/Mealy FSM and digital motor relay control logic.

### Prompt Specification
```text
Role: Automation Control and Digital FSM Engineer.
Task: Design a complete parking lot access barrier control system:
- Inputs: Entry sensor (A), Exit sensor (B), Barrier Up limit switch (U), Barrier Down limit switch (D).
- Outputs: Motor Raise (M_UP), Motor Lower (M_DN), Red Status LED, Green Status LED.
- Sequence:
  - IDLE: Barrier DOWN, Red LED ON.
  - CAR APPROACH (A=1): Raise barrier (M_UP=1) until limit U=1.
  - PASSING: Barrier stays UP, Green LED ON.
  - CAR CLEARED (B=1): Lower barrier (M_DN=1) until limit D=1.

Deliverables:
1. State transition diagram and state table using D Flip-Flops.
2. K-Map minimization for Next-State logic and motor outputs.
3. Complete Logisim Evolution schematic.
```

### ChatGPT Session Link
- [ChatGPT Verification Transcript (Assignment 7)](https://chatgpt.com/share/6ae4a8b7-afb0-83ee-b6cb-1e2961fbfb67)

### Key Circuit Artifacts
- `circuits/a7_parking_barrier_circuit.jpg`: Logisim Evolution schematic featuring Next-State logic, D Flip-Flops, LED decoders, and motor relay driver outputs.

---

## Assignment 8: 4-Bit ALU & Synchronous Program Counter

### Objective
Design and simulate a 4-bit Arithmetic Logic Unit (ALU) with register storage datapath and a 4-bit synchronous Program Counter (PC).

### Prompt Specification
```text
Role: Computer Organization & Processor Design Specialist.
Task: Build the execution and fetch unit datapath for a 4-bit educational microprocessor:
1. 4-Bit ALU:
   - Inputs: 4-bit Operand A, 4-bit Operand B, 2-bit Operation Select (S1, S0), Mode bit (M).
   - Functions: ADD (with Carry), SUBTRACT (2's complement), Bitwise AND, Bitwise OR, Bitwise XOR, Pass A.
   - Status Flags: Zero (Z), Carry-Out (Cout), Overflow (V).
2. 4-Bit Synchronous Program Counter (PC):
   - Features: Sequential Increment (+1), Parallel Load (Branching), Synchronous Reset.
   - Built using four D Flip-Flops (A, B, C, D) with synchronized clock pulse.

Deliverables:
- Gate-level datapath schematics in Logisim Evolution.
- Comprehensive trace table verifying arithmetic and logical operations.
```

### Key Circuit Artifacts
- `circuits/a8_alu_datapath_circuit.jpg`: 4-bit register file and ALU execution datapath.
- `circuits/a8_program_counter_circuit.jpg`: 4-bit synchronous up-counter with reset.

---

## Assignment 9: Code-to-Gate Explainer (Software to Hardware Synthesis)

### Objective
Map a high-level software conditional branch (`if-else`) into gate-level combinational digital hardware.

### High-Level Code
```c
if (A > B) {
    C = A;
} else {
    C = B;
}
```

### Prompt Specification
```text
Role: Logic Synthesis & Hardware Compilation Engineer.
Task: Synthesize the given C code conditional statement into gate-level digital hardware for 2-bit unsigned inputs A (A1 A0) and B (B1 B0):
1. Identify the architectural datapath components: Relational Comparator vs. Steering Multiplexer.
2. Express the 2-bit comparator logic equation for condition (A > B).
3. Connect the comparator output as the select line (Sel) of two 2:1 multiplexers to generate 2-bit output C (C1 C0).
4. Verify the complete 16-row Truth Table.
5. Provide critical evaluation: What does AI identify correctly, and what real-world hardware constraints require human engineering intervention?
```

### ChatGPT Session Link
- [ChatGPT Verification Transcript (Assignment 9)](https://chatgpt.com/share/6ae6d927-5c3c-83ee-86f5-664cb06aa2d0)

### AI Output Summary
- **Comparator Logic:**
  $$A > B = A_1 \bar{B}_1 + (A_1 \odot B_1) A_0 \bar{B}_0$$
- **Multiplexer Logic:**
  - $C_1 = \text{Sel} \cdot A_1 + \overline{\text{Sel}} \cdot B_1$
  - $C_0 = \text{Sel} \cdot A_0 + \overline{\text{Sel}} \cdot B_0$
- **Verification:** Tested across all 16 permutations ($A, B \in \{00, 01, 10, 11\}$), confirming $C = \max(A, B)$.

### Key Circuit Artifacts
- `circuits/a9_mux_code_to_gate_circuit.jpg`: Gate-level 2:1 multiplexer circuit in Logisim.
- `circuits/a9_truth_table.jpg`: 16-row simulation truth table.

---

## Summary of Human-in-the-Loop AI Engineering Best Practices

1. **Explicit Constraint Provision:** AI logic synthesizers perform best when exact variable naming, active-high/active-low polarity, and target gate families are specified upfront.
2. **Hazard & Timing Verification:** AI models frequently overlook dynamic hazards, race conditions, and propagation skew. Human designers must verify setup/hold times and insert synchronizing registers.
3. **Simulation-Backed Validation:** Always cross-verify AI-generated truth tables and excitation equations against deterministic simulation tools (**Logisim Evolution**, **ModelSim**, **Icarus Verilog**).
