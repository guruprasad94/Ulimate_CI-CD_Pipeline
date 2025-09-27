# Ulimate_CI-CD_Pipeline
This repo sets up an ultimate CI/CD pipeline with GitOps. Git webhook triggers Jenkins CI: Maven build, SonarQube scan, tests. On success, build/push Docker image to hub. Failures notify via Slack/email. CD: Image Updater refreshes manifests repo; Argo CD auto-deploys to K8s. Automates quality-gated releases.
