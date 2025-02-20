# UNIX Assignment

## Data Inspection

### Attributes of `fang_et_al_genotypes`

```
$ cat fang_et_al_genotypes.txt
$ less fang_et_al_genotypes.txt
$ head fang_et_al_genotypes.txt
$ tail fang_et_al_genotypes.txt
$ awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt
$ ls -lh fang_et_al_genotypes.txt 
$ column -t fang_et_al_genotypes.txt | head -n 3
$ file fang_et_al_genotypes.txt
$ grep "ZMMMR" fang_et_al_genotypes.txt | cut -f3 | sort | uniq -c
$ grep "ZMMIL" fang_et_al_genotypes.txt | cut -f3 | sort | uniq -c
$ grep "ZMMLR" fang_et_al_genotypes.txt | cut -f3 | sort | uniq -c
$ grep "ZMPJA" fang_et_al_genotypes.txt | cut -f3 | sort | uniq -c
$ grep "ZMPIL" fang_et_al_genotypes.txt | cut -f3 | sort | uniq -c
$ grep "ZMPBA" fang_et_al_genotypes.txt | cut -f3 | sort | uniq -c
```

By inspecting this file I learned that:

1. The file has 986 columns, 2783 rows/lines, 2744038 words and 11054722 bytes/characters.
2. The file is 11 megabytes large.
3. It is an ASCII text, with very long lines.
4. The groups, ZMMIL, ZMMLR, ZMMMR, ZMPBA, ZMPIL, ZMPJA are 290, 1256, 27, 900, 41 and 34 lines each.


### Attributes of `snp_position.txt`

```
$ cat snp_position.txt
$ column -t snp_position.txt | head -n 3
$ less snp_position.txt
$ head snp_position.txt
$ tail snp_position.txt
$ awk -F "\t" '{print NF; exit}' snp_position.txt
$ ls -lh snp_position.txt 
$ file snp_position.txt
```

By inspecting this file I learned that:

1. It is 82 kilobytes large.
2. The file has 15 columns, 984 rows/lines, 13198 words and 83747 bytes/characters.
3. It is an ASCII text.


## Data Processing

### Maize Data

```
$ awk -f transpose.awk fang_et_al_genotypes.txt > transposed_genotypes.txt
$ tail -n +3 transposed_genotypes.txt > transposed_1header.txt
$ wc transposed_1header.txt
$ sed 's/Group/SNP_ID/g' transposed_1header.txt > transgroup.txt
$ join -1 1 -2 1 -t $'\t' snp_position.txt transgroup.txt > transjoin.txt
$ wc transjoin.txt
$ awk -F "\t" '{print NF; exit}' transjoin.txt
$ vim transjoin.txt
$ awk '{for(i=1;i<=NF;i++) if($i == "ZMMIL") count++} END{print count}' transjoin.txt
$ awk '{for(i=1;i<=NF;i++) if($i == "ZMMLR") count++} END{print count}' transjoin.txt
$ awk '{for(i=1;i<=NF;i++) if($i == "ZMMMR") count++} END{print count}' transjoin.txt
$ cut -d $'\t' -f1,3,4,2508-2797,1225-2480,2481-2507 transjoin.txt > M.txt
$ wc M.txt
$ tail -n +6 M.txt | awk -F "\t" '{print NF; exit}'
$ vim M.txt
$ (head -n 1 M.txt && tail -n +2 M.txt | sort -k3,3n) > Msorted.txt
$ wc Msorted.txt
$ tail -n +6 Msorted.txt | awk -F "\t" '{print NF; exit}'
$ grep -v "^#" Msorted.txt | cut -f2 | sort | uniq -c
$ { head -n 1 Msorted.txt && awk '$2 == 1' Msorted.txt; } > Mchr1.txt
$ wc Mchr1.txt
$ tail -n +6 Mchr1.txt | awk -F "\t" '{print NF; exit}'
$ vim Mchr1.txt
$ { head -n 1 Msorted.txt && awk '$2 == 2' Msorted.txt; } > Mchr2.txt
$ { head -n 1 Msorted.txt && awk '$2 == 3' Msorted.txt; } > Mchr3.txt
$ { head -n 1 Msorted.txt && awk '$2 == 4' Msorted.txt; } > Mchr4.txt
$ { head -n 1 Msorted.txt && awk '$2 == 6' Msorted.txt; } > Mchr6.txt
$ { head -n 1 Msorted.txt && awk '$2 == 7' Msorted.txt; } > Mchr7.txt
$ { head -n 1 Msorted.txt && awk '$2 == 8' Msorted.txt; } > Mchr8.txt
$ { head -n 1 Msorted.txt && awk '$2 == 9' Msorted.txt; } > Mchr9.txt
$ { head -n 1 Msorted.txt && awk '$2 == 10' Msorted.txt; } > Mchr10.txt
$ { head -n 1 Msorted.txt && awk '$2 == "multiple"' Msorted.txt; } > Mchrm.txt
$ { head -n 1 Msorted.txt && awk '$2 == "unknown"' Msorted.txt; } > Mchru.txt
$ wc Mchr1.txt Mchr2.txt Mchr3.txt Mchr4.txt Mchr5.txt Mchr6.txt Mchr7.txt Mchr8.txt Mchr9.txt Mchr10.txt Mchrm.txt Mchru.txt
$ awk -F "\t" '{print NF; exit}' Mchr1.txt Mchr2.txt Mchr3.txt Mchr4.txt Mchr5.txt Mchr6.txt Mchr7.txt Mchr8.txt Mchr9.txt Mchr10.txt Mchrm.txt Mchru.txt
$   mkdir M_ascend
$ mv Mchr1.txt Mchr2.txt Mchr3.txt Mchr4.txt Mchr5.txt Mchr6.txt Mchr7.txt Mchr8.txt Mchr9.txt Mchr10.txt Mchrm.txt Mchru.txt M_ascend
```

