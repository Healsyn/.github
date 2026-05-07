# Healsyn Platform — `.github`

Repositório org-level que centraliza **workflows reutilizáveis**, **composite actions** e **templates** para todos os serviços da Healsyn (microsserviços .NET, frontend React, charts Helm, manifestos K8s).

> Engenharia de Plataforma + DevSecOps em um único lugar. Mude aqui, propaga para todos os repos.

## 🧩 Reusable Workflows

Chamados em qualquer repo da org via `uses: Healsyn/.github/.github/workflows/<arquivo>@main`.

| Workflow | Stack | Função |
|---|---|---|
| `_dotnet-quality-gate.yml` | .NET | Build + Test + (opt) Format check |
| `_dotnet-security-gate.yml` | .NET | GitLeaks + Semgrep + Trivy + dotnet vuln |

### Exemplo de uso (em `microservice/.github/workflows/ci.yml`):
```yaml
name: 🔄 CI - Quality Gate
on:
  pull_request:
    branches: [main, develop]
  push:
    branches: [main]

jobs:
  ci:
    uses: Healsyn/.github/.github/workflows/_dotnet-quality-gate.yml@main
    with:
      solution-file: AuthService.sln
      dotnet-version: '10.0.x'
```

```yaml
name: 🔒 Security Gate
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
  schedule:
    - cron: '0 6 * * *'

jobs:
  security:
    uses: Healsyn/.github/.github/workflows/_dotnet-security-gate.yml@main
    with:
      service-name: auth-service
      solution-file: AuthService.sln
    secrets: inherit
```

## 🛣️ Roadmap

- [x] Wave 1 — `_dotnet-quality-gate`, `_dotnet-security-gate`
- [ ] Wave 2 — `_node-quality-gate`, `_node-security-gate` (doutoron-app)
- [ ] Wave 2 — `_helm-package` (jitsi-private, charts), `_k8s-validate` (k8s-config)
- [ ] Wave 2 — Composite actions: `sbom-generate`, `image-sign` (cosign keyless), `slsa-provenance`
- [ ] Wave 3 — Backstage + Software Templates + scorecards
- [ ] Wave 3 — Org rulesets + required workflows

## 📝 Convenções

- **Versão**: por enquanto `@main`. Quando estabilizar, adotar `@v1` com tags.
- **Secrets**: serviços passam `secrets: inherit` para herdar `GITLEAKS_LICENSE`, etc.
- **Linguagem**: copy/comments em pt-BR (alinhado ao restante da org).
