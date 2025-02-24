
# **PiCube-ISA 108 VDSP Instruction Set Open Source White Paper**  

## **Our Work**
We have created a brand-new technical system called **PiCube (Processing In Cube)** to overcome various limitations encountered in high-performance computing using CPU, GPU, or DSA technology.  

The first-generation **PiCube DEMO** project features the following:  

- **8-stage pipeline VDSP architecture** with **dynamic branch prediction**
- **Native parallel computing instruction set (PiCube-ISA 108)**
- **A 3D FP16 tensor register file (16×32×32)**
- **VDSP pipeline embedded computational resources**, including:
  - Two **32×32 FP16 DMA matrices**
  - One **32×32 FP16 MAC matrix**
  - One **32×32 FP16 division matrix**
  - One **32×32 FP16 iterative matrix**
  - One **32×32 FP16 transpose matrix**
- **Polymorphic data conversion engine**, supporting scalars, vectors, matrices, and tensor data objects
- **Supported data types**:
  - Native **32-bit integer scalars**, dual/quad/octa **32-bit integer vectors**
  - Native **single-bit Boolean scalars**, **32-column Boolean vectors**, **32×32 Boolean matrices**
  - Native **FP16 floating-point scalars**, **32-column floating-point vectors**, **32×32 floating-point matrices**
- **One 5120-bit wide data memory read bus** (4096-bit + 1024-bit)
- **One 5120-bit wide data memory write bus** (4096-bit + 1024-bit)
- **8MB FP16 shared memory (ShareMemory)**

---

## **Our Open Source Project**
We are now **open-sourcing** the **PiCube-ISA 108 base instruction set white paper**. In the future, we will successively open-source:  

- **The PiCube-ISA 108 compiler**
- **The PiCube-ISA 108 LLM inference and training ecosystem with source code**
- **The PiCube-ISA 108 computing interconnection architecture**

---

## **Our Open Source License**
This project follows the **GNU GPLv3 open-source license**, meaning:  

- Users can freely use the open-source **instruction set** for **academic, commercial, or any other purposes**.
- Users may **modify or redesign** the instruction set based on this white paper.
- Users must **open-source** any modified instruction set and include **written attribution** to the original authors in their license agreement.

**Authors & Contact**  
Edward Yang - [edward_yxg@outlook.com](mailto:edward_yxg@outlook.com)  
Susan Huang - [susanhuangai@outlook.com](mailto:susanhuangai@outlook.com)  

---

# **Chapter 1: PiCube Data Types**
PiCube supports a **diverse set of data object types**, including PiCube-specific types:

| Data Type | Description | Notation |
|-----------|-------------|-----------|
| **FLAG Scalar** | Single event flag scalar | `flgs` |
| **FLAG Vector** | Series of event flags in a vector | `flgv` |
| **FLAG Matrix** | 2D matrix of event flags | `flgm` |
| **FP16 Scalar** | 16-bit floating point scalar | `spot` |
| **FP16 Vector** | 32-element floating-point vector | `vect` |
| **FP16 Matrix** | 32×32 floating-point matrix | `mtrx` |
| **8 Adjacent UI32 Registers** | A block of 8 adjacent UI32 registers | `r32m` |
| **4 Adjacent UI32 Registers** | A block of 4 adjacent UI32 registers | `r32q` |
| **2 Adjacent UI32 Registers** | A block of 2 adjacent UI32 registers | `r32v` |
| **SRAM Matrix Buffer** | SRAM-based matrix buffer | `smem` |
| **Intermediate Data Vector** | Intermediate state transfer vector | `band` |
| **Intermediate Data Matrix** | Intermediate state transfer matrix | `tile` |
| **Intermediate Data Result Matrix** | Intermediate computation result matrix | `fxra` |

PiCube also supports **traditional CPU/DSP/GPU data types**:

| Data Type | Description | Notation |
|-----------|-------------|-----------|
| **8-bit Unsigned Immediate Integer** | 8-bit unsigned constant | `#ui08` |
| **16-bit Unsigned Immediate Integer** | 16-bit unsigned constant | `#ui16` |
| **16-bit Signed Immediate Integer** | 16-bit signed constant | `#si16` |
| **16-bit Floating Point Immediate** | 16-bit floating-point constant | `#fp16` |
| **24-bit Signed Immediate Integer** | 24-bit signed constant | `#si24` |
| **32-bit Unsigned Immediate Integer** | 32-bit unsigned constant | `#ui32` |
| **32-bit General Purpose Register** | 32-bit integer register | `r32r` |

---

## **1.1 SPOT and VECT Addressing**
PiCube can directly or indirectly address **256 16-bit data registers** (`spot`).  
- **32 spots** with the same high 3-bit address form a **32-element vector (`vect`)**.  
- **256 spots** are **organized into 8 rows** of 32-element vectors.

### **Accessing VECT**
- **Read a specific VECT** to retrieve **32 `spot` values**.
- **Modify a specific VECT** to update **32 `spot` values**.

`spot` registers support **unsigned `ui16` and floating-point `fp16`** types.  
- **Explicit format conversion** is required when switching between `ui16` and `fp16`.  
- The conversion follows an **unsigned transformation**, where `fp16` values are **treated as positive** before conversion.

---

## **1.2 MTRX Addressing**
PiCube can directly or indirectly address **16 FP16 data matrix registers (`mtrx`)**,  
where **each matrix consists of 32 rows × 32 columns of FP16 data registers**.

Each **`mtrx` register, row vector, and column vector** can be **directly or indirectly addressed**.

---

## **1.3 FLGS and FLGV Addressing**
PiCube can **directly or indirectly address** **256 single-bit flag registers (`flgs`)**.  
- **32 `flgs` values with the same high 3-bit address** form a **32-column flag vector (`flgv`)**.
- **256 `flgs` values form 8 rows of `flgv` vectors**.

### **Accessing FLGV**
- **Read a specific FLGV** to retrieve **32 `flgs` values**.
- **Modify a specific FLGV** to update **32 `flgs` values**.

---

## **1.4 FLGM Addressing**
PiCube can directly or indirectly address **4 flag matrices (`flgm`)**,  
where **each matrix contains 32 rows × 32 columns of flag registers**.

Each **`flgm` register, row vector, and column vector** can be **directly or indirectly addressed**.

---

## **1.5 R32R, R32V, R32Q, and R32M Addressing**
PiCube can directly or indirectly address **256 32-bit general-purpose registers (`r32r`)**.  
- **Two `r32r` registers** form a **register pair (`r32v`)** for **matrix computation coordinates**.
- **Four `r32r` registers** form a **descriptor register group (`r32q`)** for **data access descriptions**.
- **Sixteen `r32r` registers** form a **data register vector (`r32m`)** for **stack operations**.

---
