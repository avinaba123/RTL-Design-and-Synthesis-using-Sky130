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

## Labs using iverilog and gtkwave :

Let's look at a 2x1 MUX as an example. 

Logic Diagram
![image](https://user-images.githubusercontent.com/61839839/123401280-4140bc80-d5c4-11eb-9424-c8b9fe8a56d5.png)

Truth Table
| Sel | y |
| --- | - |
| 0   | i0|
| 1   | i1|

Boolean Expression
```
 Y = Sel'.i0 + Sel.i1

```
Circuit Diagram

![image](https://user-images.githubusercontent.com/61839839/123402842-caa4be80-d5c5-11eb-8122-d7aac897f44a.png)

Verilog Code 

> Use Command : __vim good_mux.v__
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
Test Bench

> Use Command : **vim tb_good_mux.v**
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
- Now we have to go to our design folder verilog_files which contains all our design files.
- Load the design, which we want to simulate in iVerilog.
```
iverilog good_mux.v tb_good_mux.v
```
Here as a design we are using good_mux.v and tb_good_mux.v as test bench

Fig: Shows loading good_mux.v and tb_good_mux.v into the simulator 

![image](https://user-images.githubusercontent.com/61839839/123406486-9bdc1780-d5c8-11eb-845d-c5c418f4a008.png)

- Dump the file
```
./a.out
```
- Load the VCD file into the GTKwave form viewer
```
gtkwave tb_good_mux.vcd
```
- Shows the gtkwaveform viewer opened to view the functionality of the design
> In wavefrom viwer we can notice when ever the Sel goes high the output follows the i1 and when ever Sel goes low the output follows i0. Which meets our characteristics of a 2-to1 Mux
> 
Fig: Shows executing ./a.out, dumping the VCD file and showing the output waveform in GTKwave

![image](https://user-images.githubusercontent.com/61839839/123408212-62a4a700-d5ca-11eb-82e3-5e39262ad5fb.png)

## Introduction to Yosys and Logic Synthesis











## 3. Day 2 :








## 5. Day 3:

In Digital logic we know there are two types of logic namely combinational and sequential. We need to optimise the logic since we want to obtain the best design in terms of area and power savings. There are a variety of basic and sophisticated optimization techniques (State Optimization, Cloaning Optimization, Retiming Optimization etc.) available, but we'll focus on constant propagation (which is a direct optimization technique) and boolean logic optimization here. 

### Constant Propagation

This is an optimization of sequential logic. A boolean expression can be reduced to a simpler expression for some particular input values. 
#### Combinational Constant Propagation

> For an example, there are two input of an AND gate A , B and output of AND feeds into input of NOR, where C is the other input and the output is fetched from the NOR gate. Implimenting this design in CMOS technology requires total of 6 MOS transistors.

```verilog
The boolean expression of the above example can be reduced by using some constant propagation 

y = ~((A*B)+C)
if A = 0
Y=~((0)+C) => Y = ~C

```
> So for A = 0 this can be reduced to a single inverter for a total of 2 MOS transistors.    

#### Sequential Constant Propagation

> Consider a D flip-flop with a reset line connected to it and its input tied low. Is there any possiability that the Q bocome 1? The ans is no (Wheather we apply Reset of clk the Q is always 0
> But every flop with D tied off is not a sequential constant, for the flop to become sequential constant, the q pin must always take constant value.
```  
Note: 
	- If it is a reset flop, having D tied off, the it's sequential constant.
	- If it is a set flop, it's not sequential constant, q can be either 0 or 1
	  depending on set or clk pins.

```

### Boolean Logic Optimization

K-maps and Quine McKluskey are two techniques that may be used to do this task, and information about both can be obtained online. 
> For an example, We'll look at a few modules from the lab to demonstrate the optimizations. 
> The structure of the file opt check.v is as follows: 

```verilog
module opt_check (input a , input b , output y);
assign y = a?b:0;
endmodule

```



## Combinational Logic Optimization Lab :

Here, we will take a look at a few RTL modules from the lab to show the optimizations. For the first excercise we will consider opt_check module.

```verilog
module opt_check (input a , input b , output y);
	assign y = a?b:0;
endmodule
```

> It is basically a 2:1 mux representation , when a is 1, it will assign b - else it will assign 0.
```
Commands we use for Synthesis:

yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> read_verilog opt_check.v
yosys> synth -top opt_check
yosys> opt_clean -purge        // command to perform constant propagation and optimization
yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
```
### How it looks after Synthesis

![image](https://user-images.githubusercontent.com/61839839/123464927-13329b00-d60b-11eb-8fdd-6d3df679cec0.png)

So after the synthesis as we expected here we can see a simple AND gate replacing the 2:1 Mux.

```
As y = a'.b + a
     = a + b    // Which is a simple AND gate representation
```
For the **second** experiment now we will consider the opt_check3 module. opt_check3 works by assigning y depending on the value of a, if a is 0 then assign 0 and if a is one then look at c. Similarly if c is 1, assign b else assign 0.  As shown in the figure bellow :

![image](https://user-images.githubusercontent.com/61839839/123470820-8b508f00-d612-11eb-81a0-475d68e40e57.png)

```verilog
module opt_check3 (input a , input b, input c , output y);
	assign y = a?(c?b:0):0;
endmodule
```
### How it looks after Synthesis

![image](https://user-images.githubusercontent.com/61839839/123470774-7bd14600-d612-11eb-8952-be154fab7cc9.png)


```
Commands we use for Synthesis:

yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> read_verilog opt_check3.v
yosys> synth -top opt_check3
yosys> opt_clean -purge        // command to perform constant propagation and optimization
yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
```

So after the synthesis as we expected here we can see a simple AND gate replacing the 2:1 Mux.

```
As y = a'.0 + a.[c'.0 +c.b]
     = 0 + a.b.c
     = a.b.c   // This is a simple 3 input AND gate 
```

For the **third** experiment we will consider the opt_check4 module. This is also a Mux based design as given in the figure:

![image](https://user-images.githubusercontent.com/61839839/123471218-129e0280-d613-11eb-958d-36c8bda1e56f.png)

```verilog
module opt_check4 (input a , input b , input c , output y);
 assign y = a?(b?(a & c ):c):(!c);
 endmodule

```
### How it looks after Synthesis

![image](https://user-images.githubusercontent.com/61839839/123471467-732d3f80-d613-11eb-9943-7b1ce6ad43ce.png)

```
Commands we use for Synthesis:

yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> read_verilog opt_check4.v
yosys> synth -top opt_check4
yosys> opt_clean -purge        // command to perform constant propagation and optimization
yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
```
After the synthesis asper our expectation here we can see a simple XNOR gate replacing the Mux based design.

```
As y = [a.b.c + b'.c].a + a'c
     = a.b.c + a.b'.c
     = a.c (b + b')
     =a.c + a'.c'         // Simple 2 input XNOR gate
```

For the **fourth** and **fifth** experiment we will consider the multiple_modules and multiple_modules_2 designs, respectively.
In this case as this design consists of several submodules, we have to flatten the design before constant propagation and optimization.

```
Commands we use for Synthesis:

yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> read_verilog multiple_modules.v
yosys> synth -top multiple_modules
yosys> flatten                  // to flatten the design
yosys> opt_clean -purge        // command to perform constant propagation and optimization
yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
```

Verilog file for the multiple_modules design is

```verilog
module sub_module1(input a , input b , output y);
 assign y = a & b;
endmodule


module sub_module2(input a , input b , output y);
 assign y = a^b;
endmodule


module multiple_module_opt(input a , input b , input c , input d , output y);
wire n1,n2,n3;

sub_module1 U1 (.a(a) , .b(1'b1) , .y(n1));
sub_module2 U2 (.a(n1), .b(1'b0) , .y(n2));
sub_module2 U3 (.a(b), .b(d) , .y(n3));

assign y = c | (b & n1); 


endmodule
```
### How it looks after Synthesis

According to our expectation Yosys done a great job and synthesized it optimally using a21o_1 module, as shown in the figure:

![image](https://user-images.githubusercontent.com/61839839/123473850-c6ed5800-d616-11eb-8d7b-ea9dea6e9fe9.png)


Verilog file for the multiple_modules_2 design is

```verilog
module sub_module(input a , input b , output y);
 assign y = a & b;
endmodule



module multiple_module_opt2(input a , input b , input c , input d , output y);
wire n1,n2,n3;

sub_module U1 (.a(a) , .b(1'b0) , .y(n1));
sub_module U2 (.a(b), .b(c) , .y(n2));
sub_module U3 (.a(n2), .b(d) , .y(n3));
sub_module U4 (.a(n3), .b(n1) , .y(y));


endmodule

```
### How it looks after Synthesis

According to our expectation Yosys done a great job and synthesized it optimally, as shown in the figure:

![image](https://user-images.githubusercontent.com/61839839/123474743-0b2d2800-d618-11eb-8eae-3ba744df54e4.png)

## Sequential Logic Optimization Lab :

For the **first** Lab on Sequential Logic Optimization we will consider dff_const1.v file. The circuit diagram representation of the verilog module is given bellow :

![image](https://user-images.githubusercontent.com/61839839/123476658-a3c4a780-d61a-11eb-8348-f73eb69a2f3d.png)

```verilog
module dff_const1(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b0;
	else
		q <= 1'b1;
end

endmodule
```
### Fig: Waveform after Simulating the dff_const1.v using iverilog 

![image](https://user-images.githubusercontent.com/61839839/123478253-d079be80-d61c-11eb-912e-35776fac7e77.png)


Since Reset is applied like this, Q will be alwas zero untill the reset goes low. But it does not mean that after the removing the reset the Q will be high immediately, it is going to wait for the next rising edge of the clock to become high (as shown in the figure above). So it's not a simple inverter (Q = Reset, is not true, in one time period the nature will be different as shown in the fig.)

### How it looks after Synthesis

```
Commands we use for Synthesis:

yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> read_verilog dff_const1.v
yosys> synth -top dff_const1
yosys> dfflibmap -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib     //dfflibmap is the switch, which is telling the sinthesizer for sequaltial circuits (mainly for dff and latches) what libreary does to pick
yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
```
![image](https://user-images.githubusercontent.com/61839839/123479306-5ba78400-d61e-11eb-85e4-d1a7b1b937f8.png)

So here is a flop which has D, Reset and Clk pin. But this standard cell libreary is expecting to have active low reset, but we have codded it active high insted of that. Thats why the synthesizer infering a inverter in the reset path. 

> Note :In case you seeing the statistics it has infered an dff for dff_const1


In the **second** lab we will be working on dff_const2.v file. The circuit diagram representation of the verilog module is given bellow :

![image](https://user-images.githubusercontent.com/61839839/123479955-467f2500-d61f-11eb-90f9-4a6e2e8a6804.png)

```verilog
module dff_const2(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b1;
	else
		q <= 1'b1;
end

endmodule
```

### Fig: Waveform after Simulating the dff_const2.v using iverilog 

![image](https://user-images.githubusercontent.com/61839839/123480157-8e9e4780-d61f-11eb-9549-bffb946f70f8.png)

Here we have a flop which is getting a set and D is tied to logic high. Now if there is a reset, here the reset pin is working as set (as mentioned in the verilog file). So according to the waveform the Q is high when the reset is high and even if at the edge of the clock it continous to be logic high. As a result everywhere the Q will be logic high, it's a constant.

### How it looks after Synthesis

```
Commands we use for Synthesis:

yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> read_verilog dff_const2.v
yosys> synth -top dff_const2.v
yosys> dfflibmap -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib     //dfflibmap is the switch, which is telling the sinthesizer for sequaltial circuits (mainly for dff and latches) what libreary does to pick
yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
```
![image](https://user-images.githubusercontent.com/61839839/123481131-f0ab7c80-d620-11eb-9781-507da44d46d5.png)

Inspite of having clk, reset and all my Q is alway at logic high. So there is no need of any flop. It just removed everything and connected 1'b1 to Q through a buffer.
> Note :In case you seeing the statistics it has not infered an dff for dff_const2

For the **third** lab we are considering the dff_const3.v design. The circuit diagram representation of the verilog module is given bellow :

```verilog
module dff_const3(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
	if(reset)
	begin
		q <= 1'b1;
		q1 <= 1'b0;
	end
	else
	begin
		q1 <= 1'b1;
		q <= q1;
	end
end

endmodule
```
![image](https://user-images.githubusercontent.com/61839839/123485880-a4fcd100-d628-11eb-8ae2-bd2cbaa521b3.png)

Here we have two flop clearly. Both are getting the same clk and same reset, but upon reset Q is becoming 1'b1 and Q1 is becomig 1'b0. So the flop A is a reset flop and the flop B is a set flop (as shown in the figure).  

### Fig: Waveform after Simulating the dff_const2.v using iverilog 

![image](https://user-images.githubusercontent.com/61839839/123484928-c2c93680-d626-11eb-8893-27e61bc83ed4.png)

So Q1 is a reset flop, so when ever reset is applied Q1 is going to be zero. After the reset getting zero Q1 will wait for the next positive clk edge to become logic high and it will be permanently logic high after that.So itis clear that Q1 can't be optimized.
Now Q is a set flop, upon reset Q will become one (as in the design reset is acting as a set input for the Q). So at the portion mentioned in the figure Q1 is going to be logic high after Tcq (clock to Q delay), so in this edge flop b will sample the value of zero and it will become logic high at the next positive edge of the clock. As a result Q will be always high except one clk cycle.
So we can't remove flop A or flop B, connected to constant.

### How it looks after Synthesis

```
Commands we use for Synthesis:

yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> read_verilog dff_const3.v
yosys> synth -top dff_const3
yosys> dfflibmap -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib     //dfflibmap is the switch, which is telling the sinthesizer for sequaltial circuits (mainly for dff and latches) what libreary does to pick
yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
```
![image](https://user-images.githubusercontent.com/61839839/123486817-6831d980-d62a-11eb-8bda-9a2264b876f7.png)

So according to out assumtion Yosys did the same, it is clear from the picture that first flop is the reset and the second flop is the set flop. As we have used active high set and reset thats why we can see inverter in the path.

> Pint to be remember :
	
	For Sequential optimizations; not every flop that has a constant at the input is getting optimized, we need to look at the set/reset connections and see if q is getting a constant value or changing. we can optimize it only if the value og q is fixed.


## Sequential Optimization for unused outputs :

For understanding the Unused output Optimization we have considered a three bit up counter, counter_opt.v design. 

```verilog
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = count[0];

always @(posedge clk ,posedge reset)
begin
	if(reset)
		count <= 3'b000;
	else
		count <= count + 1;
end

endmodule
```
Basically this module is accepting clk, reset and generating output q. If there is a reset the counter is initialized to zero else the counter is incremented. As it is a 3 bit sequance it can;t go beyond 7, after 7 it rolls backs to the initial stage. Acording to the functionality output q has no dependecy on count[1] and count[2], thats why we are least bothered about them. So these are our unused outputs and our objective to optimise the design. So if we have a close look at cout[0], there is togling at every clock cycle.  

| cout[2] | cout[1] | cout[0] |
| ------- | ------- | ------- |
|    0    |    0    |    0    |
|    0    |    0    |    1    |              
|    0    |    1    |    0    |
|    0    |    1    |    1    |
|    1    |    0    |    0    |
|    1    |    0    |    1    |
|    1    |    1    |    0    |
|    1    |    1    |    1    |
|    0    |    0    |    0    |


### How it looks after Synthesis

```
Commands we use for Synthesis:

yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> read_verilog counter_opt.v
yosys> synth -top counter_opt
yosys> dfflibmap -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib     //dfflibmap is the switch, which is telling the sinthesizer for sequaltial circuits (mainly for dff and latches) what libreary does to pick
yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
```

![image](https://user-images.githubusercontent.com/61839839/123490408-9535ba80-d631-11eb-90f1-d0f7ea594766.png)

So as we can see there is only one flop, but the 3 bit counter should have three flop. But why it's showing like this? according the netlist if we redraw the circuit let's see what we get.

![image](https://user-images.githubusercontent.com/61839839/123490840-c6fb5100-d632-11eb-82b7-e6ec13d96978.png)

This is a toggling scenerio (where q is not of q) and the unused two bits are complitely optimized away. So as a conclussion any logic on which output is not dependent those must be optimized like what we saw in this example.



## 6. Day 4:

### Gate Level Simulation, Blocking vs Non-Blocking Assignments, Synthesis-Simulation Mismatch

So the first question comes what is Gate Level Simulation? and why we need dhis?
It is nothing but running the test bench with netlist as Design Under Test (DUT). As the netlist is logically same as the RTL code, then why we do this? There are two main casues:
	
- We want to verify the logical correctness of the design after synthesis. (Why we need to check this is our main concern for this day)
- Ensuring the timing of the design is met
	
![image](https://user-images.githubusercontent.com/61839839/123517099-eccd3800-d6bc-11eb-80cd-ea3ece29b700.png)

For the simulation setup for netlist is shown above. The only diffrance is that here we are running the test bench with netlist as Design Under Test (DUT). And the netlist is going to have all the standard cells are instanciated in it. So we need to tell to iverilog that what is the meaning of those standard cells. Thats why we proided the extra files to iverilog. Gate level verilog models may be of two types :
	
- Timing aware - Where we can validate functionality and timing bith of them
- Functional - We can only validate the functionality 

### Synthesis-Simulation Mismatch

Synthesis-Simulation Mismatch is one of the common cause for which we go for GLS. Again synthesis and simulation missmatch can be happen because o some resons:

1. Missing sensitivity List
2. Blocking vs Non blocking assignments
3. Non Standard Verilog coading

### 1.  Missing sensitivity List

Simulator works on activity, the output only change when there is a change in input. To elastrate this phenomenon we are considering a verilog code of 2:1 Mux.

```verilog
module mux (input i0 , input i1 , input sel , output reg y);
always @ (sel)
begin 
       if(sel)
                y <= i1;
        else
                y <= i0;
 end
 endmodule
 ```
So the alwasys block is geting evaluated only when sel is changing. Now when sel is low there are activities on i1 and when sel is high there are activities on i2. But according to our simulator principle if there is no change in sel, the output will not be evalutaed for those changes in i1 and i2. At the simulation it will show it's behaviour like a double edged flop. So how to came over from this scenario ?

```verilog
module mux (input i0 , input i1 , input sel , output reg y);
always @ (*)
begin 
       if(sel)
                y <= i1;
        else
                y <= i0;
 end
 endmodule
 ```
So if we want to simulate the verilog now, the always block will be exicuted for any change of inputs, that may be sel or i1 or i2. In this case the simulator will show it's behaviour like a 2:1 Mux, according to our expectation.

### 2. Blocking vs Non blocking assignments

Before we begin, keep in mind that both statements are indeed contained within **always** blocks. 

#### Non blocking statements

1. At first when always block is entered the expression of the RHS is exicuted and only then it is assigned to LHS
2. all the expressions are evaluated parallelly

#### Blocking statements

1. Executes sequentially, in the order it has been written
2. Caveat 1: order matters
So to explain how the order of blocking statement matters, we have to take two examples:

```verilog
module code (inpu clk, input reset, input d, output reg q);
reg q0;
always@(posedge clk, posedge reset)
begin
if(reset)
begin  
	q0=1'b0;
	q=1'b0;
end
else
begin 
	q=q0;
	q0=d:
end endmodule
```

```verilog
module code (inpu clk, input reset, input d, output reg q);
reg q0;
always@(posedge clk, posedge reset)
begin
if(reset)
begin  
	q0=1'b0;
	q=1'b0;
end
else
begin 
	q0=d;
	q=q0:
end endmodule
```
Comparing the above codes, the first one will show the correct value. But in the second code in the third begin statement instead of q=qo then q0=d but we've q0=d followed by q=q0 so by the time q0 is assigned to q, q0 will be already having the value of d that means there will be only one flop instead of two in the first code of this section.

So to get rid of the problem associated wih the second code we can just use non blocking statements insted of the blocking assignment. As for non blocking statements the order does not matter, it will first execute the RHS and then the value will be assign to the RHS. As shown bellow:  

```verilog
module code (inpu clk, input reset, input d, output reg q);
reg q0;
always@(posedge clk, posedge reset)
begin
if(reset)
begin  
	q0=1'b0;
	q=1'b0;
end
else
begin 
	q0<=d;
	q<=q0:
end endmodule
```
3. Caveat 2: this will cause Synthesis-Simulation Mismatch

To further grasp the problem, we use a different example. 

```verilog
module code(input a, input b, input c, output reg y);
reg q0
always@(*)

begin 
	y=q0&c;
	q0=a|b;
end
endmodule
```
Here in the above code, first y=q0&c is exicuted and then q0=a|b  is exicuted, so for the first step q0 will be taking the previous value to evaluate y then a|b is assigned to q0. So this code mimic a delay or a flop.
As a solution to this problem, we can suggest to order the expressions as shown bellow:

```verilog
module code(input a, input b, input c, output reg y);
reg q0
always@(*)

begin 
	y=q0&c;
	q0=a|b;
end
endmodule
```
So as a conclussion, because of this type of issues it becomes paramount importance to run the GLS on the netlist and match the functionality.

### Lab for GLS Synthesis Simulation Mismatch

In this lab we will the design called ternary_operator_mux.v, that is coded to function properly, below we see the simulation waveforms befor synthesis.
Here is the Verilog code of the design

```verilog
module ternary_operator_mux (input i0 , input i1 , input sel , output y);
	assign y = sel?i1:i0;
	endmodule

```

```terminal
The codes we have to use for the simulation of the RTL file is similar to the previous cases

 iverilog ternary_operator_mux.v tb_ternary_operator_mux.v
 ./a.out
 gtkwave tb_ternary_operator_mux.vcd	
 
```
### Fig: How the Output Waveform looks before Synthesis

![image](https://user-images.githubusercontent.com/61839839/123521129-5dcb1a80-d6d2-11eb-81bf-8c83d4091598.png)

So as we can see the waveform is showing perfect behaviour of an mux. So new we want to Synthesize the design and looking for if there is some changes.

```terminal
yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> read_verilog ternary_operator_mux.v
yosys> synth -top ternary_operator_mux
yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> write_verilog -noattr ternary_operator_mux_net.v
yosys> show
```
![image](https://user-images.githubusercontent.com/61839839/123521529-d501ae00-d6d4-11eb-9e4a-cddb44800be6.png)

So as we can see Yosys also invoked an 2:1 Mux for this design. Now we want to check the GLS, for any un expected synthesis simulation mismatch.
Steps to do GLS:

```terminal 
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_
mux.vcd

```
We can clearly see that, we need to invoke the two extra .v files during the simulation phase. Those files have detaild description about the standard cells, which is used for the synthesis. after opening gtkwave, next to the uut, we can see an expand symbol. This is how we know this is the netlist simulation, there was no such interconnection in RTL simulations.

![image](https://user-images.githubusercontent.com/61839839/123521408-12b20700-d6d4-11eb-9f6a-78eab1cb3740.png)

In the next session the demonstration the mismatch between synthesis and simulation, we use the file bad_mux.v

```verilog
module bad_mux (input i0 , input i1 , input sel , output reg y);
always @ (sel)
begin
	if(sel)
		y <= i1;
	else 
		y <= i0;
end
endmodule
```
This is the same example that we have discussed yearlier. So the alwasys block is geting evaluated only when sel is changing. Now when sel is low there are activities on i1 and when sel is high there are activities on i2. But according to our simulator principle if there is no change in sel, the output will not be evalutaed for those changes in i1 and i2. At the simulation it will show it's behaviour like a double edged flop.
To simulate the design before synthesis and after synthesis the commands should be followed as the previous case.

### Fig: How the Output Waveform looks before Synthesis

![image](https://user-images.githubusercontent.com/61839839/123523980-03878500-d6e5-11eb-9fd0-cc5d9961fae3.png)


So clearly this is not working like Mux. When select is low, i0 should be selected. But after that their is no activity on select, so the activities of the io will not be senced by the always block. Now when select is raised high, i1 is selected. So the output goes high. After that again there is no activity on select but the i1 is toggling, which is not senced by the always block.

Synthesizing and writing to file for GLS

```terminal
yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> read_verilog bad_mux.v
yosys> synth -top bad_mux
yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> write_verilog -noattr bad_mux_net.v
yosys> show
```

![image](https://user-images.githubusercontent.com/61839839/123524763-046ee580-d6ea-11eb-8f17-d8eba8c0b91b.png)

By comparing the RTL sim and the GLS, it is clear that they vary, in the former output y is not following i0 when select is 0.
We can clearly observe that a missing sensitivity list will have this effect. If you're not sure, use always @(*), which indicates the always block will be evaluated every time a signal changes. 

### Fig: How the Output Waveform looks at the time of GLS

![image](https://user-images.githubusercontent.com/61839839/123524805-46982700-d6ea-11eb-8059-5de3fef5f4cf.png)


### Lab for GLS Synthesis Simulation Mismatch due to blocking Statements

As previously stated mismatches can arise due to gaps in logic layout. To illustrate this we will consider a designe named as blocking_caveat.v. The verilog file is given bellow. 

```verilog
module blocking_caveat (input a , input b , input  c, output reg d); 
reg x;
always @ (*)
begin
	d = x & c;
	x = a | b;
end
endmodule
```
Let's assume for a certaintime a and b is low, so according the logical expression x is low. In the next expression x is ANDed with c, so overall output d must be zero for this case. Let's see how the simulation result shows.
To simulate the design before synthesis and after synthesis the commands should be followed as the previous case.

### This is what the RTL simulation looks like for the blocking_caveat.v module

![image](https://user-images.githubusercontent.com/61839839/123525357-a04e2080-d6ed-11eb-855c-c5a364c610b2.png)


But at 100ns for the same combination of inputs, that we have discussed early, we are getting output d = 1. But why so?
The ans is a|b is evaluated using the value it was assigned from the last round of execution, and now it is ANDed with the C value in current execution round. As a result, the computation's outcome will be incorrect. In simulation, X will look like a flop. 

Synthesizing;

```terminal
yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> read_verilog blocking_caveat.v
yosys> synth -top blocking_caveat
yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> write_verilog -noattr blocking_caveat_net.v
yosys> show
```

Now after Synthesis we got a netlist like shown bellow. Clearly there is no latch involved, it is a OR to AND gate which is invoked by Yosys (here a and b are ORed and the output of that is ANDed with c)

![image](https://user-images.githubusercontent.com/61839839/123525971-49971580-d6f2-11eb-83c5-1fbc0dfb03ab.png)


Performing GLS;

To simulate the design before synthesis and after synthesis the commands should be followed as the previous case.

![image](https://user-images.githubusercontent.com/61839839/123525829-3d5e8880-d6f1-11eb-948f-2c9cb34e2f3a.png)

So compairing those two waveform provided for this case we can observe in RTL Simulation; when A is low, B is low, output is low. But for Gate Level Simulation; A is low, B is low, output is high. So the next question comes why so?
The ans is very simple for RTL simulation it's looking at the past value of a and b.

> Point to Remember:
> Blocking statements should be used with caution; if you must use them, do so carefully. 



## 7. Day 5:

### If Statement:

In Verilog, statements are used in innovative ways to describe hardware, similar to how they are used in software programming. The If/Else statement is the most fundamental. The following are some examples of different combinations: 

```verilog
if(expression)
     single statement


if (expression)
  begin
	multiple statements
  end
else (expression)
   begin
	multiple statements
   end


if(expression)
	single statement
else if (expression)
   begin
	multiple statements
   end else
    single statement

```

In Verilog, improper statement use might result in unexpected results. The incomplete if is one of such repercussions. Any if statement that does not finish with a proper else clause is called an incomplere if. As a result we can see Inferred latches in the design, which is hazardous. So as a solution to this, consider the hardware you'll be writing code for when you're writing it. Is "incomplete if" good or bad, this purely depends on  if it is intended or not. For few cases like counter the inferred latch is intentionally introduced. But we can't have inferred latches in a combinational circuit. 

### Case Statement :

```verilog
case(signal)
    <case1> = begin
	     	...
		     end
    <case2> = begin
	          ...
		      end
    default = begin
              ...
              end

```
There are very few caveats with case:

1. Incomplete case statement: It is just like incomplete if. As a result it can cause inferred latches in design, which is hazardous. So as a solution it is suggested to code a default case at the end.
2. Partial assignments in case:  





































## ACKNOWLEDGEMENT
## REFERENCES
  

