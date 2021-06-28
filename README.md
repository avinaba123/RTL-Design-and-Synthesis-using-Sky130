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

1. Some glimps about YOSYS OPEN SYNTHESIS SUITE, SKY130 TECHNOLOGY, iVerilog SIMULATOR and GTKwave
2. Day 1
    - Introduction to Verilog RTL design and Synthesis
      - Introduction to the iverilog open-source simulator and the iverilog design testbench
      - Using iverilog and gtkwave in the lab 
    - Introduction to Yosys and Logic Synthesis
      - Introduction to YOSYS OPEN SYNTHESIS SUITE
      - Introduction to Logic Synthesis 
      - Labs using Yosys and SKY130 PDKs
3. Day 2
4. Day 3
5. Day 4
6. Day 5


## 1. Some glimps about YOSYS OPEN SYNTHESIS SUITE, SKY130 TECHNOLOGY, iVerilog SIMULATOR and GTKwave

### YOSYS OPEN SYNTHESIS SUITE

The synthesiser tool [Yosys](http://www.clifford.at/yosys/) is used to convert RTL to netlist. It's open-source software with an **ISC licence**. Any Synthesizable **Verilog - 2005 design** is processed. Synthesis scripts are used to regulate it. 

### SKY130 TECHNOLOGY

**Cypress Semiconductor** created the [SKY130 hybrid technology](https://skywater-pdk.readthedocs.io/en/latest/), which is 180nm-130nm. The **SKY130 foundry technology** is currently accessible through the SKYWater technology Foundry. The **SkyWater Open Source PDK** is the result of a partnership between **Google** and **SkyWater Technology Foundry** to create an open source process design kit. 

### iVerilog SIMULATOR

**Icarus Verilog**, often known as [iVerilog](http://iverilog.icarus.com/), is a free and open source simulator for Linux, Windows, and Mac OS X that is licenced under the GNU General Public License. The implementation of the verilog hardware description language is known as **Iverilog**. 

### GTKWave

Based on the **GTK library**, [GTK]9http://gtkwave.sourceforge.net/) is a VCD waveform viewer. With regard to **iVerilog**, it is used to see the simulated output of the verilog code and supports VCD and LXT formats for signal dumps. 



## 2. Day 1 :
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

### Introduction to Yosys

### What is a synthesizer?

Synthesizer is the tool, which is used for converting the RTL to netlist with the help of liberty (.lib) file. Yosys is an opensource Synthesizer tool, which is use for this workshop.

### Levels of Abstraction

### Fig: Different levels of abstraction of synthesis

![image](https://user-images.githubusercontent.com/61839839/123626563-98e26080-d82e-11eb-9d35-4d9bc411b0f6.png)

### Flow of Synthesis

![image](https://user-images.githubusercontent.com/61839839/123626798-d9da7500-d82e-11eb-9b76-c922e48e3aeb.png)

Consider we have a design and a liberty file which we will be applying to Yosys to get netlist from the synthesizer.

### What is a netlist?

Netlist is the representation of the RTL design in the form of standard cells provided in the liberty file.

### How to verify our Synthesis

![image](https://user-images.githubusercontent.com/61839839/123627575-d0054180-d82f-11eb-80dc-13aa4177b2f0.png)

Let us consider, we have a netlist and the testbench associated with it. We apply both to the simulator. The only diffrance with the RTL simulation is that here we are running the test bench with netlist as Design Under Test (DUT). And the netlist is going to have all the standard cells are instanciated in it. So we need to tell to iverilog that what is the meaning of those standard cells. Thats why we proided the extra files to iverilog. The output is generated from the simulator is the VCD output file. We load VCD file to the GTKwave to observe the stimulus.

>   Note :
     - The output obtained during synthesis simulation should be same as the output observed during RTL simulation.
     - We can use the same test bench which we had used during RTL simulation.The only diffrance with the RTL simulation is that here we are running the test bench        with netlist as Design Under Test (DUT).
     - The set of primary inputs or primary outputs will remain same between the RTL design and the synthesized Netlist.

### Performing Logic Synthesis

Synthesis is the transformation of an behavioural code into a manufacturable device to carry out an desired functionality.

Now for better illustration, we are considering consider the RTL code below:

```verilog
module sample_code(
input clk, rst,
output result, done);

always@(posedge clk, posedge rst)
if(rst)
...
else
...
endmodule
..................
..................

```
Simply this is a behavioural RTL design but we want a digital circuit looking like bellow

![image](https://user-images.githubusercontent.com/61839839/123629294-bf55cb00-d831-11eb-9c7a-9f8c1cfed8b5.png)

### But the question is how?
Here comes the role of Synthesis.

### What happens in the Synthesis?

Synthesis converts a basic RTL design into a gate-level netlist that includes all of the designer's constraints. In other words, it is the process of converting an abstract design into a correctly implemented chip in terms of logic gates. 

In general synthesis is done in multiple steps:

1. Translating RTL into simple logic gates.
2. Mapping those gates to original technology-dependent logic gates available in the liberty file.
3. Optimizing the generated netlist keeping the constraints set by the designer.

So as an aerial perspective we can say RTL + .lib -> synthesize to digital circuit.

### What is .lib?

.lib file is collection of logical modules, which includes basic logic gates such as NOT AND OR NAND NOR etc. .lib file also contains slow, medium,
and fast versions of different modules. Below, lies an explanation why so.

### Setup time

Setup time described as the amount of time before the clock edge that the data must be stable. It is denoted by Tsetup. Using an analogy, from flip flop A to combinatorial logic block to flip flop B, the clock time must be less than the propagation delay + the time to propagate through the combinatorial circuit + the setup time. The data must be stable for a set amount of time before the clock arrives. Now here comes the utility of Fast cells, it make combinatorial time smaller, to satisfy setup requirements. Clock time indicates maximum clock frequency. A perticular circuit having smaller the clock time, the higher the speed, the better the performance.

```
Freq_clock = 1 / clock_time

```

### Hold time

Hold time is the amount of time after the clock edge that the data must be held stable. It is denoted by Tholdtime. We use faster cells to meet up the requirement of setup times, and slower cells to meet up the requirement of hold times.

### Deep Drive into the differance between Faster cell and Slower cell

- The propagation delay is stated as the time required for the input to cause a change in the output across a logic cell.
- Whenever we look at the faster cell and the slower cell in a digital logic circuit, we look at the load which is actually a capacitor. So the cell delay fully depends on charging rate of the capacitance.
- The faster the capacitor charges / discharges, the higher the current and lower the delay.
- In order to charge/discharge the capacitor fast, we essentially need those transistors which are capable of sourcing more current i.e, it is essentially to be a wide transistor as the current carrying of a transistor is directly proportional to its width.
- So **wider the transistors, lower will be the delay with the cost of more area and more power**.
- **Narrow Transistors will take less area and less power in the cost of more delay**.

### How to select the cell?

- We need to direct the synthesiser to choose the cell flavour that is best for implementing logic circuits. 
- Increased usage of quicker cells leads to a poor circuit in terms of area and power, as well as the possibility of hold time violations. 
- More slower cells may result in a sluggish circuit that fails to fulfil performance requirements. 
- As a result, we must provide guidance to the synthesiser in order for it to select the proper collection of cells, which we refer to as **CONSTRAINTS**. 

### Running Yosys

**Step 1**: Invoke yosys

```
yosys
```
### Fig: Invoking yosys

![image](https://user-images.githubusercontent.com/61839839/123636070-0cd63600-d83a-11eb-9b65-f0980c57beb3.png)


**Step 2**: Read the library

```
                    read_liberty -lib ../my_lib/lib/SKY130_fd_sc_hd_-tt_025C_1v80.lib
                    
                    Where,
                    
                    read_liberty : Read cells from liberty file as modules into current design.
                    
                    -lib : Only create empty blackbox modules.
                    
                    SKY130_fd_sc_hd_-tt_025C_1v80.lib : Name of the library.

```
### Fig: Reading the library

![image](https://user-images.githubusercontent.com/61839839/123638147-72c3bd00-d83c-11eb-94d1-d3a80d6a6260.png)

**Step 3**: Reading the design

```
                    read_verilog good_mux.v
                    
                    Where,
                    
                    read_verilog : Load modules from a Verilog file to the current design.
                    
                    good_mux.v : verilog filename
```

### Fig: Reading the design

![image](https://user-images.githubusercontent.com/61839839/123638308-a999d300-d83c-11eb-985c-600ad0793590.png)

**Step 4**: Synthesize the design

```                    synth -top good_mux
                    
                       Where,
                    
                       synth : This command runs the default synthesis script.
                    
                       -top :  use the specified module as top module
```
### Fig: Pinpoint top-level module

![image](https://user-images.githubusercontent.com/61839839/123638670-0f865a80-d83d-11eb-9101-66f80aeb0fe1.png)

### Fig: Synth infers the following below

![image](https://user-images.githubusercontent.com/61839839/123638742-22992a80-d83d-11eb-9ba3-e3b3eabdd34f.png)

**Step 5**: Generate the netlist

```
                    abc -liberty ../my_lib/lib/SKY130_fd_sc_hd_-tt_025C_1v80.lib
                    
                    Where,
                    
                    abc : Does technology mapping of yosys's internal gate library to a target architecture.
```
### Fig: Generating the netlist

![image](https://user-images.githubusercontent.com/61839839/123638939-583e1380-d83d-11eb-8e8d-b6af6067998c.png)

### Fig: Doing abc infers

![image](https://user-images.githubusercontent.com/61839839/123639017-6a1fb680-d83d-11eb-9d41-eb5a08dbac72.png)

**Step 6**: Look for the graphical version of logic which has been realized

```
                    show
                    
                    Where,
                    
                    show : Generates schematics using graphviz.
```
### Fig: Schematic representation of the realized logic

![image](https://user-images.githubusercontent.com/61839839/123639221-989d9180-d83d-11eb-8872-d7438f6298b4.png)




## 3. Day 2 :

## Timing libs, Hierarchical vs Flat Synthesis & Efficient FlipFlop coding styles

## INTRODUCTION TO TIMING .lib

**Library Name : SKY130_fd_sc_hd__tt_025C_1v80.lib is a High Density Standard Cell Library**

```
     SKY130_fd_sc_hd__tt_025C_1v80.lib
     
     Where,
     
     SKY130 : Technology
     
     tt : Typical Process
     
     025C : Temperature
     
     1v80 : Voltage
     
     SKY130_fd_sc_hd : It is designed for High Density. It contains standard cells that are smaller, utilizing a 0.46 x 2.72um site, equivalent to 9 met1 tracks. This 
                       library enables higher routed gated density, lower dynamic power consumption, and comparable timing and leakage power. As a trade-off it has lower                                drive strength and does not support any drop in replacement medium or high speed library.
                       sky130_fd_sc_hd includes clock-gating cells to reduce active power during non-sleep modes.

                       - Latches and flip-flops have scan equivalents to enable scan chain creation.

                       - Multi-voltage domain library cells are provided.

                       - Routed Gate Density is 160 kGates/mm^2 or better.

                       - leakage @ttleak_1.80v_25C (no body bias) is 0.86 nA / kGate

                       - sky130_fd_sc_XX__buf_16 max cap (ss_1.60v_-40C, in/out tran=1.5ns) is 0.746 pF

                       - Body Bias-able
```

.lib file contains the following informations :

- A detailed timing information of Standard cells, Soft macros, Hard macros.

- Information about the functionality of Standard cells, Soft macros.

- Design rules like max capacitance, max fanout, and max transition.

- Timing information of the cells like Setup, Hold time, abd cell delays.

- Cell delay as a function of input transition and output load.

- It provides information about leakage power for each logical combination of the cell - number of combinations is two ^ the amount of inputs

- It Contains information about power port information, individual pin capacitance, power of each pin / differentiate between power pins and data pins

- Lookup table is used to calculate the cell delay.

- Cell delays are generally obtained by using CCS models, linear delay models,and nonlinear delay models.

- It also contain some PVT condition and derating factor which will provide from foundry only.
- 

The timing and power parameters of Standard cells and Macros are basically stored in the logical library file (.lib), which is in ASCII format.
These parameters were determined by modelling the cell under a range of conditions. 

### Source of Variation

1. PROCESS VARIATION (P) : Deviations in the semiconductor fabrication process are accounted for by this variation. 

2. SUPPLY VOLTAGE (V) : As we get to lower nodes, the supply voltage for a chip will decrease. Assume the chip is powered at 1.2 volts. As a result, there's a risk that the voltage will fluctuate at different times. It may be set to 1.5 or 0.8 volts. We consider voltage fluctuation to deal with this issue. 

3. OPERATING TEMPERATURE (T) : Semiconductors are extremely temperature sensitive. Because it must function efficiently in all locations, the various that occur must be factored. 


## Labs associated with Introduction to timing.lib

In the sirst step we have to open the libreary file

```terminal
  Command used : vim ../my_lib/lib/SKY130_fd_sc_hd__tt_025C_1v80.lib
```
## Fig: Shows the Technology, Delaymodel i.e., Look up table, Unit information

![image](https://user-images.githubusercontent.com/61839839/123650887-a3115880-d848-11eb-91b0-01e9f47da59b.png)

## Different flavours of a cell that exist in the library

![image](https://user-images.githubusercontent.com/61839839/123651082-cd631600-d848-11eb-8985-6f6e5a9fa9ca.png)

![image](https://user-images.githubusercontent.com/61839839/123651115-d6ec7e00-d848-11eb-85a2-bf2493428096.png)

For an example we had considered the **and2_0** and **and2_1**. It is clear from description that and2_0 is manufactured with smaller transistor than and2_1. So we can observe that the delay will be greater in case of and2_0 rather than and2_1. But and2_0 consumes larger area and also the leakage power is also high for and2_0. If we observe carefully we can have detailed information about the leakage power for dirrerent input combination of of a pericular cell. For example in and2_0 there are two input A and B, so we can get detailed information of peakage power for different input combinations like **!A&B**, **!A&!B**, **A&!B**, and **A&B**.

## Hierarchical Synthesis vs Flat Synthesis

1. Hierarchical Synthesis

To illustrate about Hierarchical Synthesis we have considered multiple_modules.v as an example. It is a top level module that comprises of instantiations of lower level modules (one is of AND gate and the other one is OR), which individually implement pieces of functionality. Those individual modules are then tied together to provide a larger piece of funtionality.

```verilog
module sub_module2 (input a, input b, output y);
	assign y = a | b;
endmodule

module sub_module1 (input a, input b, output y);
	assign y = a&b;
endmodule


module multiple_modules (input a, input b, input c , output y);
	wire net1;
	sub_module1 u1(.a(a),.b(b),.y(net1));  //net1 = a&b
	sub_module2 u2(.a(net1),.b(c),.y(y));  //y = net1|c ,ie y = a&b + c;
endmodule
```

![image](https://user-images.githubusercontent.com/61839839/123655849-02716780-d84d-11eb-91fc-d12c005b292d.png)

Running the following commands for synthesis, we had achived with the following outputs

```terminal
yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> read_verilog multiple_modules.v
yosys> synth -top mutiple_modules
yosys> abc -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show multiple_modules
yosys> write_verilog -noattr multiple_modules_hier.v
yosys> !gvim multiple_modules_hier.v
```

![image](https://user-images.githubusercontent.com/61839839/123675853-d06a0080-d860-11eb-8274-cd39bdc69d35.png)

As we can see in the  output doesn't show the AND and the OR gates, it shows them instead as individual modules because they are instances of the submodules. Thus, hierarchies are preserved. Each module and sub module is separate. Hierarchical design abstracts modules.

```verilog
module multiple_modules(a, b, c, y);
  input a;
  input b;
  input c;
  wire net1;
  output y;
  sub_module1 u1 (
    .a(a),
    .b(b),
    .y(net1)
  );
  sub_module2 u2 (
    .a(net1),
    .b(c),
    .y(y)
  );
endmodule

module sub_module1(a, b, y);
  wire _0_;
  wire _1_;
  wire _2_;
  input a;
  input b;
  output y;
  sky130_fd_sc_hd__and2_2 _3_ (
    .A(_0_),
    .B(_1_),
    .X(_2_)
  );
  assign _0_ = b;
  assign _1_ = a;
  assign y = _2_;
endmodule

module sub_module2(a, b, y);
  wire _0_;
  wire _1_;
  wire _2_;
  wire _3_;
  wire _4_;
  input a;
  input b;
  output y;
  sky130_fd_sc_hd__clkinv_1 _5_ (
    .A(_0_),
    .Y(_3_)
  );
  sky130_fd_sc_hd__clkinv_1 _6_ (
    .A(_1_),
    .Y(_4_)
  );
  sky130_fd_sc_hd__nand2_1 _7_ (
    .A(_3_),
    .B(_4_),
    .Y(_2_)
  );
  assign _0_ = b;
  assign _1_ = a;
  assign y = _2_;
endmodule
```

As we can observe in the verilog output, the OR gate is implemented using a NAND gate and two inverters. This is related to the stacked PMOS concept, which results in poor mobility when implemented in this manner. We observe PMOS stacking when employing a NOR gate and two inverters, therefore that technique is avoided.


2. Flat Synthesis

By running the commands after !gvim multiple_modules_hier.v, we obtain a flat synthesis version of the design. the netlist is given bellow

```verilog
module multiple_modules(a, b, c, y);
  wire _00_;
  wire _01_;
  wire _02_;
  wire _03_;
  wire _04_;
  wire _05_;
  wire _06_;
  wire _07_;
  input a;
  input b;
  input c;
  wire net1;
  wire \u1.a ;
  wire \u1.b ;
  wire \u1.y ;
  wire \u2.a ;
  wire \u2.b ;
  wire \u2.y ;
  output y;
  sky130_fd_sc_hd__and2_2 _08_ (
    .A(_00_),
    .B(_01_),
    .X(_02_)
  );
  sky130_fd_sc_hd__clkinv_1 _09_ (
    .A(_03_),
    .Y(_06_)
  );
  sky130_fd_sc_hd__clkinv_1 _10_ (
    .A(_04_),
    .Y(_07_)
  );
  sky130_fd_sc_hd__nand2_1 _11_ (
    .A(_06_),
    .B(_07_),
    .Y(_05_)
  );
  assign \u1.a  = a;
  assign \u1.b  = b;
  assign net1 = \u1.y ;
  assign _00_ = \u1.b ;
  assign _01_ = \u1.a ;
  assign \u1.y  = _02_;
  assign \u2.a  = net1;
  assign \u2.b  = c;
  assign y = \u2.y ;
  assign _03_ = \u2.b ;
  assign _04_ = \u2.a ;
  assign \u2.y  = _05_;
endmodule

```
![image](https://user-images.githubusercontent.com/61839839/123658617-862c5380-d84f-11eb-96b9-021429c99156.png)

Sometimes, rather than synthesising a whole design, we'll need to do it on a module level. This is usually needed when we:

- need multiple instances of same module, synthesize once and replicate as many times as needed
	- Remember to use required module name in synth -top command
	
- adopt the divide and conquer approach
	- If it's a large design and the tool isn't up to the task
	- To do a better job, we give smaller portions
	- Then, at the top level, netlists of submodules are combined to achieve a superior design.



## Flops

### Why we need flops?

Let's assume a combinational circuit, given a set of inputs, after a propagation delay, the changed output is going to reflect. There are very few cases, where because of the propagation delay, the output is going to glitch and it will be unstable.

For example let's consider an 3 input combinational circuit, an 2 input AND gate followed by an 2 input OR gate, where the output of the AND gate is an inputnto the OR gate. The output i0 and y are the result of a&b and i0 | c.

![image](https://user-images.githubusercontent.com/61839839/123662416-115b1880-d853-11eb-80d1-e182c3d6eb5e.png)

Now lets assume a goes 0 to 1, b goes 0 to 1 and c goes 1 to 0 at the same time instance. Now When input C goes low, the OR gate sees it instantly. At that time, at the output y, because of the propagation delay of the AND gate, the value of i0 will not be updated and c will be ORed with the previous value of i0. We obtain time periods throughout the combinatorial circuit switching procedure where the output is not the intended value since the logical expression is still being evaluated, this is referred to as a glitch. The more combinational circuits there are, the more glitchy the outputs will be. When you daisy link combo circuits, the outputs never settle down. 

Now to avid this type of glitches we introduce flops in between the combinational circuits kind of like buffers/storage elements to hold the values (Either logical 1 or logical 0). As the output of rhe flop changes only on the edges of the clock, it can provide stability as while the output of the combinational circuit glitches. The property of retaining the stable values for the next stages, helps us to design circuit to function correctly. So as a conclussion we need to init
the flops else the circuit will have garbage outputs, hence the reset or set pins on the flops comes into the picture.
 
- Set and Reset - they can be synchronous or asynchronous.

- Flip-flops can be triggered at a positive or negative edge of the clock.

- In the case of Async reset: this can reset irrespecctive of the edge of the clock (+/-)

- In the case of Sync reset: flop resets at a predetermined edge (+/-)

- In the sensitivity list for a async flop, we have the clock and the async set/reset triggers.

- In the sensitivity list for a sync flop, we have the clock trigger only.

The if statement is only evaluated when the async reset trigger in the sensitivity list is activated for the flop with both sync and async reset.
At the edge of the clock, the if and else are assessed. 


## Lab for Flip-Flop Simulation

```verilog
module dff_asyncres ( input clk ,  input async_reset , input d , output reg q );
always @ (posedge clk , posedge async_reset)
begin
	if(async_reset)
		q <= 1'b0;
	else	
		q <= d;
end
endmodule
```

![image](https://user-images.githubusercontent.com/61839839/123670070-70705b80-d85a-11eb-8c8f-9b9de2acec15.png)

The example above shows a flip flop with an asynchronous reset. Closer examination of the waveform reveals that after the reset went low, d can occur at any time, while q only changes at the next clock edge. But when the reset goes high the q will be change instantaneously, it will not wait for the clock age. This is how asynchronous reset flop works. 

Let see how the schematic representation of the realized logic is

```terminal
yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> read_verilog dff_asyncres.v
yosys> synth -top dff_asyncres
yosys> dfflibmap -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
```

![image](https://user-images.githubusercontent.com/61839839/123671388-d8737180-d85b-11eb-9624-751089b5f72b.png)

> Note: The reset is an active high in the flip flop implementation below. 
> Yosys has added an inverter and converted it into an active low. 


Now for the next example we will consider the dff_async_set.v module the study the asynchronous set behaviour.

```verilog
module dff_async_set ( input clk ,  input async_set , input d , output reg q );
always @ (posedge clk , posedge async_set)
begin
	if(async_set)
		q <= 1'b1;
	else	
		q <= d;
end
endmodule
```

![image](https://user-images.githubusercontent.com/61839839/123671801-4cae1500-d85c-11eb-9847-1885d69816ee.png)

As we can see we main differance with asynchronous reset is the output is going to be high when set is high.  Closer examination of the waveform reveals that after the set went low, d can occur at any time, while q only changes at the next clock edge. But when the set goes high the q will be change instantaneously, it will not wait for the clock age. This is how asynchronous set flop works. 

Now to study the synchronous behaviour we will consider the verilog module dff_syncres.v. 

```verilog 
module dff_syncres ( input clk , input async_reset , input sync_reset , input d , output reg q );
always @ (posedge clk )
begin
	if (sync_reset)
		q <= 1'b0;
	else	
		q <= d;
end
endmodule
```
![image](https://user-images.githubusercontent.com/61839839/123673108-b67aee80-d85d-11eb-8efa-f4744432eb82.png)

The above is a simulation of a synchronous reset flop. Upon closer examination of the waveform, we can notice that when synchronus reset goes high, q waits for the next clock edge to go low. Reset and d signals are present, reset gets higher precedence because of how we've coded the flop.

Let see how the schematic representation of the realized logic is

```terminal
yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> read_verilog dff_syncres.v
yosys> synth -top dff_syncres
yosys> dfflibmap -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
```

![image](https://user-images.githubusercontent.com/61839839/123674023-c47d3f00-d85e-11eb-8d57-a7f68b691c76.png)

Now to study both asynchronous and a synchronous reset present in a flop, we will consider the verilog module dff_asyncres_syncres.v

```verilog
module dff_asyncres_syncres ( input clk , input async_reset , input sync_reset , input d , output reg q );
always @ (posedge clk , posedge async_reset)
begin
	if(async_reset)
		q <= 1'b0;
	else if (sync_reset)
		q <= 1'b0;
	else	
		q <= d;
end
endmodule
```
![image](https://user-images.githubusercontent.com/61839839/123674960-d6131680-d85f-11eb-8b0f-52c6d91ef283.png)

Upon closer inspection of the waveform, we notice that changes at the d pin are reflected at the q pin at clock edge. As long as async reset is high, q stays at one, when it goes low, q now depends on and follows next the clock edge as soon as set goes back to one, q locks at one irrespective of the clock.








## 4. Day 3:
## Combinatorial and Sequential Optimizations

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



## 5. Day 4:

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



## 6. Day 5:

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
2. Partial assignments in case: To illustrate the partial assignments we consider the verilog module partial_case_assign.v

```verilog
module partial_case_assign (input i0 , input i1 , input i2 , input [1:0] sel, output reg y , output reg x);
always @ (*)
begin
	case(sel)
		2'b00 : begin
			y = i0;
			x = i2;
			end
		2'b01 : y = i1;
		default : begin
		           x = i1;
			   y = i2;
			  end
	endcase
end
endmodule
```
Here we never specified what is the value of y  in case 2; that is called a partial assignment. By doing that we created an unexpected inferred latch. So as a solution it is suggested to assign all the outputs in all the segments of case.

3. Overlapping case statements: 

```verilog
case(signal)
  2'b00 = begin
       	...
  	    end
  2'b01 = begin
      	...
 	    end
  2'b10 = begin
      	...
 	    end
  2'b1? = begin
      	...
 	    end
 endcase

```
As we know when we and dealing with case statements, it checks every case even if it finds a match. In the above mentioned verilog design for the 10 value of the signal, case 3 and 4 both matchs, and as a result it causes unpredictable output. This types of problem is not common with the if staement because it exits at a matched condition.

### Lab: Incomplete if

The following code snippet, given bellow is taken from the labs which describes an incomplete if situation pefectly.

```verilog 
 module incomp_if (input i0 , input i1 , input i2 , output reg y);
   always @ (*)
   begin
     if(i0)
	    y <= i1;
   end
 endmodule

```
As we had discussed earlier, an if statement is always translates to a mux by the sithesizer. So this design should be translated into mux, having i0 as the select signal
i1 as one of the inputs, y as the output and the second input is tied with the output. So it will essentially work as an D latch.

![image](https://user-images.githubusercontent.com/61839839/123560848-70bd1800-d7c2-11eb-807c-41330190fc16.png)

So first we need to check the RTL Simulation before synthesis.

```terminal
 iverilog incomp_if.v tb_incomp_if.v
 ./a.out
 gtkwave tb_incomp_if.vcd
```
![image](https://user-images.githubusercontent.com/61839839/123561164-7ca9d980-d7c4-11eb-8f6b-f541870898c7.png)

After the simulation we can clearly observe when ever our i0 (select)) is high our output follows the i1, but at the time i0 goes low y latches on to something. So when ever i0 goes low the ckt is latching the previous value of the output. There is no alteration in y whenever i0 is low, this is what happens with an incomplete if.

Now we are interested to have a glace into synthesis and observe what happens when this design get synthesized.

```terminal
yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> read_verilog incomp_if.v
yosys> synth -top tb_incomp_if
yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
```
![image](https://user-images.githubusercontent.com/61839839/123561171-86334180-d7c4-11eb-80f8-73f7f003507c.png)

As we are expected after the synthesis it is clear that there is an inferred latch. Even if we had tried to design a mux but the incomplete if caveat caused Yosys to synthesize it using a latch. 

To look more further about the problems associated with incomplete if caveat. In the next session we had considered a verilog module namely incomp_if2.v

```verilog
module incomp_if2 (input i0 , input i1 , input i2 , input i3, output reg y);
always @ (*)
begin
	if(i0)
		y <= i1;
	else if (i2)
		y <= i3;

end
endmodule
```

In this design again it is a clear scenerio of Mux based design. The differance is that here is two mux associated with the design as shown in the figure. 

![image](https://user-images.githubusercontent.com/61839839/123561576-c98eaf80-d7c6-11eb-88f4-5aa768019174.png)

This design is quite similar with the previous case when both i0 and i2 are logic low y latches on to the previous value of y.
First we need to check the RTL Simulation and observe it's behaviour before synthesis.

```terminal
 iverilog incomp_if2.v tb_incomp_if.v
 ./a.out
 gtkwave tb_incomp_if.vcd
```

![image](https://user-images.githubusercontent.com/61839839/123561754-e5df1c00-d7c7-11eb-96c0-2c0541b5befd.png)

So from the simulation report it is clear that when ever i0 is high y exactly follows i1. When i0 is low it looks towords i2. Now when both i0 and i2 both are low it get latched on the previous value of y and at the end when i0 is low and i2 is high y follows i3. 

For our confirmation we are interested to have a deep drive into synthesis part and observe what happens when this design get synthesized.

```terminal
yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> read_verilog incomp_if2.v
yosys> synth -top tb_incomp_if2
yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
```

![image](https://user-images.githubusercontent.com/61839839/123561913-df04d900-d7c8-11eb-9a42-4e657d97990d.png)

So according to our expectation we can clearly observe the latch when i0 and i2 both are low.

> Point to remember :
> 	According to our observation without the else clause, it insists Yosys to synthesize using a latch.


### Lab: Incomplete Case

The following code snippet, given bellow is taken from the labs which describes an incomplete case situation pefectly.

```verilog
module incomp_case (input i0 , input i1 , input i2 , input [1:0] sel, output reg y);
always @ (*)
begin
	case(sel)
		2'b00 : y = i0;
		2'b01 : y = i1;
	endcase
end
endmodule
```
As we have already know that just like an if statement, a case statement also translates into a mux. Here we have inputs i0, i1, i2 there is a 2 bit select line and output  y. When select is 00 we get i0 if select is 01 then we gen i1. Now when select is 10 and 11 there is nothng specified and also default is not present, so for this two cases we well see a latching scenerio. 

![image](https://user-images.githubusercontent.com/61839839/123589995-acc69c00-d807-11eb-939b-61fe781de658.png)

| sel[1] | sel[0] |    y  |
| ------ | ------ | ----- |
|   0    |    0   |   I0  |
|   0    |    1   |   I1  |
|   1    |    0   | Latch |
|   1    |    1   | Latch |

Now we are looking for the functional simulation part to see the behaviour of our design.

```terminal
iverilog incomp_case.v tb_incomp_case.v
./a.out
gtkwave tb_incomp_case.vcd
```
![image](https://user-images.githubusercontent.com/61839839/123590264-0c24ac00-d808-11eb-9015-0e9a7777a649.png)

S0 clearly we we have expected, when select is 0 the output follows i0, when select is 1 its following i1. But when select is 10 and 11 as there is nothng specified and also default is not present it is getting confused and latchinfg the previous value of y.

Let see what happens we we synthesze the design

```teminal
 yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
 yosys> read_verilog incomp_case.v
 yosys> synth -top incomp_case
 yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
 yosys> show
```
![image](https://user-images.githubusercontent.com/61839839/123590798-c4eaeb00-d808-11eb-8123-5017fdc3f586.png)

Here we can observe that there is a muxing logic followed by an latch at the end. This is what we have expected and what we are seeing. This is completely in allignment with our understanding.


### Complete Case:

Here we see the same implementation using a default case. So as a result latch disappears and it becomes an proper combinational logic.

```verilog
module comp_case (input i0 , input i1 , input i2 , input [1:0] sel, output reg y);
always @ (*)
begin
	case(sel)
		2'b00 : y = i0;
		2'b01 : y = i1;
		default : y = i2;
	endcase
end
endmodule
```
![image](https://user-images.githubusercontent.com/61839839/123592829-61ae8800-d80b-11eb-8b18-30b9b3f62955.png)

Here every thing is quite similar with the previous example. The only differance is we have added a default statement 

Now we are looking for the functional simulation part to see the behaviour of our design.

```terminal
iverilog comp_case.v tb_comp_case.v
./a.out
gtkwave tb_comp_case.vcd
```

![image](https://user-images.githubusercontent.com/61839839/123592976-96bada80-d80b-11eb-9f8d-ac204a26e201.png)

See here we are seeing when select is 10 or 11 the output is exaclty following i2 there is no latching action. Otherwise when select is 00 and 01 output follows i0 and i1 respectively.

Let see what happens we we synthesze the design

```teminal
 yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
 yosys> read_verilog comp_case.v
 yosys> synth -top incomp_case
 yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
 yosys> show
```

If you see there is no latch infered, all are purely combinational standard cells are involved in the design. This will effectivly simplified to a 4x1 Mux. This is completely in allignment with our understanding.

![image](https://user-images.githubusercontent.com/61839839/123593651-658eda00-d80c-11eb-93cf-2d2df96d72c7.png)

### Lab: Partial assignment case

In the case of partial assignment, we will encounter a latch as well. The synthesizer doesn't know what to do, so it defaults to a latch. For this example we will consider the verilog design partial_case_assign.v

```verilog
module partial_case_assign (input i0 , input i1 , input i2 , input [1:0] sel, output reg y , output reg x);
always @ (*)
begin
	case(sel)
		2'b00 : begin
			y = i0;
			x = i2;
			end
		2'b01 : y = i1;
		default : begin
		           x = i1;
			   y = i2;
			  end
	endcase
end
endmodule

```
![image](https://user-images.githubusercontent.com/61839839/123596044-45ace580-d80f-11eb-84fd-782a1db585ea.png)

So let's analyse what happens in case of x. For sel 00 output will be i0, but for sel 01 nothing is specified. So for the case sel = 01 there will be latching.

Let's synthesize and check

```terminal
 yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
 yosys> read_verilog partial_case_assign.v
 yosys> synth -top partial_case_assign
 yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
 yosys> show
```
![image](https://user-images.githubusercontent.com/61839839/123596259-8efd3500-d80f-11eb-814b-260b4fab8824.png)

Here we can see in the path of y, there is no latch. But in the path of x there is latch. As we have expected.

>Point to remember:
	Some times we can have a missconception, If we write default statement we can avoid infering of latches. But this not completely correct, there are some caveats

### Lab: Overlapping cases

In case of case even though the condition matches the whole case will be evaluated as the statements are not mutually exclusive. So if there are overlapping condition in case there will be a problem. To demonstrate this concept we will consider a verilog design called bad_case.v

```verilog 
module bad_case (input i0 , input i1, input i2, input i3 , input [1:0] sel, output reg y);
always @(*)
begin
	case(sel)
		2'b00: y = i0;
		2'b01: y = i1;
		2'b10: y = i2;
		2'b1?: y = i3;
		//2'b11: y = i3;
	endcase
end

endmodule
```
In this case we can observe if sel = 00, sel = 01, and sel = 10 y is assigned to i0, i1 and i2 respectively. But 2'b1?: y = i3 condition will be be exicute even sel = 10 or 11. The simulator get confused. In the overlaping case different simulator works different way, as it is not expected to be happen.

Let's synthesize and check

![image](https://user-images.githubusercontent.com/61839839/123598879-9ffb7580-d812-11eb-93cf-939df6a9c410.png)

It is clear when sel is becoming 11, it is neither following i2 or i3 as the conditon, the tool becomes confused and latching to a value of 1. This is not a desired condition at all.

Let's synthesize and check how the synthesizer handle this scenario

```terminal
 yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
 yosys> read_verilog bad_case.v
 yosys> synth -top bad_case
 yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
 yosys> show
```

Performing GLS to check how it behaves after the synthesis

```terminal
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky_fd_sc_hd.v bad_case_net.v tb_bad_case.v
./a.out
gtkwave tb_bad_case.vcd

```

![image](https://user-images.githubusercontent.com/61839839/123599748-90c8f780-d813-11eb-9315-aa199c11430e.png)

So here is no latching asction. After synthesis now the tool is not getting confused when sel = 00 , 01, 10, 11 y is getting i0, i1, i2 and i3. So overlapping is a bad way of coding. When ever we are coding we have to be cautious about all the legs of the case statement should be mutually exclusive.


### For Loops

There are maily two type of looping constructs, with having distinct functionalities.
1. For loop

```verilog
 for loop
```
For evaluating expressions, the simple for loop is utilised. In is used every time inside the 'always' block. 
For further illustration, below we have considered a mux that has been coded with a for loop:

### Lab - Mux using a case statement

```verilog
module mux_generate (input i0 , input i1, input i2 , input i3 , input [1:0] sel  , output reg y);
 wire [3:0] i_int;
 assign i_int = {i3,i2,i1,i0};
 integer k;
  always @ (*)
   begin
    for(k = 0; k < 4; k=k+1) begin
     if(k == sel)
	  y = i_int[k];
     end
    end
 endmodule

```
This is basically a 4:1 mux, having a 4 bit bus i, a 2 bit input Select, an integer namely k. For every time when ever a signal changes, if the integer held in k matches with the two bit input in select, after that whatever be the value of select is, the corresponding input is assigned to output.

Now we will further run RTL Simulation to check the behaviour of this design

```verilog
iverilog mux_generate.v tb_mux_generate.v
./a.out
gtkwave tb_mux_generate.vcd
```
![image](https://user-images.githubusercontent.com/61839839/123604919-ee137780-d818-11eb-9b3b-cfdc6773f5a2.png)
 
 When select is 00, 01, 10 and 11 the y is assigned to i0, i1 i2, and i3. So this 4x1 Mux is working properly.
 
 > So very naturally their will be a few question like Why write it this way? and Why not we are using a case statement?
 
 > The answer is very simple when we are using case statement for this design it scales very badly. For a 256 mux we need to type out 256 cases manully. Whereas,      with that for loop, we only need to change 1 number.
 
 Let's synthesize and compair the GLS output with the RTL simulation
 
 ```terminal
 yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> read_verilog mux_generate.v
yosys> synth -top mux_generate
yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
yosys> write_verilog -noattr mux_generate_net.v

 ```
![image](https://user-images.githubusercontent.com/61839839/123610080-e2767f80-d81d-11eb-9964-79bee7269ceb.png)

Performing GLS

```terminal
 iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky_fd_sc_hd.v mux_generate_net.v tb_mux_generate.v
 ./a.out
 gtkwave tb_mux_generate.vcd
```
![image](https://user-images.githubusercontent.com/61839839/123612685-6467a800-d820-11eb-8eee-418f580c8011.png)


### Lab - Demux using a case statement

In a demultiplexe, there several outputs correspoding to a single input. Depending on the select, one of the outputs will follow input. To check it's functionality we have considered demux_generate.v

```verilog
module demux_generate (output o0 , output o1, output o2 , output o3, output o4, output o5, output o6 , output o7 , input [2:0] sel  , input i);
reg [7:0]y_int;
assign {o7,o6,o5,o4,o3,o2,o1,o0} = y_int;
integer k;
always @ (*)
begin
y_int = 8'b0;
for(k = 0; k < 8; k++) begin
	if(k == sel)
		y_int[k] = i;
end
end
endmodule
```

Now we will further run RTL Simulation to check the behaviour of this design

![image](https://user-images.githubusercontent.com/61839839/123619543-e78bfc80-d826-11eb-9f4d-6859c5930d9b.png)

We can clearly observe when select is 00,01,10,11 exactly o1, o2, o3 and o4 is following y, respectively. Whatever line is selected by sel that perticular line follows the input, remaining all are at zero. 

Let's synthesize and check how the synthesizer handle this scenario

![image](https://user-images.githubusercontent.com/61839839/123619596-f5418200-d826-11eb-9c0c-517d922bc980.png)


2. Generate For loop

```verilog
 generate   
   for loop 
```
The for loop mentioned above is used to instantiate hardware. In is used every time outside the always block, and should not/cannot be used inside an always block.
For further illustration, below we have considered adder circuit that has been coded with a for generate:

### Lab: Ripple Carry Adder

We utilise an RCA when we need to add two numbers. In order to process n bit numbers, an RCA needs n full adders. This is a chain of full adders, where carry is propagated through 1st full adder to the n th full adder. We would have to manually create and link x instances if we used the case statement, which would be inefficient, so we use For Generate loops instead. We can replicate as many times as needed by instantiating one full adder. For this lab we have considered rca.v module.

```verilog
module rca (input [7:0] num1 , input [7:0] num2 , output [8:0] sum);
wire [7:0] int_sum;
wire [7:0]int_co;

genvar i;
generate
	for (i = 1 ; i < 8; i=i+1) begin
		fa u_fa_1 (.a(num1[i]),.b(num2[i]),.c(int_co[i-1]),.co(int_co[i]),.sum(int_sum[i]));
	end

endgenerate
fa u_fa_0 (.a(num1[0]),.b(num2[0]),.c(1'b0),.co(int_co[0]),.sum(int_sum[0]));


assign sum[7:0] = int_sum;
assign sum[8] = int_co[7];
endmodule
```

This is an example of an 8-bit RCA. The way the index is coded in the for block within the generate enclosure allows the instances to link to one other.
It's a more refined look. When we try to simulate the RCA with its test bench, iVerilog gives error, since the complete adder instantiation is in a separate file, so we have to call it as well, just like the primitives. 

```verilog
module fa (input a , input b , input c, output co , output sum);
	assign {co,sum}  = a + b + c ;
endmodule
```
Now we will further run RTL Simulation to check the behaviour of this design

![image](https://user-images.githubusercontent.com/61839839/123622954-8534fb00-d82a-11eb-8884-94789a154804.png)

As we can observe the addition is perfectly done.

Let's synthesize and check how the synthesizer handle this scenario

![image](https://user-images.githubusercontent.com/61839839/123623114-b8778a00-d82a-11eb-81cc-420cca36aece.png)

Performing GLS and comairing with RTL simulation

```terminal
iverilog fa.v rca.v
./a.out
gtkwave tb_rca.vcd
```
![image](https://user-images.githubusercontent.com/61839839/123623327-ea88ec00-d82a-11eb-80b1-5f4ddf705743.png)

As we can see the GLS and the RTL simulation perfectly matched with each other.


## ACKNOWLEDGEMENT
I do want to thank Mr. Kunal Ghosh and Mr. Shon Taware for clearing all my questions and helping me comprehend the ideas. I wish to express my gratitude to the entire team for providing us with such an informative session. I also want to thank Ms. AnaghaGhosh, Mr. Tapan Das, and Mr. Mukesh for their help. 

# Referances
I have also used those materials as referances for understanding those concepts provided in the workshop

1. https://www.vlsisystemdesign.com/?v=a98eef2a3105
2. https://vsdiat.com/
3. http://smdpc2sd.gov.in/downloads/IEP/IEP%208/24-02-18%20Rejender%20pratap.pdf
4. http://www.ecs.umass.edu/ece/tessier/courses/221/lecture/timing-notes.pdf
5. http://home.iitj.ac.in/~sptiwari/DLD/Lecture31_32_DLD.pdf
6. http://www.clifford.at/yosys/documentation.html
