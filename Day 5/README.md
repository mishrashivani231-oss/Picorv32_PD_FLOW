## Final steps for the Physical Design od PicoRV32 using tritonRoute and openSTA
Tasks performed:
1. Perform generation of Power Distribution Network (PDN) and explore the PDN layout.
Commands executed:
```bash
cd Desktop/work/tools/openlane_working_dir/openlane
docker
./flow.tcl -interactive
package require openlane 0.9
directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

set ::env(SYNTH_STRATEGY) "DELAY 3"
set ::env(SYNTH_SIZING) 1

run_synthesis

init_floorplan
place_io
tap_decap_or

run_placement
unset ::env(LIB_CTS)

run_cts
gen_pdn 
```
<img width="1182" height="553" alt="image" src="https://github.com/user-attachments/assets/c26c7f57-b35a-4db7-907c-ac96690b6371" />

Commands to load PDN def in magic in another terminal
```bash
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/26-03_08-45/tmp/floorplan/

magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read 14-pdn.def &
```
PDN def:
<img width="1126" height="628" alt="image" src="https://github.com/user-attachments/assets/61e25783-2111-4511-8510-815d2e9e9426" />

2. Perfrom detailed routing using TritonRoute and explore the routed layout.
Command to perform routing:
```bash
# Check value of 'CURRENT_DEF'
echo $::env(CURRENT_DEF)

# Check value of 'ROUTING_STRATEGY'
echo $::env(ROUTING_STRATEGY)

# Command for detailed route using TritonRoute
run_routing
```
<img width="916" height="474" alt="image" src="https://github.com/user-attachments/assets/63e60a4a-c298-4906-a7b1-21cd83e59ca2" />

Commands to load routed def in magic:
```bash

cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/26-03_08-45/results/routing/

magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.def &
```
Routed Def:
<img width="1537" height="787" alt="image" src="https://github.com/user-attachments/assets/743b387f-ea05-497e-8393-5d796506358c" />

3. Post-Route parasitic extraction using SPEF extractor.
Command used:
```bash
cd Desktop/work/tools/SPEF_EXTRACTOR
python3 main.py /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/26-03_08-45/tmp/merged.lef /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/26-03_08-45/results/routing/picorv32a.def
```
4. Post-Route OpenSTA timing analysis
Commands:
```bash
# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/26-03_08-45/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/26-03_08-45/results/routing/picorv32a.def

# Creating an OpenROAD database to work with
write_db pico_route.db

# Loading the created database in OpenROAD
read_db pico_route.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/26-03_08-45/results/synthesis/picorv32a.synthesis_preroute.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design and library
link_design picorv32a

# Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Read SPEF
read_spef /openLANE_flow/designs/picorv32a/runs/26-03_08-45/results/routing/picorv32a.spef

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Exit to OpenLANE flow
exit
```
