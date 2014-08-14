# About

I have the extra biom and map files from all of the splitting hidden. You need to have QIIME installed to be able to reproduce these tables. I have provided some of the code where ever the end result does not appear to be so obvious. 

First, to see the available keys (or classes) that are available: 
```bash 
head -1 AmericanGut.txt | tr '\t' '\n'
```

# Processing & Splitting the Data

Split the data up by the `SEX` field and rename the map files that QIIME outputs. I am going to only merge the `male` and `female` samples. I am not sure what the 47 and 48 keys are, so I left them out of the merged biom and map files.  

```bash
split_otu_table.py -i AmericanGut.biom -m AmericanGut.txt -f SEX -o AmericanGut-Sex
cd AmericanGut-Sex/
merge_mapping_files.py -m mapping_male.txt,mapping_female.txt -o ../AmericanGut-Sex.txt
merge_otu_tables.py -i AmericanGut_male.biom,AmericanGut_female.biom -o ../AmericanGut-Sex.biom
cd ..
```

Processing the other data subsets is approximately the same. I did reduce the `DIET_TYPE` down to just a two column map file using a little bit of `awk`.

```bash 
merge_otu_tables.py -i AmericanGut_Omnivore.biom,AmericanGut_Omnivore_but_no_red_meat.biom,AmericanGut_Vegan.biom,AmericanGut_Vegetarian.biom,AmericanGut_Vegetarian_but_eat_seafood.biom -o ../AmericanGut-DietType.biom
merge_mapping_files.py -m mapping_Omnivore.txt,mapping_Omnivore_but_no_red_meat.txt,mapping_Vegan.txt,mapping_Vegetarian_but_eat_seafood.txt,mapping_Vegetarian.txt -o tmp_map.txt
head -1 tmp_map.txt |tr '\t' '\n' | awk '{n=n+1;print n,$1}' | grep DIET\_TYPE   # find the column with DIET_TYPE
cat tmp_map.txt | awk -F $'\t' '{print $1,"\t",$163}' > ../AmericanGut-DietType.txt
```

 
