# `@actions/upload-artifact-with-wget`

Upload [actions artifacts](https://forgejo.org/docs/next/user/actions/#artifacts) from your workflows runs.

This is a tryout to make a lighter version of
[actions/upload-artifact](https://github.com/actions/upload-artifact) made for
simple workflows where most of the work could run on a alpine shell and only
the artifact uploading required a full blown NodeJS container.

## Usage

This actions won't copy all the features of the original NodeJS version but
please report differences on the main ones.

### Inputs

```yaml
- uses: actions/upload-artifact@v4
  with:
    # Name of the artifact to upload.
    # Optional. Default is 'artifact'
    name:

    # A file, directory or wildcard pattern that describes what to upload
    # Required.
    path:
```

### Outputs

| Name | Description | Example |
| - | - | - |
| `artifact-id` | GitHub ID of an Artifact, can be used by the REST API | `1234` |

## Examples

### Upload an Individual File

```yaml
steps:
- run: mkdir -p path/to/artifact
- run: echo hello > path/to/artifact/world.txt
- uses: actions/upload-artifact@v4
  with:
    name: my-artifact
    path: path/to/artifact/world.txt
```
