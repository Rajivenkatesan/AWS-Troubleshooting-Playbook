AWS Troubleshooting Playbook

Reproducible AWS failure scenarios, each with a step-by-step diagnosis, the root cause, and the fix. Every lab is built with Terraform so you can break it, troubleshoot it, and fix it yourself.

Why this project ?

Cloud support work is mostly diagnosis. A customer reports a symptom ("my site times out"), and the job is to find the root cause quickly and explain the fix clearly. This playbook practices exactly that. Each scenario is written like a support case: the symptom first, then how to reproduce it, how to diagnose it in a logical order, the root cause, the fix, and how to prevent it.

All scenarios are recreated from common AWS failure patterns in a personal sandbox account. No employer or customer data is used.

Scenarios

| No | Scenario | Services | Status |
| 01 | EC2 instance unreachable: security group blocks traffic | EC2, VPC | In progress |
| 02 | S3 Accessenied: bucket policy or IAM conflict | S3, IAM | Planned |
| 03 | IAM permission gap: missing action or wrong trust policy | IAM, STS | Planned |
| 04 | EC2 disk full: find the cause and fix it | EC2, EBS, CloudWatch | Planned |

Ideas for later: ALB unhealthy targets, RDS connection failures, a Lambda function in a VPC with no internet access, and CPU credit exhaustion on burstable instances.
How each scenario is structured

1. Symptom: the problem as a customer would describe it
2. Lab setup: what Terraform builds
3. Reproduce: the exact commands that create the fault
4. Diagnose: the checks, in order, and what each one tells you
5. Root cause: why it happened
6. Fix: how to resolve it and verify
7. Prevention: how to stop it happening again
8. Clean up: how to remove everything

Run a scenario

Prerequisites

- A personal AWS account 
- AWS CLI v2 installed, with credentials configured
- Terraform installed
- A bash shell 
- An AWS Budget alert set up before you start

Steps

```
cd scenarios/01-ec2-unreachable-security-group
terraform init
terraform apply -var="broken=true"    # builds the lab with the fault in place
```

Follow the scenario's README to diagnose the problem. When you are ready to apply the fix through code:

```
terraform apply -var="broken=false"
```

Always clean up when you finish:

```
terraform destroy
```

Some scenarios need a `terraform.tfvars` file (it is git-ignored). Each scenario's README says what to put in it.

## Cost and safety

- Labs use small instance types and avoid NAT gateways, but resources can still incur AWS charges.
- Run `terraform destroy` after every lab, and delete anything you created by hand.
- Never commit access keys, `.pem` files, `terraform.tfvars`, or Terraform state.
- Use a personal sandbox account only. You are responsible for any charges from running these labs.

Repository structure

```

├── README.md
├── docs/
│   ├── requirements.md       # what the playbook needs to achieve
│   ├── decisions/            # short notes on design choices
│   └── diagrams/
└── scenarios/
    └── 01-ec2-unreachable-security-group/
        ├── README.md         # the playbook entry
        ├── main.tf           # the lab, with a `broken` variable
        ├── variables.tf
        └── outputs.tf
```

Tools and AWS features used

Terraform, AWS CLI, VPC Reachability Analyzer, VPC Flow Logs, CloudTrail, IAM Policy Simulator, and CloudWatch.

About

Built by Raji V as hands-on practice for cloud support and cloud engineering work on AWS.
Connect with me on [LinkedIn](https://www.linkedin.com/in/raji-v-30d012003/).

