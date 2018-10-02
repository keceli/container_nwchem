Bootstrap: docker
From: debian

# Based on https://hub.docker.com/r/edoapra/nwchem
%labels
MAINTAINER Murat Keceli <keceli@gmail.com>
%post
apt-get update \
&& apt-get -y upgrade \
&& apt-get install -y python-dev gfortran  mpich2 libmpich2-dev  make curl subversion  unzip cmake perl tcsh ssh \
&&  apt-get clean \
&& mkdir -p /opt \
&& cd /opt \
&&  mkdir -p lib  \
&&  curl -L https://github.com/xianyi/OpenBLAS/archive/develop.zip -o develop.zip && unzip develop.zip ; rm develop.zip \
&& cd     OpenBLAS-develop \
&& make -j4  TARGET=CORE2 NO_CBLAS=1 NO_LAPACKE=1 && cp libopenblas.* /opt/lib \
&& rm -rf /opt/OpenBLAS-develop \
&& cd     /opt \
svn --trust-server-cert co https://icl.cs.utk.edu/svn/scalapack-dev/scalapack/trunk scalapack
            &&         curl http://www.netlib.org/scalapack/scalapack-2.0.2.tgz -o scalapack-2.0.2.tgz \ 
            &&  tar xzf scalapack-2.0.2.tgz && rm scalapack-2.0.2.tgz \
            && mkdir -p /opt/scalapack-2.0.2/build \
            && cd   /opt/scalapack-2.0.2/build \
            &&      cmake ../ -DCMAKE_BUILD_TYPE=MinSizeRel -DBUILD_TESTING=OFF -DBUILD_SHARED_LIBS=ON -DUSE_OPTIMIZED_LAPACK_BLAS=ON -DBLAS_blas_LIBRARY="/opt/lib/libopenblas.so" -DLAPACK_lapack_LIBRARY="/opt/lib/libopenblas.so" \
            && make -j3 \
            && cp /opt/scalapack-2.0.2/build/lib/libscalapack.* /opt/lib/. \
            && rm -rf /opt/scalapack-2.0.2
NWCHEM_TOP="/opt/nwchem"
NWCHEM_TARGET=LINUX64
NWCHEM_MODULES="all python"
PYTHONVERSION=2.7
PYTHONHOME="/usr"
USE_PYTHONCONFIG=Y
ARMCI_NETWORK=MPI-PT
BLASOPT="-L/opt/lib -lopenblas"
BLAS_SIZE=4
SCALAPACK_LIB="-L/opt/lib -lscalapack"
SCALAPACK_SIZE=4
LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/lib
USE_OPENMP=y
USE_NOIO=y
USE_64TO32=y
USE_MPI=y
USE_MPIF=y
USE_MPIF4=y
MRCC_METHODS=y
NWCHEM_EXECUTABLE=${NWCHEM_TOP}/bin/LINUX64/nwchem
NWCHEM_BASIS_LIBRARY=${NWCHEM_TOP}/src/basis/libraries/
NWCHEM_NWPW_LIBRARY=${NWCHEM_TOP}/src/nwpw/libraryps/
FFIELD=amber
AMBER_1=${NWCHEM_TOP}/src/data/amber_s/
AMBER_2=${NWCHEM_TOP}/src/data/amber_q/
AMBER_3=${NWCHEM_TOP}/src/data/amber_x/
AMBER_4=${NWCHEM_TOP}/src/data/amber_u/
SPCE=${NWCHEM_TOP}/src/data/solvents/spce.rst
CHARMM_S=${NWCHEM_TOP}/src/data/charmm_s/
CHARMM_X=${NWCHEM_TOP}/src/data/charmm_x/
PATH=$PATH:/opt/nwchem/bin/LINUX64
cd /opt ; mkdir -p nwchem  \
&& cd nwchem \
&& svn export https://svn.pnl.gov/svn/nwchem/trunk/src \
&& rm -rf /opt/nwchem/.svn /opt/nwchem/src/.svn \
&& cd    ${NWCHEM_TOP}/src/tools \
&& svn export --non-interactive --username nwchem --password nwchem https://svn.pnl.gov/svn/hpctools/branches/ga-5-5  \
&& cd .. \
&& sed -i 's|-march=native||' config/makefile.h \
&& sed -i 's|-mtune=native|-mtune=generic|' config/makefile.h \
&& sed -i 's|-mfpmath=sse||' config/makefile.h \
&& sed -i 's|-msse3||' config/makefile.h  \
&& make nwchem_config && (make 64_to_32 >& 6log &) && make -j3 \
&& rm -rf  /opt/nwchem/src/tools/ga-5-5/.svn \
&& rm -rf tce tools nwdft NWints geom symmetry util nwxc ddscf lapack blas rism argos peigs rmdft gradients symmetry property smd lucia dplot propery hessian ccsd mp2_grad moints cafe analyz dimqm /opt/nwchem/lib \
&&       apt-get -y remove  make curl subversion  unzip cmake perl tcsh  &&  apt-get -y autoremove && apt-get clean

cd /data

#CMD ["/bin/bash"]

%environment
export NWCHEM_TOP="/opt/nwchem"
export NWCHEM_TARGET=LINUX64
export NWCHEM_MODULES="all python"
export PYTHONVERSION=2.7
export PYTHONHOME="/usr"
export USE_PYTHONCONFIG=Y
export ARMCI_NETWORK=MPI-PT
export BLASOPT="-L/opt/lib -lopenblas"
export BLAS_SIZE=4
export SCALAPACK_LIB="-L/opt/lib -lscalapack"
export SCALAPACK_SIZE=4
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/lib
export USE_OPENMP=y
export USE_NOIO=y
export USE_64TO32=y
export USE_MPI=y
export USE_MPIF=y
export USE_MPIF4=y
export MRCC_METHODS=y
export NWCHEM_EXECUTABLE=${NWCHEM_TOP}/bin/LINUX64/nwchem
export NWCHEM_BASIS_LIBRARY=${NWCHEM_TOP}/src/basis/libraries/
export NWCHEM_NWPW_LIBRARY=${NWCHEM_TOP}/src/nwpw/libraryps/
export FFIELD=amber
export AMBER_1=${NWCHEM_TOP}/src/data/amber_s/
export AMBER_2=${NWCHEM_TOP}/src/data/amber_q/
export AMBER_3=${NWCHEM_TOP}/src/data/amber_x/
export AMBER_4=${NWCHEM_TOP}/src/data/amber_u/
export SPCE=${NWCHEM_TOP}/src/data/solvents/spce.rst
export CHARMM_S=${NWCHEM_TOP}/src/data/charmm_s/
export CHARMM_X=${NWCHEM_TOP}/src/data/charmm_x/
export PATH=$PATH:/opt/nwchem/bin/LINUX64
%runscript
exec /bin/bash "$@"

