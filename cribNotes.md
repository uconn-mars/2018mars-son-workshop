### download files from basespace

cd _path to downloaded files_

mkdir fastq

for i in */Data/Intensities/BaseCalls/*.gz; do mv $i "fastq""/"${i%%-*}"."`basename $i`; done

### launch mothur

mothur

make.file(inputdir=Workshop-31942914/fastq, outputdir=Workshop-31942914/,type=gz)

### open another terminal window to edit, remove dashes from H2O samples, save as better name

make.contigs(file=may18ws.files)

### may need to unzip if error thrown

summary.seqs(fasta=current)

screen.seqs(fasta=current, group=current, summary=current, maxambig=0, maxlength=275)

summary.seqs(fasta=current)

unique.seqs(fasta=current)

summary.seqs(fasta=current, name=current)

