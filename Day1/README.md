# Day 1 – Introduction, RISC-V Overview & Environment Setup

---

## 🌟 Introduction to RISC-V
RISC-V is an open-source Instruction Set Architecture (ISA) based on the Reduced Instruction Set Computing (RISC) principle. 
Unlike proprietary ISAs such as ARM or x86, RISC-V is license-free, extensible, and ideal for academia, research, and industry.

### Why RISC-V?
- Open Source and royalty-free
- Supports modular design
- Highly extensible and scalable
- Strong ecosystem & rapid adoption

---

## 🧠 PicoRV32 Overview
PicoRV32 is a compact RISC-V compliant CPU core. It is:
- 32-bit RISC-V core
- Area-efficient
- Suitable for embedded SoCs and FPGA/ASIC designs
- Supports RV32IMC features

In this project, PicoRV32 acts as the RTL design that we implement physically on silicon.

---

## 🏭 What is Physical Design?
Physical design converts digital RTL (Verilog code) into an actual silicon layout.

### RTL → GDSII Flow Steps
1️⃣ RTL Design  
2️⃣ Synthesis  
3️⃣ Floorplanning  
4️⃣ Placement  
5️⃣ CTS  
6️⃣ Routing  
7️⃣ DRC/LVS  
8️⃣ GDS Export

We perform this complete flow using OpenLANE and Sky130 PDK.

---

## 🧰 Tools Used
OpenLANE is an open-source RTL → GDSII flow consisting of:
- Yosys → Synthesis  
- OpenROAD → PnR  
- OpenSTA → Timing analysis  
- Magic & KLayout → Layout visualization  
- Netgen → LVS  

---

## 🔧 Environment Setup
Commands:
a. docker
b. cd OpenLane
c. make mount
d. ./flow.tcl -interactive
e. package require openlane
f. prep -design picorv32a


---

## 📌 Output Understanding
After prep:
- Design is copied to run directory
- Technology files & libraries loaded
- Configurations applied

---

## 🖼 Screenshots
![Screenshot](Screenshot%202026-01-05%20101248.png)
![Screenshot](Screenshot%202026-01-05%20105455.png)
![Screenshot](Screenshot%202026-01-05%20105533.png)


---

## ✅ Conclusion
Day 1 established:
✔ Understanding of RISC-V and PicoRV32  
✔ Overview of Physical Design  
✔ Successfully setup OpenLANE  
✔ Prepared the design for next steps


