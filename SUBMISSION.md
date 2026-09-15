# Movie Picture Pipeline Submission

Complete this file from the public GitHub repository after the four workflows have successful runs. Do not submit the old LoadBalancer hostnames from a previous cluster unless the live checks below succeed again.

## Public GitHub Repository

* Repository: `REPLACE_BEFORE_SUBMISSION: https://github.com/<owner>/<public-repository>`
* Actions: `REPLACE_BEFORE_SUBMISSION: https://github.com/<owner>/<public-repository>/actions`

## Required Workflows

| Workflow | File | Required trigger and result |
| --- | --- | --- |
| Frontend CI | [frontend-ci.yaml](.github/workflows/frontend-ci.yaml) | Pull request to `main` or manual run; lint and test run in parallel, then the Docker build passes. |
| Backend CI | [backend-ci.yaml](.github/workflows/backend-ci.yaml) | Pull request to `main` or manual run; lint and test run in parallel, then the Docker build passes. |
| Frontend CD | [frontend-cd.yaml](.github/workflows/frontend-cd.yaml) | Push to `main` or manual run; image is pushed to ECR and the EKS rollout succeeds. |
| Backend CD | [backend-cd.yaml](.github/workflows/backend-cd.yaml) | Push to `main` or manual run; image is pushed to ECR and the EKS rollout succeeds. |

Successful run links:

* Frontend CI: `REPLACE_BEFORE_SUBMISSION: https://github.com/<owner>/<public-repository>/actions/workflows/frontend-ci.yaml`
* Backend CI: `REPLACE_BEFORE_SUBMISSION: https://github.com/<owner>/<public-repository>/actions/workflows/backend-ci.yaml`
* Frontend CD: `REPLACE_BEFORE_SUBMISSION: https://github.com/<owner>/<public-repository>/actions/workflows/frontend-cd.yaml`
* Backend CD: `REPLACE_BEFORE_SUBMISSION: https://github.com/<owner>/<public-repository>/actions/workflows/backend-cd.yaml`

## Completion Order

1. Push the project to a public GitHub repository. Do not push `.env` or any AWS credential values.
2. Add `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, and `AWS_REGION` as GitHub Actions secrets. Add `EKS_CLUSTER_NAME` as a repository variable if the cluster is not named `cluster`.
3. Run both CI workflows manually, or create a pull request targeting `main`, and save one successful run link for each.
4. Run Backend CD. Copy the backend LoadBalancer hostname from its `Record deployment evidence` step.
5. Set `REACT_APP_MOVIE_API_URL` to `http://<backend-load-balancer-hostname>` as a repository variable or secret.
6. Run Frontend CD. Copy the frontend LoadBalancer hostname, deployed image SHA, and successful run link.
7. Open both application URLs, verify the movie list and `/movies` JSON response, then capture the required screenshots before deleting the infrastructure.

## Live Application URLs

Copy these values from the `Record deployment evidence` step in the latest successful CD runs. The backend URL must include `/movies` for the API check. The frontend build variable must contain the backend host without `/movies`.

* Frontend application: http://a7c024243e31f43b78c74430083db0ae-618431148.us-east-1.elb.amazonaws.com/
* Backend API: http://a02c2114e2ffb4b0b8976d392607e56c-1166134380.us-east-1.elb.amazonaws.com/movies
* Current frontend image: `545852992340.dkr.ecr.us-east-1.amazonaws.com/frontend:35d5c5e3892cb0ecf507bb3221c674ab2fdcc95e`
* Current backend image: `545852992340.dkr.ecr.us-east-1.amazonaws.com/backend:35d5c5e3892cb0ecf507bb3221c674ab2fdcc95e`
* Frontend image digest: `sha256:75450884947cad36676bbfb5b12bf2d37cb6f748a58e4c942d321f4fc8b2baf9`
* Backend image digest: `sha256:b8be4c6325089a09c0bcbe6de892aa0bbcb399760d9a3cfc9001806d89ecfbba`
* Image tag source commit: `35d5c5e3892cb0ecf507bb3221c674ab2fdcc95e`
* Latest local evidence commit: `e852305b9d9335099822c08ee600b226721732a0`

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

## Evidence If Infrastructure Is Deleted

Attach screenshots with the URL or command output visible:

1. Frontend application open in a browser using its LoadBalancer URL.
2. `kubectl get all -n default`.
3. `kubectl describe deployment frontend -n default` and `kubectl describe deployment backend -n default`.
4. Latest image details for both ECR repositories, including the Git SHA tag.

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

The generated `movie_picture_pipeline_submission.zip` includes this `SUBMISSION.md` file and all four workflow files under `.github/workflows/`.