# 在Linux Ubuntu LTS 20.04 上安装 WRF-4.6.1
#### 安装并测试编译脚本
安装build-essential, csh, bash, gfortran, m4, cpp：
```
sudo apt-get install build-essential csh bash gfortran m4 cpp
```
检查是否系统中可用,输出相对应包的路径即为可用：
```
which gcc
which cpp
which gfortran
```
检查gcc版本（需要大于4.4.0）：
```
gcc --version
```
##### 检查是否正确构建这些编译器，以及它们是否安装正确且彼此兼容：
创建一系列新的文件夹以便后续使用：
```
mkdir Build_WRF
cd ./Build_WRF/
mkdir TESTS
```
进入这个TESTS文件夹下载七个测试文件：
```
 cd {path_to_dir}/TESTS
 wget https://www2.mmm.ucar.edu/wrf/OnLineTutorial/
 compile_tutorial/tar_files/Fortran_C_tests.tar
 tar-xvf Fortran_C_tests.tar
```
可以使用rm -r 命令删除压缩包:
```
rm -r Fortran_C_tests.tar
```
第一个测试gfortran：
```
gfortran TEST_1_fortran_only_fixed.f
./a.out
```
出现SUCCESS标志代表没问题（后面以此类推，不再赘述）！
之后必须再做一个Free Format Fortran测试：
```
gfortran TEST_2_fortran_only_free.f90
./a.out
```
第二个测试gcc：
```
gcc TEST_3_c_only.c
./a.out
```
第三个测试Fortran是否能够调用C编译器：
```
gcc -c -m64 TEST_4_fortran+c_c.c
gfortran -c -m64 TEST_4_fortran+c_f.f90
gfortran -m64 TEST_4_fortran+c_f.o TEST_4_fortran+c_c.o
./a.out
```
第四个测试脚本csh、Perl、sh是否在系统中正常工作，所以需要再进行三个测试：
```
csh ./TEST_csh.csh
./TEST_perl.pl
./TEST_sh.sh
```
如果以上测试存在失败情况，需要去检查安装是否正确，可以简单的重新下载或者尝试在清除中间文件后重新安装，然后检查这些测试是否正确。否则如果你继续安装而不更正这些步骤，后面的错误会更复杂，浪费更多时间。

