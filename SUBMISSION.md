# Movie Picture Pipeline Submission

This document records the public repository, workflow pages, deployed endpoints, image references, and evidence for the Movie Picture Pipeline project.

## Public GitHub Repository

* Repository: [ahsan-raza-sheikh-1/coworking-udacity-project-3](https://github.com/ahsan-raza-sheikh-1/coworking-udacity-project-3)
* Actions: [GitHub Actions](https://github.com/ahsan-raza-sheikh-1/coworking-udacity-project-3/actions)

## Required Workflows

| Workflow | File | Required trigger and result |
| --- | --- | --- |
| Frontend CI | [frontend-ci.yaml](.github/workflows/frontend-ci.yaml) | Pull request to `main` or manual run; lint and test run in parallel, then the Docker build passes. |
| Backend CI | [backend-ci.yaml](.github/workflows/backend-ci.yaml) | Pull request to `main` or manual run; lint and test run in parallel, then the Docker build passes. |
| Frontend CD | [frontend-cd.yaml](.github/workflows/frontend-cd.yaml) | Push to `main` or manual run; image is pushed to ECR and the EKS rollout succeeds. |
| Backend CD | [backend-cd.yaml](.github/workflows/backend-cd.yaml) | Push to `main` or manual run; image is pushed to ECR and the EKS rollout succeeds. |

Workflow run history:

* Frontend CI: [frontend-ci.yaml workflow](https://github.com/ahsan-raza-sheikh-1/coworking-udacity-project-3/actions/workflows/frontend-ci.yaml)
* Backend CI: [backend-ci.yaml workflow](https://github.com/ahsan-raza-sheikh-1/coworking-udacity-project-3/actions/workflows/backend-ci.yaml)
* Frontend CD: [frontend-cd.yaml workflow](https://github.com/ahsan-raza-sheikh-1/coworking-udacity-project-3/actions/workflows/frontend-cd.yaml)
* Backend CD: [backend-cd.yaml workflow](https://github.com/ahsan-raza-sheikh-1/coworking-udacity-project-3/actions/workflows/backend-cd.yaml)

The repository contains two CI workflows and two CD workflows. Record one successful run for each workflow in the corresponding workflow history page before final submission. The deployed service URLs and image references below are from the verified EKS deployment.

Evidence assets:

* [Frontend application](evidence/frontend-application.png)
* [Backend API](evidence/backend-api.png)
* [LoadBalancer DNS list](2.LB.png)
* [`kubectl get all` output](evidence/kubectl-get-all.png)
* [`kubectl describe deployment` output](evidence/kubectl-describe-deployments.png)
* [Backend ECR SHA-tagged image](3.ECR.png)
* [Frontend ECR SHA-tagged image](4ECR.png)

## Live Application URLs

Copy these values from the `Record deployment evidence` step in the latest successful CD runs. The backend URL must include `/movies` for the API check. The frontend build variable must contain the backend host without `/movies`.

* Frontend application: http://a7c024243e31f43b78c74430083db0ae-618431148.us-east-1.elb.amazonaws.com/
* Backend API: http://a02c2114e2ffb4b0b8976d392607e56c-1166134380.us-east-1.elb.amazonaws.com/movies
* Current frontend image: `545852992340.dkr.ecr.us-east-1.amazonaws.com/frontend:35d5c5e3892cb0ecf507bb3221c674ab2fdcc95e`
* Current backend image: `545852992340.dkr.ecr.us-east-1.amazonaws.com/backend:35d5c5e3892cb0ecf507bb3221c674ab2fdcc95e`
* Frontend image digest: `sha256:75450884947cad36676bbfb5b12bf2d37cb6f748a58e4c942d321f4fc8b2baf9`
* Backend image digest: `sha256:b8be4c6325089a09c0bcbe6de892aa0bbcb399760d9a3cfc9001806d89ecfbba`
* Image tag source commit: `35d5c5e3892cb0ecf507bb3221c674ab2fdcc95e`
* Latest local evidence commit: `870fcc43d12f175e0ad5aa0210ac1fb83672af56`

Verify both URLs from a browser or HTTP client before submitting. The frontend must load the movie list and the backend endpoint must return HTTP 200 JSON containing a `movies` list.

Latest live verification on 2026-09-15:

* Backend: HTTP 200, JSON response with 3 movies.
* Frontend: HTTP 200, React root served.
* Frontend bundle: contains the live backend hostname and `/movies` path.

Infrastructure status at the same verification:

* EKS node: `ip-10-0-1-241.ec2.internal` is `Ready`.
* Backend pod and frontend pod: `Running`.
* Backend deployment: `1/1` available and ready.
* Frontend deployment: `1/1` available and ready.

## Evidence

If the AWS infrastructure is removed, retain screenshots showing the frontend LoadBalancer URL, `kubectl get all -n default`, `kubectl describe deployment frontend -n default`, `kubectl describe deployment backend -n default`, and the latest frontend and backend ECR image details including the Git SHA tag.

Commands used to recreate the evidence:

```bash
kubectl get all -n default
kubectl describe deployment frontend -n default
kubectl describe deployment backend -n default
aws ecr describe-images --repository-name frontend --region us-east-1
aws ecr describe-images --repository-name backend --region us-east-1
```

## Required GitHub Configuration

Deployment workflows require these repository secrets:

* `AWS_ACCESS_KEY_ID`
* `AWS_SECRET_ACCESS_KEY`
* `AWS_REGION` (optional; defaults to `us-east-1`)

Set `REACT_APP_MOVIE_API_URL` as a repository variable or secret and optionally set `EKS_CLUSTER_NAME` as a repository variable. Do not commit `.env` or any AWS credential values.

## Package Contents

This submission package excludes local `.env` files, Terraform state and backups, `.terraform`, `node_modules`, React build output, test caches, and Python cache files.

The `movie_picture_pipeline_submission.zip` archive includes this `SUBMISSION.md` file and all four workflow files under `.github/workflows/`.

