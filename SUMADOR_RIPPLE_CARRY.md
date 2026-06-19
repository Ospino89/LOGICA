# Sumador Ripple Carry (RCA) - 4 bits

## Descripción General
El **Sumador Ripple Carry** es una arquitectura simple que encadena múltiples **Full Adders** en cascada. El acarreo (carry) se propaga de una etapa a la siguiente, de ahí su nombre "ripple" (ondulación).

## Ventajas
- ✅ Arquitectura simple y directa
- ✅ Fácil de implementar
- ✅ Requiere pocos componentes adicionales
- ✅ Escalable a cualquier número de bits

## Desventajas
- ❌ Mayor tiempo de propagación del carry
- ❌ Lentitud en operaciones con números grandes

---

## Estructura del Circuito (4 bits)

### Entradas
- **A[3:0]** - Primer número de 4 bits
- **B[3:0]** - Segundo número de 4 bits
- **Cin** - Acarreo inicial (generalmente 0)

### Salidas
- **Sum[3:0]** - Resultado de la suma (4 bits)
- **Cout** - Acarreo final (overflow)

---

## Diagrama de Bloques

```
    A[0]  B[0]                A[1]  B[1]                A[2]  B[2]                A[3]  B[3]
      |     |                   |     |                   |     |                   |     |
      +--+--+                   +--+--+                   +--+--+                   +--+--+
         |                         |                         |                         |
      [FA0]----Cout0           [FA1]----Cout1           [FA2]----Cout2           [FA3]----Cout3
         |         |              |         |              |         |              |         |
       Sum[0]      |            Sum[1]      |            Sum[2]      |            Sum[3]   Cout
                   |                        |                        |
                   +--------Cin1            +--------Cin2            +--------Cin3
                   
    Cin -----> Cin0
```

---

## Especificación de Cada Full Adder

### Full Adder (FA)
**Ecuaciones lógicas:**
```
Sum = A XOR B XOR Cin
Cout = (A AND B) OR (Cin AND (A XOR B))
```

### Tabla de Verdad del Full Adder
| A | B | Cin | Sum | Cout |
|---|---|-----|-----|------|
| 0 | 0 |  0  |  0  |  0   |
| 0 | 0 |  1  |  1  |  0   |
| 0 | 1 |  0  |  1  |  0   |
| 0 | 1 |  1  |  0  |  1   |
| 1 | 0 |  0  |  1  |  0   |
| 1 | 0 |  1  |  0  |  1   |
| 1 | 1 |  0  |  0  |  1   |
| 1 | 1 |  1  |  1  |  1   |

---

## Conexión en Cascada

```
Etapa 0:  A[0], B[0], Cin=0      → Sum[0], Cout0
Etapa 1:  A[1], B[1], Cin=Cout0  → Sum[1], Cout1
Etapa 2:  A[2], B[2], Cin=Cout1  → Sum[2], Cout2
Etapa 3:  A[3], B[3], Cin=Cout2  → Sum[3], Cout3=Cout
```

---

## Compuertas Lógicas Necesarias

**Por cada Full Adder:**
- 2 compuertas XOR
- 2 compuertas AND
- 1 compuerta OR

**Total para 4 bits (4 Full Adders):**
- 8 compuertas XOR
- 8 compuertas AND
- 4 compuertas OR

---

## Ejemplo de Operación

**Suma: 5 + 3 = 8**
```
  0101  (A = 5)
+ 0011  (B = 3)
------
  1000  (Sum = 8, Cout = 0)
```

**Desglose por etapa:**
```
Etapa 0: 1 + 1 + 0 = 0, Cout = 1
Etapa 1: 0 + 1 + 1 = 0, Cout = 1
Etapa 2: 1 + 0 + 1 = 0, Cout = 1
Etapa 3: 0 + 0 + 1 = 1, Cout = 0
```

---

## Tiempo de Propagación

- **Cada Full Adder:** ~2 retardos de compuerta
- **RCA de 4 bits:** ~8 retardos de compuerta
- **RCA de n bits:** ~2n retardos de compuerta

---

## Implementación en Compuertas Lógicas

### Full Adder usando XOR, AND, OR

```
[A]----\
        XOR----\
[B]----/        XOR----[Sum]
                /
         [Cin]--

[A]----\
        AND----\
[B]----/        OR----[Cout]
           /    
[A XOR B]--AND--
           \   /
        [Cin]--
```

---

## Notas
- El Ripple Carry es ideal para números de pocos bits (4-8 bits)
- Para números mayores, se recomienda usar Sumador Carry Lookahead (CLA)
- El acarreo final (Cout) indica overflow en operaciones sin signo
