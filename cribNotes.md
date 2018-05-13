# download files from basespace

cd _path to downloaded files_

mkdir fastq

for i in */Data/Intensities/BaseCalls/*.gz; do mv $i "fastq""/"${i%%-*}"."`basename $i`; done

mothur
