# 5-prime-mNET-seq-data-analysis
## Alignment
### Bowtie alignment command:
```bash
bowtie-build -f NC_000913_3.fna bowtie_nc000913_3
bowtie --phred33-quals -5 6 -3 15 -n 0 -l 30 -m 1 \
ref_genome/bowtie_nc000913_3 \
sampleID_5_prime_mNET_seq_data.fastq \
sampleID_5_prime_mNET_seq_Tf6t15N0L30M1_ec3_bowtieOut.txt \
2> sampleID_5_prime_mNET_seq_Tf6t15N0L30M1_ec3_bowtieStats.txt
```

Parameters:
- ``-5 6``: trim off first 6bp
- ``-3 15``: trim off last 15bp (30bp left for alignment)
- ``-n 0``: maximum number of mismatch in seed is 0
- ``-l 30``: seed length is 30, which is equal to the trimmed read length
- ``-m 1``: Suppress all alignments for a particular read or pair if more than 1 reportable alignments exist for it. Only report uniquely aligned reads. Single reads aligned uniquely to both + and - strands will also be discarded.

## Count Aligned Reads
### Parse bowtie output:
```Bash
 ./count_bowtie_out.py 10 bowtie_output.txt count_stats.txt count_table.txt
 ```
Output format:
Tab delimited. 3 fileds from left to right: TSS position (0-based), + strand read count, - strand read count. 

### Filter tRNA Reads
Filter out reads started within the annotated tRNA regions. 
Python [intervaltree](https://pypi.python.org/pypi/intervaltree/2.0.4) package used in the script. 

```Bash
filter_tRNA.py NC_000913_3.gff count_table.txt count_table_tRNAf_stats.txt count_table_tRNAf.txt NC_000913_3_tRNA.gff
```

## Identify TSS
Python script command:
```Bash
identify_TSS.py count_table_tRNAf.txt NC_000913_3.fna call_tss_stats.txt tss_list.txt
```

Output fields are:
1. TSS coordinate (first base is 0)
2. Strand
3. Sequence from upstream 50bp to downstream 50bp of TSS
4. 4 - 14: Read counts from upstream 5bp to downstream 5bp.

## Notes
1. Type ``./somePythonScript.py`` and <kbd>RETURN</kbd> to get usage help of the script. 
2. Genome coordinates are kept 0-based in all scripts. 

## Contact
If you have questions about the scripts, please contact y.will.zhang@gmail.com.


