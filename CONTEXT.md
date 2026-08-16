# Mentorship Context & Project Handoff

Paste the "Mentorship context" block below into a new Claude chat to resume the mentorship without losing the thread.

---

## Mentorship context (paste this into a new chat)

> You are my senior DevOps engineer mentor. I'm an intern building a 3-project portfolio for my first cloud/DevOps role. You explain the *why*, ask me questions before giving answers, and let me struggle before stepping in rather than just handing me commands to paste.
>
> **Project 1 (done):** Full-Stack Deploy Pipeline — Flask API + Docker + Nginx reverse proxy on AWS EC2, with a GitHub Actions pipeline that tests and auto-deploys on every push to main. Live and working.
>
> **Project 2 (in progress):** Infrastructure as Code with Terraform — a VPC, public + private subnets, an internet gateway, route table + association, a scoped security group, and an EC2 instance. State is stored remotely in an S3 bucket. All 8 resources build, destroy, and rebuild cleanly (reproducibility proven). **Remaining work:** write the GitHub Actions pipeline that runs `terraform plan` on pull requests and `terraform apply` on merge to main.
>
> **Project 3:** not yet defined.
>
> Resume at writing the Project 2 CI/CD pipeline. Before giving me the workflow file, ask me questions to check my understanding first.

---

## Where things live (machine-independent)

- **Code:** this repo (GitHub). Clone it on the new machine.
- **Terraform state:** S3 bucket `devops-proj-2-tfstate-258227418586` (region `us-east-1`). Reachable from any machine — this is the whole point of remote state.
- **AWS account:** 258227418586, region `us-east-1`.

## Resuming on a new machine (MacBook)

```bash
# install toolchain
brew install terraform awscli git

# authenticate to AWS (re-enter access key + secret, region us-east-1)
aws configure

# get the code
git clone <this-repo-url>
cd devops-proj-2

# reconnect to the S3 backend and rebuild
terraform init
terraform apply
```

## Project 2 status checklist

- [x] variables.tf, terraform.tfvars, outputs.tf, main.tf written
- [x] S3 state bucket bootstrapped manually + versioning enabled
- [x] Backend configured, `terraform init` successful
- [x] `terraform apply` provisions 8 resources
- [x] Destroy + recreate verified (reproducibility proven)
- [ ] GitHub Actions pipeline: `plan` on PR, `apply` on merge to main
- [ ] README.md (to be written by me, Cally, using Project 1's README as a template)

## Reminders

- Destroy resources (`terraform destroy`) whenever stepping away to avoid charges; `terraform apply` rebuilds in ~1 min.
- DNS on the old Windows laptop was flaky (`no such host` errors). If it recurs, flush DNS / set 8.8.8.8.
- Don't commit state files — `.gitignore` already excludes `.terraform/` and `*.tfstate`.
- Use an IAM user, not root credentials, for real work.
