# SG-mol
SG-mol is a software package for smoothing the trajectories of atoms using 
the Savitzky-Golay filter after simulating fluids and constructing radial 
distribution functions.

    Alexei M. Nikitin
    12.28.2021


The use of the package is demonstrated in the liquid acetonitrile example.
The file mecn-pos-1.xyz contains a 3 ps trajectory of 29 acetonitrile molecules 
with a step of 1 fs. Model under periodic boundary conditions 13.6658 A.
So a detailed trajectory is not needed for subsequent calculations, 
and it makes sense to reduce the file size to save time.

    Preaveraging.exe mecn-pos-1.xyz 10
    
Every tenth point is selected from the trajectory and saved in the 
trajectory.xmol file.


We smooth this trajectory using the 4th degree Sovitsky-Golay filter, with 
a window of 81 points (the half-width is specified for the program - 1 = 40).
We repeat the procedure 4 times to improve.

    SG.exe trajectory.xmol 40
    copy SG.xmol SGn.xmol
    SG.exe SGn.xmol 40
    copy SG.xmol SGn.xmol
    SG.exe SGn.xmol 40
    copy SG.xmol SGn.xmol
    SG.exe SGn.xmol 40
    copy SG.xmol SGn.xmol

The SGn.xmol file contains the smoothed path.


We build the radial distribution function.

    RDF.exe SG.xmol 13.6658

The result suitable for plotting in a spreadsheet is in three files:

    Bin-200.xls - bins positions in angstroms. They are used for the X axis.
    RDF-200.xls - is the radial distribution function itself. For the Y-axis.
    Int-200.xls - integral of the radial distribution function.

The RDF.in file contains options for building RDF:

    atoms_in_mol 6 - the number of atoms in a molecule.
    group_1 0      - list of the first group of equivalent atoms. In this case, it is the nitrogen atom in acetonitoil.
    group_2 0      - list of the second group of equivalent atoms. In this case, it is the same atom.
    period  1      - in this case, all the trajectory points are used to construct the RDF. 
