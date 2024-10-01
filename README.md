# Week-4-5

## Problem 1
Count of amino acids: 30<br>
Count of bases: 31×3=93 bases

```bash
echo -n "KVRMFTSELDIMLSVNGPADQIKYFCRHWT" | wc -c
```

## Problem 2
Count of genes: 3248

```bash
prodigal -i GCA_000008565.1_ASM856v1_genomic.fna -o prodigal_output.gbk
grep -c "CDS" prodigal_output.gbk
```

## Problem 3
Genome with the highest number of genes: ./GCA_000005845.2_ASM584v2_genomic.fna, 4319
```bash
#!/bin/bash                                                                                                                                                                       

output_file="gene_counts.txt"
echo "Genome file, Gene count" > $output_file

for genome in $(find $genomes_dir -name 'GCA*.fna')
do
   echo "Processing $genome..."

   prodigal_output="prodigal_output.gbk"
   prodigal -i $genome -o $prodigal_output

   gene_count=$(grep -c "CDS" $prodigal_output)

   echo "$genome, $gene_count" >> $output_file

   rm $prodigal_output
done

max_genome=$(sort -t ',' -k2 -n $output_file | tail -1)

echo "Genome with the highest number of genes: $max_genome"


```
