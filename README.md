# Terraform Modules Library

This repository forms the basis of my own personal implimentation of a Terraform Modules Library.

It's made up of pre-coded modules to get you started quickly.

It also uses tools, such as TFEnv, Terraform Docs, Linters, VSCode's Dev / Remote containers and many more to provide effectively a Terraform / DevOps engineer development lab to develop and test Terraform Modules.

You are most welcome and in fact encouraged to contribute back to this repository.

![Terraform Modules Library](images/009-owl.png?raw=true "Terraform Modules Library")

## Diagram

On a high level this project can be visualised as the following diagram

![Terraform Modules Library](images/terraform-modules-library.drawio.png?raw=true "Terraform Modules Library")

## Demo

A quick video walk through demo of this solution

![Terraform Modules Library](images/demo.gif?raw=true "Terraform Modules Library")

## Information

This repository consists basically of 3 topics.

1. Develppment Environment

- Dev Containers
- Docker
- VSCode Local Dev
- Github Codespaces Remote Dev
- Github Actions
- Github Secrets and Variables
- run.sh

2. Governance and Compliance

- Pre-Commit
- TFSec and Checkov
- Terraform fmt and validate
- Linters
- Terraform docs
- Terraform Cloud

3. Terraform Modules and Patterns Library

- Modules and Patterns
- Versioning with SemVer
- Module Promotion: Monorepo -> Multirepos -> Terraform Cloud Private Registry
- Terraform Cloud Private Module Registry

## Features

This repository uses some best practice tools to help us with our modules. Tools such as TFENV which automatically installs the correct Terraform Version, Terraform Docs and Terraform Lint, the comprehensive feature list is detailed below.

| Product | State | URL |
|---------|-------|-----|
| VSCode Dev Container | ✓ | https://code.visualstudio.com/docs/devcontainers/containers |
| Pre-Commit | ✓ | https://pre-commit.com/ |
| Terratest | ✘ | https://terratest.gruntwork.io/ |
| Terraform | ✓ | https://www.terraform.io/ |
| Terraform Cloud | ✓ | https://app.terraform.io/ |
| Terragrunt | ✓ | https://terragrunt.gruntwork.io/ |
| TFEnv | ✓ | https://github.com/tfutils/tfenv |
| TGEnv | ✓ | https://github.com/cunymatthieu/tgenv |
| TFsec | ✓ | https://github.com/aquasecurity/tfsec |
| Checkov | ✓ | https://www.checkov.io/ |
| AWS | ✓ | https://aws.amazon.com/free/ |
| GCP | ✓ | https://cloud.google.com/free/ |
| Azure | ✓ | https://azure.microsoft.com/en-au/free/ |
| Custom | ✓ | - |
| Terraform Lint | ✓ | https://github.com/terraform-linters/tflint |
| Terraform Format | ✓ | https://developer.hashicorp.com/terraform/cli/commands/fmt |
| Terraform Validate | ✓ | https://developer.hashicorp.com/terraform/cli/commands/validate |
| Terraform Docs | ✓ | https://github.com/terraform-docs/terraform-docs |

- Pre-commit: Runs pre-commit with the given config in `.pre-commit-config.yaml.

- Linters: terraform-lint tflint yamllint

- Terraform lint: terraform fmt -check -recursive -diff

- Terraform format: terraform fmt -check -recursive -diff

- Clean: Clean up .terraform and .terrgrunt-cache folders

- Terraform docs: Create README.md files for each Terraform Module on the fly, generated from your code

- Terraform Plan: Using Terragrunt we run a plan on all modules

## Module and Pattern Publishing

You can think of this repository as your library of modules and patterns. Although you can work on each module and pattern seperately using their own repository, you'd have to duplicate many things, for example the devcontainer, the pre-commit rules and so on.

:bulb: Your Terraform module names are important and should be in the format of `terraform-<PROVIDER>-<NAME>` Examples: terraform-google-vault or terraform-aws-ec2-instance.

See: https://developer.hashicorp.com/terraform/registry/modules/publish

For that reason, we've chosen a Monorepo.

The publishing mechanism works as follows:

Terraform Modules Library -> Modules Own Gihub Repository - Terraform Cloud Private Registry

1. You code and commit and test your modules in this repo.

![Terraform Modules Library](images/terraform-modules-library-repository-codespace-editor.png?raw=true "Terraform Modules Library")

2. Once your module is ready, you create a `.module-version` file in the module's directory.

![Terraform Modules Library](images/terraform-modules-library-aws-modules-debug-module-version-file.png?raw=true "Terraform Modules Library")

The Github Actions pipeline will run and detect a `.module-version` file and publish the module to it's own repository.

:bulb: Note for the module to be published you need a Personal Access Token with repository rights and this should be added in your repository Github Actions Secrets as:

```
- name: Push Modules and Patterns Upstream
  run: make push-modules-and-patterns-upstream
  env:
    ACCESS_TOKEN_GITHUB: ${{ secrets.ACCESS_TOKEN_GITHUB }}
