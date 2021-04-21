Intel-Compilers
Portland-Compilers

F77
```
# yum install compat-gcc-34-g77
```
Install 32-bit compilers on a 64-bit host

On a 64-bit host, GCC will build executables that can only run on 64-bit hosts. 
However, GCC can be used to build executables that will run both on 64-bit hosts and on 32-bit hosts.

To build 32-bit binaries on a 64-bit host, 
first install 32-bit versions of any supporting libraries the executable may require. 
This must at least include supporting libraries for glibc and libgcc, and libstdc++ if the program is a C++ program. 
On Intel 64 and AMD64, this can be done with: 
```
# yum install libgfortran.i686 glibc-devel.i686 libgcc.i686 libstdc++-devel.i686
```
There may be cases where it is useful to to install additional 32-bit libraries that a program may require. 
For example, if a program uses the db4-devel libraries to build, 
the 32-bit version of these libraries can be installed with: 
```
# yum install db4-devel.i686   
```

Download GCC ftp://ftp.mirrorservice.org/sites/sourceware.org/pub/gcc/releases/

Configuring and building GCC 4.X.X 
``` 
# cd /opt
# mkdir build_gcc
# cd build_gcc/
# cp /home/axisadmin/Downloads/gcc* .
# tar zxvf gcc-4.8.3.tar.gz
# cd gcc-4.8.3
# ./contrib/download_prerequisites
# cd ..
# mkdir objdir
# cd objdir
# /opt/build_gcc/gcc-4.8.3/configure --prefix=/opt/gcc/4.8.3
# make
# make install
```
MPI - Building GCC with OpenMPI 
```
# cd /opt/build_openmpi
# tar zxvf openmpi-1.8.3.tar.gz
# cd openmpi-1.8.3
# ./configure --prefix=/opt/openmpi-1.8.3/gcc-4.8.3
# make all install
```
MPI - Building Intel with OpenMPI 
```
# gunzip -c openmpi-1.8.3.tar.gz | tar xf -
# cd openmpi-1.8.3
# ./configure --prefix=/opt/openmpi-1.8.3/intel-2013 CC=icc CXX=icpc FC=ifort
# make all install
```
MPI - Building pgi with OpenMPI 
```
# module load pgi/12.10
# cd /opt/build_openmpi
# tar zxvf openmpi-1.8.3.tar.gz
# ./configure --prefix=/opt/openmpi-1.8.3/pgi-1.8.4 CC=pgcc FC=pgfortran CXX=pgcpp CPP=cpp
# make all install
```
