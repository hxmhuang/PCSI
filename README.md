-----------------------------------------------------
!********* PCSI-Solver ******** Version 1.0 ********!
-----------------------------------------------------

Scalable barotropic solver for CESM ocean component.


1. Create a new case which can run normally as usual, refer to README.txt for details.

2. Copy all of these 6 files (POP_SolversMod.F90, barotropic.F90, build-namelist, initial.F90, namelist_defaults_pop2.xml, namelist_definition_pop2.xml) into the path $YOUR_CASE_DIR/SourceMods/src.pop2/

3. ./YOUR_CASE.build

4. Add two more lines at the end of the file: $YOUR_CASE_DIR/user_nl_pop2
   preconditionerchoice = 'evp'
   solverchoice = 'PCSI'

5. Excute $YOUR_CASE_DIR/preview_namelists

6. Congratulation! submit your job!
