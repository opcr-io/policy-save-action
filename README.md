# Policy Save Action

## policy-save-action

Save policy image to file.


## Inputs

### `tag`

**Required** The tag of local image to be saved. 

Default: empty

### `file`

**Required** The output file path. 

Default: empty

### `verbosity`

**Required** The logging verbosity level [ `info` | `error` | `debug` | `trace` ] used.

Default: `error`


## Outputs

None defined


## Example

```
name: policy-build-release

on:
  workflow_dispatch:
  push:
    tags:
    - '*'

jobs:
  release_policy:
    runs-on: ubuntu-latest
    name: build
    steps:
    
    - uses: actions/checkout@v2

    - name: Policy Login
      id: policy-login
      uses: opcr-io/policy-login-action@v1
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      with:
        username: policy-bot
        password: "$GITHUB_TOKEN"

    - name: Policy Build
      id: policy-build
      uses: opcr-io/policy-build-action@v1
      with:
        src: peoplefinder/src
        tag: datadude/peoplefinder:$(sver -n patch) 
        revision: "$GITHUB_SHA"

    - name: Policy Push
      id: policy-push
      uses: opcr-io/policy-push-action@v1
      with:
        tag: datadude/peoplefinder:$(sver -n patch)

    - name: Policy Logout
      id: policy-logout
      uses: opcr-io/policy-logout-action@v1

```
