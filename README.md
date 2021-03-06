# dockstore-tool-bamstats

A repo for the `Dockerfile` to create a Docker image for the BAMStats command. Also contains the
`Dockstore.yml` which is used by the [Dockstore](http://www.dockstore.org) to register
this container and describe how to call BAMStats for the community.

## Building Manually

Normally you would let [Quay.io](http://quay.io) build this.  But, if you need to build
manually you would execute:

    docker build -t briandoconnor/dockstore-tool-bamstats:1.25-3 .

## Running Manually

```
$ wget ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/phase3/data/NA12878/alignment/NA12878.chrom20.ILLUMINA.bwa.CEU.low_coverage.20121211.bam
$ docker run -it -v `pwd`/NA12878.chrom20.ILLUMINA.bwa.CEU.low_coverage.20121211.bam:/NA12878.chrom20.ILLUMINA.bwa.CEU.low_coverage.20121211.bam briandoconnor/dockstore-tool-bamstats:1.25-3

# within the docker container
$ /usr/local/bin/bamstats 4 /NA12878.chrom20.ILLUMINA.bwa.CEU.low_coverage.20121211.bam
```
You'll then see a file, `bamstats_report.zip`, in the current directory, that's the report file. You can use `-v` to mount the result out of the container.

## Running Through the Dockstore CLI

This tool can be found at the [Dockstore](https://dockstore.org), login with your GitHub account and follow the 
directions to setup the CLI.  It lets you run a Docker container with a CWL descriptor locally, using Docker and the CWL command
line utility.  This is great for testing.

### Make a Parameters JSON

This is the parameterization of the BAM stat tool, a copy is present in this repo called `sample_configs.json`:

```
{
  "bam_input": {
        "class": "File",
        "path": "ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/phase3/data/NA12878/alignment/NA12878.chrom20.ILLUMINA.bwa.CEU.low_coverage.20121211.bam"
    },
    "bamstats_report": {
        "class": "File",
        "path": "/tmp/bamstats_report.zip"
    }
}
```

### Run with the CLI

Run it using the `dockstore` CLI:

```
Usage:
# fetch CWL
$> dockstore cwl --entry quay.io/briandoconnor/dockstore-tool-bamstats:1.25-3 > Dockstore.cwl
# make a runtime JSON template and edit it (or use the content of sample_configs.json above)
$> dockstore convert cwl2json --cwl Dockstore.cwl > Dockstore.json
# run it locally with the Dockstore CLI
$> dockstore launch --entry quay.io/briandoconnor/dockstore-tool-bamstats:1.25-3 \
    --json Dockstore.json
```
