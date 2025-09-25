# Day 1 - Introduction to Verilog RTL Design and Synthesis
## Introduction to open-source simulator Iverilog


## Various Flop Coding Styles and Optimization
### Why do we need flops in digital circuits?

The main to resolve the "glitchy output in combination circuits
>Due to the propogation delay in gates we encounter a glitch at the ouput
>We use flip-flop sto restrict the glitches, coz they'll only change on the edge of the clock
>Even though the input might be glicthy, the ouput will be stable which will speed the circuit
>Also settles downs the the value

To initialize a flop we got control pins like `Reset` and `Set` 

