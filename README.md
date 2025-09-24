# SIMULATION AND IMPLEMENTATION OF 4:1 MULTIPLEXER

## AIM
To design and simulate a 4:1 Multiplexer (MUX) using Verilog HDL in four different modeling styles—Gate-Level, Data Flow, Behavioral, and Structural—and to verify its functionality through a testbench using the Vivado 2023.1 simulation environment. The experiment aims to understand how different abstraction levels in Verilog can be used to describe the same digital logic circuit and analyze their performance.

## APPARATUS REQUIRED
- **Vivado 2023.1**

## Procedure

1. Open **Vivado 2023.1**.  
2. Create a **New RTL Project** and give a name (e.g., `Mux4_to_1`).  
3. Add/create your Verilog files and testbench.  
4. Select an FPGA part (e.g., `xc7a35ticsg324-1L`).  
5. Run **Synthesis** to check for errors.  
6. Run **Simulation** → **Run Behavioral Simulation**.  
7. Observe the waveforms of inputs and outputs.  
8. Adjust simulation time if needed (e.g., 1000ns).  
9. Save the project and take screenshots of results.  
10. Close simulation.  

---

## Logic Diagram
![image](https://github.com/user-attachments/assets/d4ab4bc3-12b0-44dc-8edb-9d586d8ba856)

---

## Truth Table
![image](https://github.com/user-attachments/assets/c850506c-3f6e-4d6b-8574-939a914b2a5f)

---

## Verilog Code

### 4:1 MUX Gate-Level Implementation
```verilog
module mux41(I,S,Y);
input [3:0]I;
input [1:0]S;
output Y;
wire [3:0]w;
and (w[0], I[0], ~S[1], ~S[0]); 
and (w[1], I[1], ~S[1], S[0]); 
and (w[2], I[2], S[1], ~S[0]); 
and (w[3], I[3], S[1], S[0]);  
or (Y, w[0], w[1], w[2], w[3]);
endmodule
```
### 4:1 MUX Gate-Level Implementation- Testbench
```verilog
module MUX_41_tb;
reg [3:0]I;
reg [1:0]S;
wire Y;
mux41 uut(I,S,Y);
initial
begin
I=4'b1001;
S=2'b00;
#10;
$display("selection is %b  %b ,output  : %b",S[1],S[0],Y);
S=2'b01;
#10;
$display("selection is %b  %b ,output  : %b",S[1],S[0],Y);
S=2'b10;
#10;
$display("selection is %b  %b ,output  : %b",S[1],S[0],Y);
S=2'b11;
#10;
$display("selection is %b  %b ,output  : %b",S[1],S[0],Y);
$finish;
end
endmodule
```
## Simulated Output Gate Level Modelling
<img width="2880" height="1800" alt="Screenshot 2025-09-24 082812" src="https://github.com/user-attachments/assets/782ee401-69e9-4f45-a6d8-9f732e4c438a" />

---
### 4:1 MUX Data flow Modelling
```verilog
module mux41(I, S, Y);
input [3:0] I;     
input [1:0] S;   
output Y;        
assign Y = (~S[1] & ~S[0] & I[0]) |
             (~S[1] &  S[0] & I[1]) |
             ( S[1] & ~S[0] & I[2]) |
             ( S[1] &  S[0] & I[3]);
endmodule

```
### 4:1 MUX Data flow Modelling- Testbench
```verilog
module MUX_41_tb;
reg [3:0]I;
reg [1:0]S;
wire Y;
mux41 uut(I,S,Y);
initial
begin
I=4'b1001;
S=2'b00;
#10;
$display("selection is %b  %b ,output  : %b",S[1],S[0],Y);
S=2'b01;
#10;
$display("selection is %b  %b ,output  : %b",S[1],S[0],Y);
S=2'b10;
#10;
$display("selection is %b  %b ,output  : %b",S[1],S[0],Y);
S=2'b11;
#10;
$display("selection is %b  %b ,output  : %b",S[1],S[0],Y);
$finish;
end
endmodule

```
## Simulated Output Dataflow Modelling
<img width="2880" height="1800" alt="image" src="https://github.com/user-attachments/assets/6e0d8ab2-dc66-4a23-84a3-bcec3012f0ff" />

---
### 4:1 MUX Behavioral Implementation
```verilog
`timescale 1ns / 1ps
module MUX_41(I,S,Y);
input [3:0]I;
input [1:0]S;
output reg Y;
always @(I,S)
    begin
        case(S)
        2'b00:Y=I[0];
        2'b01:Y=I[1];
        2'b10:Y=I[2];
        2'b11:Y=I[3];
        endcase
    end
endmodule
```
### 4:1 MUX Behavioral Modelling- Testbench
```verilog
module MUX_41_tb;
reg [3:0]I;
reg [1:0]S;
wire Y;
MUX_41 uut(I,S,Y);
initial
begin
I=4'b1001;
S=2'b00;
#10;
$display("selection is %b  %b ,output  : %b",S[1],S[0],Y);
S=2'b01;
#10;
$display("selection is %b  %b ,output  : %b",S[1],S[0],Y);
S=2'b10;
#10;
$display("selection is %b  %b ,output  : %b",S[1],S[0],Y);
S=2'b11;
#10;
$display("selection is %b  %b ,output  : %b",S[1],S[0],Y);
$finish;
end
endmodule
```
## Simulated Output Behavioral Modelling
<img width="2880" height="1800" alt="image" src="https://github.com/user-attachments/assets/a59f7f26-4533-421b-befc-d949f7254877" />

### 4:1 MUX Structural Implementation

![image](https://github.com/user-attachments/assets/eea81c2c-7dfa-43aa-9cea-1ab4ed54db6c)


```verilog
`timescale 1ns / 1ps
module mux2to1(Y, I0, I1, S);
  input I0, I1, S;
  output Y;
  wire w1, w2, w3;
  not (w1, S);
  and (w2, I0, w1);
  and (w3, I1, S);
  or  (Y, w2, w3);
endmodule
module mux4to1 (Y, I, S);
  output Y;
  input [3:0] I;
  input [1:0] S;
  wire w1, w2;
  mux2to1 m1(w1, I[0], I[1], S[0]);
  mux2to1 m2(w2, I[2], I[3], S[0]);
  mux2to1 m3(Y, w1, w2, S[1]);
endmodule

```
### Testbench Implementation
```verilog

module mux4to1_tb;
reg [3:0]I;
reg [1:0]S;
wire Y;
mux4to1 uut(Y,I,S);
initial
begin
I=4'b0111;
S=2'b00;
#10;
$display("selection is %b %b ,output : %b",S[1],S[0],Y);
S=2'b01;
#10; 
$display("selection is %b %b ,output : %b",S[1],S[0],Y);
S=2'b10;
#10;
$display("selection is %b %b ,output : %b",S[1],S[0],Y);
S=2'b11;
#10;
$display("selection is %b %b ,output : %b",S[1],S[0],Y);
$finish;
end
endmodule
```
## Simulated Output Structural Modelling
<img width="2880" height="1800" alt="image" src="https://github.com/user-attachments/assets/0153004f-7ec8-4b23-a76b-ee143c0ac96d" />

---
### CONCLUSION

In this experiment, a 4:1 Multiplexer was successfully designed and simulated using Verilog HDL across four different modeling styles: Gate-Level, Data Flow, Behavioral, and Structural.The simulation results verified the correct functionality of the MUX, with all implementations producing identical outputs for the given input conditions.

