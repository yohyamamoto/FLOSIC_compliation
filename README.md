# FLOSIC code compliation

## FLOSIC code

## 0. Install software dependencies

### Linux (Redhat based distro)

    sudo dnf install gcc-gfortran openblas-devel lapack-devel

### Mac OSX

The Fortran compilers and libraries are available on homebrew (https://brew.sh/). Once you set up the homebrew, install the necessary packages with

    brew install gcc openblas lapack
    
### Windows    

## 1. Download the code from github (https://github.com/FLOSIC/PublicRelease_2020) either using your web browser or using your terminal. 

`git clone https://github.com/FLOSIC/PublicRelease_2020.git`

Go to the source code directory

`cd PublicRelease_2020/flosic`

Edit Makefile

`vim Makefile.fedora`

### Linux

The makefile is already set up for Fedora linux. You can adjust the serial/parallel compilation with
Line 10 `MPI=Y`


### Mac OSX

Change line 10 to `MPI=N`

Uncomment Line 27-29

    CC = gcc
    FC = gfortran 
    FFF = gfortran

Comment Line 261

`#        $(FFF) $(LFLAGS) $(OBJ) -o $(BIN) -llapack -lblas $(LIBS)`

Uncomment Line 265

`       $(FFF) $(LFLAGS) $(OBJ) -o $(BIN) -llapack -lblas`
    
### Windows    
    
Compile the code with `make -f Makefile.fedora`. If the compilation is successful, you will find a nrlmol_exe binary file.


