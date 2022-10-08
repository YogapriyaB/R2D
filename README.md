# Mixed Signal Circuit Design and Simulation Marathon

# IMPLEMENTATION-OF-RESISTANCE TO DIGITAL CONVERTER
- [ABSTRACT](#abstract)
- [REFERENCE CIRCUIT DIAGRAM](#reference-circuit-diagram)
- [CIRCUIT DETAILS](#circuit-details)
- [PROPOSED METHODOLOGY](#proposed-methodology)
- [EDA TOOLS USED](#eda-tools-used)
- [MAKERCHIP](#makerchip)
- [MAKERCHIP WAVEFORM](#makerchip-waveform)
- [CREATING MODELS OF NGVERI](#creating-models-of-ngveri)
- [SCHEMATICS](#schematics)
- [OUTPUT WAVEFORMS](#output-waveforms)
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

• Step 1 : Writing Verilog code for RVMYTH MIXED SIGNAL(RISC-V) & simulating on Makerchip

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
 
 
 
 ![image](https://user-images.githubusercontent.com/101329190/158007378-16fd4dda-62e1-4f61-93fa-7ef8b7f1f6bd.png)


 ![image](https://user-images.githubusercontent.com/101329190/158007391-e7c5c384-ed61-44c0-ba0c-76d600979693.png)

  
 # NETLIST
 
 https://github.com/YogapriyaB/rvmyth/blob/main/esim_project_files/mixed.cir.out

  
 # CREATING MODELS OF NGVERI
 
 <img width="960" alt="Ngveri" src="https://user-images.githubusercontent.com/101329190/157720443-e1d8b271-34d7-4865-8709-d44935108f3b.png">


 # SCHEMATICS
 
 <img width="960" alt="schematic diagram" src="https://user-images.githubusercontent.com/101329190/157716665-9ffba1a5-446f-43f7-88fa-c3f9fc007325.png">
 

 # OUTPUT WAVEFORMS
 
 <img width="960" alt="output v_cout" src="https://user-images.githubusercontent.com/101329190/157720830-c8e92401-7b3d-4505-9517-fb7b23312f9b.png">
 
 
 
 <img width="959" alt="output vclk" src="https://user-images.githubusercontent.com/101329190/157720811-cbf2e499-1739-4852-98c1-2a9e3344dbab.png">
 
 
 
 ![image](https://user-images.githubusercontent.com/101329190/157805639-4902a5e3-75c8-496c-a71d-8a38fd923ca2.png)
 
 

  <img width="955" alt="output result" src="https://user-images.githubusercontent.com/101329190/157720819-5ee59fbe-ec6c-4ace-b771-7f81a2763539.png">
  
  
  
 # GAW WAVEFORMS
 
 <img width="960" alt="gaw output" src="https://user-images.githubusercontent.com/101329190/157721382-f9025518-3cc7-4de0-b77d-95922807910b.png">


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
