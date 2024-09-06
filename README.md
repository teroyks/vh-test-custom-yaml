# Test Custom `valohai.yaml`

Test using custom `valohai.yaml` file(s) on Valohai

- no `valohai.yaml` in the repo root
- multiple different `valohai.yaml` files in subdirectories
- directory paths with different length

## Creating a Project

1. Go to the Valohai app and start creating a new project.
2. Give the project a name (e.g. `custom-yaml-test`)
3. Probably best to select a suitable organization as the project owner.
4. Click on _Create Project_.
5. Go to _Settings > Repository_ and add this repository as _URL_.
6. Type `local_conf/valohai.yaml` as _YAML Path_ and _Save changes_.
7. Click _Fetch repository_. Valohai should fetch 1 new commit to the project.
8. Check that everything is ok by goingo to _Settings > Repository > Commits_, selecting the commit, and making sure you see the configuration defined in `local_conf/valohai.yaml`.

## Running an Execution

You should be able to run the `hello-world` execution in the Valohai app UI.

To run the execution from the command line, you need to link the local repository directory to the project:

```shell
vh project link
```

Now, you can run the execution from the command line:

```shell
vh execution run say-hello
```

## Running a Pipeline

Let’s add a pipeline to the mix.

(Very trivial pipeline is included in the YAML).

- ✔︎ Running the pipeline as ad-hoc from CLI
- 🚫 Cannot run the pipeline with `vh pipeline run hello`
  - Complains about `valohai.yaml` – trying to look for it in the root directory.
- ✔︎ Explicitly setting the project ID works around the issue:
  - `vh --project=<PROJECT_UUID> pipeline run hello`

## Adding Alternate Config

Let’s add a second configuration file to test how these behave.

Add a new file `local_conf/subconf/valohai.yaml` with still trivial, but a bit different, content (a step named `another-hello` and a pipeline `hello-too`).

To be able to use the new configuration:

1. Push the new configuration to the repository.
2. Change the YAML path in the project settings.
3. Fetch the repository into Valohai (needs to be done _after_ updating the YAML path).

The latest commit is now using the new configuration.
You can verify this by running the command:

```shell
vh --project=<PROJECT_UUID> pipeline run hello-too
```

_Note_: CLI commands are now run by default agains the configuration in the subfolder.
But you can still run commands from the old YAML by specifying a commit ID that used that configuration.

```shell
vh --project=<PROJECT_UUID> pipeline run hello --commit=<COMMIT_ID>
```
