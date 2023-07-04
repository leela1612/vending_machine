//# vending_machine
//vending machine using fsm(verilog).
dut:
module vending(rst,clk,cash_in,out,pre_st,nex_st,change);
input rst;
input clk;
input [3:0]cash_in;
output reg out;
output reg [3:0]change;
output reg [3:0]pre_st,nex_st;
parameter s0=4'b0000;
parameter s5=4'b0101;
parameter s10=4'b1010;
always@(posedge clk)
if(rst)
begin
pre_st<=4'b0000;
out<=0;
change<=4'b0000;
end
else
begin
pre_st<=nex_st;
end
always@(pre_st,nex_st)
begin
case(pre_st)
s0:if(cash_in==0)
begin
nex_st=s0;
out=0;
change=4'b0000;
end
else if(cash_in==4'b0101)
begin
nex_st=s5;
change=4'b0101;
out=0;
end
else
begin
nex_st=s10;
change=4'b1010;
out=0;
end
s5:if(cash_in==0)
begin
nex_st=s0;
out=0;
change=4'b0101;
end
else if(cash_in==4'b0101)
begin
nex_st=s10;
out=0;
end
else if(cash_in==4'b1010)
begin
nex_st=s0;
out=1;
end
s10:if(cash_in==0)
begin
nex_st=s0;
out=0;
change=4'b1010;
end
else if(cash_in==4'b0101)
begin
nex_st=s0;
out=1;
change=4'b0000;
end
else if(cash_in==1010)
begin
nex_st=s0;
out=1;
change=4'b0101;
end
endcase
end
endmodule

testbench:
`timescale 1ns / 1ps

////////////////////////////////////////////////////////////////////////////////
// Company: 
// Engineer:
//
// Create Date:   14:24:51 06/06/2023
// Design Name:   vending
// Module Name:   /home/ise/counter/vendind1_tb.v
// Project Name:  counter
// Target Device:  
// Tool versions:  
// Description: 
//
// Verilog Test Fixture created by ISE for module: vending
//
// Dependencies:
// 
// Revision:
// Revision 0.01 - File Created
// Additional Comments:
// 
////////////////////////////////////////////////////////////////////////////////

module vendind1_tb;

	// Inputs
	reg rst;
	reg clk;
	reg [3:0] cash_in;

	// Outputs
	wire out;
	wire [3:0] pre_st;
	wire [3:0] nex_st;
	wire [3:0] change;

	// Instantiate the Unit Under Test (UUT)
	vending uut (
		.rst(rst), 
		.clk(clk), 
		.cash_in(cash_in), 
		.out(out), 
		.pre_st(pre_st), 
		.nex_st(nex_st), 
		.change(change)
	);
initial clk=1'b0;
always #5 clk=~clk;
initial
begin
rst=1;
#11 rst=0;cash_in=4'b0000;
#10 cash_in=4'b0101;
#10 cash_in=4'b1010;
#100 $stop;
end
endmodule
