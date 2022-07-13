# MOAD Ligand Finder
This script was originally conceived to build reliable databases of structures (in PQR format) and relative ligands extracted from structures. The structures are given as an input in PDB format.

However, this tool could be handy in general. Indeed, is not always obvious how to identify the ligand within a pdb and the naming scheme can be cumbersome (combinations of Residue ID, Chain, Residue Name can represent a single ligand or not). I hope this script could help navigate through this. 
Ligand names are fetched from the **binding MOAD database**: "http://bindingmoad.org/pdbrecords/index/"  . Only valid ligands (according to MOAD's criteria) are considered.
The naming scheme is un-ambiguous and inspired from the binding MOAD database. 
*Note*: if a (ligand) file with the same name exists in the working directory, 
a number will be appended to the created ligand (pdb or xyz) file (see also below).

**NOTE**: The ligands extracted contain only HEAVY atoms (purged of hidrogens). Please, if this behavior is not always desiderable, write me a feedback to make it user-defined.


## Requirements:
Python3 and:
### Few non-builtin libraries:
* Numpy
* Requests

**Only if the option `--PQR` is activated:**
### Pdb2Pqr installed:
This software can be found here: https://github.com/Electrostatics/pdb2pqr 
I do not take any credit on the very nice pdb2pqr software

## Usage:
### "Manual" Mode
`python3 lfetch.py`
will ask to the user to insert the name of the PDB structure. 
If the pdb is not found in the working directory, it will be automatically downloaded.
The user can keep inserting other queries, the behavior will be identical and new info appended in the ligandMap file (see below).
To exit type ctrl-C .

* Output: 
  1. ligandMap.txt file containing ligand(s) name for queried structure(s).
  1. pdb or xyz file containing ligand heavy atoms (ligand extraction filters out Hidrogens)
Ligand names are fetched from the MOAD database: "http://bindingmoad.org/pdbrecords/index/"  . Only **valid** ligands are considered.
The naming scheme is un-ambiguous and inspired from the MOAD database. CAREFUL: if a (ligand) file with the same name exist in the working directory, 
a number will be appended to the created ligand (pdb or coordinate) file.

### "Database" Mode
`python3.py lfetch -d`
Will look for all pdb files in the working directory (*not containing "_"*) and 
automatically produce a ligandMap.txt file for them (1 line per structure followed by its ligands) and extract all valid ligands in separate files.
The ligands will be placed in an "output" folder created by the script. If the `--PQR` option is present, also the pqr files will be placed in the "output" folder.

### Other options
* **NEW** `--purgePDB` : A new pdb named: \<original name\>_clean.pdb is created and contains the original structure purged of any HETATM and *extracted* ligand(s) (non extracted ones which are *not* HEDATM, due to failed matching scheme -- note: the  `--safe` option will increase those scenarios-- will be present in the purged file)
* `--PQR` : A pqr file purged of all *found* ligands and *HETATM* will be produced using pdb2pqr with AMBER force field (if needed it could be extended to any force field, please post an issue and I will modify the code)
* `--XYZ`: The ligands are saved in coordinates files (.xyz) containing only their coordinates.
* `-c`: "Chunk mode" working only if `-d` option is also activated. The ligand (and pqr, if the above option is set) files with their correspondend ligandMap are distributed in separate folders each one including the number of structures indicated by the user. The folder are simply indicated with numbers. This option was conceived to create a database split in different chunks for parallel use in codes.. This functionality is still very basic: if there are some skipping of structures (for instance due to the `--excludeLarge` option below), no rearrangement of the folder content is performed.
* `--safe`: Partial matches with respect to what expected from the MOAD database are excluded. This means that this type of ligands are not extracted and their entry in the ligandMap.txt file is commented
* `--quiet`: do *not* print info while running.
* Advanced: `--excludeLarge` : large pqr files (more than 10000 lines) will be excluded. This means that the line concerning those structures in ligandMap is commented and their relative ligands are *not* extracted.


Please, do not hesitate to post issues, improvements suggestion or report bugs. I will try to answer them.
### Recomendations
Check the "error_log.txt" file after running. Will report the reasons for which a ligand or a full structure was skipped and other useful info.

### Example of usage and ligandMap file produced:

`python3 lfetch.py`

`Insert pdb name`

`6gj6`

`^C `

`TOTAL NUMBER OF STRUCTURES SUCCESFULLY PROCESSED = 1`

Content of ligandMap.txt: name of the structure with a number in front indicating the number of valid ligands found (according to binding MOAD) followed by the names of the ligands (one line per distint ligand). The name of the ligands appearing in ligandMap.txt is identical to the name of the file containing the ligand coordinates.
Example of ligand.txt produced from the above call:


`\# ************** PDB ligand Map ***************`

`\#       Created using '*BuildMap module*`

`\# Ligand validation based on binding MOAD database.`

`\# Author L. Gagliardi, Istituto Italiano di Tecnologia`

`\# LOCAL TIME = ---`

`\# --------------`

`2       6gj6`

`EZZ_A:204`

`GCP_A:203`
                                    ~               

# ACKNOLEDGMENTS:
This is a small accessory project in my work, but still I would very much appreciate an acknowledgment if this helped you save some time :). 
*Luca Gagliardi, MOAD ligandFinder, (2021) GitHub repository* and or:

## Cite
(*) L. Gagliardi et al, SHREC 2022: Protein-ligand binding site recognition, *Computers & Graphics* https://doi.org/10.1016/j.cag.2022.07.005 (2022)
