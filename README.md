# Day 1 - Introduction to Verilog RTL Design and Synthesis
## Introduction to open-source simulator Iverilog


## Various Flop Coding Styles and Optimization
### Why do we need flops in digital circuits?

The main to resolve the "glitchy output in combination circuits
* Due to the propogation delay in gates we encounter a glitch at the ouput
* We use flip-flop sto restrict the glitches, coz they'll only change on the edge of the clock
* Even though the input might be glicthy, the ouput will be stable which will speed the circuit
* Also settles downs the the value

To initialize a flop we got control pins like `Reset` and `Set` and there  asynchronous n synchrnous flops
* Hence depending on these flops are classified into 4 different types
* In a D flip-flop with a
* for ex: ansynchronous rest will set q to 0 irrespective of the clock while sync one see if there is any sync_reset at the positive edge

## Flop synthesis Simulations

1.AsyncReset
 Just before the reset the q=1 because d=1, bu the moment reset came, q didn't wait for the subsequent clockedge but went immediately to zero 
<img width="1180" height="638" alt="Screenshot 2025-09-25 at 7 12 19 PM" src="https://github.com/user-attachments/assets/c5b02b4b-baf1-43a5-9d41-30e338fa231d" />


2. AsyncSet
When set = 1 q was alo 1 irrespective of d, once the set = 0, chages in d are apprent in q upon the posistive clock edge

<img width="1181" height="637" alt="Screenshot 2025-09-25 at 7 07 58 PM" src="https://github.com/user-attachments/assets/16c0c91a-5f84-4502-a748-40aec7c1c198" />

3. SyncReset
When reset bcomes 1 q is not changed till the subsequent clock edge n doeasn't become zero immediately. so the reset apllies only upon posedge here

<img width="1141" height="621" alt="Screenshot 2025-09-25 at 7 23 17 PM" src="https://github.com/user-attachments/assets/7b6bbbe4-4586-4722-a396-d88d29d02fc7" />

### Synthesising the 3 above circuits

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_asyncres.v
synth -top dff_asyncres
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
<img width="1848" height="879" alt="Screenshot 2025-09-25 at 7 44 13 PM" src="https://github.com/user-attachments/assets/7bd22346-2c5d-4489-ba33-da193cafa835" />

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_async_set.v
synth -top dff_async_set
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
<img width="1848" height="893" alt="Screenshot 2025-09-25 at 7 48 50 PM" src="https://github.com/user-attachments/assets/7ceb78c9-8c23-4c03-875b-f26570665048" />

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_syncres.v
synth -top dff_syncres
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
<img width="1848" height="927" alt="Screenshot 2025-09-25 at 7 55 53 PM" src="https://github.com/user-attachments/assets/b6e5e97b-4cca-4fbd-a49f-0d979a8b9e16" />

### Optimizatios

Just by rewiring we can achieve few logic functionalities without using standard cells
1. mult2
 ```
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   read_verilog mult_2.v
   synth -top mul2
   show
   write_verilog -noattr mult2_net.v
   !gvim mult2_net.v
 ```

 
Here we don't any hardware(i.e the standard cells)  to implement this, just appnding 0 works fine
<img width="882" height="262" alt="Screenshot 2025-09-26 at 9 39 58 AM" src="https://github.com/user-attachments/assets/c515e9fa-4b32-4caa-94d4-7553eeb220db" />


<img width="1274" height="795" alt="Screenshot 2025-09-26 at 9 22 27 AM" src="https://github.com/user-attachments/assets/1f43ca43-4d5d-4d32-9f59-e41250b08301" />



2. mult8
   ```
   read_verilog mult_8.v
   synth -top mult_8
   show
   write_verilog -noattr mult8_net.v
   !gvim mult8_net.v
   ```
We can generalize for power of 2. for ex: for 8 3 zeros are appended 
<img width="1209" height="947" alt="Screenshot 2025-09-26 at 9 38 32 AM" src="https://github.com/user-attachments/assets/0ff78876-8a7b-4073-85c8-2d29aecfee9d" />

### Day 3 
## Combinational logic optimization

1. We expect this mux `opt_check.v` to get simplified to AND  gate
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check.v
synth -top opt_check
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
<img width="1089" height="618" alt="Screenshot 2025-09-26 at 11 27 39 AM" src="https://github.com/user-attachments/assets/702e3fcb-ab13-4446-94a7-fc609c4c2886" />


2. `opt_check3` to a 3 input AND gate
<img width="972" height="668" alt="Screenshot 2025-09-26 at 11 41 58 AM" src="https://github.com/user-attachments/assets/7010b0a3-ee4a-4a91-8e2b-77583be3e1c0" />

3. `multiple_module_opt.v` to 2 input AND  feeding a 2 input OR (a21o)
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_module_opt.v
synth -multiple_module_opt
flatten
opt_clean -purge 
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
<img width="1236" height="846" alt="Screenshot 2025-09-26 at 12 10 29 PM" src="https://github.com/user-attachments/assets/0744100a-76c0-46e4-9bc0-b1b278cdb875" />

## Sequential logic optimizations

1. Here q doesn't change immidiately as reset changes, but changes only at next posedge clock, this will require to infer a dff(flop)
<img width="1129" height="753" alt="Screenshot 2025-09-26 at 12 35 56 PM" src="https://github.com/user-attachments/assets/43366a98-1152-4440-8987-9726897d5dcb" />

2. While here it's not coz irresptive clock, q wis always 1
<img width="929" height="993" alt="Screenshot 2025-09-26 at 3 05 47 PM" src="https://github.com/user-attachments/assets/fc2dc72d-377f-41a1-af03-f24022764363" />





