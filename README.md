# decoil-viz

Visualize ecDNA reconstruction threads and summarize all the results generated by `decoil`.

- [Getting started using docker or singularity](#gettingstarted)
- [Decoil-viz configuration](#decoil-usage)
- [Citation](#citation)
- [License](#license)

## Getting started using docker or singularity <a name="gettingstarted"></a> 

<img src="./decoil-viz.gif" width="600">
<br/>
To run `decoil-viz` you need to have installed [docker](https://docs.docker.com/engine/install/), and have docker engine running, or, [singularity](https://anaconda.org/conda-forge/singularity).

### 1. Install

This image contains all the dependencies needed to run the software.
No additional installation needed.

```commandline
git clone https://github.com/madagiurgiu25/decoil-viz.git
cd decoil-viz
```

To download the docker image:

```
# for docker
./install.sh --docker
```

To download the singularity image:

```
# for singularity
./install.sh --singularity
```

### 2. Run decoil-viz

With docker:

```commandline
decoil-viz --docker --coverage <coverage_file> --summary <summary_file> --reference <reference_file> --gtf <gtf_file> --bed <bed_file> --links <links_file> --outdir <output_directory> --name <output_name>
```

With singularity:

```commandline
decoil-viz --singularity --coverage <coverage_file> --summary <summary_file> --reference <reference_file> --gtf <gtf_file> --bed <bed_file> --links <links_file> --outdir <output_directory> --name <output_name>
```

### 3. Run test example on your machine

0. Create output directory

```commandline
mkdir -p $PWD/example
chmod 777 $PWD/example
```

1. Download test files from [zenodo:10679429](https://zenodo.org/records/10679429)

```commandline
# configure variables
REFERENCE=$PWD/example/GRCh38.primary_assembly.genome.fa
GTF=$PWD/example/gencode.v42.primary_assembly.basic.annotation.gtf
COVERAGE=$PWD/example/coverage.bw
BED=$PWD/example/reconstruct.ecDNA.filtered.bed
LINKS=$PWD/example/reconstruct.links.ecDNA.filtered.txt
SUMMARY=$PWD/example/summary.txt
OUTDIR=$PWD/example
NAME=test

# download example data
wget -O - https://ftp.ebi.ac.uk/pub/databases/gencode/Gencode_human/release_44/GRCh38.primary_assembly.genome.fa.gz | gunzip -c > $REFERENCE
wget -O - https://ftp.ebi.ac.uk/pub/databases/gencode/Gencode_human/release_44/gencode.v44.primary_assembly.basic.annotation.gtf.gz | gunzip -c > $GTF
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

2. Run test example:

With docker

```commandline
bash test_docker.sh
```

With singularity

```commandline
bash test_singularity.sh
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
Usage: decoil-viz [--docker|--singularity|--version|--help] --coverage <coverage_file> --summary <summary_file> --reference <reference_file> --gtf <gtf_file> --bed <bed_file> --links <links_file> --outdir <output_directory> --name <output_name>

Options:
	-h,--help	      		Display this help message
	--version	    		Display version
	--docker        		Flag: run decoil-viz using docker (set by default if --singularity not active)
	--singularity			Flag: run decoil-viz using singularity
	-o,--outdir <output_directory> Output directory (absolute path)
	--name				Sample name
	--coverage <coverage_file>	Coverage file in .bw format (absolute path)
	--bed <bed_file>		Reconstruction regions file in .bed like format (absolute path)
	--links <links_file>		Reconstruction links file in .txt format (absolute path)
	--summary <summary_file>	Reconstructions summary (absolute path)
	-r,--reference <reference_file>	Reference genome in .fasta format (absolute path)
	-g,--gtf <gtf_file>		Genes annotation file in .gtf format (absolute path)
```

## Documentaiton for advanced users

To access all features of `decoil-viz`, e.g. specify the plot-window, check [Documentation](docs/docs_main.md)


## Citation <a name="citation"></a>

If you use Decoil for your work please cite our pre-print:

Madalina Giurgiu, Nadine Wittstruck, Elias Rodriguez-Fos, Rocio Chamorro Gonzalez, Lotte Bruckner, Annabell Krienelke-Szymansky, Konstantin Helmsauer, Anne Hartebrodt, Richard P. Koche, Kerstin Haase, Knut Reinert, Anton G. Henssen.
_Decoil: Reconstructing extrachromosomal DNA structural heterogeneity from long-read sequencing data_. bioRxiv, 2023, DOI: [https://doi.org/10.1101/2023.11.15.567169](https://www.biorxiv.org/content/10.1101/2023.11.15.567169v1)

## License <a name="license"></a>

Decoil-viz is distributed under BSD-3-Clause license. See [LICENSE](LICENSE) for details.

### Disclaimer
Decoil-viz and the content of this research-repository (i) is not suitable for a medical device; and (ii) is not intended for clinical use of any kind, including but not limited to diagnosis or prognosis.
