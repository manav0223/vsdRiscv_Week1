# Day 4: Gate-Level Simulation (GLS), Blocking vs. Non-Blocking in Verilog, and Synthesis-Simulation Mismatch

Welcome to Day 4 of the RTL Workshop! Today’s session focuses on three essential topics in digital design:

- **Gate-Level Simulation (GLS)**
- **Blocking vs. Non-Blocking Assignments in Verilog**
- **Synthesis-Simulation Mismatch**

You’ll learn both the theory and practical implications, complete with hands-on labs to reinforce your understanding.

---

## Table of Contents
1. Gate-Level Simulation (GLS)
2. Synthesis-Simulation Mismatch
3. Blocking vs. Non-Blocking Assignments in Verilog
4. Lab work

## 1. Post-Synthesis Verification: The Final Check-Up ✅

Once your human-written Register Transfer Level (**RTL**) code is compiled into a physical **gate-level netlist** (the actual blueprint for the hardware), we run a crucial check called **Post-Synthesis Simulation**.

Think of this as the final quality control before manufacturing:

### Key Functions

- **Logic Confirmation:** It verifies that the gate-level blueprint still performs the **exact same logic** as your original RTL code.
- **Timing Insurance:** It ensures all your **timing constraints** (like speed requirements) are met, especially now that real-world gate delays have been factored in.
- **Power Reality Check:** It provides a much more **accurate estimate of power consumption** than the initial RTL simulation could.
- **Test Logic Validation:** It confirms that extra test features, like **scan chains**, have been implemented correctly for manufacturing tests.

### When Do We Run It?

This step happens **after synthesis** but primarily **before the physical layout** (placing and routing the wires on the chip). Catching bugs here saves massive time and money later!

---

## 2. Synthesis-Simulation Mismatch: The Design Discrepancy 😵‍💫

A **Synthesis-Simulation Mismatch** is a big problem: it means your original design (**RTL simulation**) behaves differently than the resulting hardware blueprint (**gate-level simulation**) or the actual chip.

### What Causes the Headache?

- **Forbidden Code:** Using Verilog constructs that synthesis tools simply can't translate, like fixed **delays** or `initial` blocks. The simulator uses them, but the synthesizer ignores them.

- **Ambiguous Instructions:** Writing code that leaves room for interpretation, like an `always` block with a missing `else` statement or an incomplete sensitivity list. The simulator might guess one way, and the synthesizer another.

- **Tool Turf War:** Sometimes different tools interpret ambiguous code slightly differently, leading to small but crucial discrepancies.

### The Key Takeaway

To avoid this mess, always write **clear, predictable, and fully synthesizable RTL code**. Good coding standards ensure your behavior is consistent across every single tool and ultimately on the hardware!

---

## 3. Blocking vs. Non-Blocking Assignments: Clocking Your Code ⏱️

In Verilog, how you assign a value is extremely important, especially when dealing with time. We have two main types of assignments:

### a. Blocking Assignment (`=`)

- **Syntax:** Use the plain equals sign (`=`).
- **Execution:** It's executed **sequentially and immediately**. Think of it as a **to-do list**—one task must finish before the next one starts.
- **Best For:** **Combinational logic** (like basic math or logic gates) and for **temporary variables** within a block, especially in `always @(*)` blocks.


### b. Non-Blocking Assignment (`<=`)

- **Syntax:** Use the less-than-or-equal-to symbol (`<=`).
- **Execution:** Execution is **scheduled to happen all at once** at the very end of the current time step. It executes **concurrently**. Think of it as **taking a snapshot** of all values at the same moment.
- **Best For:** **Sequential logic** (anything with memory, like flip-flops or registers), and it should always be used in your **clock-triggered blocks** (e.g., `always @(posedge clk)`). This ensures all registers update simultaneously, mimicking real hardware behavior.

### Quick Reference Guide

| Assignment Type | Symbol | Use Case | Execution Style |
|----------------|--------|----------|----------------|
| Blocking | `=` | Combinational Logic | Sequential/Immediate |
| Non-Blocking | `<=` | Sequential Logic | Concurrent/Scheduled |

