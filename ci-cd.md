# CI/CD

<https://blog.bytebytego.com/i/135732871/cicd-pipeline-explained-in-simple-terms>
<https://www.youtube.com/watch?v=R8_veQiYBjI>
<https://www.youtube.com/watch?v=eB0nUzAI7M8>
<https://levelup.gitconnected.com/basics-of-ci-cd-a98340c60b04>

## What is CI/CD

It's the process of building, testing, deploying new code to server automatically. This process is called workflow or CI/CD pipeline

A common workflow is when you commit your code, the workflow starts. It tests your code, builds it into an artifact (build artifact: which are files created by a build process), pushes the artifacts in some storage and then deploys the application on an server

In practical, CI deploys code to Testing environments. Continuous delivery deploys code to Staging and Continuous Deployment deploys code to Production. However, Continuous Deployment is hard to implement because production enviroment is user's environment, so we still need to check and deploy manually.

There are many CI/CD tools on the market but if you are using github for hosting your code you can use Github Actions to implement these kind of workflow.

## Why CI/CD

It resolves the problem of manual deploying. Deploying manually is error-prone and time consuming: we have to build by hand, wait for the build then copy code to the server, restart the server, some code might be missing when we copy code to server by hand.

## Typical CI/CD pipeline

- Developer commits code changes to source control
- CI server detects changes and triggers build
- Code is compiled, tested (unit, integration tests)
- Test results reported to developer
- On success, artifacts are deployed to staging environments
- Further testing may be done on staging before release
- CD system deploys approved changes to production

## CI

Is the automatic process before deployment which is triggered when developer makes an event. An event here could be a new issue, new pull request, a new merge into branch or a new commit.

When CI starts, it will perform a combination of actions. Actions are seperated tasks like testings, building the code.

## CD

Is the process of deploying code to server (using ssh, rsync...), run UI test, API test...

## Github actions

<https://zellwk.com/blog/understanding-github-actions/>

### Concept

- `on`: lists of event
- `jobs`: job is set of actions. A github actions might have multiple jobs
- `runs-on`: Github actions support 3 environment to run the pipeline: `ubuntu`, `window` and `macOS`
- `steps`: define steps
  + `name`: name of the step which is up to you
  + `uses`: in each step of job, you can use pre-defined actions which are hosted on github, ex: action/checkout@v2, in which `checkout` is the name of that actions. Search [https://github.com/actions](here) for more
  + `with`: action has attributes, `with` defines attributes you want to use with the action.
  + `run`: run an command 
  + `need`: by default, jobs run in parallel. If the second job depends on the first one, you need to use `need`.

Here is an example of ssh and rsync data to server

```yaml
on:
  push:
    branches:
      - master

env:
  VITE_BASE_URL: ${{ VITE_BASE_URL }}

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: install package
        run: |
          cd frontend && npm pkg delete scripts.prepare && rm -rf node_modules && yarn install --frozen-lockfile && yarn build
          cd ../backend && rm -rf node_modules && yarn install --frozen-lockfile && yarn build

      - name: Install SSH key
        uses: shimataro/ssh-key-action@v2
        with:
          key: ${{ secrets.SSH_KEY }}
          known_hosts: ${{ secrets.KNOWN_HOST }}
      - name: Deploy with rsync
        run: |
          rsync -avz -e ssh --exclude={'.git','.github','.gitignore'} frontend/dist/ ${{ secrets.SSH_USERNAME }}@${{ secrets.SERVER_IP }}:${{ secrets.FRONTEND_DEPLOY_DIR }} --delete-after
          rsync -avz -e ssh --exclude={'.git','.github','.gitignore'} backend/dist/ ${{ secrets.SSH_USERNAME }}@${{ secrets.SERVER_IP }}:${{ secrets.BACKEND_DEPLOY_DIR }}

```
