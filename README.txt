
This is a example of installing cesm1_2_1 on the server named ‘cess’ in Center for Earth System Science of Tsinghua University.

(1)Download cesm1_2_1 as follows:

svn co --username guestuser --password friendly  https://svn-ccsm-models.cgd.ucar.edu/cesm1/release_tags/cesm1_2_1 cesm1_2_1


(2)Suppose the work path is /home/hxm/WORK1/cesm1_2_1，edit the file ./scripts/ccsm_utils/Machines/config_machines.xml, add some information at the end of the file before the flag </config_machines>：
<machine MACH="cess">
        <DESC>SuperMicro, os is Linux, 20 pes/node, batch system is PBS</DESC>
        <OS>LINUX</OS>
        <COMPILERS>intel</COMPILERS>
        <MPILIBS>impi</MPILIBS>
        <RUNDIR>/home/hxm/WORK1/cesm1_2_1/scripts/$CASE/run</RUNDIR>
        <EXEROOT>/home/hxm/WORK1/cesm1_2_1/scripts/$CASE/bld</EXEROOT>
        <DIN_LOC_ROOT>/home/share/cesm/cesm-input</DIN_LOC_ROOT>
        <DOUT_S_ROOT>/home/hxm/WORK1/cesm1_2_1/archive/$CASE</DOUT_S_ROOT>
        <BATCHQUERY>qstat -f</BATCHQUERY>
        <BATCHSUBMIT>qsub &lt;</BATCHSUBMIT>
        <SUPPORTED_BY>hxm-at- tsinghua.edu.cn</SUPPORTED_BY>
        <GMAKE_J>20</GMAKE_J>
        <MAX_TASKS_PER_NODE>20</MAX_TASKS_PER_NODE>
        <PES_PER_NODE>20</PES_PER_NODE>
</machine>

(3)Edit the file ./scripts/ccsm_utils/Machines/config_compilers.xml at the position of 200 line:
<compiler COMPILER="intel">
  <!-- http://software.intel.com/en-us/articles/intel-composer-xe/ -->
  <ADD_CPPDEFS> -DFORTRANUNDERSCORE -DNO_R16</ADD_CPPDEFS>
  <ADD_CFLAGS compile_threaded="true"> -openmp </ADD_CFLAGS>
  <ADD_FFLAGS compile_threaded="true"> -openmp </ADD_FFLAGS>
  <ADD_LDFLAGS compile_threaded="true"> -openmp </ADD_LDFLAGS>
  <FREEFLAGS> -free </FREEFLAGS>
  <FIXEDFLAGS> -fixed -132 </FIXEDFLAGS>
  <ADD_FFLAGS DEBUG="TRUE"> -g -CU -check pointers -fpe0 </ADD_FFLAGS>
  <FFLAGS> -O2 -fp-model source -convert big_endian -assume byterecl -ftz -traceback </FFLAGS>
  <CFLAGS> -O2 -fp-model precise </CFLAGS>
  <FFLAGS_NOOPT> -O0 </FFLAGS_NOOPT>
  <FC_AUTO_R8> -r8 </FC_AUTO_R8>
  <SFC> ifort </SFC>
  <SCC> icc </SCC>
  <SCXX> icpc </SCXX>
  <MPIFC> mpiifort </MPIFC>
  <MPICC> mpiicc  </MPICC>
  <MPICXX> mpicxx </MPICXX>
  <CXX_LINKER>FORTRAN</CXX_LINKER>
  <CXX_LDFLAGS> -cxxlib </CXX_LDFLAGS>
  <SUPPORTS_CXX>TRUE</SUPPORTS_CXX>
  <NETCDF_PATH>${ESM_SOFT}</NETCDF_PATH>
  <PNETCDF_PATH>${ESM_SOFT}</PNETCDF_PATH>
  <ADD_SLIBS>-L${ESM_SOFT}/lib -lnetcdff -lnetcdf -lpnetcdf -lhdf5_hl -lhdf5 -ldl -lm -lz -lcurl</ADD_SLIBS>
