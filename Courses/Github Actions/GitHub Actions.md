## Setup Github flow
- [JSON Schema Store](http://schemastore.org/json/)

### IntelliJ

![[json_schema_githuba_workflow.png]]

## File structure

- .github
	- workflows
		- ci.yml

## Commands and events

- on: [push]
- jobs
	- build `arbitrary name`
		- runs-on
		- steps
			- uses: actions/checkout@v1

## Actions
- actions/checkout@v2
- actions/cache@v1

## Tips and tricks

### Monorepositories
In a mono- repository, we need to run test only in the project that has changes. 

```
git diff --name-status origin/master | grep "src" | awk -F '/' '{print $2}' | uniq
```

### Trigger workflows
- https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows

### Secrets
- https://github.com/awslabs/git-secrets

## Repository

- https://github.com/CodelyTV/ci_with_github_actions-course/tree/master/lessons