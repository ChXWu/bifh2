# Codebook for hw2 of bioinformatics 

### Part 2

1) Using Bio.Entrez.efetch and SeqIO, download from GenBank, the mRNA sequences
for the human genes HBA1(NM_000558)  and HBA2 (NM_000517) . Print the sequence
ID, name, and description of these sequence records.
#### Code 
```
from Bio import Entrez, SeqIO
Entrez.email='cw3114@nyu.edu'
temp =Entrez.efetch(db="nucleotide",rettype="gb",id="NM_000558") 
out = open("NM_000558.fasta",'w') 
gbseq1 =SeqIO.read(temp,"genbank")
SeqIO.write(gbseq1,out,"fasta")

temp =Entrez.efetch(db="nucleotide",rettype="gb",id="NM_000517") 
out = open("NM_000517.fasta",'w') 
gbseq2 =SeqIO.read(temp,"genbank")
SeqIO.write(gbseq2,out,"fasta")

temp.close()
out.close()
print(gbseq1)
print(gbseq2)
```
#### Result 
```
ID: NM_000558.5
Name: NM_000558
Description: Homo sapiens hemoglobin subunit alpha 1 (HBA1), mRNA
...

ID: NM_000517.6
Name: NM_000517
Description: Homo sapiens hemoglobin subunit alpha 2 (HBA2), mRNA
...

```

2)
Read the sequence records from a list of GenBank IDs in a text file (seq_id.list) into a
Python list, and import them using
Bio.Entrez.efetch
into a Python list of SeqRecords.
Print the sequence name and the length of each of these sequences.
3)
Import a large set of sequences from a Fasta file: ecogene.fasta
a.
How many sequences are in this file?
b.
Using Biopython, read the sequence ID, name, and description for the first
Sequence Record. What do you get? Compare to the metadata available for the
GenBank records above.
c.
What is the total length of all of the sequences in the file (just the DNA, not
headers)
d.
Make a new FASTA file with just the sequences >= 300 bp in length
i.
Show the Python code that you used
e.
Make a new FASTA file with just the sequences with %GC > 60
i.
Show the Python code that you used

