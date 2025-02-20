## Breakdown of Folders

This shows an outline of the files and folders in this repository.

* `UNIX_Assignment_Template.md` : This provides a breakdown of the codes used and their overall description.
* `Teosinte`: This contains the all intermediate and final files created during the data procession stage for sorting and extracting the groups needed. 
    `T_ascend` : This folder contains 12 files of all chromosomes in ascending order of their position values.
    `T_descend` : This contains files of chromosomes in descending order of their position values except for those with multiple and unkwown identifiers.
* `Maize` : This contains the all intermediate and final files created during the data procession stage of extracting the groups needed. 
    `M_ascend` : This folder contains 12 files of all chromosomes in ascending order of their position values.
    `M_descend` : This contains files of all chromosomes in descending order of their position values except for those with multiple and unkwown identifiers.

* The `fang_et_al_genotypes.txt` and `snp_positions.txt` are the original data files used to work on this task.
* The `transpose.awk` script was used to transpose `fang_et_al_genotypes.txt` to `transposed_genotypes.txt`.
* The `transposed_1header.txt` file is an intermidiate file obtained after removing the first two rows from `transposed_genotypes.txt`.   
* `transgroup.txt` is another intermediate file which was merged with `snp_position.txt` to create `transjoin.txt` after creating a common column by changing the name 'Group' from `transposed_1header.txt` to 'SNP_ID'.
