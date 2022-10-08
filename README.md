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
 
 ![image](https://user-images.githubusercontent.com/101329190/157805477-f4102d7c-9eac-4caa-ada4-fd066115ac9f.png)
 


# CIRCUIT DETAILS

 As shown in the figure we have analog circuit and digital circuit in which altogether formed a mixed circuit signal in the RVMYTH mixed signal circuit.
The analog part consists of a clockwise generator connected to resistor capacitor and finally all this grounded. Digital circuit consists of digital board. In between analog and digital circuit ADC and DAC bridges are used as a connector between analog and digital circuit which together forms a mixed signal circuit. 
The purpose of this project is to integrate rvmyth (RISC-V) with digital to analog converter (DAC) and perform simulation using end-to- end open-source EDA tool.


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

\m4_TLV_version 1d -p verilog --bestsv --noline: tl-x.org
\SV
   m4_include_lib
   m4_ifelse_block(M4_MAKERCHIP, 1,['
   m4_makerchip_module   
   '],['

//////////////////////////////////////////////////////////////////////////////////
    // Mixed Signal Design Hackathon
    //
    // (Organized by FOSSEE IIT Bombay, VSD Corp. Pvt.Ltd.)
   //////////////////////////////////////////////////////////////////////////////////

   module yogapriya_rvmyth (input wire clk, input wire reset, output reg [7:0] out);
   ']
   )   
\TLV
   // External to function:
   m4_asm(ADDI, r10, r0, 1010)          // Initialize r10 to 10.
   m4_asm(ADD, r14, r0, r0)             // Initialize r14 to 0.      
   m4_asm(ADDI, r12, r0, 11111111)     // Store count of 255s in register r12.
   m4_asm(ADDI, r13, r0, 1)             // Initialize intermediate register r13 with 1
   // Function:
   // Loop_INC:
   m4_asm(ADD, r14, r14, r13)           // Add r14 and r13 and write to r14
   m4_asm(BLT, r14, r12, 1111111111100) // If r14 is less than a2, branch to label named Loop_INC
   // Loop_DEC:
   m4_asm(SUB, r14, r14, r13)           // Sub r13 from r14 and write to r14
   m4_asm(BLT, r0, r14, 1111111111100)  // If r14 is less than r0, branch to label named Loop_DEC
   m4_asm(ADD,r0,r0,r0)                 // Dummy Instruction
   m4_asm(SUB,r10,r10,r13)              // Dummy Instruction
   
   m4_asm(BLT,r0,r10,1111111101000)     // Outer loop till r10 becomes zero.

   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)

   |cpu
      @0
         $reset = *reset;
      
      //Fetch
         // Next PC
         $pc[31:0] = (>>1$reset) ? 32'b0 : 
                     (>>3$valid_taken_br) ? >>3$br_tgt_pc : 
                     (>>3$valid_load) ? >>3$inc_pc : 
                     (>>3$valid_jump && >>3$is_jal) ? >>3$br_tgt_pc :
                     (>>3$valid_jump && >>3$is_jalr) ? >>3$jalr_tgt_pc : >>1$inc_pc;
         
         $imem_rd_en = !$reset;
         $imem_rd_addr[31:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         
      @1         
         $instr[31:0] = $imem_rd_data[31:0];
         $inc_pc[31:0] = $pc + 32'd4;  
      // Decode   
         $is_i_instr = $instr[6:2] == 5'b00000 ||
                       $instr[6:2] == 5'b00001 ||
                       $instr[6:2] == 5'b00100 ||
                       $instr[6:2] == 5'b00110 ||
                       $instr[6:2] == 5'b11001;
         $is_r_instr = $instr[6:2] == 5'b01011 ||
                       $instr[6:2] == 5'b10100 ||
                       $instr[6:2] == 5'b01100 ||
                       $instr[6:2] == 5'b01101;                       
         $is_b_instr = $instr[6:2] == 5'b11000;
         $is_u_instr = $instr[6:2] == 5'b00101 ||
                       $instr[6:2] == 5'b01101;
         $is_s_instr = $instr[6:2] == 5'b01000 ||
                       $instr[6:2] == 5'b01001;
         $is_j_instr = $instr[6:2] == 5'b11011;
         
         $imm[31:0] = $is_i_instr ? { {21{$instr[31]}} , $instr[30:20] } :
                      $is_s_instr ? { {21{$instr[31]}} , $instr[30:25] , $instr[11:8] , $instr[7] } :
                      $is_b_instr ? { {20{$instr[31]}} , $instr[7] , $instr[30:25] , $instr[11:8] , 1'b0} :
                      $is_u_instr ? { $instr[31:12] , 12'b0} : 
                      $is_j_instr ? { {12{$instr[31]}} , $instr[19:12] , $instr[20] , $instr[30:21] , 1'b0} : 32'b0;
         
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         $rs1_valid = $is_r_instr || $is_s_instr || $is_b_instr || $is_i_instr;
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         $funct3_valid = $is_r_instr || $is_s_instr || $is_b_instr || $is_i_instr;
         $funct7_valid = $is_r_instr;
         
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
            
         $opcode[6:0] = $instr[6:0];
         
         $dec_bits[10:0] = {$funct7[5],$funct3,$opcode};
         
         // Branch Instruction
         $is_beq = $dec_bits[9:0] == 10'b000_1100011;
         $is_bne = $dec_bits[9:0] == 10'b001_1100011;
         $is_blt = $dec_bits[9:0] == 10'b100_1100011;
         $is_bge = $dec_bits[9:0] == 10'b101_1100011;
         $is_bltu = $dec_bits[9:0] == 10'b110_1100011;
         $is_bgeu = $dec_bits[9:0] == 10'b111_1100011;
         
         // Arithmetic Instruction
         $is_add = $dec_bits == 11'b0_000_0110011;
         $is_addi = $dec_bits[9:0] == 10'b000_0010011;
         $is_or = $dec_bits == 11'b0_110_0110011;
         $is_ori = $dec_bits[9:0] == 10'b110_0010011;
         $is_xor = $dec_bits == 11'b0_100_0110011;
         $is_xori = $dec_bits[9:0] == 10'b100_0010011;
         $is_and = $dec_bits == 11'b0_111_0110011;
         $is_andi = $dec_bits[9:0] == 10'b111_0010011;
         $is_sub = $dec_bits == 11'b1_000_0110011;
         $is_slti = $dec_bits[9:0] == 10'b010_0010011;
         $is_sltiu = $dec_bits[9:0] == 10'b011_0010011;
         $is_slli = $dec_bits == 11'b0_001_0010011;
         $is_srli = $dec_bits == 11'b0_101_0010011;
         $is_srai = $dec_bits == 11'b1_101_0010011;
         $is_sll = $dec_bits == 11'b0_001_0110011;
         $is_slt = $dec_bits == 11'b0_010_0110011;
         $is_sltu = $dec_bits == 11'b0_011_0110011;
         $is_srl = $dec_bits == 11'b0_101_0110011;
         $is_sra = $dec_bits == 11'b1_101_0110011;
         
         // Load Instruction
         $is_load = $dec_bits[6:0] == 7'b0000011;
         
         // Store Instruction
         $is_sb = $dec_bits[9:0] == 10'b000_0100011;
         $is_sh = $dec_bits[9:0] == 10'b001_0100011;
         $is_sw = $dec_bits[9:0] == 10'b010_0100011;
         
         // Jump Instruction
         $is_lui = $dec_bits[6:0] == 7'b0110111;
         $is_auipc = $dec_bits[6:0] == 7'b0010111;
         $is_jal = $dec_bits[6:0] == 7'b1101111;
         $is_jalr = $dec_bits[9:0] == 10'b000_1100111;
         
         $is_jump = $is_jal || $is_jalr;
         
      @2   
      // Register File Read
         $rf_rd_en1 = $rs1_valid;
         ?$rf_rd_en1
            $rf_rd_index1[4:0] = $rs1[4:0];
         
         $rf_rd_en2 = $rs2_valid;
         ?$rf_rd_en2
            $rf_rd_index2[4:0] = $rs2[4:0];
            
      // Branch Target PC       
         $br_tgt_pc[31:0] = $pc + $imm;
      
      // Jump Target PC
         $jalr_tgt_pc[31:0] = $src1_value + $imm;
         
      // Input signals to ALU
         $src1_value[31:0] = ((>>1$rd == $rs1) && >>1$rf_wr_en) ? >>1$result : $rf_rd_data1[31:0];
         $src2_value[31:0] = ((>>1$rd == $rs2) && >>1$rf_wr_en) ? >>1$result : $rf_rd_data2[31:0];
         
      @3   
         
      // ALU
         $sltu_result = $src1_value < $src2_value ;
         $sltiu_result = $src1_value < $imm ;
         
         $result[31:0] = $is_addi ? $src1_value + $imm :
                         $is_add ? $src1_value + $src2_value : 
                         $is_or ? $src1_value | $src2_value : 
                         $is_ori ? $src1_value | $imm :
                         $is_xor ? $src1_value ^ $src2_value :
                         $is_xori ? $src1_value ^ $imm :
                         $is_and ? $src1_value & $src2_value :
                         $is_andi ? $src1_value & $imm :
                         $is_sub ? $src1_value - $src2_value :
                         $is_slti ? (($src1_value[31] == $imm[31]) ? $sltiu_result : {31'b0,$src1_value[31]}) :
                         $is_sltiu ? $sltiu_result :
                         $is_slli ? $src1_value << $imm[5:0] :
                         $is_srli ? $src1_value >> $imm[5:0] :
                         $is_srai ? ({{32{$src1_value[31]}}, $src1_value} >> $imm[4:0]) :
                         $is_sll ? $src1_value << $src2_value[4:0] :
                         $is_slt ? (($src1_value[31] == $src2_value[31]) ? $sltu_result : {31'b0,$src1_value[31]}) :
                         $is_sltu ? $sltu_result :
                         $is_srl ? $src1_value >> $src2_value[5:0] :
                         $is_sra ? ({{32{$src1_value[31]}}, $src1_value} >> $src2_value[4:0]) :
                         $is_lui ? ({$imm[31:12], 12'b0}) :
                         $is_auipc ? $pc + $imm :
                         $is_jal ? $pc + 4 :
                         $is_jalr ? $pc + 4 : 
                         ($is_load || $is_s_instr) ? $src1_value + $imm : 32'bx;
                         
      // Register File Write
         $rf_wr_en = ($rd_valid && $valid && $rd != 5'b0) || >>2$valid_load;
         ?$rf_wr_en
            $rf_wr_index[4:0] = !$valid ? >>2$rd[4:0] : $rd[4:0];
      
         $rf_wr_data[31:0] = !$valid ? >>2$ld_data[31:0] : $result[31:0];
      
      // Branch
         $taken_br = $is_beq ? ($src1_value == $src2_value) :
                     $is_bne ? ($src1_value != $src2_value) :
                     $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                     $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                     $is_bltu ? ($src1_value < $src2_value) :
                     $is_bgeu ? ($src1_value >= $src2_value) : 1'b0;
                     
         $valid_taken_br = $valid && $taken_br;
         
      // Load
         $valid_load = $valid && $is_load;
         $valid = !(>>1$valid_taken_br || >>2$valid_taken_br || >>1$valid_load || >>2$valid_load || >>1$valid_jump || >>2$valid_jump);
      
      // Jump
         $valid_jump = $valid && $is_jump;
                  
      @4
         $dmem_rd_en = $valid_load;
         $dmem_wr_en = $valid && $is_s_instr;
         $dmem_addr[3:0] = $result[5:2];
         $dmem_wr_data[31:0] = $src2_value[31:0];
         
      @5   
         $ld_data[31:0] = $dmem_rd_data[31:0];                     
         
      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.

         `BOGUS_USE($is_beq $is_bne $is_blt $is_bge $is_bltu $is_bgeu $is_sb $is_sh $is_sw)
      
   m4_ifelse_block(M4_MAKERCHIP, 1,['
   *passed = !clk || *cyc_cnt > 2100;
   *failed = !clk || 1'b0;
   '],['
   \SV_plus
      always @ (posedge clk) begin
         *out = |cpu/xreg[14]>>5$value;
      end   
   '])
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   //  o CPU visualization
   |cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@2, @3)  // Args: (read stage, write stage) - if equal, no register bypass is required
      m4+dmem(@4)    // Args: (read/write stage)
   m4_ifelse_block(M4_MAKERCHIP, 1,['
   m4+cpu_viz(@4)   
   '],[''])  
   //m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic   
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
