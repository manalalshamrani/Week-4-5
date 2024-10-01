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
