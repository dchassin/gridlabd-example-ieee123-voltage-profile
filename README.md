[![Simulation](https://github.com/dchassin/gridlabd-example-ieee123-voltage-profile/actions/workflows/main.yml/badge.svg)](https://github.com/dchassin/gridlabd-example-ieee123-voltage-profile/actions/workflows/main.yml)

# IEEE-123 Voltage Profile Example

This example illustrates the following.

1. How to setup a GitHub project using GitHub Actions to run a GridLAB-D simulation.

2. How to use caching to avoid repeating steps that are network or compute intensive.

3. How to setup a simulation to run.

4. How to save artifacts from a simulation so they can be downloaded later.

This example shows how to use GridLAB-D to download and solve the IEEE-123 model and output a plot of the voltage profile that can be downloaded later for viewing.

To view the output, select [**Actions**](https://github.com/dchassin/gridlabd-example-ieee123-voltage-profile/actions) to show the most recent simulation runs. Select the most recent run completed and download the artifact named `profile.png`.

## How it works

### Workflow

The gridlabd simulation is started by the [workflow file](.github/workflows/main.yml).  There are four important parts to this file

1. The [`on`](.github/workflows/main.yml#L3) step determines when the simulation is run. It currently specifies that the simulation will run when a change is made to a file on the `main` or `develop` branches, or when a file is changed on a branch that has an open pull request to the `main` or `develop` branches.

2. The [`cache`](.github/workflows/main.yml#L17) step checks to see whether the specified file(s) have been downloaded during a previous run and reuses them if possible.  In this case, the [`123.glm`](https://github.com/arras-energy/gridlabd-models/blob/master/gridlabd-4/IEEE/123.glm) file is cached to avoid downloading more often than necessary.

3. The [`run`](.github/workflows/main.yml#L28) step run the model files itself, the details of which are discussed below.

4. The [`save`](.github/workflows/main.yml#L31) step makes the specified files available for download as artifacts from the [**Actions**](https://github.com/dchassin/gridlabd-example-ieee123-voltage-profile/actions) tab after the simulation is complete.

### Model

The simulation model is defined in the file [`mail.glm`](main.glm).  There are three parts to this file.

1. The [`#ifmissing`](main.glm#L1) macro checks for the existence of the [`123.glm`](https://github.com/arras-energy/gridlabd-models/blob/master/gridlabd-4/IEEE/123.glm) model file. If the file is not found, then the next command [`#model`](main.glm#L2) is used to get a copy of the model from the [GridLAB-D models repository](https://github.com/arras-energy/gridlabd-models).

2. The [`#include`](main.glm#L4) macro loads the [`123.glm`](https://github.com/arras-energy/gridlabd-models/blob/master/gridlabd-4/IEEE/123.glm) model that was downloaded (or cached). This is the only model component loaded in this example.

3. The [`#output`](main.glm#L5) macro is used to generate the voltage profile PNG image.  The file generated is used to provide the artifact saved by the [`save`](.github/workflows/main.yml#L31) step in the [workflow file](.github/workflows/main.yml).

