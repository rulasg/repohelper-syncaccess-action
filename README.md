# RepoHelper SyncAccess Action

This action will take a list of users from a file and will update the repo access for those users with the specific role.

> [!Note]
>  Use [Making authenticated API requests with a GitHub App in a GitHub Actions workflow](https://docs.github.com/en/apps/creating-github-apps/authenticating-with-a-github-app/making-authenticated-api-requests-with-a-github-app-in-a-github-actions-workflow) to generate the needed token to update the repo.

```yaml

name: Repo Access Workflow

# Controls when the workflow will run
on:
  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

permissions:
  # To run test we only need to read the repository
  contents: read

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "test"
  SyncAccess:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Checkout the repository to the GitHub Actions runner
      - name: Checkout code
        uses: actions/checkout@v3

      # Run test with TestingHelper using local branch version
      # - uses: rulasg/actions-composite-template@main
      - name: Sync Repo Access
        id: sync-repo-access
        uses: rulasg/repohelper-syncaccess-action@v1.2
        with:
            role: 'triage'
            what_if_mode: 'true'
            filename: 'CONTRIBUTORS'
            gh_token: ${{ secrets.GITHUB_TOKEN }}
      
      - name: Display action output
        shell: pwsh
        env:
          RESULT_JSON: ${{ steps.sync-repo-access.outputs.resultJson}}
        run: | 
          $result = $env:RESULT_JSON | convertfrom-json

          $result | Write-Host

```