### Best Practice Rules

1. **Use blocking (`=`) for combinational logic** in `always @(*)` blocks
2. **Use non-blocking (`<=`) for sequential logic** in `always @(posedge clk)` blocks
3. **Never mix both types** in the same `always` block
4. **Think hardware-first:** Non-blocking assignments better represent how real flip-flops update simultaneously on a clock edge

## Lab Work:
#### Verilog code for a 2:1 multiplexer using a ternary operatoris given below:

```
module ternary_operator_mux (input i0, input i1, input sel, output y);
  assign y = sel ? i1 : i0;
endmodule
```
We used the function : y = i1 if sel = 1; else y = i0.

#### Simulated the rtl_design of ternary_operator_mux.v and got waveforms in the gtkwave :
<img width="1854" height="573" alt="GTKWave Ternary_Operator" src="https://github.com/user-attachments/assets/ef33b725-b6da-4fa2-b6df-e300f447929c" />

#### Synthesizing in Yosys
<img width="1854" height="871" alt="Show ternary operator mux" src="https://github.com/user-attachments/assets/b1b1896c-684f-4f78-bcff-31877139f4d6" />

#### Creating a Netlist for GLS:
<img width="1846" height="585" alt="Netlist ternary_operator_mux" src="https://github.com/user-attachments/assets/d01f90db-6b8c-4cab-ac7f-c31cd71397dc" />

#### Synthesis after GLS
<img width="1846" height="882" alt="GLS Ternary_Operator_MUX" src="https://github.com/user-attachments/assets/40ed52d5-eb65-4f3b-9454-985233ccda0f" />

#### Bad MUX Example (Common Pitfalls)
Let's take a code with mistakes

```
module bad_mux (input i0, input i1, input sel, output reg y);
  always @ (sel) begin
    if (sel)
      y <= i1;
    else 
      y <= i0;
  end
endmodule
```
##### Major issues:
Incomplete sensitivity list: Should include i0, i1, and sel.
Non-blocking assignment in combinational logic: Should use blocking assignments (=).

##### The correct code is :
```
always @ (*) begin
  if (sel)
    y = i1;
  else
    y = i0;
end
```

#### Simulation of bad_mux :
<img width="1846" height="573" alt="bad_mux GTKWave" src="https://github.com/user-attachments/assets/53ea8a58-3fd2-4bfa-bf37-cdaa72d592f5" />

##### Performing GLS for bad_mux :

<img width="1846" height="868" alt="GLS Bad_MUX" src="https://github.com/user-attachments/assets/429e8a91-8652-46c8-b8ea-5718175e2798" />
<img width="1846" height="587" alt="bad_mux netlist" src="https://github.com/user-attachments/assets/a8c50c53-cc36-49bf-8509-9844fd991c71" />

#### Blocking Assignment Caveat

Verilog code of blocking_caveat :

```
module blocking_caveat (input a, input b, input c, output reg d);
  reg x;
  always @ (*) begin
    d = x & c;
    x = a | b;
  end<img width="1300" height="736" alt="day4_block_gtkwave" src="https://github.com/user-attachments/assets/26061217-eb61-4585-b8b7-568f28369ff8" />

endmodule
```
#### Fault in the code :
The order of assignments causes d to use the old value of x—not the newly computed value.
To avoid this assign intermediate variables before using them.

#### Correct Code :
```
always @ (*) begin
  x = a | b;
  d = x & c;
end
```
##### Simulation of blocking_caveat :
<img width="1854" height="589" alt="Blocking Caevat GTKWave" src="https://github.com/user-attachments/assets/a8e4c067-c181-4250-b9b6-1621297aaa0a" />

#### Synthesis of blocking_caveat :
<img width="1854" height="883" alt="Blocking Caevat Show" src="https://github.com/user-attachments/assets/b25e287b-8f56-4766-b030-82e754f4783a" />


