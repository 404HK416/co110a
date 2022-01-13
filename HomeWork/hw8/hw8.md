### 1.CPU
```{r}
 CHIP CPU {

    IN  inM[16],         // M value input  (M = contents of RAM[A])
        instruction[16], // Instruction for execution
        reset;           // Signals whether to re-start the current
                         // program (reset==1) or continue executing
                         // the current program (reset==0).

    OUT outM[16],        // M value output
        writeM,          // Write to M? 
        addressM[15],    // Address in data memory (of M)
        pc[15];          // address of next instruction

    PARTS:
    // Put your code here:
    //判斷輸入是否為A instruction[15] == 0>> A      instruction[15] == 1>> C
    Mux16(a=instruction, b=AluOP, sel=instruction[15], out=MUX0);

    //載入A暫存器
    //如果是A, load1 = 1
    //如果是C AND instruction[5] == 1, load1 = 1

    And(a=instruction[5], b=instruction[15], out=and1);
    Not(in=instruction[15], out=not1);
    Or(a=not1, b=and1, out=load1);

    //儲存地址在A暫存器

    ARegister(in=MUX0, load=load1, out=A, out[0..14]=addressM);

    //instruction[12] = a
    // 如果 a=1, 讀取 A (A暫存器)
    // 如果 a=0, 讀取 M (RAM[A])

    Mux16(a=A, b=inM, sel=instruction[12], out=MUX1);

    //載入D暫存器
    //C指令 AND d2 == 1, load2 = 1

    And(a=instruction[15], b=instruction[4], out=load2);
    DRegister(in=AluOP, load=load2, out=D);

    //ALU

    ALU(x=D, y=MUX1, zx=instruction[11], nx=instruction[10], zy=instruction[9], ny=instruction[8], f=instruction[7], no=instruction[6], out=outM, out=AluOP, zr=zr, ng=ng);

    //如果A AND d3 == 1, writeM = 1

    And(a=instruction[15], b=instruction[3], out=writeM);
    Or(a=zr, b=ng, out=zrORng); //輸入0或負數
    Not(in=zrORng, out=GT); //輸入非0或非負數(greater than)
    And(a=GT, b=instruction[0], out=JGT);
    And(a=zr, b=instruction[1], out=JEQ);
    Or(a=JEQ, b=JGT, out=JGE);
    And(a=ng, b=instruction[2], out=JLT);
    Or(a=JLT, b=JGE, out=Pass);
    And(a=instruction[15], b=Pass, out=pcjump);

    //PCload()=是C指令 AND 滿足上面條件>>JUMP

    PC(in=A, load=pcjump, inc=true, reset=reset, out[0..14]=pc);
}
```
<img src="hw8.jpg"/> 
