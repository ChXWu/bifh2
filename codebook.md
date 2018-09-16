# Lab notebook for CSE185 Final Project

## Obtain the sequencing data
```
fastq-dump SRR5854183
fastq-dump SRR5853383 

fastq-dump SRR5858561
fastq-dump SRR5858469

fastq-dump SRR5853462

```
Note: this data is from emm genotype 1,2 and 11, since the data is large, we keep at most 3 out them at once 

## Check the quality of the sequencing data and filter before trimming 
```
fastqc -o ./fastqc_result *.fastq 
```
## Filter the reads
According to the fastqc report these files have reads with good quailty and do not need to be trimmed
```
sickle se -f SRR5854183.fastq -o trimmed_SRR5854183.fastq -t sanger
sickle se -f SRR5853383.fastq -o trimmed_SRR5853383.fastq -t sanger
sickle se -f SRR5858561.fastq -o trimmed_SRR5858561.fastq -t sanger
sickle se -f SRR5858469.fastq -o trimmed_SRR5858469.fastq -t sanger
sickle se -f SRR5853462.fastq -o trimmed_SRR5853462.fastq -t sanger
```

## Check the quality of the sequencing data and filter after trimming 
```
fastqc -o ./fastqc_result *.fastq 
```
## Align the sequencing data respectively to the emm_Gene reference 
```
bwa mem GAS_Reference_DB/emm_Gene-DB_Final.fasta trimmed_SRR5854183.fastq  | samtools view -S -b | samtools sort > 4183.bam
samtools index 4183.bam
samtools mpileup -f GAS_Reference_DB/emm_Gene-DB_Final.fasta 4183.bam > 4183.mpileup

java -jar /home/linux/ieng6/cs185s/public/tools/VarScan.jar mpileup2snp \
    4183.mpileup \
    --min-var-freq 0.95 \
    --variants --output-vcf 1 > vcf_result/4183.vcf
```
5 SNP mutatons detected 
```
bwa mem GAS_Reference_DB/emm_Gene-DB_Final.fasta trimmed_SRR5853383.fastq  | samtools view -S -b | samtools sort > 3383.bam
samtools index 3383.bam
samtools mpileup -f GAS_Reference_DB/emm_Gene-DB_Final.fasta 3383.bam > 3383.mpileup

java -jar /home/linux/ieng6/cs185s/public/tools/VarScan.jar mpileup2snp \
    3383.mpileup \
    --min-var-freq 0.95 \
    --variants --output-vcf 1 > vcf_result/3383.vcf
```
5 SNP mutatons detected 
```
bwa mem GAS_Reference_DB/emm_Gene-DB_Final.fasta trimmed_SRR5858561.fastq  | samtools view -S -b | samtools sort > 8561.bam
samtools index 8561.bam
samtools mpileup -f GAS_Reference_DB/emm_Gene-DB_Final.fasta 8561.bam > 8561.mpileup

java -jar /home/linux/ieng6/cs185s/public/tools/VarScan.jar mpileup2snp \
    8561.mpileup \
    --min-var-freq 0.95 \
    --variants --output-vcf 1 > vcf_result/8561.vcf
```
9 SNP mutatons detected 
```
bwa mem GAS_Reference_DB/emm_Gene-DB_Final.fasta trimmed_SRR5858469.fastq  | samtools view -S -b | samtools sort > 8469.bam
samtools index 8469.bam
samtools mpileup -f GAS_Reference_DB/emm_Gene-DB_Final.fasta 8469.bam > 8469.mpileup

java -jar /home/linux/ieng6/cs185s/public/tools/VarScan.jar mpileup2snp \
    8469.mpileup \
    --min-var-freq 0.95 \
    --variants --output-vcf 1 > vcf_result/8469.vcf
```
7 SNP mutatons detected 
```
bwa mem GAS_Reference_DB/emm_Gene-DB_Final.fasta trimmed_SRR5853462.fastq  | samtools view -S -b | samtools sort > 3462.bam
samtools index 3462.bam
samtools mpileup -f GAS_Reference_DB/emm_Gene-DB_Final.fasta 3462.bam > 3462.mpileup

java -jar /home/linux/ieng6/cs185s/public/tools/VarScan.jar mpileup2snp \
    3462.mpileup \
    --min-var-freq 0.95 \
    --variants --output-vcf 1 > vcf_result/3462.vcf
```
11 SNP mutatons detected 

## Install vcr tools
```
git clone https://github.com/vcftools/vcftools.git
cd vcftools
./autogen.sh
./configure
make
make install
```

Command fails, no sudo permmsion 
try install SnpSift instead
```
wget http://sourceforge.net/projects/snpeff/files/snpEff_latest_core.zip
unzip snpEff_latest_core.zip
```

## Run comparison
```
java -jar ~/snpEff/SnpSift.jar concordance -v ~/final/vcf_result/3383.vcf ~/final/vcf_result/4183.vcf > 3383_4183.txt
java -jar ~/snpEff/SnpSift.jar concordance -v ~/final/vcf_result/8469.vcf ~/final/vcf_result/8561.vcf > 8469_8561.txt
java -jar ~/snpEff/SnpSift.jar concordance -v ~/final/vcf_result/3383.vcf ~/final/vcf_result/8561.vcf > 3383_8561.txt
java -jar ~/snpEff/SnpSift.jar concordance -v ~/final/vcf_result/3383.vcf ~/final/vcf_result/3462.vcf > 3383_3462.txt

java -jar ~/snpEff/SnpSift.jar concordance -v ~/final/vcf_result/3383.vcf ~/final/vcf_result/8561.vcf ~/final/vcf_result/4183.vcf ~/final/vcf_result/8469.vcf ~/final/vcf_result/3462.vcf > overall.txt

```
