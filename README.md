# RTL-Design-and-Synthesis-using-Sky130

That's the documentation for the five-day workshop on "RTL DESIGN USING VERILOG USING SKY130 TECHNOLOGY". Incorporating tools like iverilog, yosys, and gtkwave, this repository contains the design and synthesis of RTL codes. 
<!-- Images -->
![vsdbanner](https://user-images.githubusercontent.com/61839839/123227107-e0e14a80-d4f1-11eb-828b-5e2b2c09c49a.png)

## GLIMPS OF THE WORKSHOP

This Workshop is as a training program that covers everything from the fundamentals to advanced topics. It was a five-day cloud-based workshop that covered topics like Verilog RTL design and synthesis, Timing Libraries, Hierarchical vs Flat synthesis, Efficient flop coding styles, followed by Combinational and Sequential optimizations, GLS, blocking vs non-blocking, and Synthesis-Simulation mismatch, and optimization in synthesis. The workshop requires a basic understanding of the Verilog HDL, so if you're unfamiliar with Verilog,
this might not be the ideal place to start.  In this workshop I was offered the chance to work in an opensource environment. Yosys and Sky130 PDKs were used for the assessments. 

## ABOUT THE AUTHOR 
#### Mr. Kunal Ghosh
*Co-founder of VLSI System Design (VSD) Corporation Private Limited*

## ROADMAP OF THE WORKSHOP

1. ASIC Design Flow / RTL TO GDS Flow
2. Some glimps about YOSYS OPEN SYNTHESIS SUITE, SKY130 TECHNOLOGY, iVerilog SIMULATOR and GTKwave
3. Day 1
    - Introduction to Verilog RTL design and Synthesis
      - Introduction to the iverilog open-source simulator and the iverilog design testbench
      - Using iverilog and gtkwave in the lab 
    - Introduction to Yosys and Logic Synthesis
      - Introduction to YOSYS OPEN SYNTHESIS SUITE
      - Introduction to Logic Synthesis 
      - Labs using Yosys and SKY130 PDKs
4. Day 2
5. Day 3
6. Day 4
7. Day 5

## 3. Day 1 :
## Introduction to Verilog RTL design and Synthesis :
### Register Transfer Level (RTL)
The Register Transfer Level is an abstract description of a digital circuit, consisting of two elements: Sequential circuits (flip-Flops) and Combinational circuits (Logic Gates).
An RTL design may construct any circuit with the aid of these two components. 

Among the different types of Hardware Descriptive Languages, the most popular ones are :

1. [Verilog HDL](https://en.wikipedia.org/wiki/Verilog)
2. [VHDL](https://en.wikipedia.org/wiki/VHDL)

### Design
The actual Verilog code (or collection of Verilog codes) that has the desired functionality to fulfil the requisite specifications is referred to as the design. 

Example : Verilog design of a 2-input MUX
```verilog
module good_mux (input i0 , input i1 , input sel , output reg y);
always @ (*)
begin
	if(sel)
		y <= i1;
	else 
		y <= i0;
end
endmodule
```
### Test Bench

A file containing the instantiation of a top-level design object for a design, as well as simulation input and output vectors. A test bench file can be a normal Verilog Design File (with the extensions.v,.verilog, or.vh), or a VHDL Design File (with the extensions.v,.verilog, or.vh) (with the extension .vhd or .vhdl) The actual Verilog code (or collection of Verilog codes) that has the desired functionality to fulfil the requisite standards is referred to as the design. 

Example : TestBench for the above mentioned design of 2:1 MUX

```verilog
`timescale 1ns / 1ps
module tb_good_mux;
        // Inputs
        reg i0,i1,sel;
        // Outputs
        wire y;
        
        // Instatiate the Unit Under Test (UUT)
        good_mux uut (
                .sel(sel),
                .i0(i0),
                .i1(i1),
                .y(y)
        );
        
        initial begin
        $dumpfile("tb_good_mux.vcd");
        $dumpvars(0,tb_good_mux);
        // Initialize Inputs
        sel = 0;
        i0 = 0;
        i1 = 0;
        #300 $finish;
        end
        
always #75 sel = ~sel;
always #10 i0 = ~i0;
always #55 i1 = ~i1;
endmodule
```         












## ACKNOWLEDGEMENT
## REFERENCES
  

