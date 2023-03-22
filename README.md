# GitHub Action: Rich Workflow Run

This action downloads jobs, logs, and artifacts information from a specified workflow run in a GitHub repository.

It is especially useful when used in combination with the [workflow_run](https://docs.github.com/en/actions/reference/events-that-trigger-workflows#workflow_run) event, which is triggered when a workflow run is requested or completed.

## Inputs

### `jobs-url`

**Optional** The URL of the jobs API endpoint for the workflow run. If the workflow is triggered on the workflow run event, you can use the `github.event.workflow_run.jobs_url` context variable.
### `logs-url`

**Optional** The URL of the logs API endpoint for the workflow run. If the workflow is triggered on the workflow run event, you can use the `github.event.workflow_run.logs_url` context variable.

### `artifacts-url`

**Optional** The URL of the artifacts API endpoint for the workflow run. If the workflow is triggered on the workflow run event, you can use the `github.event.workflow_run.artifacts_url` context variable.

### `artifact-names`

**Optional** A comma-separated list of artifact names to download. If not provided, none of the artifacts will be downloaded. It doesn't have any effect if the `artifacts-url` input is not provided.

## Outputs

### `jobs`

A JSON object containing the jobs information for the workflow run.

### `logs`

A JSON object containing the logs information for the workflow run.

### `artifacts`

A JSON object containing the artifacts information for the workflow run.

## Example Usage

```yaml
name: Enrich Workflow Run Event
on:
  workflow_run:
    types: [completed]
jobs:
  enrich-workflow-run:
    runs-on: ubuntu-latest
    steps:
      - name: Enrich Workflow Run Event
        uses: pl-strflt/rich-workflow-run@v1
        id: workflow-run
        with:
          jobs-url: ${{ github.event.workflow_run.jobs_url }}
          logs-url: ${{ github.event.workflow_run.logs_url }}
          artifacts-url: ${{ github.event.workflow_run.artifacts_url }}
          artifact-names: artifacts1,artifacts2
      - name: Print information about the workflow run jobs
        run: echo "${{ steps.download-info.outputs.jobs }}"
      - name: Print information about the workflow run logs
        run: echo "${{ steps.download-info.outputs.logs }}"
      - name: Print information about the workflow run artifacts
        run: echo "${{ steps.download-info.outputs.artifacts }}"
```
