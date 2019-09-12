# Introduction to Quantum_ESPRESSO



## Table of Contents
1. [Linux commands](## Basic Linux Commands)
2. [Start](## Getting Started)



## Basic Linux Commands
![](Figures/linuxlogo.png)



|Command            |  Desription                        |
|-------------------|-----------------------------------------------------------------------------------------------------|
| *ls*              | directory listing                                                                                   |
| *ls -al*          | formatted listing with hidden files                                                                 |
| *cd dir*          | change directory to dir                                                                             |
| *cd*              | change to home                                                                                      |
| *pwd*             | show current directory                                                                              |
| *mkdir dir*       | create a directory dir                                                                              |
| *rm file*         | delete file                                                                                         |
| *rm -r dir*       | delete directory dir                                                                                |
| *rm -f file*      | force remove file                                                                                   |
| *rm -rf dir*      | force remove directory dir *                                                                        |
| *cp file1 file2*  | copy file1 to file2                                                                                 |
| *cp -r dir1 dir2* | copy dir1 to dir2; create dir2 if it  doesn't exist                                                 |
| *mv file1 file2*  | rename or move file1 to file2 if file2 is an existing directory  |
| *ln -s file link* | create symbolic link link to file                                                                   |
| *touch file*      | create or update file                                                                               |
| *cat > file*      | places standard input into file                                                                     |
| *more file*       | output the contents of file                                                                         |
| *head file*       | output the first 10 lines of file                                                                   |
| *tail file*       | output the last 10 lines of file                                                                    |
| *tail -f file*    | output the content                                                                 |











###  Bash scripting (`for & while`  loops)

+ Example 1. (For Loop)

    `for i in {1..10}; do echo $i ; done`

+ Example 2.(While loop)
    ```Bash
    ls > list
    `while read -r fname
    echo $fname
    done < list
    ```

## Getting Started
### Installation  

1. Download package from [Quantum-ESPRESSO webpage](https://www.quantum-espresso.org)
2. make a directory and copy downloaded file into your directory and extract it
```#!/usr/bin/env bash
mkdir SOURCE
cd SOURCE
cp ~/Downloads/q-e-qe-6.4.1.tar.gz .
tar -xvf q-e-qe-6.4.1.tar.gz
cd q-e-qe-6.4.1
```
3. run config file and then make main codes in the pakage (pw, pp, ld1, neb, cp, ...)  
We can run `make all` but it is not recommended at this level
```#!/usr/bin/env bash
./configure
make pw
make pp
make cp
make ld1
make ph
make neb
```
! Check **bin** directory to find `pw.x pp.x cp.x ...` there

4. To Set PATH on Linux (you can add all executables in the bin dir to your path ),  use `pwd` command to find your QE top directory path and then copy following line to the end of your `.bashrc`
``` #!/usr/bin/env bash
export PATH=QETOPDIR/bin:$PATH
```

5. **VISULIZER installation (XCRYSDEN)**  
__xcrysden__   is a standard visulizer for qe input/output files and If you are **ubuntu** user you can easily type `
sudo apt install xcrysden ` to install it from ubuntu rep.

6. **PWgui**  is a GUI for PWscf based programs from Quantum-ESPRESSO integrated suite of codes for electronic structure calculations and materials modeling. [Download Link](http://www-k3.ijs.si/kokalj/pwgui)  
This code can be used to generate input files for different packages.

### Excercise 1. (make INPUT file & run a simple calculations)
- Generating input files using easy tools  
PWgui is an easy grahical tool for beginners to create an input file with basic knowledge about the system, for example we know that "The atoms in crystalline silicon are arranged in a diamond lattice structure with a lattice constant of 5.4307Å (10.261 Bohr)."
-- It should be noted Quantum-ESRESSO works with pseudopotentials, then we need to download and put pseudopotential file in an appropriate address


- after choosing your parameters for the input file save final setting in an input, you can find a sample input file for silicon here [sample.pw.in](Files/sample.pw.in)




### SCF and convergency test for GaAs
In this exercise we will first perform simple scf (self-consistent field) calculations on GaAs structure

+ STEP 1. Use Xcrysden to view the structure of input file and explore different utilities

+ STEP 2. Open and read the input file [GaAs.scf.in](Files/GaAs.scf.in)    
Note that in the `&control` namelist, we have specified that we want to run an **scf** calculation.       
GaAs has the diamond structure. Note that we are using a (primitive) unit cell that corresponds to an fcc lattice, with 2 atoms in the atomic basis.      
Notice the values given for `ibrav, nat, ntyp` and ATOMIC-POSITIONS.         
In this file, the lattice constant **celldm(1)** has been set equal to 10.86626 bohr, which is the experimental value. \\
A 2×2×2 Monkhorst-Pack  <span style="color:purple"> k-point mesh </span> has been used.      
The plane-wave cut-off for wavefunctions, `ecutwfc` has been set to 30 Ry. Since we are not using an ultrasoft pseudopotential.   



+ STEP 3. Run your input using following command

*Hands on*


```Bash
      cd /WORKSHOP_QE/BULK/SECTION-SCF
      xcrysden --pwi GaAs.scf.in
      pw.x < GaAs.scf.in | tee GaAs.scf.out
```



+ STEP 4. How to extract data from output file?

   For total energy:  `grep ! GaAs.scf.out`\
   For Total Force:  `grep Total force GaAs.scf.out`

   For **electron number** : `grep "of electrons    " GaAs.scf.out`

   Whts is the number of electrons? and Why?

   *read first lines of your pseudo files !!!*


+ STEP 5. Covergency test for K-points and ecutwf
In this step write a simple script file:Kloop.sh (k-mesh) and  file:ecutloop.sh (ecut) to run scf per different values of k-points and ecut        
Plot a Physical quantity vs. variables and find converhency limit for your data     
At the first step one can plot total energy vs. above variables:     




$$\delta$$.
