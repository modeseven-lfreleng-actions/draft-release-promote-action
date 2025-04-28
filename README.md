<!--
# SPDX-License-Identifier: Apache-2.0
# SPDX-FileCopyrightText: 2025 The Linux Foundation
-->

# ⏫ Promote Draft Release

Promotes a draft GitHub release to a full release.

## draft-release-promote-action

## Usage Example

<!-- markdownlint-disable MD046 -->

```yaml
steps:
  - name: "Promote Draft Release"
    uses: lfreleng-actions/draft-release-promote-action@main
    with:
      TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

<!-- markdownlint-enable MD046 -->

## Token Permissions

The token needs the following permissions:

`contents: write`

## Inputs

<!-- markdownlint-disable MD013 -->

| Variable Name | Required | Description                            |
| ------------- | -------- | -------------------------------------- |
| TOKEN         | True     | GitHub token with relevant permissions |
| LATEST        | False    | Mark as the latest release             |
| TAG           | False    | Tag of draft release to promote        |
| NAME          | False    | Name of draft release to promote       |
| SORT_BY       | False    | Sort by this json element parameter    |
| SORT_REVERSE  | False    | Reverse the sort order of results      |

<!-- markdownlint-enable MD013 -->

## Implementation Details

Uses the GitHub CLI command:

`gh release list --json createdAt,isDraft,isLatest,isPrerelease,name,publishedAt,tagName`

Example output:

```json
[
  {
    "createdAt": "2025-04-18T05:19:03Z",
    "isDraft": true,
    "isLatest": false,
    "isPrerelease": false,
    "name": "",
    "publishedAt": "0001-01-01T00:00:00Z",
    "tagName": "v9.9.3"
  },
  {
    "createdAt": "2025-04-17T11:45:14Z",
    "isDraft": false,
    "isLatest": false,
    "isPrerelease": false,
    "name": "",
    "publishedAt": "2025-04-19T05:00:04Z",
    "tagName": "v9.9.0"
  }
]
```

This is then passed through JQ commands to sort and filter the results.

Some examples:

`jq "[.[] | select(.isDraft==true)]"`

`jq "sort_by(.tagName)"`

`jq '. | reverse`
