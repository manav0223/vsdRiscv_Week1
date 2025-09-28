<div align="center">
 
# Week 1 : Day 1
# Introduction to Verilog RTL design and Synthesis

</div>

<div align="center">
 
[![RISC-V](https://img.shields.io/badge/RISC--V-SoC%20Tapeout-blue?style=for-the-badge&logo=riscv)](https://riscv.org/)
[![VSD](https://img.shields.io/badge/VSD-Program-orange?style=for-the-badge)](https://vsdiat.vlsisystemdesign.com/)
![Week](https://img.shields.io/badge/Week-1-green?style=for-the-badge)

</div>


## Overview :
Think of today as setting up my digital design toolbox! I was introduced to Verilog, the must-know language for hardware. The great news is we focused on open-source tools. I learned how to simulate my circuits using Icarus Verilog (iverilog), which is fantastic for seeing if my code actually works. Then, I explored logic synthesis with Yosys, another powerful, free tool that bridges the gap between code and silicon. All the explanations were super easy to follow, and the labs were a blast. I wrapped up the day feeling confident about the fundamentals of Register Transfer Level (RTL) design.

## Table of content :
1. Introduction to Verilog RTL design and Synthesis
2. Introduction to open-source simulator iverilog
3. Labs using iverilog and gtkwave
4. Introduction to Yosys and Logic synthesis

# The "How It Works" Perspective 🕵️

# 1. Introduction to Digital Detective Work 🔎

We’re kicking things off with the big ideas behind digital systems. We need that solid foundation so everything else makes sense! The most fascinating part? The digital simulator is like a super-efficient detective. It doesn't waste time looking at the whole circuit every second; it just keeps an eye on the signals (your virtual wires). The moment it detects an event—a flip from 0 to 1 or 1 to 0—it springs into action. This event-driven approach is what keeps your simulation running quickly!

# 2. Learning the Language and Building the Stage (Verilog & Testbenches) 🎬

Time to learn the language of hardware: Verilog! This is what engineers use to describe their circuits. We'll start modeling at the Register Transfer Level (RTL), which is where your design ideas start taking shape. To check if your design works, you need a testbench. Think of the Design Under Test (DUT) as the actor (it has real inputs and outputs), and the testbench as the stage manager. It creates the environment, generates all the clocks, resets, and data to poke and prod the DUT. Since the testbench is just a script running the show—not actual hardware—it doesn't need any I/O ports of its own!

# 3. Labs using Iverilog and GTKWave 🧰

Now for the fun part: putting theory into practice! We’ll be using the awesome open-source simulator Icarus Verilog (iverilog). This is where you run your Verilog code to see if your logic actually behaves the way you thought it would. It’s all about direct experimentation—running the code, watching the signals with a viewer like GTKWave, spotting errors, and building that crucial practical confidence. It’s the best way to learn!

```
mkdir VLSI
```
```
cd VLSI
```
```
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
```
<img width="1024" height="418" alt="Gitclone skyrtl" src="https://github.com/user-attachments/assets/0d9a7fb4-8ff7-45db-b167-33b805390e8b" />

```
iverilog good_mux.v tb_good_mux.v
```
Running the a.out file created
```
./a.out
```
<img width="792" height="72" alt="Running the a out file" src="https://github.com/user-attachments/assets/01b01d47-eb00-4d38-9323-d2d318f88f64" />

```
gtkwave tb_good_mux.vcd
```
The test bench instantiates the DUT to connect with it
Got a GTK Wave
<img width="1853" height="573" alt="GTKWave tb_good_mux" src="https://github.com/user-attachments/assets/a74b0422-473b-47ea-944c-fefeaaad1b86" />

# 4. Introduction to Yosys & Logic Synthesis
Now for the really cool part: Logic Synthesis with Yosys! We’re going to see how your hardware description code gets instantly transformed into actual, physical gate-level logic—the building blocks of circuits. This is the crucial step that connects your Verilog design to the real world, whether it ends up on an FPGA or a custom-made chip (ASIC). Understanding this conversion process is the cornerstone of digital design, turning abstract ideas into tangible hardware.

Open the verilog_files directory in the terminal using below commands:
```
cd VLSI
cd verilog_files
```
For Opening yosys, use the command
```
yosys
```

Now in Yosys, enter the following commands
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog good_mux.v
synth -top good_mux
```
We are reading the liberty file, verilog code of MUX & also synthesizng the RTL Code/Design
<img width="1865" height="1080" alt="Screenshot from 2025-09-27 17-15-04" src="https://github.com/user-attachments/assets/b84ec61d-c3b4-4599-8941-c33f42dea2e0" />
<img width="635" height="406" alt="Screenshot from 2025-09-27 17-17-55" src="https://github.com/user-attachments/assets/02f359e7-7986-4277-b8f4-ca80e98ac4bc" />

Now we will do the technology mapping
```
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
<img width="845" height="273" alt="Screenshot from 2025-09-27 17-19-51" src="https://github.com/user-attachments/assets/8ba635f0-9c63-4b54-b95f-74872cbad1f8" />

Got a distutils error
<img width="1865" height="449" alt="Distutils Error" src="https://github.com/user-attachments/assets/b91707f2-ba39-49d3-ab4c-2507e1c4247a" />

## To rectify the error
```
sudo apt-get update
sudo apt-get install python3-setuptools
```
To verify if graphiz, xdot and distutils installed or not
```
sudo apt install graphviz xdot
python3 -c "import distutils; print(distutils.__file__)"
```
Now to show the gate level circuit
```
show
```
<img width="1856" height="1007" alt="Screenshot from 2025-09-27 17-20-41" src="https://github.com/user-attachments/assets/348a3cc6-ebd4-456b-90bb-c9192db0be9b" />

Getting the netlist
```
write_verilog -noattr good_mux_netlist.v
!gvim good_mux_netlist.v
```
<img width="1251" height="604" alt="Netlist File" src="https://github.com/user-attachments/assets/199b7e4e-7ffb-4f9f-976c-ed3700c4d662" />


