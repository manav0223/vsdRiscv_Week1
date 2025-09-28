<div align="center">
 
# Week 1 : Day 3
# Combinational and Sequential Optimization

</div>

<div align="center">
 
[![RISC-V](https://img.shields.io/badge/RISC--V-SoC%20Tapeout-blue?style=for-the-badge&logo=riscv)](https://riscv.org/)
[![VSD](https://img.shields.io/badge/VSD-Program-orange?style=for-the-badge)](https://vsdiat.vlsisystemdesign.com/)
![Week](https://img.shields.io/badge/Week-1-green?style=for-the-badge)

</div>

## Table of Content :
1. introduction to optimizations
2. Combinational logic optimizations
3. Sequential logic optimizations
4.Sequential optimizations for unused outputs

### 1. introduction to optimizations
Optimization in synthesis makes designs smaller, faster, and more power-efficient by simplifying logic, removing unused parts, and applying techniques like retiming and state optimization. After synthesis, Gate-Level Simulation checks that the netlist still behaves correctly and meets timing. Most mismatches between RTL and GLS come from coding issues like incomplete sensitivity lists or misuse of blocking vs. non-blocking assignments, so following proper Verilog practices is key for reliable results.

### 2. Combinational logic optimizations
Combinational logic optimization focuses on simplifying circuits without altering their function by eliminating redundant gates, propagating constants, and merging equivalent logic paths. Using fundamental gates such as AND, OR, and NOT, we optimized the design through careful simulation and synthesis of the RTL description. By executing precise terminal commands, this process achieves a more compact, efficient, and higher-performance hardware implementation suitable for practical applications.

<img width="1855" height="873" alt="show opt_check4" src="https://github.com/user-attachments/assets/35efdc37-c745-4b44-be1c-9690f4853ec8" />
<img width="1855" height="873" alt="show opt_check3" src="https://github.com/user-attachments/assets/572f9328-a89d-4f8d-9c66-b4374e1ba021" />
<img width="1854" height="798" alt="Show opt_check2" src="https://github.com/user-attachments/assets/351ad3d5-0d79-4948-8219-7374aa655130" />
<img width="1854" height="788" alt="Show opt_check" src="https://github.com/user-attachments/assets/49916cd1-7257-4b42-933f-88e5305b1c99" />
<img width="1855" height="578" alt="GTKWave Opt_check3" src="https://github.com/user-attachments/assets/be6d6bd7-b465-40db-87e5-821319e01e9b" />
<img width="1115" height="545" alt="GTKWave Opt_Check2" src="https://github.com/user-attachments/assets/827b116c-d186-424f-9c48-39e827ef1d52" />
<img width="1854" height="550" alt="GTKWave Opt_check" src="https://github.com/user-attachments/assets/0c29cbc0-29a1-445a-a3d9-e480779c7309" />
<img width="1845" height="548" alt="GTKWave dff_const1" src="https://github.com/user-attachments/assets/c3050c25-4e88-4b7d-a0c9-68c679a603f7" />

### 3. Sequential Logic Optimizations
Sequential logic optimization plays a crucial role in enhancing the performance and efficiency of circuits that rely on memory elements such as flip-flops and registers. This process includes techniques like state minimization, which reduces the number of states the circuit must handle, simplifying the control logic and cutting down on unnecessary complexity. Retiming is another powerful method, where registers are repositioned within the circuit to balance timing delays and improve the achievable clock speed. Additionally, merging redundant or equivalent states streamlines the design further, reducing resource usage. Together, these optimizations lead to significant improvements in chip area, power consumption, and operating speed, resulting in a more efficient, reliable, and high-performance sequential hardware implementation suitable for demanding applications.

#### Simulated Outputs:
<img width="1573" height="436" alt="GTKWave dff_const5" src="https://github.com/user-attachments/assets/08dd54d3-7fd6-4bf8-b19c-e0d9fa20e002" />
<img width="1853" height="544" alt="GTKWave dff_const4" src="https://github.com/user-attachments/assets/26ac1df7-e854-4b2b-9fbf-03b259f68387" />
<img width="1853" height="574" alt="GTKWave dff_const3" src="https://github.com/user-attachments/assets/29dc2791-6073-4cbe-bc8f-42362418e730" />
<img width="1853" height="541" alt="gtkwave dff_const2" src="https://github.com/user-attachments/assets/f4d4df6a-1191-4e68-85ec-21eba911ec58" />
<img width="1845" height="548" alt="GTKWave dff_const1" src="https://github.com/user-attachments/assets/c007099b-6538-4fb9-82a1-a7937c47836c" />

#### Synthesis on Yosys:
<img width="1853" height="537" alt="dff_const1 show" src="https://github.com/user-attachments/assets/d6b09de0-b612-4d85-ae22-f99d454445e1" />
<img width="1853" height="748" alt="show dff_const2" src="https://github.com/user-attachments/assets/26d6749c-6c64-4b5a-8c53-6d6d20e8b05c" />
<img width="1853" height="1072" alt="Show dff_const3" src="https://github.com/user-attachments/assets/33dcb914-b19e-434e-abd4-1d6bfc198114" />
<img width="947" height="1072" alt="show dff_const4" src="https://github.com/user-attachments/assets/208fc0c2-ffda-4f5a-8c7d-9c4634bced2b" />
<img width="1857" height="723" alt="show dff_const5" src="https://github.com/user-attachments/assets/371b1395-841f-4f2a-925c-ae53d705d7fb" />

### 4. Sequential Optimizations for unused outputs:
Synthesis tools are smart—they basically clean up your design for you! If you have sequential logic (like a flip-flop) driving an output that's never actually connected to anything else, the tool recognizes this as dead logic and just throws it out. Removing these unused paths is great because it instantly slashes the number of registers you need, simplifies the overall circuit structure (reducing logic depth), saves power, and makes the synthesis process run faster with better timing!

Today, we saw this in action by optimizing a counter register design. We took the counter's code (RTL), simulated it to check the original function, and then synthesized it. By using the right terminal commands, we got to observe how the tool automatically made the counter design more efficient!

<img width="1857" height="723" alt="show counter_opt" src="https://github.com/user-attachments/assets/41e4ca4c-b5f9-4200-894d-759481f9b541" />

#### Flattening of Multiple_Module_Opt2 
<img width="1850" height="726" alt="show mutiple_module_opt2_flatten" src="https://github.com/user-attachments/assets/e1f940c7-cb04-421d-99b7-1ce473a6cd0f" />

#### Flattening of Multiple_Module_Opt 
<img width="1857" height="800" alt="show multiple_module_opt_flatten" src="https://github.com/user-attachments/assets/3a19d566-6cb5-47d6-9086-ceb8aaef614e" />
  




