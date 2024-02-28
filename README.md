# decoil-viz

Visualize ecDNA reconstruction threads and summarize all the results generated by `decoil`.

- [Getting started using docker](#gettingstarted)
- [Decoil-viz configuration](#decoil-usage)
- [Citation](#citation)
- [License](#license)

## Getting started using docker or singularity  <a name="gettingstarted"></a> 

<img src="./decoil-viz.gif" width="600">
<br/>
To run `decoil-viz` you need to have installed [docker](https://docs.docker.com/engine/install/), and have docker engine running, or, [singularity](https://anaconda.org/conda-forge/singularity).

### Download the docker image

This image contains all the dependencies needed to run the software.
No additional installation needed.

```commandline
# docker
docker pull madagiurgiu25/decoil-viz:1.0.1

# convert docker to singularity
cd decoil-viz
singularity pull decoil-viz.sif  docker://madagiurgiu25/decoil-viz:1.0.3
```

### Test decoil-viz on your machine

0. Create output directory

```commandline
mkdir -p $PWD/example
chmod 777 $PWD/example
```

1. Download test files from [zenodo:10679429](https://zenodo.org/records/10679429)

```commandline
# configure variables
ROOT=$PWD
REF=$ROOT/example/GRCh38.primary_assembly.genome.fa
ANNO=$ROOT/example/gencode.v42.primary_assembly.basic.annotation.gtf
COVERAGE=$ROOT/example/coverage.bw
BED=$ROOT/example/reconstruct.ecDNA.filtered.bed
LINKS=$ROOT/example/reconstruct.links.ecDNA.filtered.txt
SUMMARY=$ROOT/example/summary.txt
OUTDIR=$ROOT/example
NAME=test

# download example data
wget -O - https://ftp.ebi.ac.uk/pub/databases/gencode/Gencode_human/release_44/GRCh38.primary_assembly.genome.fa.gz | gunzip -c > $REF
wget -O - https://ftp.ebi.ac.uk/pub/databases/gencode/Gencode_human/release_44/gencode.v44.primary_assembly.basic.annotation.gtf.gz | gunzip -c > $ANNO
wget -O - https://zenodo.org/records/10679429/files/coverage.bw > $COVERAGE
wget -O - https://zenodo.org/records/10679429/files/reconstruct.ecDNA.filtered.bed > $BED
wget -O - https://zenodo.org/records/10679429/files/reconstruct.links.ecDNA.filtered.txt > $LINKS
wget -O - https://zenodo.org/records/10679429/files/summary.txt > $SUMMARY

# test your files exist
ls -lthr $REF
ls -lthr $ANNO
ls -lthr $COVERAGE
ls -lthr $BED
ls -lthr $LINKS
ls -lthr $SUMMARY
```

2. Run test using Docker:

```commandline
# run visualization using docker
docker run --platform=linux/amd64 \
    -v $REF:$REF \
    -v $ANNO:$ANNO \
    -v $COVERAGE:$COVERAGE \
    -v $BED:$BED \
    -v $LINKS:$LINKS \
    -v $SUMMARY:$SUMMARY \
    -v $OUTDIR:$OUTDIR \
madagiurgiu25/decoil-viz:1.0.3 decoil-viz \
    --coverage $COVERAGE \
    --bed $BED \
    --links $LINKS \
    -r $REF \
    -g $ANNO \
    -o $OUTDIR \
    --summary $SUMMARY \
    --name $NAME
```

3. Run test using Singularity


```commandline
# run visualization using singularity
singularity run \
    --bind $OUTDIR:/mnt \
    --bind $REF:/mnt/ref.fa \
    --bind $ANNO:/mnt/anno.gtf \
    --bind $COVERAGE:/mnt/coverage.bw \
    --bind $BED:/mnt/reconstruct.bed \
    --bind $LINKS:/mnt/reconstruct.links.txt \
    --bind $SUMMARY:/mnt/summary.txt \
decoil-viz.sif decoil-viz \
    --coverage /mnt/coverage.bw \
    --bed /mnt/reconstruct.bed \
    --links /mnt/reconstruct.links.txt \
    -r /mnt/ref.fa \
    -g /mnt/anno.gtf \
    -o /mnt \
    --summary /mnt/summary.txt \
    --name $NAME
```



## Install from source

If you install `decoil-viz` from source, the prequisites are `R>=4.1.1`, `python>=3.7` and `git`.

```
git clone https://github.com/madagiurgiu25/decoil-viz.git
cd decoil-viz
# install R dependencies
Rscript requirements.R

# install decoil-viz
python -m pip install .

# test installation
decoil-viz --help
```

## Decoil-viz configuration <a name="decoil-usage"></a>

Usage:

```commandline
docker run madagiurgiu25/decoil-viz:1.0.0 decoil-viz --help

usage: decoil-viz --outputdir <outputdir> --name <sample> -r <reference-genome> -g <annotation-gtf> --coverage <bw> --summary <summary.txt> --bed <reconstruct.bed> --links <reconstruct.links.txt>

Decoil-viz 1.0.1: visualize ecDNA reconstruction threads + report

optional arguments:
  -h, --help            show this help message and exit
  --version             show program's version number and exit
  -o OUTPUTDIR, --outputdir OUTPUTDIR
  --name NAME           Name of the sample
  --coverage COVERAGE   Coverage file path
  --bed BED             Bed file with reconstructions
  --links LINKS         Links file with reconstructions
  --summary SUMMARY     Summary of reconstructions
  -r REFERENCE_GENOME, --reference-genome REFERENCE_GENOME
                        Reference genome (fasta)
  -g ANNOTATION_GTF, --annotation-gtf ANNOTATION_GTF
                        GTF annotation
  --suffix SUFFIX       Output suffix
  --plot-top PLOT_TOP   Keep only the top x reconstructions (default: 50 -
                        denoting all)
  --plot-filter-score PLOT_FILTER_SCORE
                        Keep reconstructions with estimated_proportions > x
                        (default: 0 - denoting all)
  --plot-window PLOT_WINDOW
                        Keep reconstructions within defined window (path to
                        file in bed format)
  --full FULL           Generate full report
```


## Build environment -  for developers

Build decoil-viz image:

```commandline
docker compose -f docker-compose.yaml build
```

Build the base R 4.1.1 image including all the R dependencies required to run `decoil-viz`:

```commandline
docker compose -f docker-compose-decoil-base_r411.yaml build
```

## Citation <a name="citation"></a>

If you use Decoil for your work please cite our pre-print:

Madalina Giurgiu, Nadine Wittstruck, Elias Rodriguez-Fos, Rocio Chamorro Gonzalez, Lotte Bruckner, Annabell Krienelke-Szymansky, Konstantin Helmsauer, Anne Hartebrodt, Richard P. Koche, Kerstin Haase, Knut Reinert, Anton G. Henssen.
_Decoil: Reconstructing extrachromosomal DNA structural heterogeneity from long-read sequencing data_. bioRxiv, 2023, DOI: [https://doi.org/10.1101/2023.11.15.567169](https://www.biorxiv.org/content/10.1101/2023.11.15.567169v1)

## License <a name="license"></a>

Decoil-viz is distributed under BSD-3-Clause license. See [LICENSE](LICENSE) for details.

### Disclaimer
Decoil-viz and the content of this research-repository (i) is not suitable for a medical device; and (ii) is not intended for clinical use of any kind, including but not limited to diagnosis or prognosis.