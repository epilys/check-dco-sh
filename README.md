<!-- SPDX-License-Identifier: AGPL-3.0-or-later -->

# Shell script to check Developer Certificate of Origin (DCO) compliance

A portable, POSIX-compliant shell script to check that each commit in a range has a correct `Signed-off-by: ` trailer tag.

For more information on the DCO see <https://wiki.linuxfoundation.org/dco>.

## Use

Expects two inputs as environment variables:

1. `BASE_REF`: the upstream commit on top of which the new commits will be merged, it will not be included in the checks.
2. `HEAD_REF`: the latest commit that will be checked. Must have `BASE_REF` as an ancestor.

## Example CI use

Example usage for a Github Action-like CI:

```yaml
name: Verify DCO

on:
  pull_request:

jobs:
  test:
    name: Verify DCO signoff on commit messages
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
      - id: check-dco
        shell: sh
        name: Check that commit messages end with a Signed-off-by git trailer
        run: |
          env BASE_REF="origin/${{env.GITHUB_BASE_REF}}" HEAD_REF="origin/${{env.GITHUB_HEAD_REF}}" sh ./.github/check_dco.sh
```
