  # PipelineIQ Frontend  
   
Independent repository for the PipelineIQ React/Vite frontend.

## Build

```bash
docker build -t <acr-login-server>/final_capstone-frontend:local -f frontend/Dockerfile .
``` 
 
## Local Run 
   
The frontend is built with Vite and served by non-root Nginx on container port 8080. 


```bash
cd frontend
npm install
npm run dev
```

## CI/CD Pipeline

Pipeline file: `.github/workflows/service-ci.yml`

Run it from GitHub Actions with `Run workflow` on the `dev` branch, or push to `dev`:

```bash
git checkout dev
git add .
git commit -m "change frontend"
git push origin dev
```

The pipeline calls the reusable workflow in `PipelineIQproject/pipeline_main` and runs the Vite build, SonarQube Cloud, Snyk, Docker build, Trivy, HTTP smoke test, ACR push, Helm dev update, production approval, Helm prod update, and Slack notification.

## Required Secrets

| Secret | Purpose |
| --- | --- |
| `ACR_LOGIN_SERVER` | Azure Container Registry server. |
| `ACR_USERNAME` | Identity allowed to push to ACR. |
| `ACR_PASSWORD` | Password/secret for the ACR identity. |
| `SONAR_TOKEN` | SonarQube Cloud token. |
| `SNYK_TOKEN` | Snyk API token. |
| `MAIN_REPO_PAT` | PAT with write access to `PipelineIQproject/pipeline_main`. |
| `SLACK_WEBHOOK_URL` | Slack incoming webhook for success/failure notifications. |
| `SMOKE_TEST_ENV_FILE` | Optional dotenv content; usually not required for the Nginx smoke test. |

Create a protected GitHub Environment named `production` so the prod Helm value update requires approval.



