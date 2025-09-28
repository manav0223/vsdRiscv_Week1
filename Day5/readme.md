v align="center">
 
# Week 1 : Day 5
# Optimization in Synthesis

</div>

<div align="center">
 
[![RISC-V](https://img.shields.io/badge/RISC--V-SoC%20Tapeout-blue?style=for-the-badge&logo=riscv)](https://riscv.org/)
[![VSD](https://img.shields.io/badge/VSD-Program-orange?style=for-the-badge)](https://vsdiat.vlsisystemdesign.com/)
![Week](https://img.shields.io/badge/Week-1-green?style=for-the-badge)

</div>

## Table of Content :
- 1. If-Else Statements in Verilog
- 2. Inferred Latches in Verilog
- 3. Labs for If-Else and Case Statements
  - Lab 1: Incomplete If Statement
  - Lab 2: Synthesis Result of Lab 1
  - Lab 3: Nested If-Else
  - Lab 4: Synthesis Result of Lab 3
  - Lab 5: Complete Case Statement
  - Lab 6: Synthesis Result of Lab 5
  - Lab 7: Incomplete Case Handling
  - Lab 8: Partial Assignments in Case
- 4. For Loops in Verilog
- 5. Generate Blocks in Verilog
- 6. What is an RCA (Ripple Carry Adder)?
- 7. Labs on Loops and Generate Blocks
  - Lab 9: 4-to-1 MUX Using For Loop
  - Lab 10: 8-to-1 Demux Using Case
  - Lab 11: 8-to-1 Demux Using For Loop
  - Lab 12: 8-bit Ripple Carry Adder with Generate Block
 
## 1. If-Else Statements in Verilog

**If-else statements** are used for conditional execution in behavioral modeling, typically within procedural blocks (`always`, `initial`, tasks, or functions).

### Syntax

```verilog
if (condition) begin
    // Code block executed if condition is true
end else begin
    // Code block executed if condition is false
end
```

- **condition**: An expression evaluating to true (non-zero) or false (zero).
- **begin ... end**: Used to group multiple statements. Omit if only one statement is present.
- The `else` part is optional.

#### Nested If-Else

```verilog
if (condition1) begin
    // Code for condition1 true
end else if (condition2) begin
    // Code for condition2 true
end else begin
    // Code if no conditions are true
end
```

---

## 2. Inferred Latches in Verilog

**Inferred latches** occur when a combinational logic block does not assign a value to a variable in all possible execution paths. This causes the synthesis tool to infer a latch, which may not be the designer’s intention.

### Example of Latch Inference

```verilog
module ex (
    input wire a, b, sel,
    output reg y
);
    always @(a, b, sel) begin
        if (sel == 1'b1)
            y = a; // No 'else' - y is not assigned when sel == 0
    end
endmodule
```

**Problem**: When `sel` is 0, `y` is not assigned, so a latch is inferred.

#### Solution: Add Else or Default Case

```verilog
module ex (
    input wire a, b, sel,
    output reg y
);
    always @(a, b, sel) begin
        case(sel)
            1'b1 : y = a;
            default : y = 1'b0; // Default assignment
        endcase
    end
endmodule
```

---

## 3. Labs for If-Else and Case Statements

### Lab 1: Incomplete If Statement

```verilog
module incomp_if (input i0, input i1, input i2, output reg y);
always @(*) begin
    if (i0)
        y <= i1;
end
endmodule
```
<img width="1854" height="595" alt="GTKWave incomp_case" src="https://github.com/user-attachments/assets/8cc17440-c9da-48c8-a85b-e4c498ab8cfd" />


### Lab 2: Synthesis Result of Lab 1

<img width="1854" height="459" alt="Show Incomp_case" src="https://github.com/user-attachments/assets/7ef9013b-aa84-4b60-996c-f4e704ab9806" />


### Lab 3: Nested If-Else

```verilog
module incomp_if2 (input i0, input i1, input i2, input i3, output reg y);
always @(*) begin
    if (i0)
        y <= i1;
    else if (i2)
        y <= i3;
end
endmodule
```
<img width="1854" height="598" alt="GTKWave incomp_if2" src="https://github.com/user-attachments/assets/6e7cdfc8-b019-4df3-9e81-3e4185d8f9b4" />

### Lab 4: Synthesis Result of Lab 3
<img width="1854" height="550" alt="Show incomp_if2" src="https://github.com/user-attachments/assets/7069f1ec-a470-4aee-a83a-4e1671f63667" />


### Lab 5: Complete Case Statement

```verilog
module comp_case (input i0, input i1, input i2, input [1:0] sel, output reg y);
always @(*) begin
    case(sel)
        2'b00 : y = i0;
        2'b01 : y = i1;
        default : y = i2;
    endcase
end
endmodule
```
<img width="1854" height="598" alt="GTKWave comp_case" src="https://github.com/user-attachments/assets/5f6a562c-ec9b-426c-a219-1b9807e8818e" />


### Lab 6: Synthesis Result of Lab 5
<img width="1854" height="490" alt="Show comp_case" src="https://github.com/user-attachments/assets/cd5af47a-37eb-46ef-bc9b-dfed7332cfc8" />


### Lab 7: Incomplete Case Handling

```verilog
module bad_case (
    input i0, input i1, input i2, input i3,
    input [1:0] sel,
    output reg y
);
always @(*) begin
    case(sel)
        2'b00: y = i0;
        2'b01: y = i1;
        2'b10: y = i2;
        2'b1?: y = i3; // '?' is a wildcard; be careful with incomplete cases!
    endcase
end
endmodule
```
<img width="1854" height="615" alt="GTKWave bad_case" src="https://github.com/user-attachments/assets/f45e5c96-6369-4352-a378-e48e44e89c7f" />

<img width="1854" height="974" alt="Show bad_case" src="https://github.com/user-attachments/assets/98b9473f-e582-47dc-9a21-a909abb53121" />

### Lab 8: Partial Assignments in Case

```verilog
module partial_case_assign (
    input i0, input i1, input i2,
    input [1:0] sel,
    output reg y, output reg x
);
always @(*) begin
    case(sel)
        2'b00: begin
            y = i0;
            x = i2;
        end
        2'b01: y = i1;
        default: begin
            x = i1;
            y = i2;
        end
    endcase
end
endmodule
```
<img width="1861" height="876" alt="Show partial_case_assign" src="https://github.com/user-attachments/assets/8867e8c7-fb5f-488e-83bf-06d6e9cff867" />
<img width="1861" height="611" alt="GTKWave partial_case_assign" src="https://github.com/user-attachments/assets/a913226f-0fa4-4a60-96da-f303fc0c50fd" />


## 4. For Loops in Verilog

A **for loop** is used within procedural blocks (`initial`, `always`, tasks/functions) to execute statements multiple times based on a loop counter.

### Syntax  

```verilog
for (initialization; condition; increment) begin
    // Statements to execute
end
```

- Must be inside procedural blocks.
- Synthesizable only if the number of iterations is fixed at compile time.

#### Example: 4-to-1 MUX Using a For Loop

```verilog
module mux_4to1_for_loop (
    input wire [3:0] data, // 4 input lines
    input wire [1:0] sel,  // 2-bit select
    output reg y           // Output
);
    integer i;
    always @(data, sel) begin
        y = 1'b0; // Default output
        for (i = 0; i < 4; i = i + 1) begin
            if (i == sel)
                y = data[i];
        end
    end
endmodule
```

---

## 5. Generate Blocks in Verilog

A **generate block** is used to create hardware structures such as module instances or logic at compile time. Typically used with `for` loops and the `genvar` keyword.

#### Example

```verilog
genvar i;
generate
    for (i = 0; i < 4; i = i + 1) begin : gen_loop
        and_gate and_inst (.a(in[i]), .b(in[i+1]), .y(out[i]));
    end
endgenerate
```

## 7. Labs on Loops and Generate Blocks

### Lab 9: 4-to-1 MUX Using For Loop

```verilog
module mux_generate (
    input i0, input i1, input i2, input i3,
    input [1:0] sel,
    output reg y
);
wire [3:0] i_int;
assign i_int = {i3, i2, i1, i0};
integer k;
always @(*) begin
    for (k = 0; k < 4; k = k + 1) begin
        if (k == sel)
            y = i_int[k];
    end
end
endmodule
```
<img width="1847" height="1061" alt="Show 4x1 MUX" src="https://github.com/user-attachments/assets/ff0b7ca0-7c41-4823-9142-c490473a22a8" />
<img width="1847" height="666" alt="GTKWave Mux Gen" src="https://github.com/user-attachments/assets/9e1e223e-68df-4415-893b-b15a520561cf" />


### Lab 10: 8-to-1 Demux Using Case

```verilog
module demux_case (
    output o0, output o1, output o2, output o3,
    output o4, output o5, output o6, output o7,
    input [2:0] sel,
    input i
);
reg [7:0] y_int;
assign {o7, o6, o5, o4, o3, o2, o1, o0} = y_int;
always @(*) begin
    y_int = 8'b0;
    case(sel)
        3'b000 : y_int[0] = i;
        3'b001 : y_int[1] = i;
        3'b010 : y_int[2] = i;
        3'b011 : y_int[3] = i;
        3'b100 : y_int[4] = i;
        3'b101 : y_int[5] = i;
        3'b110 : y_int[6] = i;
        3'b111 : y_int[7] = i;
    endcase
end
endmodule
```
<img width="952" height="1076" alt="Show_Demux_Case" src="https://github.com/user-attachments/assets/ef27e038-e507-4c41-90da-f63bb26f49d2" />
<img width="1846" height="745" alt="GTKWave Demux_Case" src="https://github.com/user-attachments/assets/a0de6aa8-998c-44fe-afbc-566bf4bcc9a2" />


### Lab 11: 8-to-1 Demux Using For Loop

```verilog
module demux_generate (
    output o0, output o1, output o2, output o3,
    output o4, output o5, output o6, output o7,
    input [2:0] sel,
    input i
);
reg [7:0] y_int;
assign {o7, o6, o5, o4, o3, o2, o1, o0} = y_int;
integer k;
always @(*) begin
    y_int = 8'b0;
    for (k = 0; k < 8; k = k + 1) begin
        if (k == sel)
            y_int[k] = i;
    end
end
endmodule
```
<img width="1023" height="1079" alt="Show 1x8 Demux" src="https://github.com/user-attachments/assets/7d0c7c7a-472e-4a95-a904-6602bd410c2a" />
<img width="1847" height="760" alt="GTKWave Demux Gen" src="https://github.com/user-attachments/assets/5659a08f-f58b-4eda-936b-f723bda287e4" />



### Lab 12: 8-bit Ripple Carry Adder with Generate Block

```verilog
module rca (
    input [7:0] num1,
    input [7:0] num2,
    output [8:0] sum
);
wire [7:0] int_sum;
wire [7:0] int_co;

genvar i;
generate
    for (i = 1; i < 8; i = i + 1) begin
        fa u_fa_1 (.a(num1[i]), .b(num2[i]), .c(int_co[i-1]), .co(int_co[i]), .sum(int_sum[i]));
    end
endgenerate

fa u_fa_0 (.a(num1[0]), .b(num2[0]), .c(1'b0), .co(int_co[0]), .sum(int_sum[0]));

assign sum[7:0] = int_sum;
assign sum[8] = int_co[7];
endmodule
```
**Full Adder Module:**
```verilog
module fa (input a, input b, input c, output co, output sum);
    assign {co, sum} = a + b + c;
endmodule
```
<img width="1300" height="736" alt="rca v GTKWave" src="https://github.com/user-attachments/assets/3c1a6944-f420-4185-bdd5-3ee8a1af3f61" />

