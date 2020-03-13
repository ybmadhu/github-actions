This repo contains a collection of GitHub Actions:

# skaffold-build
Run `skaffold build` in your current repository.  Options:

* **login_ecr** set to `true` if you'd like to login to ECR (Default: true)
* **create_ecr** set to `true` if you'd like to create ECR repositories from skaffold.yaml (Default: true)
* **push** set to `true` if you'd like to push images that were created (Default: true)

```
# Sample workflow

jobs:
  build:
    runs-on: ubuntu-18.04
    steps:
    - uses: actions/checkout@v2
    - name: Configure AWS Credentials
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: ${{ secrets.AWS_REGION }}
        role-duration-seconds: 1200
    - name: Skaffold Build
      uses: ./.github/actions/skaffold
```

# Gitty-Up
Run `gitty-up` in your current repository.  Options:

* **url** the Git URL to push to
* **username** the Git Username to use 
* **password** the Git Password to use
* **file** the file to update
* **values** the values to set in `file`

```
# Sample workflow

jobs:
  build:
    runs-on: ubuntu-18.04
    steps:
    - uses: actions/checkout@v2
    - name: GitOps
      uses: liatrio/github-actions/gitops@master
      with:
        url: https://github.com/liatrio/lead-environments.git
        username: ${{ github.actor }}
        password: ${{ secrets.GITTY_UP_TOKEN }}
        file: aws/liatrio-sandbox/terragrunt.hcl
        values: inputs.sdm_version=1.3.16
```

# Git Extras
Get extra information about current git repo.  Creates following outputs:

* **version** determined by `git describe`

```
# Sample workflow

jobs:
  build:
    runs-on: ubuntu-18.04
    steps:
    - uses: actions/checkout@v2
    - id: git
      uses: liatrio/github-actions/git-extra@master
    - uses: liatrio/github-actions/gitops@master
      with:
        values: inputs.sdm_version=${{ steps.git.outputs.version}}
```
