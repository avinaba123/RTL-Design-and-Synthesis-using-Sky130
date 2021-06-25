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
Setup of a design nad Test Bench illustrated bellow:
![Test bench Setup](https://raw.githubusercontent.com/avinaba123/RTL-Design-and-Synthesis-using-Sky130/main/Images/Day%201/Testbench%20setup.png)

Consider a design with a set of primary inputs that can range from one to several. Now, stimulus must be created for all of these primary inputs, and stimulus must be observed for all of the primary outputs. So according to the figure here's a Stimulus Generator at the left, from where the stimuli are generated and faded to the DUT (Design under test) and according to the stimulus the changes in primary output is observed by Stimulus Observer. The design will be initiated in the testbench, and then a mechanism will be used to apply Stimulus to it.

```
Point to remember: 
         - Design might have one or more primary inputs, one or more primary outputs.
	 - Test Bench does not have any primary input or primary output.
```
### Simulator :

Simulation is a technique for testing if the RTL code works as expected by applying different input stimuli to the design at different periods. Simulation is a well-known approach for ensuring the design's resilience. It's comparable to how a manufactured chip would be operated in real life and how it will react to various inputs.

```
Dasic working principle of Simulator:
	 - Simulator looks for the changes on the input Signals.
	 - If there is no change in the input, the simulator is not going to evaluate the output.
```
### Simulation Flow :
![image](https://user-images.githubusercontent.com/61839839/123386852-b99f8180-d5b4-11eb-8978-34d1d985519e.png)

Assume a design and the test bench that was created for a particular design. Now, we already know that the Simulator exclusively checks for changes in the input and then dumps only those changes in the output. As a result, a Simulation's output will be a VCD ( Value Change Dump) File. Now, in order to examine the VCD file, we'll use a programme called GTKwave, which will allow us to see the waveform output and check the design's functionality. 

### Setting up the Evironment:

The first step is cloning the [vsdflow](https://github.com/kunalg123/vsdflow) and [sky130RTLDesignAndSynthesisWorkshop](https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop) repositories from Kunal's profile, we can get to work.

```
Note:
	- Cloning : cloning means just making a copy of the repository.
  	- SKY130RTLDesignAndSynthesisWorkshop : It is the github page for the workshop.
	-  my_lib : It contains all library files which are needed.
           	    It has two folders namely lib, verilog_model.
	- lib : It contains the standard cell library which will be used for the synthesis.
	- verilog_model : This contains all the standard cell verilog models of the standard cells which are present in the lib.
	- Verilog_files : It is a folder containing all the lab experiments. It contains all the verilog source files, test bench files and so on.
         
```
Fig : Files inside my_lib, lib, verilog_model
![image](https://user-images.githubusercontent.com/61839839/123394507-d8097b00-d5bc-11eb-8eaa-51d320696f4b.png)

Fig: List of files inside verilog_files
![image](https://user-images.githubusercontent.com/61839839/123394251-8cef6800-d5bc-11eb-9b5f-96e038764dff.png)
![image](https://user-images.githubusercontent.com/61839839/123394283-98429380-d5bc-11eb-971b-97434ad2b2e9.png)

  








## ACKNOWLEDGEMENT
## REFERENCES
  

