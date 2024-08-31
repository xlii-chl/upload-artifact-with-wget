# `@actions/upload-artifact-with-wget`

Upload [actions artifacts](https://forgejo.org/docs/next/user/actions/#artifacts) from your workflows runs.

This is a tryout to make a lighter version of
[actions/upload-artifact](https://github.com/actions/upload-artifact) made for
simple workflows where most of the work could run on a alpine shell and only
the artifact uploading required a full blown NodeJS container.

## Usage

This action won't copy all the features of the original NodeJS version but
please report differences on the main ones.

### Requirements

This action needs the following executables:

* `zip` (unless you zip the artifact yourself)
* `wget` (the full version : unfortunately, as of 2024-08-26, the busybox variant isn't capable of using the PUT method)


### Inputs

```yaml
# If you can, give the full URL :
# - uses: https://entrepot.xlii.si/actions/upload-artifact-with-wget@v4
- uses: actions/upload-artifact-with-wget@v4
  with:
    # Name of the artifact to upload.
    # Optional. Default is 'artifact'
    name:

    # A file, directory or wildcard pattern that describes what to upload
    # Required.
    path:

    # If the artifact is already a zipfile, set to false.
    # Optional. Default is true.
    compression:

    # Set the compression level of the zipfile.
    # Optional. Default is '6'.
    compression-level:
```

### Outputs

| Name | Description | Example |
| - | - | - |
| `artifact-id` | GitHub ID of an Artifact, can be used by the REST API | `1234` |
| `artifact-url` | URL to download an Artifact. | `https://github.com/example-org/example-repo/actions/runs/1/artifacts/1234` or `https://codeberg.org/forgejo/forgejo/actions/runs/1/artifacts/my-artifact` |

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
      # On Github, the token isn't readily available.
      test -z "$GITHUB_TOKEN" && GITHUB_TOKEN="${{ github.token }}"
      MY_AUTHENTICATED_URL="$( echo "$GITHUB_SERVER_URL" | sed "s#^\(https\?://\)#\1$GITHUB_TOKEN\@#" )"
      git remote add origin "$MY_AUTHENTICATED_URL"/"$GITHUB_REPOSITORY"
      # Little and optional speed optimization
      git config --local gc.auto 0
      git fetch --no-tags --prune --no-recurse-submodules --depth=1 origin "$GITHUB_SHA"
      git reset --hard "$GITHUB_SHA"
```
