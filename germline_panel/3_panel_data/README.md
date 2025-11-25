# Panel data

Generate panel data for UMCCR post-processing

```bash
mkdir -p output/

cut -f2 -d$'\t' ../2_final_panel/final_panel.tsv | \
  tail -n+2 | \
  grep -v NA | \
  sed 's/\..*$//' | \
  sort > output/umccr_predisposition_genes.gene_symbols.txt

./scripts/create_transcript_bed.py > output/umccr_predisposition_genes.transcript_regions.bed \
  --panel_fp ../2_final_panel/final_panel.tsv \
  --ensembl_transcript_data_fp ../../resources/ensembl.transcripts.bed
```

Generate panel for QCI

- the FCC 'haem malignancy predisposition' genes are excluded except `TP53`
- see `../1_panel_sources/pmcc_fcc/data/MP-MF-ADM-11v7 PMCC Familial cancer predisposition test request form.pdf`

```bash
genes_exclude='
ANKRD26
CEBPA
DDX41
ETV6
GATA2
MBD4
RUNX1
SAMD9
SAMD9L
'

grep -v -w -f <(echo "${genes_exclude}") ../2_final_panel/final_panel.tsv > output/qci_gene_panel.tsv
```
