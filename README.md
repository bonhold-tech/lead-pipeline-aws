# lead-pipeline-aws

> 🚧 **Work in progress.** This project is under active development. Parts of the
> setup described below are not implemented yet - see [Status](#status) for what
> is done and what is planned.

Lead-capture app from [lead-pipeline-infra](https://github.com/bonhold-tech/lead-pipeline-infra),
containerised with Docker and deployed to AWS, with the whole infrastructure defined
as code in Terraform.

## Why this project?

In [lead-pipeline-infra](https://github.com/bonhold-tech/lead-pipeline-infra) the same
application runs on five local Vagrant/libvirt VMs. This project takes the next step:
moving it from a local lab to the cloud, the way it would be done for a real small
business.

- **Docker** - each component runs in its own container, so the app no longer depends
  on how a specific VM was provisioned.
- **Terraform** - the AWS infrastructure is described in code, so it can be created,
  reviewed, changed and destroyed in a repeatable way.

## How the application works

1. A visitor fills in a web form.
2. The form publishes the lead to a **RabbitMQ** queue.
3. A consumer reads the message and stores the lead in **MariaDB**.
4. An email with the lead's details is sent to the sales inbox.

<!-- TODO: add an architecture diagram once the AWS layout is final -->

## Status

- [ ] Dockerfile for each component
- [ ] Docker Compose setup for running the whole app locally
- [ ] Terraform: networking (VPC, subnets, security groups)
- [ ] Terraform: compute for the containers <!-- TODO: name the AWS service, e.g. EC2 -->
- [ ] Deployment of the app to AWS
- [ ] Secrets handled outside the repository (SMTP and database credentials)
- [ ] Planned: CI/CD pipeline with Jenkins (build images and deploy on push)

## Tech stack

- **Cloud:** AWS <!-- TODO: list the services actually used -->
- **Infrastructure as code:** Terraform
- **Containers:** Docker, Docker Compose
- **Application:** Python, RabbitMQ, MariaDB
- **Planned:** Jenkins

## Project structure

<!-- TODO: fill in once the layout is stable -->

```
lead-pipeline-aws/
├── README.md
├── LICENSE
├── app/              # application code and Dockerfiles
└── terraform/        # AWS infrastructure
```

## Usage

Instructions will be added once the first version can be deployed end to end.

## Cost and security notes

- The project is meant to run within the AWS Free Tier where possible. Run
  `terraform destroy` after testing to avoid unexpected charges.
- Credentials are never committed to the repository.
- Terraform state files (`*.tfstate`) and variable files with secrets (`*.tfvars`)
  are git-ignored, because the state can contain sensitive values in plain text.

## Related projects

- [server-compliance-checker](https://github.com/bonhold-tech/server-compliance-checker) -
  multi-VM security audit tool for Ubuntu and CentOS
- [lead-pipeline-infra](https://github.com/bonhold-tech/lead-pipeline-infra) - the same
  application on local Vagrant VMs

## License

MIT
