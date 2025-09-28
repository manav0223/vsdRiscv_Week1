<div align="center">

  # Week 1 : Day 2
# Timing Libraries, Synthesis Approaches, and Efficient Flip-Flop Coding

</div>

<div align="center">
 
[![RISC-V](https://img.shields.io/badge/RISC--V-SoC%20Tapeout-blue?style=for-the-badge&logo=riscv)](https://riscv.org/)
[![VSD](https://img.shields.io/badge/VSD-Program-orange?style=for-the-badge)](https://vsdiat.vlsisystemdesign.com/)
![Week](https://img.shields.io/badge/Week-1-green?style=for-the-badge)

</div>

## Table of Content :
1. Introduction to timing .libs
2. Hierarchical vs Flat Synthesis
3. Various Flop Coding Styles and optimization

# 1. Introduction to Timing Libraries
When it comes to synthesis, the tool doesn't work alone—it relies on standard cell libraries (usually .lib files). Think of these as a catalog of all the basic logic components it can use.
The interesting part is that each component comes in different "flavors," often labeled slow, medium, or fast. This is where the major trade-offs happen:
Fast Cells: These use larger transistors to boost the current, which makes the signal travel quickly (low delay). Great for meeting timing requirements, but they take up more chip area and burn more power.
Slow Cells: These are smaller, save power and area, but they're slower. They're often used strategically to help fix tricky hold time violations (a different kind of timing problem).
Ultimately, digital design is all about juggling these three opposing factors—area, power, and timing. The synthesis tool's main job is to cleverly balance these constraints to ensure your circuit runs efficiently and reliably.

# 2. Hierarchial vs Flat Synthesis
We tackled design structuring today by talking about hierarchical synthesis. This is the smart way to build a large chip: you break it down into smaller, repeatable modules instead of dealing with one giant, unmanageable file.

We then processed our multi-module design (multiple_modules.v) step-by-step:

1.First, we ran the simulation and viewed the results in GTKWave to make sure the logic was correct.

2. Next, we used Yosys to perform the actual synthesis (turning our code into gates).

3.The final step was generating the netlist visualization. This involved running commands to get individual pictures of the gate-level structure for every single module and submodule in our hierarchy!

#### Sub module :
<img width="1856" height="542" alt="Show Sub_module1" src="https://github.com/user-attachments/assets/726dbbee-5051-4793-8d56-c3bb79d228ad" />

#### Multiple Modules:
<img width="1854" height="785" alt="Multiple Modules" src="https://github.com/user-attachments/assets/7da1f63c-53b6-49dc-b42b-d77b9fb4ae58" />

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top multiple_modules
```
```
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
```
Flattening the output so basically removing the unused files from the main file.
```
flatten
write_verilog multiple_modules_flat.v
```
```
show
```
<img width="1853" height="452" alt="Flattten Show Output" src="https://github.com/user-attachments/assets/9fb2be9b-9ae7-45c5-b0d3-6b3c5b30dcef" />


### 3. Various Flop Coding Styles and optimization
Flip-flops are the superheroes of synchronous design! They're absolutely essential because they store data and keep things stable, acting like traffic cops to prevent signal errors (glitches) caused by things moving at slightly different speeds. The whole system marches to the beat of the clock, and these sequential elements only capture data right on that rising or falling edge, which locks in the timing and keeps the data flowing correctly.

We also looked at how to get them started using synchronous or asynchronous set/reset signals. Crucially, timing isn't fixed; it changes based on Process, Voltage, and Temperature (PVT). All this variability is captured in the .lib files, making it super important to do timing-aware synthesis and simulation to ensure our hardware works reliably in the real world.
- We simulated the syncres.v , async_set and asyncres.v
- then genrated the Gtkwave
- Synthesised it using Yosys
- We generated individual netlist graphical representation for each design

#### Simulation & GTK Wave Generation:
```
iverilog dff_asyncres.v tb_dff_asyncres.v
./a.out
```
Now open the dumped/created vcd file in gtkwave
```
gtkwave tb_dff_asyncres.vcd
```
<img width="1856" height="562" alt="GTKWave for Asyncres" src="https://github.com/user-attachments/assets/6e03d09a-c365-406e-b59f-3b6f64a7f815" />
```
iverilog dff_syncres.v tb_dff_syncres.v
./a.out
```
Now open the dumped/created vcd file in gtkwave
```
gtkwave tb_dff_syncres.vcd
```
<img width="1856" height="606" alt="GTK Wave dff_syncres" src="https://github.com/user-attachments/assets/6de35c04-b5fe-447c-835b-0ecf9c9dfe42" />

```
iverilog dff_asyncres_set.v tb_dff_asyncres_set.v
./a.out
```
Now open the dumped/created vcd file in gtkwave
```
gtkwave tb_dff_asyncres_set.vcd
```
<img width="1859" height="575" alt="asyncres_set GTKWave" src="https://github.com/user-attachments/assets/7d2c7714-fe46-4375-8207-53f1649aa495" />


#### Synthesizing asyncres file in Yosys
Open yosys
```
yosys
```
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_asyncres.v
synth -top dff_asyncres
```
<img width="641" height="422" alt="Statistics dffasyncres" src="https://github.com/user-attachments/assets/797b0e2d-ed8c-4407-b312-3564af9ae601" />

```
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
```
```
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
```
show
```
<img width="1856" height="425" alt="Show asyncres" src="https://github.com/user-attachments/assets/fa6bce6b-10d0-4eba-b33c-3d7949a37b14" />

#### Synthesizing syncres file in Yosys
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_syncres.v
synth -top dff_syncres
```

<img width="683" height="425" alt="dff syncres statistics" src="https://github.com/user-attachments/assets/42e85249-3d68-4ac2-a5a9-97cf75134915" />

```
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
```

```
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

```
show
```
<img width="1859" height="468" alt="Show Syncres" src="https://github.com/user-attachments/assets/166f40fe-08d8-47e5-bb8a-ec30889ab2b8" />

#### Synthesizing async_set file

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.li
read_verilog dff_async_set.v
synth -top dff_async_set
```
<img width="648" height="380" alt="Statistics async_Set" src="https://github.com/user-attachments/assets/556b922a-e77d-4fd8-9e34-5b4990461bc6" />

```
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
```
```
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
```
show
```
<img width="1851" height="611" alt="Show asyncres_set" src="https://github.com/user-attachments/assets/e01a164e-458a-4d37-a205-cb433e830f41" />

### Optimizations.
```
gvim mult_*.v -o
```
Launch yosys
```
yosys
```
##### 1.
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.li
read_verilog mult_2.v
synth -top mul2
```
```
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
Not needed as there is nothing to map

```
show
```
<img width="525" height="261" alt="Show mul2" src="https://github.com/user-attachments/assets/26e8452c-c66a-48dc-83bf-8510508bf116" />

Writng the netlist
```
write_verilog -noattr mul2_net.v
```
```
!gvim mul2_net.v
```
<img width="1236" height="301" alt="Netlist mul2" src="https://github.com/user-attachments/assets/6bd9e644-f412-4958-8bac-96b759325640" />


##### 2. 
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.li
read_verilog mult_8.v
synth -top mult8
```
```
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
Not needed as there is nothing to map

```
show
```
<img width="548" height="258" alt="show mult8" src="https://github.com/user-attachments/assets/6ca85373-8eb9-4adb-a2e4-a7a6ef8c8472" />

Writng the netlist
```
write_verilog -noattr mult8_net.v
```
```
!gvim mult8_net.v
```
<img width="1250" height="322" alt="gvim mult8" src="https://github.com/user-attachments/assets/76034497-a39b-4d4e-bf4c-d9e77d6a76f1" />