#### 安装外部库
返回上一级Build_WRF目录并创建一个LIBRARIES目录，即外部库：
```
cd {path_to_dir}/Build_WRF
mkdir LIBRARIES
```
进入这个库，并下载外部库压缩包，用ls可以检查库是否干净：
```
cd {path_to_dir}/Build_WRF/LIBRARIES
 wget http://www2.mmm.ucar.edu/wrf/OnLineTutorial/compile_tutorial/tar_files/mpich-3.0.4.tar.gz
 wget http://www2.mmm.ucar.edu/wrf/OnLineTutorial/compile_tutorial/tar_files/netcdf-4.1.3.tar.gz
 wget http://www2.mmm.ucar.edu/wrf/OnLineTutorial/compile_tutorial/tar_files/jasper-1.900.1.tar.gz
 wget http://www2.mmm.ucar.edu/wrf/OnLineTutorial/compile_tutorial/tar_files/libpng-1.2.50.tar.gz
 wget http://www2.mmm.ucar.edu/wrf/OnLineTutorial/compile_tutorial/tar_files/zlib-1.2.7.tar.gz
```
安装NetCDF，在这之前首先必须设置几个~/.bashrc变量,第一个因人而异，进入LIBRARIES，pwd一下即可（要在后面添加，千万不要把别人的路径给替代了！）：
```
sudo nano ~/.bashrc

export DIR=/{path_to_dir}/Build_WRF/LIBRARIES
export CC=gcc
export CXX=g++
export FC=gfortran
export FCFLAGS=-m64
export F77=gfortran
export FFLAGS=-m64
```
解压缩netcdf-4.1.3文件：
```
tar -xvzf netcdf-4.1.3.tar.gz
```
“进⼊netcdf-4.1.3⽂件夹，使⽤source命令以便所有更改的环境变量或者编译器都应⽤于当前会话，然后进⾏安装和编译：
```
cd {path_to_dir}/Build_WRF/LIBRARIES/netcdf-4.1.3
source ~/.bashrc

./configure --prefix=$DIR/netcdf --disable-dap --disable-netcdf-4 --disable-shared

make
make install
```
现在开始安装第二个软件，在此之前，必须做一些路径分配，将netcdf的路径放入.bashrc中：
```
sudo nano ~/.bashrc

export PATH=$DIR/netcdf/bin:$PATH
export NETCDF=$DIR/netcdf
source ~/.bashrc
```
开始安装第二个软件：
```
cd {path_to_dir}/Build_WRF/LIBRARIES
tar -zxvf mpich-3.0.4.tar.gz
cd mpich-3.0.4
./configure --prefix=$DIR/mpich
```
如果不起作用可以尝试：
```
./configure --prefix=$DIR/mpich --with-device=ch4:ofi FFLAGS="-fallow-argument-mismatch"
FCFLAGS="-fallow-argument-mismatch"
```
我们继续完成mpich的安装：
```
make
make install

sudo nano ~/.bashrc
export PATH=$DIR/mpich/bin:$PATH
source ~/.bashrc
```
进行接下来的库安装前，我们需要配置环境以指定路径：
```
sudo nano ~/.bashrc
export LDFLAGS=-L$DIR/grib2/lib
export CPPFLAGS=-I$DIR/grib2/include
source ~/.bashrc
```
开始安装zlib：
```
cd {path_to_dir}/Build_WRF/LIBRARIES
tar -zxvf zlib-1.2.7.tar.gz
cd zlib-1.2.7

./configure --prefix=$DIR/grib2
make
make install
```
开始安装libpng：
```
cd {path_to_dir}/Build_WRF/LIBRARIES
tar -zxvf libpng-1.2.50.tar.gz
cd libpng-1.2.50

./configure --prefix=$DIR/grib2
make
make install
```
开始安装JasPer：
```
cd {path_to_dir}/Build_WRF/LIBRARIES
tar -zxvf jasper-1.900.1.tar.gz
cd jasper-1.900.1

./configure --prefix=$DIR/grib2
make
make install
```
这些重要的库都已经安装完成，现在我们必须测试这些库是否正常工作。同时需要验证这些库是否能够与将用于WPS和WRF构建的编译器一起使用。因此，现在我们需要移动到测试目录下载包含这些统计数据的tar文件并将其解压：
```
cd {path_to_dir}/TESTS

wget http://www2.mmm.ucar.edu/wrf/OnLineTutorial/compile_tutorial/tar_files/Fortran_C_NETCDF_MPI_tests.tar 
tar-xvf Fortran_C_NETCDF_MPI_tests.tar
```
测试需要包含来自netcdf的包的inc文件。这可以帮助我们知道fortran和c是否与netcdf一起工作。可以开始测试了：
```
cp ${NETCDF}/include/netcdf.inc .

gfortran -c 01_fortran+c+netcdf_f.f
gcc -c 01_fortran+c+netcdf_c.c
gfortran 01_fortran+c+netcdf_f.o 01_fortran+c+netcdf_c.o -L${NETCDF}/lib -lnetcdff -lnetcdf 

./a.out
```
SUCCESS后开始进行第二个测试，检查c、netcdf、fortran是否正在工作或者与MPI兼容：
```
cp ${NETCDF}/include/netcdf.inc .

mpif90 -c 02_fortran+c+netcdf+mpi_f.f
mpicc -c 02_fortran+c+netcdf+mpi_c.c
mpif90 02_fortran+c+netcdf+mpi_f.o 02_fortran+c+netcdf+mpi_c.o -L${NETCDF}/lib -lnetcdff -lnetcdf 

mpirun ./a.out
```
#### 安装WRF建模系统
所有的软件都可以正常工作，便可以准备安装WRF建模系统：
#### WRF安装：
创建WRF_Sys文件夹，在WRF文件夹下载并解压WRF4.6.1版本的压缩包：
```
cd {path_to_dir}/Build_WRF/
mkdir WRF_Sys
cd WRF_Sys
```

