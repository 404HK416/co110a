ALU
```{r}
/**
 * The ALU (Arithmetic Logic Unit).
 * Computes one of the following functions:
 * x+y, x-y, y-x, 0, 1, -1, x, y, -x, -y, !x, !y,
 * x+1, y+1, x-1, y-1, x&y, x|y on two 16-bit inputs, 
 * according to 6 input bits denoted zx,nx,zy,ny,f,no.
 * In addition, the ALU computes two 1-bit outputs:
 * if the ALU output == 0, zr is set to 1; otherwise zr is set to 0;
 * if the ALU output < 0, ng is set to 1; otherwise ng is set to 0.
 */

// Implementation: the ALU logic manipulates the x and y inputs
// and operates on the resulting values, as follows:
// if (zx == 1) set x = 0        // 16-bit constant
// if (nx == 1) set x = !x       // bitwise not
// if (zy == 1) set y = 0        // 16-bit constant
// if (ny == 1) set y = !y       // bitwise not
// if (f == 1)  set out = x + y  // integer 2's complement addition
// if (f == 0)  set out = x & y  // bitwise and
// if (no == 1) set out = !out   // bitwise not
// if (out == 0) set zr = 1
// if (out < 0) set ng = 1

CHIP ALU {
    IN  
        x[16], y[16],  // 16-bit inputs        
        zx, // zero the x input?
        nx, // negate the x input?
        zy, // zero the y input?
        ny, // negate the y input?
        f,  // compute out = x + y (if 1) or x & y (if 0)
        no; // negate the out output?

    OUT 
        out[16], // 16-bit output
        zr, // 1 if (out == 0), 0 otherwise
        ng; // 1 if (out < 0),  0 otherwise

    PARTS:
   // Put you code here:
   Mux16(a=x,b=false,sel=zx,out=outx1);//zx
   Mux16(a=y,b=false,sel=zy,out=outy1);//zy
   Not16(in=outx1,out=notx);
   Not16(in=outy1,out=noty);
   Mux16(a=outx1,b=notx,sel=nx,out=outx2);//nx
   Mux16(a=outy1,b=noty,sel=ny,out=outy2);//ny
   Add16(a=outx2,b=outy2,out=x2andy2);
   And16(a=outx2,b=outy2,out=y2andx2);
   Mux16(a=y2andx2,b=x2andy2,sel=f,out=outf);  
   Not16(in=outf,out=notf);
   
   //nostat
   Mux16(a=outf,b=notf,sel=no,out=out,out[0..7]=HighOut,out[8..15]=LowOut,out[15]=ng);
   
   //Or16way
   Or8Way(in=HighOut,out=High1);
   Or8Way(in=LowOut,out=Low1);
   Or(a=High1,b=Low1,out=outHL);

   Not(in=outHL,out=zr);
   
}
```
<img src="hw4.jpg"/>
