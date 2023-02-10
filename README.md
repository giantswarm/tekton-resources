# tekton-resources

Contains Tekton installation resources along with Tekton Tasks and Pipelines.

> Note: This repo and documentation is Giant Swarm specific in many ways

All resources within this repository are made available within the CI/CD cluster operater by Team Tinkerers.

## Usage

### Repo setup

// TODO: This is coming soon™️

### [Pipelines](./tekton-resources/pipelines/)

All pipelines available within the cluster can be triggered by using slash commands within comments on pull requests.

The format of these slash commands is as follows:

```
/run [pipeline-name] (optional arguments as KEY=value)
```

Examples:

```
/run help

/run example-pipeline NAMESPACE=foobar

/run example-pipeline NAMESPACE=foobar CUSTOM_ARG=extra
```

All triggers must be on their own line in a comment and can also be used within the main body description of a pull request.

Arguments can be optionally provided to a pipeline in the format of `KEY=value` where `KEY` is uppercase and the value is a single string with no spaces. Multiple arguments can be provided as long as they are on the same line as the slash command, separated with spaces. These arguments are then passed to the triggered Pipeline as parameters.

### [Tasks](./tekton-resources/tasks/)

All tasks are deployed into the `tekton-pipelines` namespace and are available to use in other Pipelines by making use of the cluster resolver.

Example:

```yaml
spec:
  tasks:
    - name: add-comment
      taskRef:
        resolver: cluster
        params:
        - name: kind
          value: task
        - name: name
          value: gh-api-add-comment
        - name: namespace
          value: tekton-pipelines
      params:
        - name: COMMENT_MESSAGE
          value: "Example"
        - name: PR_ISSUE_NUMBER
          value: $(params.NUMBER)
        - name: REPO
          value: "$(params.REPO_ORG)/$(params.REPO_NAME)"

```
