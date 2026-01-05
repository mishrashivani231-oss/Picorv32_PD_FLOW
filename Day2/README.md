# Day 2 – Logic Synthesis, Floorplan & Power Planning

---

## What is Synthesis?
Synthesis converts RTL (Verilog) into a gate-level netlist using standard cells from Sky130 library.

### Key Outputs of Synthesis:
- Netlist
- Area Report
- Timing Report
- Cell count

---

## Synthesis Command
run_synthesis


---

## 📌 What Happens During Synthesis?
- RTL is optimized
- Combinational + sequential logic mapped to real standard cells
- Constraints are applied
- Timing violations reported

---

## Floorplanning
Floorplanning defines the physical structure of the chip.

### Floorplan Decides
- Core and Die Size
- Core Utilization
- Power rails
- Pin locations
- Placement grid

---

## Floorplan Command
run_floorplan


---

## Power Distribution Network (PDN)
Power Planning ensures stable power delivery across chip.
It prevents:
1. IR drop  
2. Noise  
3. Voltage Droop  

---

## PDN Command
run_power_grid_generation


---

## 🖼 Screenshots
(Add synthesis area + floorplan + PDN images)


---

## Conclusion
✔ Successfully synthesized PicoRV32  
✔ Learned core utilization and chip dimensions  
✔ Generated stable PDN network

