-----------------------------------------------------
!********* PCSI-Solver ******** Version 1.0 ********!
-----------------------------------------------------

How to deploy the scalable barotropic solver for ocean component in CESM.

(1) Create a complete case:

./create_newcase -case YOUR_CASE -res T62_t12 -compset B1850CN -mach yellowstone 

or an ocean component:

./create_newcase -case YOUR_CASE -res T62_t12 -compset G -mach yellowstone
 
(2) Copy all of these 6 files (POP_SolversMod.F90, barotropic.F90, build-namelist, initial.F90, namelist_defaults_pop2.xml, namelist_definition_pop2.xml) into the path $YOUR_CASE_DIR/SourceMods/src.pop2/

(3) Execute the command:
./YOUR_CASE.build

(4) Add two more lines at the end of the file: $YOUR_CASE_DIR/user_nl_pop2:
   preconditionerchoice = 'evp'
   solverchoice = 'PCSI'

(5) Execute $YOUR_CASE_DIR/preview_namelists

(6) Run you case with:
./YOUR_CASE.submit

Note that the detailed configuration files are included in the config directory, which contains Readme.case, Macro, and user_nl_pop2. The CESM users can repeat our experiments following the Readme.case. Our compiler paramenters and dependant libraries have been indicated in Macro file. The user_nl_pop2 file is the namelist file provied by users.
 
