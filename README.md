# IEEE-123 Voltage Profile Example

This example illustrates how to use GridLAB-D to download and solve the IEEE-123 model and output a plot of the voltage profile that can be downloaded for viewing.

To view the output, select [Actions](https://github.com/dchassin/gridlabd-example-ieee123-voltage-profile/actions) to show the most recent simulation runs. Select the most recent run completed and download the artifact named `profile.png`.

## How it works

The gridlabd simulation is started by the file [.github/workflows/main.yml](.github/workflows/main.yml).  There are four important parts to this file

### Workflow

1. **[on](.github/workflows/main.yml#L3)**: The `on` determines when the simulation is run. It current specifies that the simulation will run when a change is made to a file on the `main` or `develop` branches, or when a file is change on a branch that has an open pull request to the `main` or `develop` branches.

2. **[Cache](.github/workflows/main.yml#L17)**: The `cache` step checks to see whether the specified file(s) have been downloaded during a previous run and reuses them if possible.  In this case, the `123.glm` file is cached to avoid downloading more often than necessary.

3. **[Run](.github/workflows/main.yml#L28)**: The `run` step run the model files itself, the details of which are discussed below.

4. **[Save](.github/workflows/main.yml#L31)**: The `save` step makes the specified files available for download as artifacts after the simulation is complete.

### Model

The simulation model is given in the file [mail.glm](main.glm).  There are three part to this file.

1. The [`#ifmissing`](main.glm#L1) macro is used to check for the existence of the `123.glm` model file. If the file is not found, then the next command [`#model`](main.glm#L2) is used to get a copy of the model from the [GridLAB-D models repository](https://github.com/arras-energy/gridlabd-models).

2. The [`#include`](main.glm#L4) macro is used to load the [123.glm(https://github.com/arras-energy/gridlabd-models/blob/master/gridlabd-4/IEEE/123.glm) model, this is the only model component loaded.

3. The [`#output`](main.glm#L5) macro is used to generate the voltage profile PNG image.  The file generated is used to provide the artifact saved by the **Save** step in the workflow.

