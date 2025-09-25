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

###Synthesising the 3 above circuits

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


