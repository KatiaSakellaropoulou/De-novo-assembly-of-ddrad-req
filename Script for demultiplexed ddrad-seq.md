When we want to do the de novo assembly, we use the same steps as in the ddrad-seq pipeline. 
Since there is no genome to map to, we skip the steps of BWA-me2 alignment and gstacks. So, after this script
the next step is to prepare for snp calling and filtering with the populations and vcftools. 
```
#PBS -N de_novo_assembly
#PBS -l select=1:ncpus=10:mem=100gb:scratch_local=150gb
#PBS -l walltime=20:00:00

trap 'clean_scratch' TERM EXIT
cd "$SCRATCHDIR" || exit 1

cp -r /storage/brno2/home/kat/demultiplex_Lim3
cp /storage/brno2/home/kat/popmap.txt .
mkdir -p denovo_output

module add stacks


denovo_map.pl -T 10 --samples apatura_denovo \
--popmap popmap.txt \
-o denovo_output \
--paired \
-X "populations: --structure --plink --vcf" -X "ustacks: -M 3 --force-diff-len"

cp -r denovo_output /storage/brno2/home/kat/RAD2_Paph_Apat/de_novo
```
