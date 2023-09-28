# FLOSIC code compliation (to accompany the NRLMOL and FLOSIC codes)

## 1. Preparation

## Install software dependencies

### Linux (Redhat based distro)

    sudo dnf install gcc-gfortran openmpi-devel openblas-devel lapack-devel git

### Mac OSX

If not done already, install `XCode commandline tool`. You need it for compiling codes. You can do so by typing `xcode-select --install` in your terminal.


The Fortran compilers and libraries are available on homebrew (https://brew.sh/). 
To set up homebrew, open up terminal and paste the following command,

    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

Once you set up the homebrew, install the necessary packages with

    brew install gcc openblas lapack
    
### Windows 10&11

Install **Windows Subsystem for Linux (WSL)** from Microsoft store if it is not already there. Here, we use `Fedora WSL`. After downloading Windows Subsystem for Linux and Fedora WSL from Miscrosoft store, you will find Fedora icon in the start menu. WSL essentailly provides a Linux environment in Windows. Now follow the Linux instruction.

Further reading: https://learn.microsoft.com/en-us/windows/wsl/install


## 2. FLOSIC_2020 code

## Download the code from github (https://github.com/FLOSIC/PublicRelease_2020) either using your web browser or using your terminal. 

`git clone https://github.com/FLOSIC/PublicRelease_2020.git`

Go to the source code directory

`cd PublicRelease_2020/flosic`

Edit Makefile

`vim Makefile.fedora`

### Linux

The makefile is already set up for Fedora linux. You can adjust the serial/parallel compilation with
Line 10 `MPI=Y`


### Mac OSX

Change line 10 to `MPI=N`. Only the serial compilation is supported on Mac OSX.

Under `# COMPILING OPTIONS`, comment Line 21-23 for Linux.

    #CC = gcc
    #FC = mpif90
    #FFF = mpif90

Uncomment Line 27-29 for OSX.

    CC = gcc
    FC = gfortran 
    FFF = gfortran

Comment linking option in Line 261 for Linux.

`#        $(FFF) $(LFLAGS) $(OBJ) -o $(BIN) -llapack -lblas $(LIBS)`

Uncomment linking option in Line 265 for OSX.

`       $(FFF) $(LFLAGS) $(OBJ) -o $(BIN) -llapack -lblas`

#### Note on MacOS

You may receive -lSystem not found error when linking the code on MacOS. If that happens, you need to add one more thing to the linking option. 
Change the line 265 to    

     $(FFF) $(LFLAGS) $(OBJ) -o $(BIN) -llapack -lblas -L/Library/Developer/CommandLineTools/SDKs/MacOSX12.1.sdk/usr/lib/

Adjust `MacOSX12.1.sdk` accordingly.
    
### Windows  

The process for Windows using WSL is similar to the process for linux.

    
Compile the code with `make -f Makefile.fedora`. If the compilation is successful, you will find a `nrlmol_exe` binary file.




## 3. NRLMOL_CORI code

Obtain the **NRLMOL_CORI** code from dropbox or zipped file. **TBA**

Open the Makefile. You may find Makefile.mpi for parallel and Makefile.ser for serial code.

`vim Makefile.ser`

Edit the lines under `# Compiler` for compiler name on your computer/environment

    CC = gcc
    FC = gfortran
    FCC = gfortran
    
Edit the lines under `# Flags` for compiler flag options 

    CFLAGS =
    FFLAGS = -O3
    LFLAGS = 
    
Compiling the code with `make -f Makefile.ser` (or  `make -f Makefile.mpi`).    
    

## 4. Notes for compiling the code on UTEP Jakar

Jakar does not have a working lapack (at the moment of writing this). But you can compile it in your home directory. You only need to do this once. Run a sequence of command in your home directory as follows,

    wget https://github.com/Reference-LAPACK/lapack/archive/refs/tags/v3.10.0.tar.gz
    tar zxvf v3.10.0.tar.gz
    cd lapack-3.10.0/
    cp make.inc.example make.inc
    make

*this make command may complain about lapack_testing.py but you can ignore it since you have **liblapack.a** at that point.

Copy the **liblapack.a** in your /home/username/lib and use that path as the **path_to_liblapack.a** 

Open FLOSIC code Makefile (Makefile.fedora) and edit your linking options. Replace `-llapack -lblas` with `-L/path_to_liblapack.a/ -L/opt/ohpc/pub/libs/gnu8/openblas/0.3.7/lib/ -llapack -lopenblas`
Your linking option line would looks something like below,

  `$(FFF) $(LFLAGS) $(OBJ) -o $(BIN) -L/home/usrname/lib/ -L/opt/ohpc/pub/libs/gnu8/openblas/0.3.7/lib/ -llapack -lopenblas $(LIBS)`

If everything is set up correctly in the FLOSIC code Makefile, you can hit: `make` (or `make -f Makefile.fedora`)


## 5. Notes

The common static parameters are specified in **PARAMA** or **PARAMA2**. Every time you make a change to this file, you need to perform `make clean -f Makefile` and `make -f Makefile`.

Some of the commonly adjusted parameters in PARAMA/PARAMA2:

**MXSPN**  Maximun spin, 1 for spin unpolarized and 2 for spin polarized calculation

**MAX_FUSET** Max number of function sets

**MAX_IDENT** Max number of inequivalent atoms

**MX_CNT** Max number of equivalent atoms

**MAX_PTS** Max grid points

**MAX_OCC** Max number of occupied states

**MAXUNSYM** Max number of contracted orbitals per atom

**NDH** Max size of Hamiltonian matrix 

**MX_GRP** Max number of group operations

**MAX_REP** Max number of group representations

Finally, if you cannot compile your FLOSIC code, don't panic! Ask for a help to someone who knows.