```

See screenshot below:

![Terraform Modules Library](images/terraform-modules-library-repository-secrets.png?raw=true "Terraform Modules Library")

The Github action will publish in the following name convention for example:

```
aws/module/debug -> terraform-aws-dbug
azure/module/resource-group -> terraform-azure-resource-group
```
and so on...

3. Create the modules own repository, for example, if your module is in `aws/module/terraform-aws-debug` you will create the following github repository `terraform-aws-dbug` see above.

![Terraform Modules Library](images/aws-modules-debug-repository.png?raw=true "Terraform Modules Library")

4. Add the module in Terraform Cloud, and everything after that is Automated.

![Terraform Modules Library](images/terraform-cloud-registry-publish-new-module.png?raw=true "Terraform Modules Library")

![Terraform Modules Library](images/terraform-cloud-registry-publish-new-module-connect-to-vcs.png?raw=true "Terraform Modules Library")

![Terraform Modules Library](images/terraform-cloud-registry-publish-new-module-select-repository.png?raw=true "Terraform Modules Library")

![Terraform Modules Library](images/terraform-cloud-registry-publish-new-module-add-module.png?raw=true "Terraform Modules Library")

![Terraform Modules Library](images/terraform-cloud-registry-publish-new-module-add-module-wait-to-become-ready.png?raw=true "Terraform Modules Library")

![Terraform Modules Library](images/terraform-cloud-registry-publish-new-module-add-module-is-ready.png?raw=true "Terraform Modules Library")

This repository has modules and patterns in the following derectories as demostrated below.

```
.
├── aws
│   ├── modules
│   │   └── terraform-aws-debug
│   └── patterns
│       ├── terraform-aws-multiple-ec2-vms
│       ├── terraform-aws-multiple-ec2-vms-behind-elb
│       └── terraform-aws-single-ec2-vm
├── azure
│   ├── modules
│   │   ├── terraform-azure-debug
│   │   └── terraform-azure-resource-group
│   └── patterns
├── custom
│   ├── modules
│   │   └── terraform-custom-debug
│   └── patterns
├── gcp
│   ├── modules
│   │   └── terraform-gcp-debug
│   └── patterns
```

## Getting started

Please install the following software

- Docker Desktop https://www.docker.com/products/docker-desktop/
- Visual Studio Code https://code.visualstudio.com/download (with Remote development in Containers extension) https://code.visualstudio.com/docs/remote/containers-tutorial
- Clone this repository
- Now Open VSCode and Open this repository

:bulb: Let's look at some Screenshots of Opening your project

In VSCode, click File -> Open Folder and navigate to this project
![VSCode open repository folder](images/vscode-open-repository-folder.png?raw=true "VSCode open repository folder")

If you have installed the Devcontainer Extension in the previous step, you'd see a Notification, bottom Right.

__Folder contains a Dev Container configuration file. Reopen folder to develop in a container__ and click `Reopen in Container`

![VSCode open repository folder open in devcontainer](images/vscode-open-repository-folder-open-in-devcontainer.png?raw=true "VSCode open repository folder open in devcontainer")

You will see the Docker Build log bottom right and once done, you will see the files of the project in VSCode.

![VSCode open repository folder open in devcontainer docker build
log](images/vscode-open-repository-folder-open-in-devcontainer-build-log.png?raw=true "VSCode open repository folder open in
devcontainer docker build log")

Now you can open a new Terminal. This will launch a new terminal inside the container, you will note that Terraform, Terragrunt and all the tools this project needs has already been installed for you.

![VSCode open repository folder open in devcontainer new Terminal](images/vscode-open-repository-folder-open-in-devcontainer-new-terminal.png?raw=true "VSCode open repository folder open in devcontainer new Terminal")

Now you need to Authenticate to your Clouds, see: __Authenticating__ below.

## Authenticating

Somewhere someone or something has to authenticate against the clouds, the pipeline will take care of this for you, however if you are developing locally, you need to take care of this yourself.

```
export AWS_ACCESS_KEY_ID=
export AWS_SECRET_ACCESS_KEY=
export AWS_REGION=

export GOOGLE_CREDENTIALS=

export ARM_CLIENT_ID=
export ARM_CLIENT_SECRET=
export ARM_SUBSCRIPTION_ID=
export ARM_TENANT_ID=
```

## Example output

When you run ./run.sh you will get the following output

`ubuntu@0c9b71981e0b:/app$ ./run.sh`

```
PIPELINE OUTPUT
```

## Contributing

### Adding/Changing a Module

1. Create a new git branch e.g. `feature/xxxxxx`
2. Either `cd` into the module folder and make the changes e.g. `modules/debug/` or create a new module.
3. Run `terraform init` to install providers for the module
4. Run `terraform fmt -check -recursive` to format any changes.
5. If format is successful run `terraform validate`
6. If validate is successful run `terraform plan`

## Testing

1. To test a Terraform module, use a test module (module/xxxxxx/test/)
2. When test module is ready for a Terraform module, run `run.sh` to check the result

## Debugging

At some stage of the game you will need to debug something, this setup, automates away a lot of toil, but with these automation and abstraction comes a lack of visibility.

- Make `make -dn docs` use the -dn flag for make

## FAQs

- Why are You using a Mono Repo?
I try to avoid code duplication and as a small team, to avoid this developers commit hell cycle, I opted for a Mono repo, you are welcome to split it up. You can always use the individual modules using the Terraform Source `source = "git::git@github.com:star3am/terraform-modules-library.git//aws/modules/terraform-aws-debug?ref=main"`

## Gotchas

- You will see in the terragrunt.hcl files where I detect the module source I post-fix it with `//.` see this bug: https://github.com/gruntwork-io/terragrunt/issues/1675

- fatal: detected dubious ownership in repository at '/app' To add an exception for this directory see this page: https://www.kenmuse.com/blog/avoiding-dubious-ownership-in-dev-containers/

- I've not found a way to force no-cache so sometimes you need to say ReBuild and ReOpen in Container.

![VSCode ReBuild and ReOpen in Container](images/remote-command-palette.png?raw=true "VSCode ReBuild and ReOpen in Container")

## Thanks!

Many thanks goes to the team at Gruntwork https://gruntwork.io/ for their amazing effort and for selflessly sharing their code in the great spirit of Open Source. In fact https://github.com/gruntwork-io/terragrunt-infrastructure-modules-example inspired this repo and set the foundation for this repo.
