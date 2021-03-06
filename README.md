# Intro

The repos contains individual wrokflows to process cohorts of human genome samples through alignment, calling, joint calling and variant quality score recalibration. Some additional workflows for pre and post BAM/gVCF manipulation/checking are also included.

## To run

Each folder contains a workflow which is part of the main workflow or a separate workflow to check/prepare the data for a specific part of the main workflow.


### Main workflow
* **align** - Need to provide a sample sheet containing paths to forward and reverse reads. BWA, Picard MarkDuplicates and GATK BaseRecalibrator are run on the samples.
* **generate-gvcf** - Need to provide a sample sheet with paths to the BAMs. GATKs HaplotypeCaller is run in gVCF mode on the samples.
* **combine-gvcfs** - Need to provide a sample sheet with paths to the gVCFs. GATKs CombineGVCFs is run on all samples.
* **genomics-db-import** - Need to provide a sample sheet with paths to the gVCFs. GATKs GenomicsDBImport is run on all samples. A new database can be created or an existing database can be updated.
* **genome-calling** - Need to provide the path to the combined gVCF. GenotypeGVCFs are run jointly on all samples on full genome. VQSR is also applied to chromosome 1 to 22, X, Y and MT. GenotypeGVCFs are run on chromosome level where SNP and INDEL VQSR are run on genome level (**according to GATKs best practices**).
* **filter-vcf** - Filter final VCFs based on the PASS flag in the FILTER column. Needed after VQSR.

**Note**: Can use **combine-gvcfs** or **genomics-db-import** for combining the gVCFs. **genomics-db-import** is however the latest method and allows for updating of an existing database / combined set.

### Separate workflows
* **bam-to-cram** - Need to provide a sample sheet with paths to the BAMs. BAMs are converted to CRAM (v3), indexed, stats are calculated and md5sums are generated.
* **cram-to-bam** - Need to provide a sample sheet with paths to the CRAMs. CRAMs are converted to BAM, indexed, stats are calculated and md5sums are generated.
* **cram-to-fastq** - Need to provide a sample sheet with paths to the CRAMs. CRAMs are converted to Fastq (forward and reverse pair).
* **index-bams** - Need to provide a sample sheet with paths to the BAMs. BAMs are indexed.
* **index-vcf** - Need to provide a sample sheet with paths to the VCFs/gVCFs. VCFs/gVCFs are indexed.
* **bam-flagstat** - Need to provide a sample sheet with paths to the BAMs/CRAMs. Flagstat reporsts are created.
* **mt-calling** - Need to provide a samples sheet with paths to BAMs/CRAMs. Mitochondrial variant calling are doen with the Mutect2 pipeline.
* **combine-lanes** - Need to provide a samplesheet to directories with the multi-lane BAMs. BAMs are merged and indexed.
* **validate-gvcf** - Need to provide a samplesheet with path to gVCFs and gender info. gVCF validation are done on chromosome level using GATK's ValidateVariants. 
* **genotype-refinement** - Need to provide a path to the VQSRed VCF. Genotype Refinment are done on chromosome level using GATK's CalculateGenotypePosteriors, VariantFiltration and VariantAnnotator. 

## Note
* The main workflow can be followed for a single sample or joint calling sample. For a single sample the **combine-gvcfs** or **genomics-db-import** can be skipped and the **genome-calling** configs can point to single sample `.g.vcf`.

## Example
The GiaB dataset can be downloaded and used for testing
```
wget -O NA12878_R1.fastq.gz ftp://ftp-trace.ncbi.nih.gov/giab/ftp/data/NA12878/Garvan_NA12878_HG001_HiSeq_Exome/NIST7035_TAAGGCGA_L001_R1_001.fastq.gz
wget -O NA12878_R2.fastq.gz ftp://ftp-trace.ncbi.nih.gov/giab/ftp/data/NA12878/Garvan_NA12878_HG001_HiSeq_Exome/NIST7035_TAAGGCGA_L001_R2_001.fastq.gz
```
