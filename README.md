-----------------------------------------------------
!********* PCSI-Solver ******** Version 1.0 ********!
-----------------------------------------------------

How to deploy the scalable barotropic solver for CESM ocean component in CESM.

(1)Create a complete case:

./create_newcase -case YOUR_CASE -res f09_g16 -compset B1850CN -mach yellowstone 

or an ocean component:

./create_newcase -case YOUR_CASE -res ne30_g16_rx1 -compset G_NORMAL_YEAR -mach yellowstone 
 
(2)Copy all of these 6 files (POP_SolversMod.F90, barotropic.F90, build-namelist, initial.F90, namelist_defaults_pop2.xml, namelist_definition_pop2.xml) into the path $YOUR_CASE_DIR/SourceMods/src.pop2/

(3)Execute
./YOUR_CASE.build

(4)Add two more lines at the end of the file: $YOUR_CASE_DIR/user_nl_pop2
   preconditionerchoice = 'evp'
   solverchoice = 'PCSI'

(5) Excute $YOUR_CASE_DIR/preview_namelists

(6) The detail configuration files is included in the conf directory. It contains: Readme.case, Macro, and user_nl_pop2. The cesm users can repeat our experiments in the P-CSI GMDD paper following the Readme.case step by step. Our compiler paramenters and dependant library have been indicated in Macro file. The user_nl_pop2 file is the namelist file provied by user.
 
