---
title: Resumen de x86_64 para el primer parcial
categories: [facultad]
comments: false
---

# Resumen de x86_64 para el primer parcial

<!-- Index -->
- [Registros](#registros)
- [Registros volátiles (caller saved)](#registros-volátiles-caller-saved)
- [Registros no volátiles (callee saved)](#registros-no-volátiles-callee-saved)
- [Valores de retorno de funciones](#valores-de-retorno-de-funciones)
- [Llamadas a funciones de C](#llamadas-a-funciones-de-c)
- [Pasaje de parámetros a funciones](#pasaje-de-parámetros-a-funciones)
- [Direccionamiento](#direccionamiento)
- [Alineación de structs](#alineación-de-structs)
- [GDB](#gdb)
	- [Imprimir registros xmm](#imprimir-registros-xmm)
	- [Examinar memoria](#examinar-memoria)
	- [Examinar la pila](#examinar-la-pila)
	- [Imprimir punteros a structs](#imprimir-punteros-a-structs)
	- [Imprimir arrays](#imprimir-arrays)
		- [Arrays de tipos simples](#arrays-de-tipos-simples)
		- [Arrays de structs](#arrays-de-structs)
		- [Arrays de punteros a structs](#arrays-de-punteros-a-structs)


## Registros

| 64 bits | 32 bits | 16 bits | 8 bits | Description       |
|---------|---------|---------|--------|-------------------|
| rax     | eax     | ax      | al     | return value      |
| rbx     | ebx     | bx      | bl     |                   |
| rcx     | ecx     | cx      | cl     | 4th argument      |
| rdx     | edx     | dx      | dl     | 3rd argument      |
| rsi     | esi     | si      | sil    | 2nd argument      |
| rdi     | edi     | di      | dil    | 1st argument      |
| r8      | r8d     | r8w     | r8b    | 5th argument      |
| r9      | r9d     | r9w     | r9b    | 6th argument      |
| r10     | r10d    | r10w    | r10b   |                   |
| r11     | r11d    | r11w    | r11b   |                   |
| r12     | r12d    | r12w    | r12b   |                   |
| r13     | r13d    | r13w    | r13b   |                   |
| r14     | r14d    | r14w    | r14b   |                   |
| r15     | r15d    | r15w    | r15b   |                   | 

## Registros volátiles (caller saved)

Pushearlos si se usan en una función y se quiere preservar su valor después de una llamada a otra función:
- **RAX, RCX, RDX, RSI, RDI, R8, R9, R10, R11, XMM0-15**

## Registros no volátiles (callee saved)

Pushearlos siempre que se usan desde dentro de cualquier función:
- **RBX, RSP, RBP, R12, R13, R14, R15**

## Valores de retorno de funciones

- RAX -> enteros/punteros
- RAX -> floats

## Llamadas a funciones de C

La pila debe estar alineada a 16 bytes (`sub rsp, X`).  
Cuando se entra a una función en asm, la pila siempre está desalineada (alineada a 8 bytes).
## Pasaje de parámetros a funciones
### Primeros 6 argumentos:

| Regsitro | N° arg   |
|----------|----------|
| **RDI**  | arg1     |
| **RSI**  | arg2     |
| **RDX**  | arg3     |
| **RCX**  | arg4     |
| **R8**   | arg5     |
| **R9**   | arg6     |

### Argumentos siguientes:
se pushean de derecha a izquierda en la pila

|      Memoria   | N° arg           |
|----------------|------------------|
| **rbp + 8**    | (**rip caller**) |
| **rbp + 16**   | arg1             |
| **rbp + 24**   | arg2             |
| ...            |                  |

## Direccionamiento

```
[ Base + ( Index * Scale ) + Displacement ]
   ^         ^       ^            ^
   |         |       |            |
  RAX       RAX      1        Cte. 32 bits
  ...       ...      2
  R15       R15      4
          (No RSP)   8
```

Size directives:
- **BYTE**   1 byte		8 bits
- **WORD**   2 bytes	16 bits
- **DWORD**  4 bytes	32 bits
- **QWORD**  8 bytes	64 bits
- **OWORD**  16 btyes	128 bits

## Alineación de structs
Cada campo en un struct tiene un requisito de alineacion que por lo general es igual a su tamaño en bytes.

Cada campo debe estar alineado en direcciones de memoria que sean múltiplos de su requisito de alineacion. El compilador puede añadir padding entre los miembros del struct para hacer cumplir estos requisitos de alineación.

Al final, el tamaño total del struct debe ser múltiplo del mayor requisito de alineacion de sus campos, el compilador puede añadir padding al final del struct para hacer cumplir esto, haciendo que el tamaño del struct aumente.

Ejemplo:
```c
typedef struct {
    char a;        // 1 byte
    uint32_t b;    // 4 bytes
    uint16_t c;    // 2 bytes
} example_t;
```
Asi queda el struct en memoria:

| a (1 byte) |  Padding (3 bytes)  | b (4 bytes) | c (2 bytes) |  Padding (2 bytes) |


## GDB 
Machete de gdb: https://macapiaggio.github.io/gdb-guide/

### Imprimir registros xmm
```p/b $xmmn.f ```

- b: base
	- /d: base 10 signed
	- /u: base 10 unsigned
	- /t: base 2
	- /x: base 16

- f: formato
	- .v4_int32: 4 registros de 32 bits c/u
	- .v2_int64: 2 registros de 64 bits c/u
	- .v8_int16: 8 registros de 16 bits c/u
	- .v16_int8: 16 registros de 8 bits c/u
	- .v4_float: 4 registros de single-precision float (32 bits)
	- .v2_double: 2 registros de double-precision float (64 bits)

### Examinar memoria
```x /Nuf expression```

- N: units to display
- u: unit size
	- b: byte
	- h: halfwords (2 bytes)
	- w: words (4 bytes)
	- g: giant words (8 bytes)

- f: formato
	- x: hexa
	- d: signed decimal
	- u: unsigned decimal
	- t: binary
	- a: address
	- c: char
	- f: float

### Examinar la pila
Acceder a un elemento específico en **[rbp - offset]**:

```x /1gx $rbp-offset```

### Imprimir punteros a structs
Ejemplo:
rdi tiene un puntero a un struct de tipo struct_t: 

```p *(struct_t*) $rdi```

### **Imprimir arrays**

#### **Arrays de tipos simples**

Ejemplo: rdi tiene la direccion de un array de n enteros de tipo uint16_t:

```p *(uint16_t(*)[n]) $rdi```

#### **Arrays de structs**

Ejemplo: rdi apunta a un array de tamaño n de structs de tipo struct_t:

```p *(struc_t(*)[n]) $rdi```

#### **Arrays de punteros a structs**

Ejemplo: rdi es un array de n punteros a structs

```p *(struct_t**(*)[n]) $rdi```
