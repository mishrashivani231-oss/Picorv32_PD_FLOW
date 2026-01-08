## Custom Standard Cell Design, Layout & Characterization
In an ASIC design flow, standard cells form the foundation of digital logic implementation.
While we usually rely on pre-built standard cell libraries, designing a custom cell helps in understanding:

1. How standard cells are physically built
2. How layout follows design rules (DRC compliance)
3. Parasitic impact through extraction
4. Functionality and timing validation using spice simulations

In this section, a CMOS Inverter standard cell is:

1. Loaded in Magic Layout
2. Analyzed for layers, transistors & connectivity
3. Extracted into SPICE format
4. Simulated using ngspice for timing behavior
5. Debugged for DRC issues in the Sky130 technology file

This ensures the cell is:
✔ Physically correct
✔ Electrically functional
✔ Ready to be used in OpenLANE flow

Implementation
Section 3 Tasks
1. Clone custom inverter standard cell GitHub repository
```bash
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Clone the repository with custom inverter design
git clone https://github.com/nickson-jose/vsdstdcelldesign

# Change into repository directory
cd vsdstdcelldesign

# Copy magic tech file to the repo directory for easy access
cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech .

# Check contents whether everything is present
ls
```

# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_inv.mag &

2. Open and explore inverter layout in Magic

4. Extract SPICE netlist from layout
5. Modify model file for simulation compatibility
6. Perform post-layout simulations in ngspice
7. Identify DRC rule errors in Sky130 tech file & correct them



Section 3 – Tasks 1 to 5 files, reports & logs:
vsdstdcelldesign
