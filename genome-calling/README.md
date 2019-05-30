# These scripts are still under development. Please do not use yet.

# Intro

The Nextflow script calls on genome level (runs GenotypeGVCFs) and VSQR SNPs and INDELs. (Currently only doing it for chr1 -> 22)

Please see `nextflow.conf` for GATK version and references databases used. Path to combined gVCF file is also specified in nextflow config.


## To run

For each dataset
1) Modify your `nextflow.config` to read the `gvcf_file` and specify the output directory e.g. `out_dir = "/global/scratch/gerrit/projects/adme/datasets/NA12878/nextflow-out"`
2) Run the workflow
```
nextflow -log nextflow.log run -w /global5/scratch/gerrit/projects/adme/datasets/NA12878-work -c /home/gerrit/code/recalling/genome-calling/nextflow.config.NA12878 /home/gerrit/code/recalling/genome-calling/main.nf -profile cbio -with-report NA12878.report.html -with-trace -with-timeline NA12878.timeline.html -resume
```

## Output

The output directory will contain
1. A per chromosome VCF file
1. A recalibrated SNP per chromsome VCF file
1. A recalibrated SNP and INDEL per chromsome VCF file
1. A combined genome recalibrated SNP and INDEL VCF file
