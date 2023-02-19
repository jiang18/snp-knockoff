# SNP knockoffs 

### Input file
- PLINK raw file
- Example: `geno` ([QTL-MAS 2012](https://bmcproc.biomedcentral.com/articles/10.1186/1753-6561-8-S5-S1))
- `plink --bfile geno --recode A --out geno`

### Python script
```sh
export OMP_NUM_THREADS=10
python3 make_knockoff.py
```
> **Note**
> `split_n` and `ext` may require tuning.

### Output files
- `extract.txt`: all the SNPs in `geno.txt`
- `geno.txt` in which SNPs are ordered the same as in the input raw file.
- `knockoff.t.txt` in which knockoffs are transposed and correspond to SNPs in geno.txt. This file can be easily converted to PLINK traw which can be further converted to PLINK bed/bim/fam by `plink2 --import-dosage`.

```sh
plink --bfile geno --extract extract.txt --make-bed --out geno2
paste geno2.bim knockoff.t.txt -d " " > knockoff.traw
plink2 --import-dosage knockoff.traw noheader skip0=1 skip1=2 single-chr=6 --make-bed --out knockoff --fam geno2.fam

```

