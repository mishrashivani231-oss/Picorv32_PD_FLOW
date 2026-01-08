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
![Screenshot](Screenshot%202026-01-08%20111916.png) 


2. Open and explore inverter layout in Magic
Custom inverter layout in magic
![Screenshot](Screenshot%202025-12-24%20150412.png)
3. Extract SPICE netlist from layout
Commands used in the Magic tkcon window for extracting the SPICE netlist of the custom inverter layout
  ```bash
# Check current directory
pwd

# Extraction command to extract to .ext format
extract all

# Before converting ext to spice this command enable the parasitic extraction also
ext2spice cthresh 0 rthresh 0

# Converting to ext to spice
ext2spice
```
![Screenshot](Screenshot%202026-01-08%20112740.png)

Created Spice file:
<img width="1183" height="586" alt="image" src="https://github.com/user-attachments/assets/f4ab1218-4917-4d17-84bb-48d49611e3f0" />


4. Modify model file for simulation compatibility

Measured the grid size and did the adjustment accordingly:
<img width="1135" height="629" alt="image" src="https://github.com/user-attachments/assets/a801e21d-2155-471d-a80f-7daf75d1fc7d" />

5. Perform post-layout simulations in ngspice
Commands for ngspice simulation:
```bash
# Command to directly load spice file for simulation to ngspice
ngspice sky130_inv.spice

# Now that we have entered ngspice with the simulation spice file loaded we just have to load the plot
plot y vs time a
```
<img width="1182" height="600" alt="image" src="https://github.com/user-attachments/assets/42676e77-e0b5-4251-9c62-6bed3c9c46f4" />

After running the ngspice, here is the plot obtained of the inverter:
<img width="1022" height="637" alt="image" src="https://github.com/user-attachments/assets/e9668098-afad-4300-89ea-64c490b2f4ba" />
<img width="949" height="639" alt="image" src="https://github.com/user-attachments/assets/a60e23fa-df0a-4d34-a32e-ee7f8c1ef862" />

6. Identify DRC rule errors in Sky130 tech file & correct them
Commands used to obtain and inspect the SkyWater Magic technology file with known DRC issues, along with the related files required for debugging:
```bash
# Change to home directory
cd

# Command to download the lab files
wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz

# Since lab file is compressed command to extract it
tar xfz drc_tests.tgz

# Change directory into the lab folder
cd drc_tests

# List all files and directories present in the current directory
ls -al

# Command to view .magicrc file
gvim .magicrc

# Command to open magic tool in better graphics
magic -d XR &
```
Incorrectly implemented poly.9 rule no drc violation even though spacing < 0.48u 
<img width="1053" height="603" alt="image" src="https://github.com/user-attachments/assets/3af477b5-8df2-4de3-8d34-c5d4ac7ce6d8" />
commands inserted in sky130A.tech file to update drc:
<img width="1160" height="603" alt="image" src="https://github.com/user-attachments/assets/eb78237a-a459-4365-b3fe-c19a8408aa62" />

Commands to run in tkcon window
```bash
# Loading updated tech file
tech load sky130A.tech

# Must re-run drc check to see updated drc errors
drc check

# Selecting region displaying the new errors and getting the error messages 
drc why
```
After implementing the rule:

<img width="635" height="563" alt="image" src="https://github.com/user-attachments/assets/5c54c8da-68c2-461a-9fe1-4d4cd53b8b74" />

