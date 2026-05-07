<div align="center">

# 🏥 Healsyn

**HealthTech brasileira por trás da plataforma DoutorOn**

_Telemedicina, prontuário eletrônico e jornada do paciente — tudo num só lugar._

</div>

---

## 💡 O que é a Healsyn

A **Healsyn** é a empresa de tecnologia em saúde que constrói e mantém a plataforma **DoutorOn** — um ecossistema completo de telemedicina pensado para conectar pacientes, médicos e clínicas no Brasil de ponta a ponta:

- 📅 **Agendamento e teleconsulta** com vídeo próprio (self-hosted Jitsi)
- 📋 **Prontuário Eletrônico do Paciente (PEP)** com criptografia em repouso e em trânsito
- 💳 **Pagamentos** integrados via Asaas (cartão, Pix, boleto)
- 🩺 **Cadastro e aprovação de médicos** com checagem de CRM/especialidades
- 🤖 **IA assistiva** (transcrição automática de consultas via Gemini)
- 📱 **Apps multiplataforma** (PWA + iOS + Android via Capacitor)

> **Compliance:** desenvolvido em conformidade com a **LGPD** e diretrizes da CFM/Telemedicina (Resolução CFM 2.314/2022).

## 🏗️ Como nos organizamos

Toda a plataforma é construída como **microsserviços independentes**, cada um com seu próprio repositório nesta organização:

| Camada | Repos |
|---|---|
| **Backend (.NET 10)** | `auth-service`, `user-service`, `doctor-service`, `patient-service`, `emr-service`, `call-service`, `payment-service`, `princing-service` |
| **Frontend** | `doutoron-app` (Ionic 8 + React 19 + Vite) |
| **Infraestrutura** | `jitsi-private`, `RabbitMQ`, `redis`, `postgres`, `k8s-config` |
| **Plataforma** | `.github` (você está aqui) |

## ⚙️ Este repositório (`.github`)

Centraliza **engenharia de plataforma** e **DevSecOps** da org:

- 🔁 **Reusable workflows** chamados por todos os repos
- 🧱 **Composite actions** (planejado)
- 📐 **Templates** de issues, PRs e workflows (planejado)

> Mude aqui, propaga para todos os repos. Sem duplicação, sem drift.

### 🧩 Reusable workflows disponíveis

Chame em qualquer repo via `uses: Healsyn/.github/.github/workflows/<arquivo>@main`.

| Workflow | Stack | Função |
|---|---|---|
| `_dotnet-quality-gate.yml` | .NET 10 | Build + Test + (opt) Format check |
| `_dotnet-security-gate.yml` | .NET 10 | GitLeaks + Semgrep + Trivy + Dependabot vuln check |

#### Exemplo (`<microservice>/.github/workflows/ci.yml`)
```yaml
name: 🔄 CI - Quality Gate
on:
  pull_request:
    branches: [main, develop]
  workflow_call:

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
  pull_request:
    branches: [main]
  schedule:
    - cron: '0 6 * * *'
  workflow_call:

jobs:
  security:
    uses: Healsyn/.github/.github/workflows/_dotnet-security-gate.yml@main
    with:
      service-name: auth-service
      solution-file: AuthService.sln
    secrets: inherit
```

## 🛣️ Roadmap de Plataforma

- [x] **Wave 1** — Reusable workflows .NET (Quality + Security Gate)
- [ ] **Wave 2** — `_node-quality-gate`, `_node-security-gate` para `doutoron-app`
- [ ] **Wave 2** — `_helm-package` (charts) e `_k8s-validate` (manifests)
- [ ] **Wave 2** — Composite actions: SBOM (Syft), assinatura (Cosign keyless OIDC), SLSA provenance
- [ ] **Wave 3** — Backstage + Software Templates (scaffolder de novos serviços)
- [ ] **Wave 3** — Org rulesets + required workflows + SLSA Level 3

## 📝 Convenções

- **Versionamento**: workflows referenciados como `@main` por enquanto. Adotaremos `@v1` (com tags semver) quando estabilizar.
- **Secrets**: callers passam `secrets: inherit` para herdar `GITLEAKS_LICENSE`, registry tokens etc da org.
- **Idioma**: copy/comments/commits em **pt-BR** (Conventional Commits).
- **LGPD**: nunca logar dado de paciente, EMR, cartão, JWT ou tokens de Jitsi.

---

<div align="center">

**Feito com ❤️ pela equipe Healsyn — saúde digital com excelência de engenharia.**

</div>
