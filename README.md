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
