### 1.Not
```{r}
/**
 * Not gate:
 * out = not in
 */

CHIP Not {
    IN in;
    OUT out;

    PARTS:
    // Put your code here:
    Nand(a=in, b=in, out=out);
}
```
### 2.And
```{r}
/**
 * And gate: 
 * out = 1 if (a == 1 and b == 1)
 *       0 otherwise
 */

CHIP And {
    IN a, b;
    OUT out;

    PARTS:
    // Put your code here:
    Nand(a=a,b=b,out=ab);
    Not(in=ab,out=out);

}
```
### 3.Or
```{r}
 /**
 * Or gate:
 * out = 1 if (a == 1 or b == 1)
 *       0 otherwise
 */

CHIP Or {
    IN a, b;
    OUT out;

    PARTS:
    // Put your code here:
    Not(in=a,out=nota);
    Not(in=b,out=notb);
    Nand(a=nota,b=notb,out=out);
}
```
### 4.Xor
```{r}
/**
 * Exclusive-or gate:
 * out = not (a == b)
 */

CHIP Xor {
    IN a, b;
    OUT out;

    PARTS:
    // Put your code here:
    Not(in=a,out=nota);
    Not(in=b,out=notb);
    And(a=nota,b=b,out=out1);
    And(a=a,b=notb,out=out2);
    Or(a=out1,b=out2,out=out);
}
```
### 5.Mux
```{r}
/** 
 * Multiplexor:
 * out = a if sel == 0
 *       b otherwise
 */

CHIP Mux {
    IN a, b, sel;
    OUT out;

    PARTS:
    // Put your code here:
    Not(in=sel,out=notsel);
    And(a=a,b=notsel,out=out1);
    And(a=b,b=sel,out=out2);
    Or(a=out1,b=out2,out=out);
}
```
### 6.DMux
```{r}
/**
 * Demultiplexor:
 * {a, b} = {in, 0} if sel == 0
 *          {0, in} if sel == 1
 */

CHIP DMux {
    IN in, sel;
    OUT a, b;

    PARTS:
    // Put your code here:
    Not(in=sel,out=notsel);
    And(a=in,b=notsel,out=a);
    And(a=in,b=sel,out=b);
}
```
<img src="hw1.jpg"/>
