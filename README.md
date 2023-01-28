# FLOSIC code compliation

## 1. Preparation

## Install software dependencies

### Linux (Redhat based distro)

    sudo dnf install gcc-gfortran openblas-devel lapack-devel git

### Mac OSX

If not done already, install `XCode commandline tool`. You need it for compiling codes. You can do so by typing `xcode-select --install` in your terminal.


The Fortran compilers and libraries are available on homebrew (https://brew.sh/). 
To set up homebrew, open up terminal and paste the following command,

    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

Once you set up the homebrew, install the necessary packages with

    brew install gcc openblas lapack
    
### Windows 10&11

Install `Windows Subsystem for Linux` (WSL) from Microsoft store if it is not already there. Here, we use `Fedora WSL`. After downloading Windows Subsystem for Linux and Fedora WSL from Miscrosoft store, you will find Fedora icon in the start menu. 
Now follow the Linux instruction.

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
    
### Windows  

The process for Windows using WSL is similar to the process for linux.

    
Compile the code with `make -f Makefile.fedora`. If the compilation is successful, you will find a `nrlmol_exe` binary file.

Note: The common static parameters are specified in `PARAMA` or `PARAMA2`. Every time you make a change to this file, you need to perform `make clean -f Makefile` and `make -f Makefile`.


## 3. NRLMOL_CORI code

Obtain the NRLMOL_CORI code from dropbox or zipped file. 

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
    
