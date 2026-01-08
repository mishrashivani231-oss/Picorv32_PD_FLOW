# Day 2 – Synthesis, Floorplan & Power Planning

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

## What Happens During Synthesis?
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
![Screenshot](Screenshot%202026-01-05%20112402.png)
![Screenshot](Screenshot%202026-01-05%20112514.png)



---

# Load generated floorplan def in magic tool and explore the floorplan.
Commands to load floorplan def in magic in another terminal

# Change directory to path containing generated floorplan def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-03_12-06/results/floorplan/

# Command to load the floorplan def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &

Floorplan def in magic
![Screenshot](Screenshot%202026-01-08%20101155.png)


Diagonally equidistant cells
![Screenshot](Screenshot%202026-01-08%20101325.png)

# Run 'picorv32a' design congestion aware placement using OpenLANE flow and generate necessary outputs.
# Day 3 – Placement and Clock Tree Synthesis

---

## What is Placement?
Placement determines the location of standard cells inside chip core.

### Phases of Placement
1) Global Placement
2) Detailed Placement

Goals of Placement:
✔ Reduce wirelength  
✔ Maintain legality  
✔ Improve timing  

---

## Placement Command
run_placement
![Screenshot](Screenshot%202026-01-08%20101943.png)

Commands to load placement def in magic in another terminal
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-03_12-06/results/placement/

# Command to load the placement def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
![Screenshot](Screenshot%202026-01-08%20102254.png)

Standard cells placed:

## Conclusion
✔ Successfully synthesized PicoRV32  
✔ Learned core utilization and chip dimensions  
✔ Generated stable PDN network

