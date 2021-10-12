---
title: "Design of adders using Verilog"
categories:
  - Computer Science
tags:
  - Hardware
  - Verilog
  - Circuit
last_modified_at: 2021-10-12
author_profile: true
sitemap:
  changefreq: daily
  priority: 1.0
---

Let's design adder circuits with Verilog. There are several options below to execute codes.

- Icarus Verilog (I use this in this post): [http://bleyer.org/icarus/](http://bleyer.org/icarus/)
- Vivado: [https://www.xilinx.com/support/download.html](https://www.xilinx.com/support/download.html)
- Compile and Execute Verilog Online: [https://www.tutorialspoint.com/compile_verilog_online.php](https://www.tutorialspoint.com/compile_verilog_online.php)

I made two different examples each adder: using (1) dataflow and (2) gatelevel modeling.<br/>

## Half adders

- S = A ⊕ B
- C = AB

```verilog
// dataflow half adder
module dataflow_half_adder (A, B, S, C);
input  A, B;
output S, C;

assign S = A ^ B;
assign C = A && B;

endmodule
```

```verilog
// gatelevel half adder
module gatelevel_half_adder (A, B, S, C);
input  A, B;
output S, C;

xor G1(S, A, B);  // G1(<output>, <input>, <input>, ...)
and G1(C, A, B);

endmodule
```

## Full adders

- S = x ⊕ y ⊕ z
- C = xy + yz + zx = (x ⊕ y)z + xy

```verilog
// dataflow full adder
module dataflow_full_adder (x, y, z, S, C);
input  x, y, z;
output S, C;

wire   w1, w2, w3;

assign w1 = x ^ y;
assign S = w1 ^ z;
assign w2 = w1 && z;
assign w3 = x && y;
assign C = w2 || w3;

endmodule
```

```verilog
// gatelevel full adder
module gatelevel_full_adder (x, y, z, S, C);
input  x, y, z;
output S, C;

wire   w1, w2, w3;

xor G1(w1, x, y);  // G1(<output>, <input>, <input>, ...)
xor G2(S, w1, z);
and G3(w2, w1, z);
and G4(w3, x, y);
or  G5(C, w2, w3);

endmodule
```

## Testbench

```verilog
module test_bench; // stimulus name is free

reg x, y, z;  // input
wire S1, C1, S2, C2; // output

dataflow_half_adder HA (x, y, S1, C1); // dataflow_half_adder.v port
dataflow_full_adder FA (x, y, z, S2, C2); // dataflow_half_adder.v port

// named mapping (.<module>(<current>))
// dataflow_half_adder HA (.A(x), .B(y), .S(S1), .C(C1)); // dataflow_half_adder.v port
// dataflow_full_adder FA (.x(x), .y(y), .z(z), .S(S2), .C(C2)); // dataflow_half_adder.v port

// gatelevel_half_adder HA (x, y, S1, C1); // gatelevel_half_adder.v port
// gatelevel_full_adder FA (x, y, z, S2, C2); // gatelevel_half_adder.v port

initial begin // simulation start 000 to 111
x=0; y=0; z=0;
#10 x=0; y=0; z=1; // assign value after 10ns
#10 x=0; y=1; z=0;
#10 x=0; y=1; z=1;
#10 x=1; y=0; z=0;
#10 x=1; y=0; z=1;
#10 x=1; y=1; z=0;
#10 x=1; y=1; z=1;
end

initial begin
// print values every 10ns
$monitor("x y z = %d %d %d C1 S1 = %d %d C2 S2 = %d %d\n", x, y, z, C1, S1, C2, S2);
end

endmodule
```

Open the terminal and execute commands below.

```
>>> iverilog -o output .\dataflow_half_adder.v .\dataflow_full_adder.v .\test_bench.v
>>> vvp .\output
```

---

💬 _Any comments and suggestions will be appreciated._
