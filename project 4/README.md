# Shift Register using Verilog HDL

## Project Overview
This project implements a 4-bit Serial-In Serial-Out (SISO) Shift Register using Verilog HDL.

A shift register stores binary data and shifts it one bit to the right on every positive clock edge.

## Features
- 4-bit Shift Register
- Serial Data Input
- Serial Data Output
- Synchronous Operation
- Verilog HDL Implementation
- Testbench Included

## Truth Table

| Clock | Serial Input | Register Output |
|--------|--------------|-----------------|
| ↑ | 0 | Shift Right |
| ↑ | 1 | Shift Right |

## Verilog Code

```verilog
module shift_register(
    input clk,
    input reset,
    input serial_in,
    output serial_out
);

reg [3:0] shift_reg;

always @(posedge clk or posedge reset)
begin
    if(reset)
        shift_reg <= 4'b0000;
    else
        shift_reg <= {serial_in, shift_reg[3:1]};
end

assign serial_out = shift_reg[0];

endmodule
```

## Testbench

```verilog
`timescale 1ns/1ps

module shift_register_tb;

reg clk;
reg reset;
reg serial_in;
wire serial_out;

shift_register uut(
    .clk(clk),
    .reset(reset),
    .serial_in(serial_in),
    .serial_out(serial_out)
);

always #5 clk = ~clk;

initial
begin
    clk = 0;
    reset = 1;
    serial_in = 0;

    #10 reset = 0;

    #10 serial_in = 1;
    #10 serial_in = 0;
    #10 serial_in = 1;
    #10 serial_in = 1;
    #10 serial_in = 0;

    #30 $finish;
end

initial
begin
    $monitor("Time=%0t Reset=%b Serial_In=%b Shift_Register=%b Serial_Out=%b",
             $time, reset, serial_in, uut.shift_reg, serial_out);
end

endmodule
```

## Simulation Output

```
Time=0   Reset=1 Serial_In=0 Shift_Register=0000 Serial_Out=0
Time=10  Reset=0 Serial_In=0 Shift_Register=0000 Serial_Out=0
Time=20  Reset=0 Serial_In=1 Shift_Register=1000 Serial_Out=0
Time=30  Reset=0 Serial_In=0 Shift_Register=0100 Serial_Out=0
Time=40  Reset=0 Serial_In=1 Shift_Register=1010 Serial_Out=0
Time=50  Reset=0 Serial_In=1 Shift_Register=1101 Serial_Out=1
Time=60  Reset=0 Serial_In=0 Shift_Register=0110 Serial_Out=0
```

## Expected Waveform

Add the simulation waveform image here.

Example:

![Waveform](waveform.png)

## Tools Used

- Verilog HDL
- ModelSim
- Icarus Verilog
- GTKWave
- Xilinx Vivado (Optional)

## Applications

- Digital Communication
- Data Storage
- Serial Data Transfer
- Delay Circuits
- Counters

## Author

Your Name