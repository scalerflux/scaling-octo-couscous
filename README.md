 ### Day 1 - Introduction to Verilog RTL Design and Synthesis

 ![WhatsApp Image 2025-09-27 at 15 01 52](https://github.com/user-attachments/assets/3a31208b-9521-4f4b-9a1d-6a7156099274)



a. ## Introduction to open-source simulator Iverilog

<img width="1076" height="877" alt="Screenshot 2025-09-27 at 2 55 09 PM" src="https://github.com/user-attachments/assets/45ea6e8e-8abf-45ba-ab29-785a47d1774b" />

b. ##Intoduction to yosys
<img width="1163" height="375" alt="Screenshot 2025-09-27 at 3 17 39 PM" src="https://github.com/user-attachments/assets/d2c307c5-9bf3-4dfc-abbc-fbdbe6e2e36d" />
<img width="1258" height="637" alt="Screenshot 2025-09-27 at 3 22 35 PM" src="https://github.com/user-attachments/assets/96592f85-4dce-4dfc-8bb8-a658ac6f6539" />

c. ## Flat vs Hierarhial synthesis
<img width="1348" height="1017" alt="Screenshot 2025-09-27 at 3 33 00 PM" src="https://github.com/user-attachments/assets/d533d9e3-cf3d-4136-85ab-8f7087fdcd88" />


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

3. `dff_const4.v`
<img width="1017" height="1158" alt="Screenshot 2025-09-26 at 4 08 16 PM" src="https://github.com/user-attachments/assets/2955da06-1f98-4c7f-b558-0cd1214816a6" />

4. `diff_const5.v`
<img width="1539" height="830" alt="Screenshot 2025-09-26 at 4 13 00 PM" src="https://github.com/user-attachments/assets/662ac61c-0c42-4e90-9e1d-ec3a2d63f34b" />

## Sequence optimization unused ouoputs

If some llogic doesn't contribute to the ouput, we can remove them
1. Here it only uses on flip flop, and optimizes the first two bits which is not used in output
<img width="1271" height="1094" alt="Screenshot 2025-09-26 at 4 28 50 PM" src="https://github.com/user-attachments/assets/8eaf8a71-e73a-4c3e-b902-54a3608397a3" />

2. Now let's use all the 3 bits of the counter, and 3 flops must be inferred 
<img width="1487" height="1123" alt="Screenshot 2025-09-26 at 4 41 46 PM" src="https://github.com/user-attachments/assets/9882b160-1fb3-4737-9271-afff71dafb36" />


### Day 4

## GLS synth sym mismatch 

```






iverilog ../my_lib/verilog_model/primitives.v  ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v
```
<img width="1707" height="1125" alt="Screenshot 2025-09-26 at 6 10 27 PM" src="https://github.com/user-attachments/assets/9f3c927c-958d-4a5e-a206-77d588720272" />

1. `bad_mux`, missing sensitivity list example. Here y will only change if select changes rendring i1 n i0 waste, the synthesis simulation mismatch can clearly seen here
<img width="1032" height="1081" alt="Screenshot 2025-09-26 at 7 14 16 PM" src="https://github.com/user-attachments/assets/359fb9be-420b-4e8a-9e1c-16d544508d4c" />

2. Blocking statement (here we'll se as if the ouput of a OR b is flopped in the simulation)
<img width="1001" height="1158" alt="Screenshot 2025-09-26 at 7 35 10 PM" src="https://github.com/user-attachments/assets/cb910366-156b-4e4a-b28b-0cca2fd925d8" />


### Day 5

## IF

1.  Incomplete IF
  The aim was to create a mux but i has infereed a dlatch during synthesis due the absence of esle i0 is latching on to the value of the ouput y 


  <img width="1024" height="922" alt="Screenshot 2025-09-27 at 9 37 12 AM" src="https://github.com/user-attachments/assets/e6398787-58f8-428b-8c0f-9bf2db1f202e" />

  ## CASE 
  
1. Incomlete case
   So when select line is zero it acts a mux ut when it's 1 then it acts as latch, hence instead of just the mux it wass be 2x1 mux onnected to the dpin of the ltach with enable condition being  1
   <img width="1552" height="1035" alt="Screenshot 2025-09-27 at 9 53 36 AM" src="https://github.com/user-attachments/assets/f687db60-97c1-4ef0-876e-53c44f04cf37" />

   so adding a defualt statement will prevent this latching, let's seet in action(and we can see that no latch was inferred during synthesis!! which is so cool)
   <img width="1546" height="1095" alt="Screenshot 2025-09-27 at 10 47 50 AM" src="https://github.com/user-attachments/assets/fa240854-b7ec-4208-bbd7-5a869525f87f" />
   checking rtl epecteactation ....

2. Partial assignment , here in ase of ouput X we'll see that when sel [1] + sel [0] , it acts a latch
<img width="1287" height="907" alt="Screenshot 2025-09-27 at 11 04 36 AM" src="https://github.com/user-attachments/assets/cf343c46-e3e7-443d-b40d-9d2db5d2d198" />
Clearly it has inferred a latch during synthesis

3. Incomplete overlapping case, with 2`b1? it will get executed both times the select is 0 n 1. Which will cause synthesis simualtion mismatch
<img width="1716" height="1117" alt="Screenshot 2025-09-27 at 11 28 43 AM" src="https://github.com/user-attachments/assets/c528a246-7ba0-445b-88d4-0933a29986dd" />

the correct way to code is to not have overlapping cases 

### Verilog coding n synthesis  styles 

## Looping construtcs 

1. For loop (used inside the always block n to evaluate expressions)
   
a. mux
<img width="1789" height="1118" alt="Screenshot 2025-09-27 at 12 25 48 PM" src="https://github.com/user-attachments/assets/6dbbfef4-fcbc-46e3-8484-2d97c63b3026" />
b. demux <img width="1756" height="1118" alt="Screenshot 2025-09-27 at 12 41 55 PM" src="https://github.com/user-attachments/assets/e0db1d94-e9b1-4e3e-8413-05b8a8b10406" />

2. Generate loop
<img width="1914" height="1109" alt="Screenshot 2025-09-27 at 2 38 03 PM" src="https://github.com/user-attachments/assets/6444acf8-ffd6-4836-8b33-486bd1950185" />





   
<img width="1539" height="830" alt="Screenshot 2025-09-26 at 4 13 00 PM" src="https://github.com/user-attachments/assets/4495ecf1-579d-4da2-b050-9ab756d5e14e" /><img width="1539" height="830" alt="Screenshot 2025-09-26 at 4 13 00 PM" src="https://github.com/user-attachments/assets/edb0f80f-eac9-44e4-8d6f-3ee6b36cc6c4" />