</compiler>

  If necessary, the NETCDF_PATH can be set as the path of your own NetCDF, we have installed the NETCDF v4.1.2 on our ‘cess’, and the HDF5 library in the bottom layer, so we need to add a series of “-lnetcdff -lnetcdf-lpnetcdf -lhdf5 -lhdf5_hl -ldl -lm -lz -lcurl”. If the HDF5 is not used, the “-lnetcdff -lnetcdf” is sufficient.


(4)Edit the file ./scripts/ccsm_utils/Machines/config_pes.xml, add some information at the end of the file before the flag </pesinfo>：

<pes MACH="cess">
    <NTASKS_ATM>120</NTASKS_ATM> <NTHRDS_ATM>1</NTHRDS_ATM>   <ROOTPE_ATM>0</ROOTPE_ATM>      <NINST_ATM>1</NINST_ATM>
    <NTASKS_LND>$NTASKS_ATM</NTASKS_LND> <NTHRDS_LND>$NTHRDS_ATM</NTHRDS_LND> <ROOTPE_LND>$ROOTPE_ATM</ROOTPE_LND> <NINST_LND>1</NINST_LND>
    <NTASKS_ICE>$NTASKS_ATM</NTASKS_ICE> <NTHRDS_ICE>$NTHRDS_ATM</NTHRDS_ICE> <ROOTPE_ICE>$ROOTPE_ATM</ROOTPE_ICE> <NINST_ICE>1</NINST_ICE>
    <NTASKS_OCN>$NTASKS_ATM</NTASKS_OCN> <NTHRDS_OCN>$NTHRDS_ATM</NTHRDS_OCN> <ROOTPE_OCN>$ROOTPE_ATM</ROOTPE_OCN> <NINST_OCN>1</NINST_OCN>
    <NTASKS_CPL>$NTASKS_ATM</NTASKS_CPL> <NTHRDS_CPL>$NTHRDS_ATM</NTHRDS_CPL> <ROOTPE_CPL>$ROOTPE_ATM</ROOTPE_CPL>
    <NTASKS_GLC>$NTASKS_ATM</NTASKS_GLC> <NTHRDS_GLC>$NTHRDS_ATM</NTHRDS_GLC> <ROOTPE_GLC>$ROOTPE_ATM</ROOTPE_GLC> <NINST_GLC>1</NINST_GLC>
    <PIO_NUMTASKS>-1</PIO_NUMTASKS><PIO_STRIDE>-1</PIO_STRIDE> <PIO_TYPENAME>netcdf</PIO_TYPENAME> <PIO_ROOT>1</PIO_ROOT>
    <PES_LEVEL>0</PES_LEVEL>
</pes>

The number of processes is 120, which can be modified to the number you wish.

(5)Copy a new file env_mach_specific.tsinghua with no modification.

 cp env_mach_specific.userdefined env_mach_specific.tsinghua
 

(6)Copy a new file for job batch processing which is suitable for the ‘cess’:

cp mkbatch.userdefined mkbatch.cess

Edit the 17~18 lines as follows：
set qname = hpcb
set tlimit = "99:59:00"

Edit the 78 line as follows：
mpirun -np ${maxtasks} \$EXEROOT/cesm.exe >&! cesm.log.\$LID

Edit the 38-44 lines as follows:
#PBS -N ${jobname}
#PBS -q ${qname}
#PBS -l nodes=${nodes}:ppn=${taskpernode}
#PBS -l walltime=${tlimit}
#PBS -r n
#PBS -j or
#PBS -S /bin/csh -V



(7)Create a complete case:

./create_newcase -case hxmcase1 -res f09_g16 -compset B1850CN -mach cess

or a ocean component:

./create_newcase -mach cess -res ne30_g16_rx1 -compset G_NORMAL_YEAR -case hxmcase2
 
 
(8)Config a case
 
cd hxmcase1

(If you want to run a case with 240 cores, edit the file env_mach_pes.xml)
 
./cesm_setup
 
 
(9)Compile
 
./hxmcase1.build
 
 
(10)Execute
 
./hxmcase1.submit