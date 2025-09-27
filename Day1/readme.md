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

# Synthesis
Inside the VLSI directory, go to verilog_files directory using following command
```
cd verilog_files
```
```
yosys
```
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog good_mux.v
synth -top good_mux
```
<img width="1865" height="1080" alt="Screenshot from 2025-09-27 17-15-04" src="https://github.com/user-attachments/assets/b84ec61d-c3b4-4599-8941-c33f42dea2e0" />
<img width="635" height="406" alt="Screenshot from 2025-09-27 17-17-55" src="https://github.com/user-attachments/assets/02f359e7-7986-4277-b8f4-ca80e98ac4bc" />

```
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
<img width="845" height="273" alt="Screenshot from 2025-09-27 17-19-51" src="https://github.com/user-attachments/assets/8ba635f0-9c63-4b54-b95f-74872cbad1f8" />

Got a distutils error
<img width="1865" height="449" alt="Distutils Error" src="https://github.com/user-attachments/assets/b91707f2-ba39-49d3-ab4c-2507e1c4247a" />

# To rectify the error
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

```
yosys
```

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top multiple_modules
```

```
show multiple_modules
```
```
write_verilog -noattr multiple_modules_hier.v
!gvim multiple_modules_hier.v
```

```
flatten
write_verilog multiple_modules_flat.v
```

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top multiple_modules
```
```
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
```
```
flatten
```
```
show
```
<img width="1853" height="452" alt="Flattten Show Output" src="https://github.com/user-attachments/assets/9fb2be9b-9ae7-45c5-b0d3-6b3c5b30dcef" />

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top sub_module1
```
```
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
```
show
```
<img width="1856" height="542" alt="Show Sub_module1" src="https://github.com/user-attachments/assets/364f4ca7-7070-447a-acaa-6e6053e8a12d" />

```
iverilog dff_asyncres.v tb_dff_asyncres.v
./a.out
```
Now open the dumped/created vcd file in gtkwave
```
gtkwave tb_dff_asyncres.vcd
```
<img width="1856" height="562" alt="GTKWave for Asyncres" src="https://github.com/user-attachments/assets/0dd62a28-1726-4a93-8eb9-f054e223a2cc" />

```
iverilog dff_syncres.v tb_dff_syncres.v
./a.out
```
Now open the dumped/created vcd file in gtkwave
```
gtkwave tb_dff_syncres.vcd
```
<img width="1856" height="606" alt="GTK Wave dff_syncres" src="https://github.com/user-attachments/assets/3e706899-6cb9-4cec-a481-bdca8de464a2" />

```
iverilog dff_asyncres_set.v tb_dff_asyncres_set.v
./a.out
```
Now open the dumped/created vcd file in gtkwave
```
gtkwave tb_dff_asyncres_set.vcd
```
<img width="1859" height="575" alt="asyncres_set GTKWave" src="https://github.com/user-attachments/assets/f15214b9-ab95-4c03-abed-b4492bfeb734" />

# Synthesizing asyncres file in Yosys
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

# Synthesizing syncres file in Yosys

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

# Synthesizing async_set file

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

# Optimizations
```
gvim mult_*.v -o
```
Launch yosys
```
yosys
```

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

Writng the netlist
```
write_verilog -noattr mul2_net.v
```
```
!gvim mul2_net.v
```
