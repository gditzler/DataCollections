# About

I have the extra biom and map files from all of the splitting hidden. You need to have QIIME installed to be able to reproduce these tables. I have provided some of the code where ever the end result does not appear to be so obvious. 

First, to see the available keys (or classes) that are available: 
```bash 
head -1 AmericanGut.txt | tr '\t' '\n'
```

# Processing & Splitting the Data

Split the data up by the `SEX` field and rename the map files that QIIME outputs. I am going to only merge the `male` and `female` samples. I am not sure what the 47 and 48 keys are, so I left them out of the merged biom and map files.  

First, we need to isolate the gut samples first. 

```bash 
split_otu_table.py -i AmericanGut.biom -m AmericanGut.txt -f BODY_SITE -o AmericanGut-Site/
cd AmericanGut-Site/
cp AmericanGut_UBERON:feces.biom ../AmericanGut-Gut.biom
cp mapping_UBERON:feces.txt ../AmericanGut-Gut.txt
cd ..
```

```bash
split_otu_table.py -i AmericanGut-Gut.biom -m AmericanGut-Gut.txt -f SEX -o AmericanGut-Sex/
cd AmericanGut-Sex/
merge_mapping_files.py -m mapping_male.txt,mapping_female.txt -o ../AmericanGut-Gut-Sex.txt
merge_otu_tables.py -i AmericanGut-Gut_male.biom,AmericanGut-Gut_female.biom -o ../AmericanGut-Gut-Sex.biom
cd ..
```

Processing the other data subsets is approximately the same. I did reduce the `DIET_TYPE` down to just a two column map file using a little bit of `awk`.

```bash 
split_otu_table.py -i AmericanGut-Gut.biom -m AmericanGut-Gut.txt -f DIET_TYPE -o AmericanGut-Diet/
merge_otu_tables.py -i AmericanGut-Gut_Omnivore.biom,AmericanGut-Gut_Omnivore_but_no_red_meat.biom,AmericanGut-Gut_Vegan.biom,AmericanGut-Gut_Vegetarian.biom,AmericanGut-Gut_Vegetarian_but_eat_seafood.biom -o ../AmericanGut-Gut-Diet.biom
merge_mapping_files.py -m mapping_Omnivore_but_no_red_meat.txt,mapping_Omnivore.txt,mapping_Vegan.txt,mapping_Vegetarian_but_eat_seafood.txt,mapping_Vegetarian.txt -o tmp_map.txt
head -1 tmp_map.txt |tr '\t' '\n' | awk '{n=n+1;print n,$1}' | grep DIET\_TYPE   # find the column with DIET_TYPE
cat tmp_map.txt | awk -F $'\t' '{print $1,"\t",$163}' > ../AmericanGut-Gut-Diet.txt
cd ..

```

We converted the `DIET_TYPE` map file to a second mapping file that reduces the data only two classes: (a) all omnivores, and (b) all styles of vegetarian (including vegans).


# Citing the data 

Refer to the following links and within

* [American Gut on GitHub - biocore](https://github.com/biocore/American-Gut)
* [The American Gut Project](http://microbio.me/americangut/FAQ.psp)
 
