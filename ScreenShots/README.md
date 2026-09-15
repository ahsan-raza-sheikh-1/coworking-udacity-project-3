# Deployment Evidence

Verified 2026-09-15 in `us-east-1` against the `cluster` EKS deployment.

## Application URLs

- Frontend: http://a7c024243e31f43b78c74430083db0ae-618431148.us-east-1.elb.amazonaws.com/
- Backend API: http://a02c2114e2ffb4b0b8976d392607e56c-1166134380.us-east-1.elb.amazonaws.com/movies

The frontend served the React Movie List and the backend returned HTTP 200 JSON containing 3 movies.

## ECR Images

- Frontend tag: `35d5c5e3892cb0ecf507bb3221c674ab2fdcc95e`
- Frontend digest: `sha256:75450884947cad36676bbfb5b12bf2d37cb6f748a58e4c942d321f4fc8b2baf9`
- Backend tag: `35d5c5e3892cb0ecf507bb3221c674ab2fdcc95e`
- Backend digest: `sha256:b8be4c6325089a09c0bcbe6de892aa0bbcb399760d9a3cfc9001806d89ecfbba`

## Kubernetes Status

- Frontend deployment: `1/1` available and ready
- Backend deployment: `1/1` available and ready
- Frontend pod: `Running`, `1/1` ready
- Backend pod: `Running`, `1/1` ready
- Node: `ip-10-0-1-241.ec2.internal`

## Screenshots

- `frontend-application.png`: frontend URL and Movie List
- `load-balancer.png`: current frontend and backend LoadBalancer DNS names and ports
- `backend-api.png`: backend `/movies` response
- `ecr-frontend.png`: frontend ECR tag and digest
- `ecr-backend.png`: backend ECR tag and digest
- `kubectl-get-all.png`: services, deployments, pods, and images
- `kubectl-describe-deployments.png`: frontend and backend deployment descriptions
