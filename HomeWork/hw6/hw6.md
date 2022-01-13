### 1.RAM64
```{r}
**
 * Memory of 64 registers, each 16 bit-wide. Out holds the value
 * stored at the memory location specified by address. If load==1, then 
 * the in value is loaded into the memory location specified by address 
 * (the loaded value will be emitted to out from the next time step onward).
 */

CHIP RAM64 {
    IN in[16], load, address[6];
    OUT out[16];

    PARTS:
    // Put your code here:
    DMux8Way(in=load,sel=address[3..5],a=l0,b=l1,c=l2,d=l3,e=l4,f=l5,g=l6,h=l7);
    RAM8(in=in,load=l0,address=address[0..2],out=out0);
    RAM8(in=in,load=l1,address=address[0..2],out=out1);
    RAM8(in=in,load=l2,address=address[0..2],out=out2);
    RAM8(in=in,load=l3,address=address[0..2],out=out3);
    RAM8(in=in,load=l4,address=address[0..2],out=out4);
    RAM8(in=in,load=l5,address=address[0..2],out=out5);
    RAM8(in=in,load=l6,address=address[0..2],out=out6);
    RAM8(in=in,load=l7,address=address[0..2],out=out7);
    Mux8Way16(a=out0,b=out1,c=out2,d=out3,e=out4,f=out5,g=out6,h=out7,sel=address[3..5],out=out);
}
```
### 2.RAM512
```{r}
/**
 * Memory of 512 registers, each 16 bit-wide. Out holds the value
 * stored at the memory location specified by address. If load==1, then 
 * the in value is loaded into the memory location specified by address 
 * (the loaded value will be emitted to out from the next time step onward).
 */

CHIP RAM512 {
    IN in[16], load, address[9];
    OUT out[16];

    PARTS:
    // Put your code here:
    DMux8Way(in=load,sel=address[6..8],a=l0,b=l1,c=l2,d=l3,e=l4,f=l5,g=l6,h=l7);
    RAM64(in=in,load=l0,address=address[0..5],out=out0);
    RAM64(in=in,load=l1,address=address[0..5],out=out1);
    RAM64(in=in,load=l2,address=address[0..5],out=out2);
    RAM64(in=in,load=l3,address=address[0..5],out=out3);
    RAM64(in=in,load=l4,address=address[0..5],out=out4);
    RAM64(in=in,load=l5,address=address[0..5],out=out5);
    RAM64(in=in,load=l6,address=address[0..5],out=out6);
    RAM64(in=in,load=l7,address=address[0..5],out=out7);
    Mux8Way16(a=out0,b=out1,c=out2,d=out3,e=out4,f=out5,g=out6,h=out7,sel=address[6..8],out=out);
}
```
### 3.RAM4K
```{r}
**
 * Memory of 4K registers, each 16 bit-wide. Out holds the value
 * stored at the memory location specified by address. If load==1, then 
 * the in value is loaded into the memory location specified by address 
 * (the loaded value will be emitted to out from the next time step onward).
 */

CHIP RAM4K {
    IN in[16], load, address[12];
    OUT out[16];

    PARTS:
    // Put your code here:
    DMux8Way(in=load,sel=address[9..11],a=l0,b=l1,c=l2,d=l3,e=l4,f=l5,g=l6,h=l7);
    RAM512(in=in,load=l0,address=address[0..8],out=out0);
    RAM512(in=in,load=l1,address=address[0..8],out=out1);
    RAM512(in=in,load=l2,address=address[0..8],out=out2);
    RAM512(in=in,load=l3,address=address[0..8],out=out3);
    RAM512(in=in,load=l4,address=address[0..8],out=out4);
    RAM512(in=in,load=l5,address=address[0..8],out=out5);
    RAM512(in=in,load=l6,address=address[0..8],out=out6);
    RAM512(in=in,load=l7,address=address[0..8],out=out7);
    Mux8Way16(a=out0,b=out1,c=out2,d=out3,e=out4,f=out5,g=out6,h=out7,sel=address[9..11],out=out);
}
```
### 4.RAM16K
```{r}
/**
 * Memory of 16K registers, each 16 bit-wide. Out holds the value
 * stored at the memory location specified by address. If load==1, then 
 * the in value is loaded into the memory location specified by address 
 * (the loaded value will be emitted to out from the next time step onward).
 */

CHIP RAM16K {
    IN in[16], load, address[14];
    OUT out[16];

    PARTS:
    // Put your code here:
    DMux4Way(in=load,sel=address[12..13],a=l0,b=l1,c=l2,d=l3);
    RAM4K(in=in,load=l0,address=address[0..11],out=out0);
    RAM4K(in=in,load=l1,address=address[0..11],out=out1);
    RAM4K(in=in,load=l2,address=address[0..11],out=out2);
    RAM4K(in=in,load=l3,address=address[0..11],out=out3);
    Mux4Way16(a=out0,b=out1,c=out2,d=out3,sel=address[12..13],out=out);
}
```
### 5.PC
```{r}
/**
 * A 16-bit counter with load and reset control bits.
 * if      (reset[t] == 1) out[t+1] = 0
 * else if (load[t] == 1)  out[t+1] = in[t]
 * else if (inc[t] == 1)   out[t+1] = out[t] + 1  (integer addition)
 * else                    out[t+1] = out[t]
 */

CHIP PC {
    IN in[16],load,inc,reset;
    OUT out[16];

    PARTS:
    // Put your code here:
    Inc16(in=PC,out=In16);
    Mux16(a=PC,b=In16,sel=inc,out=Mux1);
    Mux16(a=Mux1,b=in,sel=load,out=Mux2);
    Mux16(a=Mux2,b=false,sel=reset,out=Mux3);
    Register(in=Mux3,load=true,out=PC,out=out);
}
```
