## Pre-layout timing analysis and the role of a well-designed clock tree
Conceptual understanding of pre-layout and post-layout timing, design rule checks (DRC), clock tree synthesis (CTS), and the impact of standard-cell customization on the physical design flow.

Implementation
Steps performed:
- Resolve minor DRC violations and ensure the layout is complete and ready for integration into the design flow.
- Save the finalized layout with a custom identifier and reopen it for verification.
- Extract the LEF file from the completed layout.
- Copy the generated LEF along with the required library files into the src directory of the picorv32a design.
Task 1-4:
<img width="619" height="410" alt="image" src="https://github.com/user-attachments/assets/efdadc8e-a13c-42cb-97e4-b7402b42806e" />
- Update the config.tcl file to include the new library and reference the additional LEF in the OpenLane flow.
- Execute the OpenLane synthesis flow using the newly integrated custom inverter cell.
- Identify and mitigate any new violations introduced by the custom inverter by tuning relevant design parameters.
- After successful synthesis, proceed with floorplanning and placement to confirm that the custom cell is correctly handled during the PnR stages.
- Perform post-synthesis static timing analysis using the OpenSTA tool.
- Apply timing ECO fixes to eliminate all reported timing violations.
**Task 9-12:**
1. Fix up small DRC errors and verify the design is ready to be inserted into our flow.
Conditions to be verified before moving forward with custom designed cell layout:

1. Condition 1: The input and output ports of the standard cell should lie on the intersection of the vertical and horizontal tracks.
2. Condition 2: Width of the standard cell should be odd multiples of the horizontal track pitch.
3. Condition 3: Height of the standard cell should be even multiples of the vertical track pitch.
Commands to open the custom inverter layout:
```bash
# Change directory to vsdstdcelldesign
cd Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign

# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_inv.mag &
```
Tracks info:
<img width="1149" height="601" alt="image" src="https://github.com/user-attachments/assets/91a876ea-07ee-487f-9bb1-cc6ac5a6cf1a" />
Commands for tkcon window to set grid as tracks of locali layer:
```bash
# Syntax for grid command
help grid

# Set grid values 
grid 0.46um 0.34um 0.23um 0.17um
```
2. Save the finalized layout with any name and open it.
```bash
save sky130_vsdinv.mag
```
Command to open the saved layout:
```bash
magic -T sky130A.tech sky130_vsdinv.mag &
```
<img width="1143" height="620" alt="image" src="https://github.com/user-attachments/assets/c7289eef-19b2-4317-81f3-2c8d34ab7541" />

3. Generate lef from the layout.
Command used:
lef_write
Lef file created:
<img width="1161" height="628" alt="image" src="https://github.com/user-attachments/assets/e09b5a2a-c406-4d93-ab6e-4b3aff4f92fa" />

4. Copy the newly generated lef and associated required lib files to 'picorv32a' design 'src' directory.
Commands used:
```bash

cp sky130_vsdinv.lef ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/


ls ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/


cp libs/sky130_fd_sc_hd__* ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/


ls ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
```
5. Edit config.tcl
<img width="1132" height="203" alt="image" src="https://github.com/user-attachments/assets/e19357ab-d5e5-452f-af24-ac09e7217874" />

Edited config.tcl:
<img width="1123" height="538" alt="image" src="https://github.com/user-attachments/assets/bb0a474a-0a52-4ae7-992a-7aa63d1a45a8" />


6. Run openlane flow synthesis with newly inserted custom inverter cell.
<img width="1184" height="637" alt="image" src="https://github.com/user-attachments/assets/e9d9b25c-88bc-4763-b40c-a5bc0a0b75f4" />
<img width="1170" height="576" alt="image" src="https://github.com/user-attachments/assets/03346fc4-fbe9-4880-828c-83a924e6660c" />
After merging completed, run synthesis:
<img width="1178" height="634" alt="image" src="https://github.com/user-attachments/assets/bfeb0682-7c03-4931-ab1b-1c0f211d81e5" />
<img width="1054" height="621" alt="image" src="https://github.com/user-attachments/assets/9909c522-21b7-4947-a3a8-98d26e13f90f" />

7. Reduce the newly introduced violations with the introduction of custom inverter cell by modifying design parameters.
The chip area along with tns and wns was notes and the following commands to commit the changes were made and synthesis was ran again:
```bash

prep -design picorv32a -tag 24-03_10-03 -overwrite


set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs


echo $::env(SYNTH_STRATEGY)


set ::env(SYNTH_STRATEGY) "DELAY 3"


echo $::env(SYNTH_BUFFERING)


echo $::env(SYNTH_SIZING)


set ::env(SYNTH_SIZING) 1


echo $::env(SYNTH_DRIVING_CELL)


run_synthesis
```
After the changes and again run_synthesis:
<img width="955" height="575" alt="image" src="https://github.com/user-attachments/assets/19d3bbfd-862a-4b48-8f4e-5d7e7f9caf6d" />
Comparing to previously noted run values area has increased and worst negative slack has become 0.

8. Once synthesis has accepted our custom inverter we can now run floorplan and placement and verify the cell is accepted in PnR flow.
```bash
run_floorplan
```
<img width="1163" height="138" alt="image" src="https://github.com/user-attachments/assets/30031456-9f3e-4f7e-a3fc-bc5728bc829c" />

After this, some error was causing and with the help of the given reference repo I placed the ios and the floorplan was fixed:
<img width="1171" height="628" alt="image" src="https://github.com/user-attachments/assets/cdd46e42-d572-47db-a377-0e6651a6c2f6" />

Next, was to run_placement
<img width="1188" height="580" alt="image" src="https://github.com/user-attachments/assets/79cfd867-971a-4b4a-a5ed-d1d53ab4865c" />
Commands to load placement def in magic:
```bash
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/24-03_10-03/results/placement/
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```
<img width="1126" height="633" alt="image" src="https://github.com/user-attachments/assets/788be0a3-b455-472c-ae34-fd4da0b5e450" />

Screenshot of the inserted sky130_vsdinv:
<img width="955" height="554" alt="image" src="https://github.com/user-attachments/assets/cd8cb26f-7854-4a9e-b6fe-9f2abd9acccd" />

9. Next is to do post STA
Tool used: OpenSTA
New pre_sta.conf was created because I didnot have those files and then sta pre_sta.conf was made to run:
<img width="1193" height="609" alt="image" src="https://github.com/user-attachments/assets/6929210e-6542-44a8-8eff-52b34fc63100" />

<img width="1077" height="614" alt="image" src="https://github.com/user-attachments/assets/0d1bfa13-3628-483a-8c61-c22e57b3b0bc" />
- Replace the original netlist with the ECO-optimized netlist and rerun floorplanning, placement, and clock tree synthesis.
- Conduct post-CTS timing analysis using OpenROAD.
After that multiple changes were made because the fanout was causing high delay. After that cts was made to run. 
Command used: run_cts
<img width="1179" height="624" alt="image" src="https://github.com/user-attachments/assets/b514db43-08b4-4c02-b1c1-51a653486914" />
- Analyze the impact on post-CTS timing by modifying the clock buffer configuration, specifically by removing the sky130_fd_sc_hd__clkbuf_1 cell from the CTS_CLK_BUFFER_LIST variable.
<img width="1180" height="178" alt="image" src="https://github.com/user-attachments/assets/100fddee-1f54-4500-a701-55f4046e7614" />
