# Decompiling the most basic fn 

```
int main() { return 123; }


> objdump -d -M intel out


0000000000001129 <main>:
    1129:	f3 0f 1e fa          	endbr64
    112d:	55                   	push   rbp          # Fn prologue 
    112e:	48 89 e5             	mov    rbp,rsp
    1131:	b8 7b 00 00 00       	mov    eax,0x7b     # Moves 123 into the eax register which is commonly used for return values
    1136:	5d                   	pop    rbp          # Fn epilogue
    1137:	c3                   	ret                 # Return to the caller

```

# Decompiling the famous hello world 

```

#include <stdio.h>
int main() {

  printf("Hello world \n");
  return 0;
}

0000000000001149 <main>:
    1149:	f3 0f 1e fa          	endbr64
    114d:	55                   	push   rbp
    114e:	48 89 e5             	mov    rbp,rsp
    1151:	48 8d 05 ac 0e 00 00 	lea    rax,[rip+0xeac]          # loads the pointer for the ascii string Hello world
    1158:	48 89 c7             	mov    rdi,rax
    115b:	e8 f0 fe ff ff       	call   1050 <puts@plt>          # calls the puts fn to print the output
    1160:	b8 00 00 00 00       	mov    eax,0x0                  # return code stored here 
    1165:	5d                   	pop    rbp
    1166:	c3                   	ret                             # exit

STMFD  SP!, {R4,LR}                     # Epilogue
ADR  R0, aHelloWorld ; "hello, world"   # Offset for the string
BL  __2printf                           # Saves the addr of next ins to the lr reg and passes control to the printf fn by writing its addr to the PC.
MOV  R0, #0                             # Return value
LDMFD  SP!, {R4,PC}                     # Epilogue

```

---
> [!TIP]
> In Intel-syntax: <instruction> <destination operand> <source operand>.

> In AT&T syntax: <instruction> <source operand> <destination operand>.

> q — quad (64 bits)

> l — long (32 bits)

> w — word (16 bits)

> b — byte (8 bits)

> Win64, 4 function arguments are passed in the RCX, RDX, R8, R9 registers

> Linux, BSD and Mac OS the ﬁrst 6 arguments are passed in the RDI, RSI, RDX, RCX, R8, R9 registers, and the rest—via the stack.

> All MOV instructions in 64-bit mode that write something into the lower 32-bit register part **also clear the higher 32-bits**

---

# Prologue and epilogue in ARM 

```
 STMFD SP!, {R4,LR}  
 LDMFD SP!, {R4,PC}

```
The above cryptic code is similar to the push and pop prologue in intel.

It starts by subtracting SP and then loading the values of the registers to the new specified location of sp. 
The epilogue is the inverse and loads from the sp into r4 and the program counter.


# Cdel convention

push arg3
push arg2
push arg1
call f
add esp, 12 ; 4*3=12

Calle get their arguments using the stack 




