**所处文件夹名称随意，但是解压缩后的WRF文件夹必须是WRF！**
```
wget https://github.com/wrf-model/WRF/releases/download/v4.6.1/v4.6.1.tar.gz -O WRFV4.6.1.tar.gz
tar -xvf WRFV4.6.1.tar.gz
```
***记得将解压缩出来的WRF4.6.1的文件夹名字改为WRF***
```
cd WRF
source ~/.bashrc
```
开始配置WRF：
```
./configure
```
```
checking for perl5... no
checking for perl... found /usr/bin/perl (perl)
Will use NETCDF in dir: /usr
HDF5 not set in environment. Will configure WRF for use without.
PHDF5 not set in environment. Will configure WRF for use without.
Will use 'time' to report timing information
$JASPERLIB or $JASPERINC not found in environment, configuring to build without grib2 I/O...
------------------------------------------------------------------------
Please select from among the following Linux x86_64 options:

1. (serial)   2. (smpar)   3. (dmpar)   4. (dm+sm)   PGI (pgf90/gcc)
5. (serial)   6. (smpar)   7. (dmpar)   8. (dm+sm)   PGI (pgf90/pgcc): SGI MPT
9. (serial)  10. (smpar)  11. (dmpar)  12. (dm+sm)   PGI (pgf90/gcc): PGI accelerator
13. (serial)  14. (smpar)  15. (dmpar)  16. (dm+sm)   INTEL (ifort/icc)
17. (dm+sm)   INTEL (ifort/icc): Xeon Phi (MIC architecture)
18. (serial)  19. (smpar)  20. (dmpar)  21. (dm+sm)   INTEL (ifort/icc): Xeon (SNB with AVX mods)
22. (serial)  23. (smpar)  24. (dmpar)  25. (dm+sm)   INTEL (ifort/icc): SGI MPT
26. (serial)  27. (smpar)  28. (dmpar)  29. (dm+sm)   INTEL (ifort/icc): IBM POE
30. (serial)               31. (dmpar)                PATHSCALE (pathf90/pathcc)
32. (serial)  33. (smpar)  34. (dmpar)  35. (dm+sm)   GNU (gfortran/gcc)
36. (serial)  37. (smpar)  38. (dmpar)  39. (dm+sm)   IBM (xlf90_r/cc_r)
40. (serial)  41. (smpar)  42. (dmpar)  43. (dm+sm)   PGI (ftn/gcc): Cray XC CLE
44. (serial)  45. (smpar)  46. (dmpar)  47. (dm+sm)   CRAY CCE (ftn/cc): Cray XE and XC
48. (serial)  49. (smpar)  50. (dmpar)  51. (dm+sm)   INTEL (ftn/icc): Cray XC
52. (serial)  53. (smpar)  54. (dmpar)  55. (dm+sm)   PGI (pgf90/pgcc)
56. (serial)  57. (smpar)  58. (dmpar)  59. (dm+sm)   PGI (pgf90/gcc): -f90=pgf90
60. (serial)  61. (smpar)  62. (dmpar)  63. (dm+sm)   PGI (pgf90/pgcc): -f90=pgf90
64. (serial)  65. (smpar)  66. (dmpar)  67. (dm+sm)   INTEL (ifort/icc): HSW/BDW
68. (serial)  69. (smpar)  70. (dmpar)  71. (dm+sm)   INTEL (ifort/icc): KNL MIC

Enter selection [1-71] : 34
------------------------------------------------------------------------
Compile for nesting? (1=basic, 2=preset moves, 3=vortex following) [default 1]: 

Configuration successful!
------------------------------------------------------------------------
testing for MPI_Comm_f2c and MPI_Comm_c2f
MPI_Comm_f2c and MPI_Comm_c2f are supported
testing for fseeko and fseeko64
fseeko64 is supported
------------------------------------------------------------------------
...
```
设置完成后开始编译并检查：
```
./compile em_real >& compile.log & 
tail -f compile.log 
```
出现以下输出便代表成功！
```
--->                  Executables successfully built                  <---
 
-rwxrwxr-x 1 wrf wrf 40691640 Jul 30 12:35 main/ndown.exe
-rwxrwxr-x 1 wrf wrf 40572760 Jul 30 12:35 main/real.exe
-rwxrwxr-x 1 wrf wrf 40048888 Jul 30 12:35 main/tc.exe
-rwxrwxr-x 1 wrf wrf 44609360 Jul 30 12:35 main/wrf.exe
 
==========================================================================
```
也可以通过查看.exe文件(输出上图中的四个exe文件名代表成功！)：
```
cd run/
ls -ll *.exe
```
最后要安装的就是WPS，它要与WRF文件夹在同一目录下：
```
cd {path_to_dir}/Build_WRF/WRF_Sys

wget https://github.com/wrf-model/WPS/archive/v4.6.0.tar.gz -O WPSV4.6.0.TAR.gz
tar -zxvf WPSV4.6.0.TAR.gz

cd WPS-4.6.0
```
接下来要做的是导出文件grip2的路径，因为WPS预计会取消来自grip2格式的文件，所以必须在.bashrc文件中的jasper的路径中提及grip的这些部分：
```
sudo nano ~/.bashrc

export JASPERLIB=$DIR/grib2/lib
export JASPERINC=$DIR/grib2/include
source ~/.bashrc

./configure
```
配置如下：
```
Please select from among the following supported platforms.

1.  Linux x86_64, gfortran    (serial)
2.  Linux x86_64, gfortran    (serial_NO_GRIB2)
3.  Linux x86_64, gfortran    (dmpar)
4.  Linux x86_64, gfortran    (dmpar_NO_GRIB2)
5.  Linux x86_64, PGI compiler   (serial)
6.  Linux x86_64, PGI compiler   (serial_NO_GRIB2)
7.  Linux x86_64, PGI compiler   (dmpar)
8.  Linux x86_64, PGI compiler   (dmpar_NO_GRIB2)
9.  Linux x86_64, PGI compiler, SGI MPT   (serial)
10. Linux x86_64, PGI compiler, SGI MPT   (serial_NO_GRIB2)
11. Linux x86_64, PGI compiler, SGI MPT   (dmpar)
12. Linux x86_64, PGI compiler, SGI MPT   (dmpar_NO_GRIB2)
13. Linux x86_64, IA64 and Opteron    (serial)
14. Linux x86_64, IA64 and Opteron    (serial_NO_GRIB2)
15. Linux x86_64, IA64 and Opteron    (dmpar)
16. Linux x86_64, IA64 and Opteron    (dmpar_NO_GRIB2)
17. Linux x86_64, Intel compiler    (serial)
18. Linux x86_64, Intel compiler    (serial_NO_GRIB2)
19. Linux x86_64, Intel compiler    (dmpar)
20. Linux x86_64, Intel compiler    (dmpar_NO_GRIB2)
21. Linux x86_64, Intel compiler, SGI MPT    (serial)
22. Linux x86_64, Intel compiler, SGI MPT    (serial_NO_GRIB2)
23. Linux x86_64, Intel compiler, SGI MPT    (dmpar)
24. Linux x86_64, Intel compiler, SGI MPT    (dmpar_NO_GRIB2)
25. Linux x86_64, Intel compiler, IBM POE    (serial)
26. Linux x86_64, Intel compiler, IBM POE    (serial_NO_GRIB2)
27. Linux x86_64, Intel compiler, IBM POE    (dmpar)
28. Linux x86_64, Intel compiler, IBM POE    (dmpar_NO_GRIB2)
29. Linux x86_64 g95 compiler     (serial)
30. Linux x86_64 g95 compiler     (serial_NO_GRIB2)
31. Linux x86_64 g95 compiler     (dmpar)
32. Linux x86_64 g95 compiler     (dmpar_NO_GRIB2)
33. Cray XE/XC CLE/Linux x86_64, Cray compiler   (serial)
34. Cray XE/XC CLE/Linux x86_64, Cray compiler   (serial_NO_GRIB2)
35. Cray XE/XC CLE/Linux x86_64, Cray compiler   (dmpar)
36. Cray XE/XC CLE/Linux x86_64, Cray compiler   (dmpar_NO_GRIB2)
37. Cray XC CLE/Linux x86_64, Intel compiler   (serial)
38. Cray XC CLE/Linux x86_64, Intel compiler   (serial_NO_GRIB2)
39. Cray XC CLE/Linux x86_64, Intel compiler   (dmpar)
40. Cray XC CLE/Linux x86_64, Intel compiler   (dmpar_NO_GRIB2)
Enter selection [1-40] : 3
------------------------------------------------------------------------
Configuration successful. To build the WPS, type: compile
------------------------------------------------------------------------
```
开始编译：
```
./compile >& compile.log &
tail -f compile.log
```
检查一下它是否安装成功：（输出geogrid.exe、ungrid.exe、metgrid.exe这三个exe文件的相关信息代表成功）：
```
ls -ll *.exe
```
以上便是本次教程的全部内容！


#### 附录：安装过程中对于可能出现的问题及解决方法

**※ 在wrf编译（./compile）后，只出现successfully，但没有输出ndown.exe、real.exe、tc.exe、wrf.exe这四个exe文件。**

问题原因可能是在NetCDF、mpich、zlib、libpng、jasper这五个外部库的安装上，这五个外部库的安装不能直接通过sudo apt-get install * 来进行安装，必须在下载相应安装包和解压后，通过make和make install这两条命令进行编译。

**※ 在真实案例里对气象数据运行./ungrib.exe这条命令时出现：**
```
./ungrib.exe: error while loading shared libraries: libpng12.so.0: cannot open shared object file: No such file or directory
```
**报错原因**：libpng12在Ubuntu20.04存储库中不再可用，因此，一些没有使用新的libpng (libpng16)库构建的应用程序无法安装或正常使用。

**解决方法**：在命令窗口输入如下命令：

```
sudo add-apt-repository ppa:linuxuprising/libpng12
sudo apt update
sudo apt install libpng12-0
```




