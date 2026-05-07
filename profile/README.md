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

## 🧱 Stack & Arquitetura

A plataforma é construída como **microsserviços independentes**, cada um com seu próprio repositório nesta organização.

### Backend (.NET 10)
| Serviço | Responsabilidade |
|---|---|
| `auth-service` | OAuth Google, ASP.NET Identity, emissão/refresh de JWT |
| `user-service` | Perfis de usuário e completude de cadastro (publica eventos no RabbitMQ) |
| `doctor-service` | Médicos, especialidades e fluxo de aprovação |
| `patient-service` | Perfil clínico do paciente (LGPD-sensitive) |
| `emr-service` | Prontuário Eletrônico do Paciente (LGPD-sensitive) |
| `call-service` | Ciclo de vida de teleconsulta e metadados de salas Jitsi |
| `payment-service` | Cartões, pagamentos e webhooks Asaas |
| `princing-service` | Pricing dinâmico de teleconsulta (com Redis) |

### Frontend
| Repo | Stack |
|---|---|
| `doutoron-app` | Ionic 8 + React 19 + Vite + Capacitor 7 (PWA + iOS + Android) |

### Infraestrutura
| Repo | Função |
|---|---|
| `jitsi-private` | Servidor Jitsi self-hosted (Helm) para teleconsultas |
| `RabbitMQ` | Broker de eventos entre microsserviços |
| `redis` | Cache distribuído (pricing, sessões, rate limiting) |
| `postgres` | Banco relacional (um schema por serviço) |
| `k8s-config` | Manifestos Kubernetes compartilhados (Kustomize) |

### Tecnologias-chave
- **Linguagem backend**: C# / .NET 10
- **Linguagem frontend**: TypeScript / React 19
- **Mobile**: Capacitor 7 (iOS + Android nativo a partir do mesmo código web)
- **Vídeo**: Jitsi Meet self-hosted
- **Mensageria**: RabbitMQ (eventos `user.medico.created`, `user.patient.created`, `PriceQuotedEvent`...)
- **Cache**: Redis
- **Banco de dados**: PostgreSQL (um DB por microsserviço)
- **Pagamentos**: Asaas (cartão, Pix, boleto)
- **IA**: Google Gemini (transcrição de consultas)
- **Orquestração**: Kubernetes (manifests via Kustomize, charts via Helm)
- **CI/CD**: GitHub Actions com reusable workflows centralizados aqui no `.github`
- **Segurança**: Semgrep (SAST), Trivy (container), GitLeaks (secrets), Dependabot (deps)

---

<div align="center">

**Feito com ❤️ pela equipe Healsyn — saúde digital com excelência de engenharia.**

</div>
