
These are instructions to install and run the workflow on the command line.

The workflow uses [Nextflow](https://www.nextflow.io/) to manage
compute and software resources,
therefore Nextflow will need to be
installed before attempting to run the workflow.

The workflow can currently be run using either
[Docker](https://docs.docker.com/get-started/)
or [Singularity](https://docs.sylabs.io/guides/3.0/user-guide/index.html)
to provide isolation of the required software.
Both methods are automated out-of-the-box provided
either Docker or Singularity is installed.
This is controlled by the
[`-profile`](https://www.nextflow.io/docs/latest/config.html#config-profiles)
parameter as exemplified below.

Clone the repository and enter the workflow directory:

```
git clone https://github.com/seekgene/wf-single-cell-seekone.git
cd wf-single-cell-seekone
```

To view all available parameters and an example command, run:

```
nextflow run . --help
```

For further information about running a workflow on
the command line see https://labs.epi2me.io/wfquickstart/