Here is my brief description of what this code does: 

To ensure proper alignment of both snp_position.txt and fang_et_al_genotypes.txt when running the join command,
1. I used the transposed_genotypes file (a flip of the fang_et_al_genotypes file using the transpose.awk command).
2. Removed the first two rows with the "tail -n +3 " and created a new file (transposed_1header.txt)
3. Changed the name "Group to SNP_ID" by using the "sed" command mad moved the data to a new file, transgroup.txt.

I merged the two files (snp_postion.txt and transgroup.txt) into a new one (transjoin.txt) and checked the number of rows and columns to determine that the "join" 
command worked. Additionally, I used the "awk" command to identify the range in which each of the groups (ZMMIL, ZMMMR, and ZMMLR) appear. By applying the "cut" command,
I was able to extract the range of columns for each of the groups as well as the SNP_ID, Chromosome, and Position columns into a new file. I confirmed the new number of 
rows and columns and then sorted the files based on the position values (3rd column) in ascending order.
To effectively make a file for each chromosome, I used the "grep" command to get the total number of each for continued accuracy. After this, I extracted each unique 
chromosome into different files and moved them into a new directory, M_ascend.


```
$ sed 's/?/-/g' Msorted.txt > M-sorted.txt
$ mkdir M_descend
$ mv M-sorted.txt M_descend/
$ cd M_descend/
$ (head -n 1 M-sorted.txt && tail -n +2 M-sorted.txt | sort -k3,3nr) > Mdsorted.txt
$ vim Mdsorted.txt
$ { head -n 1 Mdsorted.txt && awk '$2 == 7' Mdsorted.txt; } > Mchr7_de.txt
$ { head -n 1 Mdsorted.txt && awk '$2 == 8' Mdsorted.txt; } > Mchr8_de.txt
$ { head -n 1 Mdsorted.txt && awk '$2 == 1' Mdsorted.txt; } > Mchr9_de.txt
$ { head -n 1 Mdsorted.txt && awk '$2 == 9' Mdsorted.txt; } > Mchr9_de.txt
$ { head -n 1 Mdsorted.txt && awk '$2 == 10' Mdsorted.txt; } > Mchr10_de.txt
$ { head -n 1 Mdsorted.txt && awk '$2 == 1' Mdsorted.txt; } > Mchr1_de.txt
$ { head -n 1 Mdsorted.txt && awk '$2 == 2' Mdsorted.txt; } > Mchr2_de.txt
$ { head -n 1 Mdsorted.txt && awk '$2 == 3' Mdsorted.txt; } > Mchr3_de.txt
$ { head -n 1 Mdsorted.txt && awk '$2 == 4' Mdsorted.txt; } > Mchr4_de.txt
$ { head -n 1 Mdsorted.txt && awk '$2 == 5' Mdsorted.txt; } > Mchr5_de.txt
$ { head -n 1 Mdsorted.txt && awk '$2 == 6' Mdsorted.txt; } > Mchr6_de.txt
$ wc Mchr1_de.txt Mchr10_de.txt Mchr2_de.txt Mchr3_de.txt Mchr4_de.txt Mchr5_de.txt Mchr6_de.txt Mchr7_de.txt Mchr8_de.txt Mchr9_de.txt
$ awk -F "\t" '{print NF; exit}' Mchr1_de.txt Mchr10_de.txt Mchr2_de.txt Mchr3_de.txt Mchr4_de.txt Mchr5_de.txt Mchr6_de.txt Mchr7_de.txt Mchr8_de.txt Mchr9_de.txt
$ vim Mchr1_de.txt
$ vim Mchr10_de.txt
$ vim Mchr2_de.txt


```

