# MIXED SIGNAL CIRCUIT SoC DESIGN AND SIMULATION MARATHON USING ESIM & SKY130

# IMPLEMENTATION OF RESISTANCE-TO-DIGITAL CONVERTER
- [ABSTRACT](#abstract)
- [REFERENCE CIRCUIT DIAGRAM](#reference-circuit-diagram)
- [CIRCUIT DETAILS](#circuit-details)
- [PROPOSED METHODOLOGY](#proposed-methodology)
- [EDA TOOLS USED](#eda-tools-used)
- [MAKERCHIP](#makerchip)
- [MAKERCHIP WAVEFORM](#makerchip-waveform)
- [CREATING MODELS OF NGVERI FOR MULTIPLEXER](#creating-models-of-ngveri-for-multiplexer)
- [CREATING MODELS OF NGVERI FOR NAND](#creating-models-of-ngveri-for-nand)
- [CREATING MODELS OF NGVERI FOR PRIORITY ENCODER](#creating-models-of-ngveri-for-priority-encoder)
- [SCHEMATICS](#schematics)
- [OUTPUT WAVEFORM](#output-waveform)
- [AUTHOR](#author)
- [ACKNOWEDGEMENTS](#acknowedgements)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

      
# ABSTRACT
 
 In this development of resistance-to-digital (R2D) Converter circuit is presented. This circuit is designed to determines the value of an external resistor in order to configure various settings within the Integrated Circuits. The R2D circuit provides several advantages for power supplies, such as the elimination of leakage current, smaller solution size, lower design cost, tighter output voltage accuracy and greater design flexibility. We use TDC in our work, TDC (Time to Digital Converter) is used to measure the fractional part of oscillation period, which realize a high resolution and conversion rate with moderate oscillation frequency. 
 
 
# REFERENCE CIRCUIT DIAGRAM
 
 ![image](https://user-images.githubusercontent.com/101329190/194714278-3d604079-f486-4bf7-a09f-3665f4c7dba8.png)

 

# CIRCUIT DETAILS

 As shown in the figure we have analog circuit and digital circuit in which altogether formed a mixed circuit signal A charge pumping circuit is generally uses capacitors as the energy storage element. 
This (R2D) circuit consists of cmos, mux, flipflop, priority encoder, charge pump circuit, resistor. The analog part consists of a resistor that is grounded. Digital circuit consists of priority encoder, mux, flipflop. For these digital blocks that is module is created in Ngveri then the module is used in the circuit. Here we are using VTC (Voltage to Time Converter) that is a delay in our circuit we use it for three time to increase the delay if we use it for one time the delay will be less. And then we have a TDC (Time to Digital Converter) block in which the output of one TDC is given as the input of other then the output is connected to the flip flop and then all these are connected to the priority encoder that is based the priority, we will get the output.  It consists of analog and digital which is altogether a mixed signal circuit is formed. 
The purpose of this project is to determine the value of resistors using end-to- end open-source EDA tools.



# PROPOSED METHODOLOGY

• Step 1 : Writing Verilog code for MUX, NAND with two and three input and for PRIORITY ENCODER & simulating on Makerchip

• Step 2 : Model creation on NgVeri

• Step 3 : Schematics creation

• Step 4 : Creating Netlist

• Step 5 : Setting simulation instance parameters on KicadToNgspice converter

• Step 6: Simulation and Verification of results


# EDA TOOLS USED

eSim

It is an Open Source EDA developed by FOSSEE, IIT Bombay. It is used for electronic circuit simulation. It is made by the combination of two software namely NgSpice and KiCAD. 
For more details refer: https://esim.fossee.in/home

NgSpice

It is an Open Source Software for Spice Simulations. 
For more details refer: http://ngspice.sourceforge.net/docs.html

Makerchip

It is an Online Web Browser IDE for Verilog/System-verilog/TL-Verilog Simulation. 
For more details refer: https://www.makerchip.com/

Verilator:

It is a tool which converts Verilog code to C++ objects. 
Refer: https://www.veripool.org/verilator/

# VERILOG

# Verilog code for MUX

<img width="212" alt="image" src="https://user-images.githubusercontent.com/101329190/194766136-a1842826-2a7f-4be7-bf1f-44f04d66ff04.png">

# VERILOG CODE FOR NAND WITH TWO INPUT

<img width="151" alt="image" src="https://user-images.githubusercontent.com/101329190/194766169-24c87cf4-6bb4-445c-bd6b-96f7af34bf21.png">

# VERILOG CODE FOR NAND WITH THREE INPUT

<img width="162" alt="image" src="https://user-images.githubusercontent.com/101329190/194766229-f4910948-bfb6-43ba-98a8-ad4b0ece1b50.png">

# VERILOG CODE FOR PRIORITY ENCODER

<img width="370" alt="image" src="https://user-images.githubusercontent.com/101329190/194766291-11a0458e-3f1b-4851-96c2-b9f54ea0e0d8.png">

# MAKERCHIP

# MULTIPLEXERS


\TLV_version 1d: tl-x.org
\SV
/* verilator lint_off UNUSED*/  /* verilator lint_off DECLFILENAME*/  /* verilator lint_off BLKSEQ*/  /* verilator lint_off WIDTH*/  /* verilator lint_off SELRANGE*/  /* verilator lint_off PINCONNECTEMPTY*/  /* verilator lint_off DEFPARAM*/  /* verilator lint_off IMPLICIT*/  /* verilator lint_off COMBDLY*/  /* verilator lint_off SYNCASYNCNET*/  /* verilator lint_off UNOPTFLAT */  /* verilator lint_off UNSIGNED*/  /* verilator lint_off CASEINCOMPLETE*/  /* verilator lint_off UNDRIVEN*/  /* verilator lint_off VARHIDDEN*/  /* verilator lint_off CASEX*/  /* verilator lint_off CASEOVERLAP*/  /* verilator lint_off PINMISSING*/   /* verilator lint_off BLKANDNBLK*/  /* verilator lint_off MULTIDRIVEN*/    /* verilator lint_off WIDTHCONCAT*/  /* verilator lint_off ASSIGNDLY*/  /* verilator lint_off MODDUP*/  /* verilator lint_off STMTDLY*/  /* verilator lint_off LITENDIAN*/  /* verilator lint_off INITIALDLY*/  

//Your Verilog/System Verilog Code Starts Here:
module yo_mux(Y, D0, D1, S);
output Y;
input D0, D1, S;
wire T1, T2, Sbar;
and (T1, D1, S), (T2, D0, Sbar);
not (Sbar, S);
or (Y, T1, T2);
endmodule

//Top Module Code Starts here:
	module top(input logic clk, input logic reset, input logic [31:0] cyc_cnt, output logic passed, output logic failed);
		logic  Y;//output
		logic  D0;//input
		logic  D1;//input
		logic  S;//input
//The $random() can be replaced if user wants to assign values
		assign D0 = $random();
		assign D1 = $random();
		assign S = $random();
		yo_mux yo_mux(.Y(Y), .D0(D0), .D1(D1), .S(S));
	
\TLV
//Add \TLV here if desired                                     
\SV
endmodule
  
   
 # MAKERCHIP WAVEFORM
 
 ![image](https://user-images.githubusercontent.com/101329190/194714513-f513497a-b785-4b19-bcfc-47e884fb509f.png)
 
 # CREATING MODELS OF NGVERI FOR MULTIPLEXER 
 
 ![image](https://user-images.githubusercontent.com/101329190/194714595-f258307d-3380-4401-b936-6f8a15b36259.png)

# NAND WITH TWO INPUT 

\TLV_version 1d: tl-x.org
\SV
/* verilator lint_off UNUSED*/  /* verilator lint_off DECLFILENAME*/  /* verilator lint_off BLKSEQ*/  /* verilator lint_off WIDTH*/  /* verilator lint_off SELRANGE*/  /* verilator lint_off PINCONNECTEMPTY*/  /* verilator lint_off DEFPARAM*/  /* verilator lint_off IMPLICIT*/  /* verilator lint_off COMBDLY*/  /* verilator lint_off SYNCASYNCNET*/  /* verilator lint_off UNOPTFLAT */  /* verilator lint_off UNSIGNED*/  /* verilator lint_off CASEINCOMPLETE*/  /* verilator lint_off UNDRIVEN*/  /* verilator lint_off VARHIDDEN*/  /* verilator lint_off CASEX*/  /* verilator lint_off CASEOVERLAP*/  /* verilator lint_off PINMISSING*/   /* verilator lint_off BLKANDNBLK*/  /* verilator lint_off MULTIDRIVEN*/   /* verilator lint_off WIDTHCONCAT*/  /* verilator lint_off ASSIGNDLY*/  /* verilator lint_off MODDUP*/  /* verilator lint_off STMTDLY*/  /* verilator lint_off LITENDIAN*/  /* verilator lint_off INITIALDLY*/   

//Your Verilog/System Verilog Code Starts Here:
module yo_nand(a,b,c);
input a,b;
output c;
nand n1(c,a,b);
endmodule

//Top Module Code Starts here:
	module top(input logic clk, input logic reset, input logic [31:0] cyc_cnt, output logic passed, output logic failed);
		logic  a;//input
		logic  b;//input
		logic  c;//output
//The $random() can be replaced if user wants to assign values
		assign a = $random();
		assign b = $random();
		yo_nand yo_nand(.a(a), .b(b), .c(c));
	
\TLV
//Add \TLV here if desired                                     
\SV
endmodule

# MAKERCHIP WAVEFORM
 
![image](https://user-images.githubusercontent.com/101329190/194714716-ae53ea10-c566-48e8-8f25-6942f682f81c.png)

# CREATING MODELS OF NGVERI FOR NAND
 
![image](https://user-images.githubusercontent.com/101329190/194714753-6e7d1b25-9570-4377-bd29-b16a7bdbbaea.png)

# NAND WITH THREE INPUT

\TLV_version 1d: tl-x.org
\SV
/* verilator lint_off UNUSED*/  /* verilator lint_off DECLFILENAME*/  /* verilator lint_off BLKSEQ*/  /* verilator lint_off WIDTH*/  /* verilator lint_off SELRANGE*/  /* verilator lint_off PINCONNECTEMPTY*/  /* verilator lint_off DEFPARAM*/  /* verilator lint_off IMPLICIT*/  /* verilator lint_off COMBDLY*/  /* verilator lint_off SYNCASYNCNET*/  /* verilator lint_off UNOPTFLAT */  /* verilator lint_off UNSIGNED*/  /* verilator lint_off CASEINCOMPLETE*/  /* verilator lint_off UNDRIVEN*/  /* verilator lint_off VARHIDDEN*/  /* verilator lint_off CASEX*/  /* verilator lint_off CASEOVERLAP*/  /* verilator lint_off PINMISSING*/   /* verilator lint_off BLKANDNBLK*/  /* verilator lint_off MULTIDRIVEN*/   /* verilator lint_off WIDTHCONCAT*/  /* verilator lint_off ASSIGNDLY*/  /* verilator lint_off MODDUP*/  /* verilator lint_off STMTDLY*/  /* verilator lint_off LITENDIAN*/  /* verilator lint_off INITIALDLY*/  

//Your Verilog/System Verilog Code Starts Here:
module sam_nand(a,b,c,d);
input a,b,c;
output d;
nand n1(d,a,b,c);
endmodule

//Top Module Code Starts here:
	module top(input logic clk, input logic reset, input logic [31:0] cyc_cnt, output logic passed, output logic failed);
		logic  a;//input
		logic  b;//input
		logic  c;//input
		logic  d;//output
//The $random() can be replaced if user wants to assign values
		assign a = $random();
		assign b = $random();
		assign c = $random();
		sam_nand sam_nand(.a(a), .b(b), .c(c), .d(d));
	
\TLV
//Add \TLV here if desired                                     
\SV
endmodule

# MAKERCHIP WAVEFORM
 
![image](https://user-images.githubusercontent.com/101329190/194714840-705f4c49-d6bb-48f3-a01b-13e2aa40f06b.png)

# CREATING MODELS OF NGVERI FOR NAND
 
![image](https://user-images.githubusercontent.com/101329190/194714859-0831dedc-313e-44d1-9480-a606dcecd320.png)


# PRIORITY ENCODER
\TLV_version 1d: tl-x.org
\SV
/* verilator lint_off UNUSED*/  /* verilator lint_off DECLFILENAME*/  /* verilator lint_off BLKSEQ*/  /* verilator lint_off WIDTH*/  /* verilator lint_off SELRANGE*/  /* verilator lint_off PINCONNECTEMPTY*/  /* verilator lint_off DEFPARAM*/  /* verilator lint_off IMPLICIT*/  /* verilator lint_off COMBDLY*/  /* verilator lint_off SYNCASYNCNET*/  /* verilator lint_off UNOPTFLAT */  /* verilator lint_off UNSIGNED*/  /* verilator lint_off CASEINCOMPLETE*/  /* verilator lint_off UNDRIVEN*/  /* verilator lint_off VARHIDDEN*/  /* verilator lint_off CASEX*/  /* verilator lint_off CASEOVERLAP*/  /* verilator lint_off PINMISSING*/    /* verilator lint_off BLKANDNBLK*/  /* verilator lint_off MULTIDRIVEN*/ /* verilator lint_off WIDTHCONCAT*/  /* verilator lint_off ASSIGNDLY*/  /* verilator lint_off MODDUP*/  /* verilator lint_off STMTDLY*/  /* verilator lint_off LITENDIAN*/  /* verilator lint_off INITIALDLY*/   

//Your Verilog/System Verilog Code Starts Here:
`timescale 1ns / 1ps

module yogapriya(d_out, d_in);
   output [2:0] d_out;
   input [7:0] d_in ;

assign d_out = (d_in[7] ==1'b1 ) ? 3'b111:
               (d_in[6] ==1'b1 ) ? 3'b110:
               (d_in[5] ==1'b1 ) ? 3'b101:
               (d_in[4] ==1'b1) ? 3'b100:
               (d_in[3] ==1'b1) ? 3'b011:
               (d_in[2] ==1'b1) ? 3'b010:
               (d_in[1] ==1'b1) ? 3'b001:
               (d_in[0] ==1'b1) ? 3'b000: 3'bxxx;

endmodule

//Top Module Code Starts here:
	module top(input logic clk, input logic reset, input logic [31:0] cyc_cnt, output logic passed, output logic failed);
		logic  [2:0] d_out;//output
		logic  [7:0] d_in;//input
//The $random() can be replaced if user wants to assign values
		assign d_in = $random();
		yogapriya yogapriya(.d_out(d_out), .d_in(d_in));
	
\TLV
//Add \TLV here if desired                                     
\SV
endmodule

# MAKERCHIP WAVEFORM
 
![image](https://user-images.githubusercontent.com/101329190/194714976-63002d3f-1a06-4596-834e-7e5326ddf55b.png)

# CREATING MODELS OF NGVERI FOR PRIORITY ENCODER

![image](https://user-images.githubusercontent.com/101329190/194715005-4b4f99ab-71dd-4201-85d3-f6817cf8426c.png)

 
# NETLIST
 
https://github.com/YogapriyaB/R2D/blob/d4aa10dc4023b0310272deb854ac5ab442dfac89/esim_project_files/yoga.cir.out 
 
# SCHEMATICS
 
 ![image](https://user-images.githubusercontent.com/101329190/194715096-f0704ed7-78fc-4434-8d63-1debe5154cfc.png)
 
 ![image](https://user-images.githubusercontent.com/101329190/194715221-95bcb47e-2a24-4e2f-a599-6b9cd8c72be4.png)
 
 ![image](https://user-images.githubusercontent.com/101329190/194715374-264c6903-a885-4ace-8aad-5392d51ed987.png)
 
 ![image](https://user-images.githubusercontent.com/101329190/194715407-371b1216-ff9a-46f2-8885-ca6651b06bd2.png)
 
 ![image](https://user-images.githubusercontent.com/101329190/194715454-778aac3e-8af6-44b5-8cfc-8d3b7c34ad2d.png)


 # EXPECTED OUTPUT WAVEFORM
 
 ![1](https://user-images.githubusercontent.com/101329190/194715520-4fcd8cf7-6aad-4e38-aa0a-f5512cd8d583.PNG)

  
 # AUTHOR
 
    B. Yogapriya
    Third year, B.E, ECE
    Easwari Engineering College 
    Mail : yogapriyab2001@gmail.com
    
    
    
 # ACKNOWEDGEMENTS
 
 1. Kunal Ghosh (Co-Founder, VLSI System Design Pvt. Ltd.)
 2. FOSSEE, IIT Bombay
 3. Steve Hoover (Founder, Redwood EDA)
 4. Sumanto Kar (eSim Team, FOSSEE, IIT Bombay)
