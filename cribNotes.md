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

count.seqs(name=current, group=current)

pcr.seqs(fasta=silva.nr_132.fasta, start=13860, end=23450, keepdots=F)

align.seqs(fasta=may18ws.trim.contigs.good.unique.fasta, reference=../../silva.seed_v132.pcr.align)

summary.seqs(fasta=current, count=current)

screen.seqs(fasta=current, count=current, summary=current, start=2, end=9584, maxhomop=8)

filter.seqs(fasta=current, vertical=T)

summary.seqs(fasta=current, count=current)

pre.cluster(fasta=current, diffs=2, count=current)

summary.seqs(fasta=current, count=current)

chimera.uchime(fasta=current, count=current, dereplicate=t)

remove.seqs(fasta=current, accnos=current, count=current)

summary.seqs(fasta=current, count=current)

classify.seqs(fasta=current, count=current, reference=../../silva.seed_v132.pcr.align, taxonomy=../../silva.seed_v132.tax, cutoff=80)

remove.lineage(fasta=current, count=current, taxonomy=current, taxon=Chloroplast-Mitochondria-unknown-Archaea-Eukaryota)

summary.tax(taxonomy=current, count=current)

dist.seqs(fasta=current, countends=F, cutoff= 0.03)

cluster(column=current, count=current, method=opti)

make.shared(list=current, count=current)

classify.otu(list=current, count=current, taxonomy=current)

get.oturep(fasta=current, count=current, list=current, method=abundance)

count.groups(shared=current)

summary.single(shared=current, calc=nseqs-sobs-coverage-shannon-shannoneven-invsimpson, subsample=5000)

dist.shared(shared=current, calc=braycurtis-jest-thetayc, subsample=5000)

sub.sample(shared=current, size=5000)
 
sub.sample(taxonomy=current, count=current, list=current, size=5000, persample=true, label=0.03)
summary.tax(taxonomy=current, count=current)

