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
Genome with the highest number of genes: ./GCA_000006745.1_ASM674v1_genomic.fna, 3594
```bash
File: ./GCA_000006745.1_ASM674v1_genomic.fna_genes.gbk - Number of CDS: 3594
File: ./GCA_000006825.1_ASM682v1_genomic.fna_genes.gbk - Number of CDS: 2032
File: ./GCA_000006865.1_ASM686v1_genomic.fna_genes.gbk - Number of CDS: 2383
File: ./GCA_000007125.1_ASM712v1_genomic.fna_genes.gbk - Number of CDS: 3152
File: ./GCA_000008525.1_ASM852v1_genomic.fna_genes.gbk - Number of CDS: 1579
File: ./GCA_000008545.1_ASM854v1_genomic.fna_genes.gbk - Number of CDS: 1866
File: ./GCA_000008565.1_ASM856v1_genomic.fna_genes.gbk - Number of CDS: 3248
File: ./GCA_000008605.1_ASM860v1_genomic.fna_genes.gbk - Number of CDS: 1009
File: ./GCA_000008625.1_ASM862v1_genomic.fna_genes.gbk - Number of CDS: 1776
File: ./GCA_000008725.1_ASM872v1_genomic.fna_genes.gbk - Number of CDS: 897
File: ./GCA_000008745.1_ASM874v1_genomic.fna_genes.gbk - Number of CDS: 1063
File: ./GCA_000008785.1_ASM878v1_genomic.fna_genes.gbk - Number of CDS: 1505
File: ./GCA_000027305.1_ASM2730v1_genomic.fna_genes.gbk - Number of CDS: 1748
File: ./GCA_000091085.2_ASM9108v2_genomic.fna_genes.gbk - Number of CDS: 1063
```

```bash
#!/bin/bash                                                                                                                                                                       

# Script to find the genome with the highest number of genes                                                                                                                      

max_gene_count=0
top_genome=""

# Loop through each genome directory                                                                                                                                              
for genome_folder in GCA_*; do
  genomic_file=$(find "$genome_folder" -name "*_genomic.fna")

  if [[ -f "$genomic_file" ]]; then
    output_genes="${genome_folder}_genes.gbk"
    output_proteins="${genome_folder}_proteins.faa"

    prodigal -i "$genomic_file" -o "$output_genes" -a "$output_proteins"

    cd_count=$(grep -c "CDS" "$output_genes")
    echo "Genome: $genomic_file - Number of genes: $cd_count"

    if (( cd_count > max_gene_count )); then
      max_gene_count=$cd_count
      top_genome=$genomic_file
    fi
  else
    echo "No genomic file found in $genome_folder"
  fi
done

# Display the genome with the highest number of genes                                                                                                                             
echo "Genome with the highest number of genes: $top_genome with $max_gene_count genes."

# ---------------------------                                                                                                                                                     

# Script to count the number of CDS entries in .gbk files                                                                                                                         

for genes_file in $(find . -name "*_genes.gbk"); do
  cds_total=$(grep -c "CDS" "$genes_file")
  echo "File: $genes_file - Number of CDS: $cds_total"
done


```

## Problem 4
Genome with the highest count using Prokka: GCA_000006745.1_ASM674v1_genomic.fna Total CDS count: 3589
```bash
Directory: GCA_000006745.1_ASM674v1_genomic.fna_prokka_results - Total CDS count: 3589
Directory: GCA_000006825.1_ASM682v1_genomic.fna_prokka_results - Total CDS count: 2028
Directory: GCA_000006865.1_ASM686v1_genomic.fna_prokka_results - Total CDS count: 2383
Directory: GCA_000007125.1_ASM712v1_genomic.fna_prokka_results - Total CDS count: 3150
Directory: GCA_000008525.1_ASM852v1_genomic.fna_prokka_results - Total CDS count: 1577
Directory: GCA_000008545.1_ASM854v1_genomic.fna_prokka_results - Total CDS count: 1861
Directory: GCA_000008565.1_ASM856v1_genomic.fna_prokka_results - Total CDS count: 3245
Directory: GCA_000008605.1_ASM860v1_genomic.fna_prokka_results - Total CDS count: 1001
Directory: GCA_000008625.1_ASM862v1_genomic.fna_prokka_results - Total CDS count: 1771
Directory: GCA_000008725.1_ASM872v1_genomic.fna_prokka_results - Total CDS count: 892
Directory: GCA_000008745.1_ASM874v1_genomic.fna_prokka_results - Total CDS count: 1058
Directory: GCA_000008785.1_ASM878v1_genomic.fna_prokka_results - Total CDS count: 1504
Directory: GCA_000027305.1_ASM2730v1_genomic.fna_prokka_results - Total CDS count: 1748
Directory: GCA_000091085.2_ASM9108v2_genomic.fna_prokka_results - Total CDS count: 1056
```

```bash
#!/bin/bash

# Annotate genomes with Prokka
for genome_directory in GCA_*; do
  genomic_file=$(find "$genome_directory" -name "*_genomic.fna")

  if [[ -f "$genomic_file" ]]; then
    output_directory="${genome_directory}_prokka_results"
    prokka --outdir "$output_directory" --prefix "${genome_directory}" "$genomic_file"

    result_file="$output_directory/${genome_directory}.txt"

    if [[ -f "$result_file" ]]; then
      cds_annotation_count=$(grep -w "CDS" "$result_file" | awk '{print $2}')
      echo "Genome: $genomic_file - Annotated CDS count: $cds_annotation_count"
    else
      echo "Could not find Prokka output file for $genomic_file"
    fi
  else
    echo "Genomic file not found in $genome_directory"
  fi
done

# Collect CDS counts from Prokka output
for prokka_output in *_prokka_results; do
  text_file=$(find "$prokka_output" -name "*.txt")

  if [[ -f "$text_file" ]]; then
    cds_count_total=$(grep "CDS:" "$text_file" | awk '{print $2}')
    echo "Directory: $prokka_output - Total CDS count: $cds_count_total"
  else
    echo "No text file found in $prokka_output"
  fi
done
```
There is a slight difference in gene counts between Prokka and Prodigal can occur due to various factors. Prodigal (3594) might identify slightly more coding sequences than Prokka (3589) because they use different algorithms for gene prediction. Prokka is designed for annotating genes using various biological data sources, rather than solely making predictions based on sequence features like Prodigal. This comprehensive approach can lead to the elimination of some predicted genes during the annotation process. Prokka also includes annotations for non-coding genes, while Prodigal focuses strictly on protein-coding sequences.
