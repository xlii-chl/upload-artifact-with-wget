# `@actions/upload-artifact-with-wget`

Upload [actions artifacts](https://forgejo.org/docs/next/user/actions/#artifacts) from your workflows runs.

This is a tryout to make a lighter version of
[actions/upload-artifact](https://github.com/actions/upload-artifact) made for
simple workflows where most of the work could run on a alpine shell and only
the artifact uploading required a full blown NodeJS container.

## Usage

This actions won't copy all the features of the original NodeJS version but
please report differences on the main ones.

### Requirements

This action needs the following executables:

* zip (unless you zip the artifact yourself)
* wget (the full version : unfortunately, as of 2024-08-26, the busybox variant isn't capable of using the PUT method)


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

## Miscellaneous

If you seek a similar alternative for checkout, look at https://github.com/marketplace/actions/checkout-action or use the code below :
```yaml
steps:
  - name: Simple checkout
    run: |
      git init
      GITHUB_AUTHENTICATED_URL="$( echo "$GITHUB_SERVER_URL" | sed "s#^\(https\?://\)#\1$GITHUB_TOKEN\@#" )"
      git remote add origin "$GITHUB_AUTHENTICATED_URL"/"$GITHUB_REPOSITORY"
      # Little and optional speed optimization
      git config --local gc.auto 0
      git fetch --no-tags --prune --no-recurse-submodules --depth=1 origin "$GITHUB_SHA"
      git reset --hard "$GITHUB_SHA"
```
