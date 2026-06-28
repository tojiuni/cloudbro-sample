# cloudbro-sample

CloudBro(Zadara) PoC — **Tekton CI → ArgoCD CD** 샘플.

- `app/` — 정적 사이트 소스 + Dockerfile
- `ci.yaml` — CI 빌드 설정 (kaniko가 `app/Dockerfile` 빌드 → cloudbro registry push)
- `deploy/` — ArgoCD가 watch하는 k8s 매니페스트 (ns/deploy/svc/ingress)

## 흐름
1. `git push` → GitHub webhook → cloudbro Tekton EventListener
2. Tekton: git-clone(public) → kaniko build → `registry.121.78.39.181.sslip.io/cloudbro-sample:<sha>`,`:latest` push → rollout
3. ArgoCD: `deploy/` 동기화 → https://sample.121.78.39.181.sslip.io

자격증명 불필요(public repo + 개방 registry + in-cluster SA).
