# Releasing — Healsyn/.github

Este documento descreve o processo de mudança e de release deste repositório, que
hospeda os workflows reusáveis consumidos pelos serviços da Healsyn e o perfil
público da organização.

## Perfil de mudança

Todo Pull Request que toque em `.github/workflows/**`, `.github/dependabot.yml`,
`/CODEOWNERS`, `.github/CODEOWNERS`, `docs/CODEOWNERS` ou este arquivo
(`RELEASING.md`) é tratado como perfil **`critical`** (ver
`automation/profiles.yaml` em `engineering-control-plane`): exige **≥2 revisões
aprovadoras**, sendo pelo menos uma de Code Owner (branch protection/Ruleset de
`main` com "Require review from Code Owners" habilitado), e não pode ser
mesclado por push direto ou force-push.

## Dono e SLA

- **Dono nomeado (2026-07-31):** squad **@Healsyn/sq-infra** (decisão explícita
  do usuário, em resposta ao achado F2 da revisão de segurança — ver abaixo).
  Membro fundador: @almirjuniordev, mapeado a "Junior Fagundes", decisão
  registrada em `engineering-control-plane` — mesma pessoa assumida, não
  confirmada por segundo canal.
- **SLA:** best-effort até um valor numérico formal ser decidido e registrado aqui.
  Não bloqueia nenhuma fase de iniciativas em andamento.
- **F2 — pendência ainda EM ABERTO (revisão de segurança, 2026-07-31):** um
  único Code Owner nunca aprova o próprio PR, o que forçaria bypass em toda
  mudança de rotina e inviabiliza a exigência de ≥2 aprovações. Migrar o dono
  para o time `@Healsyn/sq-infra` resolve o problema **apenas quando o time
  tiver ≥2 membros com write access** neste repositório. Em 2026-07-31,
  `sq-infra` tinha só 1 membro (@almirjuniordev) — F2 continua aberto até um
  segundo membro (ex.: @afernandes97, já admin do repositório) ser adicionado
  ao time na UI do GitHub (`Organization → Teams → sq-infra → Members`). Sem
  isso, o uso do bypass do dono em PRs de rotina permanece necessário e deve
  ser tratado como risco residual documentado, não como controle efetivo.

## Corte de tag (`v*`)

**Status da configuração (2026-07-31): pendente.** O Ruleset de tags para `v*`
bloqueando `update` e `delete` por qualquer ator ainda precisa ser criado na UI
do GitHub (Settings → Rules → Rulesets → New tag ruleset) — esta seção descreve
o comportamento pretendido, não um estado já em vigor. Atualizar esta nota para
"em vigor" assim que o Ruleset for criado e confirmado (`git push --delete` de
uma tag de teste `v*` deve ser rejeitado).

Por processo (enquanto o bloqueio de `criação` por identidade nomeada estiver
adiado — ver seção seguinte), uma tag só deve ser cortada quando:

1. o commit for alcançável a partir de `main`;
2. o SHA for exatamente o mesmo validado pelo(s) canário(s) relevante(s) (pipeline
   completo verde, verificado por digest — não por reexecução);
3. o número da execução verde do canário for registrado como evidência na página
   do release.

Nunca cortar uma tag a partir de um SHA fora de `main` ou sem essa evidência.

## Identidade de release — status: adiado (2026-07-31)

O Ruleset de tags ainda **não** restringe quem pode *criar* uma tag `v*` a uma
identidade nomeada. Esta é uma decisão explícita e registrada do responsável pelo
repositório (não uma omissão): a identidade de release será nomeada e o Ruleset
completado com o bloqueio de criação **antes de abrir a Fase 3 / `TASK-003`**
(corte da tag `v1.0.0`) da iniciativa DEM-0003 — não "antes do próximo corte de
tag" de forma genérica, para não deixar margem a interpretar que algum corte
futuro poderia prosseguir sem essa decisão. Até lá, o processo acima (item
anterior) é a única salvaguarda para criação de tags, e é responsabilidade de
quem cortar a tag segui-lo integralmente.

## Isolamento do runner self-hosted contra fork PRs

`Organization Settings → Actions → General → "Approval for running fork pull
request workflows from contributors"` está configurado como **"Require approval
for all external contributors"** (corrigido em 2026-07-31 — o valor anterior,
"first-time contributors", permitia que um colaborador externo rodasse workflow
sem aprovação assim que tivesse qualquer PR anterior mergeado).

Esta configuração **não pode regredir** para "first-time contributors" ou para
"never require approval" sem revisão do DevSecOps Lead — o runner é self-hosted e
este repositório é público.

## Follow-up registrado (fora do escopo da Fase 0 de DEM-0003)

`profile/README.md` (perfil público da organização) não está coberto pelo
CODEOWNERS. É a vitrine pública da Healsyn e alvo plausível de defacement, mas
os critérios de aceite 8–11 da Fase 0 de DEM-0003 não o exigem. Considerar
adicionar `/profile/ @almirjuniordev` em um follow-up.