Here is my brief description of what this code does:
Using the "sed" command, I to changed the "?" to "-" and moved the data into a new file, M-sorted.txt. I created a new directory, M_descend where I moved the M-sorted.txt 
to. Then, I applied the reverse sort command to arrange the data in descending position values. To confirm that this worked, I used "vim" to look at the data. After which, 
I extracted each of the of the unique chromosome identifiers into different files.


### Teosinte Data

```
$ mkdir Teosinte
$ cp transjoin.txt Teosinte/
$ cd Teosinte/
$ awk '{for(i=1;i<=NF;i++) if($i == "ZMPIL") count++} END{print count}' transjoin.txt
$ awk '{for(i=1;i<=NF;i++) if($i == "ZMPBA") count++} END{print count}' transjoin.txt
$ awk '{for(i=1;i<=NF;i++) if($i == "ZMPJA") count++} END{print count}' transjoin.txt
$ cut -d $'\t' -f1,3,4,89-988,1178-1218,989-1022 transjoin.txt > T.txt
$ vim T.txt
$ awk -F "\t" '{print NF; exit}' T.txt
$ wc T.txt
$ (head -n 1 T.txt && tail -n +2 T.txt | sort -k3,3n) > Tsort.txt
$ vim Tsort.txt
$ awk -F "\t" '{print NF; exit}' Tsort.txt
$ wc Tsort.txt
$ for i in {1..10}; do     
    { head -n 1 Tsort.txt && awk -v chr="$i" '$2 == chr' Tsort.txt; } > "Tchr${i}.txt"; 
  done
$ { head -n 1 Tsort.txt && awk '$2 == "multiple"' Tsort.txt; } > Tchrm.txt
$ { head -n 1 Tsort.txt && awk '$2 == "multiple"' Tsort.txt; } > Tchru.txt
$ vim Tchrm.txt
$ { head -n 1 Tsort.txt && awk '$2 == "unknown"' Tsort.txt; } > Tchru.txt
$ mkdir T_ascend
$ wc Tchr10.txt  Tchr2.txt  Tchr4.txt  Tchr6.txt  Tchr8.txt  Tchrm.txt  transjoin.txt  T.txt Tchr1.txt   Tchr3.txt  Tchr5.txt  Tchr7.txt  Tchr9.txt  Tchru.txt  Tsort.txt
$ awk -F "\t" '{print NF; exit}' Tchr10.txt  Tchr2.txt  Tchr4.txt  Tchr6.txt  Tchr8.txt  Tchrm.txt  transjoin.txt  T.txt Tchr1.txt   Tchr3.txt  Tchr5.txt  Tchr7.txt  Tchr9.txt  Tchru.txt  Tsort.txt
$ mv Tchr10.txt  Tchr2.txt  Tchr4.txt  Tchr6.txt  Tchr8.txt  Tchrm.txt Tchr1.txt   Tchr3.txt  Tchr5.txt  Tchr7.txt  Tchr9.txt  Tchru.txt  T_ascend/

$ mkdir T_descend
$ sed 's/?/-/g' Tsort.txt > T-sort.txt
$ vim T-sort.txt
$ mv T-sort.txt T_descend/
$ cd T_descend/
$ wc T-sort.txt
$ awk -F "\t" '{print NF; exit}' T-sort.txt
$ (head -n 1 T-sort.txt && tail -n +2 T-sort.txt | sort -k3,3nr) > T-sorted.txt
$ for i in {1..10}; do     
    out_file="Tchr_de${i}.txt";     
    { head -n 1 T-sorted.txt && awk -v chr="$i" '$2 == chr' T-sorted.txt; } > "$out_file";    
    echo "Chromosome $i: $(wc -l < "$out_file") lines"; 
  done
```

Here is my brief description of what this code does:

I created a new directory and named it Teosinte. From the transjoin.txt file, I identified the range in which each group (ZMPIL, ZMPBA, and ZMPJA) appears. Using the "cut" command, I extracted the relevant 
columns for each group along with the SNP_ID, Chromosome, and Position columns into a new file, T.txt. After confirming the updated number of rows and columns, I sorted the data based on the position values 
in the third column in increasing order. To create individual files for each chromosome, I used the a for loop command to extract each unique chromosome into separate files and organized them into a new 
directory named T_ascend.

I replaced the "?" to "-" and saved the modified data into a new file, T-sort which I moved into a new directory, T_descend. Then, I applied the reverse sort command to arrange the data in descending position 
values. To confirm that this worked, I used "vim" to look at the data. After which, I extracted each of the of the unique chromosome identifiers into different files.