# MOAD Ligand Finder
This script was originally conceived to build reliable databases of structure (in PDB format) and relative ligands extracted from the structure.

However, this tool could be handy in general. Indeed, is not always obvious how to identify the ligand within a pdb and the naming scheme can be cumbersome (combinations of Residue ID, Chain, Residue Name can represent a single ligand or not). I hope this script could help navigate through this. 
Ligand names are fetched from the **binding MOAD database**: "http://bindingmoad.org/pdbrecords/index/"  . Only valid ligands (according to MOAD criteria) are considered.
The naming scheme is un-ambiguous and inspired from the MOAD database. CAREFUL: if a (ligand) file with the same name exist, 
a number will be appended to the created ligand (pdb or xyz) file (see also below).


## Requirements:

### Few non-builtin libraries:
* Numpy
* Requests

### Pdb2Pqr:
Only if the option `--PQR` is activated.
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

Will automatically produce a ligandMap file (1 line per structure followed by its ligands) and extract all valid ligands in separate files.
The ligands will be placed in an "output" folder created by the script.

### Other options

* `--PQR` : A pqr file purged of all *found* ligands and *HETATM* will be produced using pdb2pqr with AMBER force field (if needed this behavior can be extended to any force field)
* `--XYZ`: The ligands are saved in coordinates files (.xyz) containing only their coordinates.
* `-c`: "Chunk mode" working only if `-d` option is also activated. The ligand (and pqr, if the above option is set) files with their correspondend ligandMap are distributed in separate folders each one including the number of structures indicated by the user. The folder are simply indicated with numbers. This option was conceived to create a database split in different chunks for parallel use in codes.. This functionality is still very basic: if there are some skipping of structures, no rearrangement of the folder content is performed.
* `--safe`: Partial matches with respect to what expected from the MOAD database are excluded. This means that this type of ligands are not extracted and their entry in the ligandMap.txt file is commented
* `--quiet`: do *not* print info while running.
* Advanced: `--excludeLarge` : large pqr files (more than 10000 lines) will be excluded. This means that the line concerning those structures in ligandMap is commented and their relative ligands are *not* extracted.

### Example of usage and ligandMap file produced:

* `python3 lfetch.py`

`Insert pdb name`

`6gj6`

`^C `

`TOTAL NUMBER OF STRUCTURES SUCCESFULLY PROCESSED = 1`

Content of ligandMap.txt:

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
Luca Gagliardi, MOAD ligandFinder, (2021) GitHub repository.
