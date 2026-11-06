# Exp-4-Mux-Demux
# Design and Verification of 2:1 Multiplexer and 1:2 Demultiplexer
## Aim
To design and verify a 2:1 Multiplexer and a 1:2 Demultiplexer using Verilog HDL.
## Theory / Synopsis
A 2:1 Multiplexer (MUX) selects one of two data inputs, A or B, and routes it to a single output Y based on the value of the select line S. A 1:2 Demultiplexer (DEMUX) performs the reverse operation: it routes a single data input D to one of two outputs, Y0 or Y1, based on the select line S.
Boolean Functions
### 2:1 Multiplexer:
•	Y = S'.A + S.B
### 1:2 Demultiplexer:
•	Y0 = S'.D
•	Y1 = S.D
## Files to be created
<img width="720" height="156" alt="dllcd 4(3)" src="https://github.com/user-attachments/assets/8b94f1b7-fbe2-40d5-8603-0cc28965ae1e" />


DESIGN /RTL 

// gedit mux_demux.v
module mux2to1 (
    input  wire A,      // input line 0
    input  wire B,      // input line 1
    input  wire S,      // select line
    output wire Y       // output
);
    assign Y = (~S & A) | (S & B);
endmodule
 
module demux1to2 (
    input  wire D,      // data input
    input  wire S,      // select line
    output wire Y0,     // output line 0
    output wire Y1      // output line 1
);
    assign Y0 = (~S) & D;
    assign Y1 = S & D;
endmodule

## **Testbench Program

// gedit tb4.v
module tb4;
    reg  A, B, S, D;
    wire Y;
    wire Y0, Y1;
 
    // Instantiate the 2:1 Multiplexer
    mux2to1 mux_uut (
        .A(A),
        .B(B),
        .S(S),
        .Y(Y)
    );
 
    // Instantiate the 1:2 Demultiplexer
    demux1to2 demux_uut (
        .D(D),
        .S(S),
        .Y0(Y0),
        .Y1(Y1)
    );
 
    initial begin
        // ---- VCD dump setup ----
        $dumpfile("mux_demux.vcd");   // name of the VCD file to be generated
        $dumpvars(0, tb4);             // dump all signals in this testbench hierarchy
 
        // ---- Apply all input combinations ----
        $monitor("Time=%0t S=%b A=%b B=%b Y=%b | D=%b Y0=%b Y1=%b",
                  $time, S, A, B, Y, D, Y0, Y1);
 
        // MUX select = 0 -> Y should follow A
        S = 0; A = 0; B = 0; D = 0; #10;
        S = 0; A = 0; B = 1; D = 1; #10;
        S = 0; A = 1; B = 0; D = 1; #10;
        S = 0; A = 1; B = 1; D = 0; #10;
 
        // MUX select = 1 -> Y should follow B
        S = 1; A = 0; B = 0; D = 0; #10;
        S = 1; A = 0; B = 1; D = 1; #10;
        S = 1; A = 1; B = 0; D = 1; #10;
        S = 1; A = 1; B = 1; D = 0; #10;
 
        #10 $finish;
    end
endmodule



## Truth Table

### 2:1 Multiplexer
<img width="720" height="116" alt="dlcd 4 (1)" src="https://github.com/user-attachments/assets/baa4e283-418e-44ca-a8ee-79c0c10a00bc" />



### 1:2 Demultiplexer
<img width="719" height="269" alt="dlcd 4(2)" src="https://github.com/user-attachments/assets/e5205475-37a6-4754-acd8-c808e92a823a" />


## Simulation Procedure

STEP 1 – Open Terminal
Open a terminal in the experiment folder.

STEP 2 – Load Synopsys Environment
source /synopsys/start.sh

STEP 3 – Compile Using VCS
vcs mux_demux.v mux_demux_tb.v -full64
If compilation is successful, VCS generates the simulation executable:
simv

STEP 4 – Run Simulation
./simv
The terminal displays the input combinations and corresponding outputs Y, Y0, and Y1.
A VCD waveform file is also generated:
mux_demux.vcd

STEP 5 – Open DVE

dve -full64
Other option
dve -full64 &
A DVE environment will open.
DVE Waveform Verification
In DVE:
•	Open the testbench hierarchy.
•	Locate the signals:
o	A
o	B
o	S
o	Y
o	D
o	Y0
o	Y1
•	Add the signals to the waveform window.
•	Run/inspect the waveform.
•	Verify that Y = A whenever S = 0, and Y = B whenever S = 1.
•	Verify that Y0 = D and Y1 = 0 whenever S = 0, and Y1 = D and Y0 = 0 whenever S = 1.
•	Confirm that Y0 and Y1 are never both high at the same time, since only one output is active per select value.
The waveform should agree with the truth table.

## Expected Result

The 2:1 Multiplexer and 1:2 Demultiplexer were realized using Verilog HDL:

•	Y = S'.A + S.B
•	Y0 = S'.D, Y1 = S.D

The design was compiled and simulated using Synopsys VCS, and the functionality was verified using DVE waveform analysis, matching the expected truth table.

## Output

<img width="1600" height="846" alt="dlcd 4 output" src="https://github.com/user-attachments/assets/31f1ba3f-ee77-4ab1-a1bd-a06c44bfd8d9" />
<img width="1600" height="842" alt="dlcd ouput 2" src="https://github.com/user-attachments/assets/347d28a9-5eaa-491f-bcdd-f808171cfe5a" />





## Result

Thus, the 2:1 Multiplexer and 1:2 Demultiplexer were designed, implemented using Verilog HDL, successfully simulated using Synopsys VCS, and verified using Synopsys DVE.



