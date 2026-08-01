# Releasing — Healsyn/.github

Este documento descreve o processo de mudança e de release deste repositório, que
hospeda os workflows reusáveis consumidos pelos serviços da Healsyn e o perfil
público da organização.

## Perfil de mudança

Todo Pull Request que toque em `.github/workflows/**`, `.github/dependabot.yml`,
`/CODEOWNERS`, `.github/CODEOWNERS`, `docs/CODEOWNERS` ou este arquivo
(`RELEASING.md`) é tratado como perfil **`critical`** (ver
`automation/profiles.yaml` em `engineering-control-plane`): deve exigir **≥2
revisões aprovadoras**, sendo pelo menos uma de Code Owner, e não deve poder
ser mesclado por push direto ou force-push.

**⚠️ Status da aplicação técnica (2026-08-01): ADIADO, não configurado.** O
Ruleset de proteção de `main` que faria o GitHub aplicar (não só documentar)
as regras acima **ainda não existe**. Por decisão explícita do usuário
("deixe essas rulesets pra depois"), a criação do Ruleset de `main` e do
Ruleset de tags `v*` (ver seção seguinte) foi adiada — não é uma tarefa
esquecida, é um risco aceito conscientemente por enquanto. Enquanto isso:
push direto e merge sem revisão em `main` continuam **tecnicamente possíveis**
para qualquer colaborador com write access; este documento e o CODEOWNERS
funcionam apenas como convenção, não como controle técnico. Não presumir
proteção de `main` em nenhuma outra decisão (ex.: Fase 1 de DEM-0003) sem
verificar primeiro se o Ruleset já existe.

## Dono e SLA

- **Dono nomeado (2026-07-31):** squad **@Healsyn/sq-infra** (decisão explícita
  do usuário, em resposta ao achado F2 da revisão de segurança — ver abaixo).
  Membro fundador: @almirjuniordev, mapeado a "Junior Fagundes", decisão
  registrada em `engineering-control-plane` — mesma pessoa assumida, não
  confirmada por segundo canal.
- **SLA:** best-effort até um valor numérico formal ser decidido e registrado aqui.
  Não bloqueia nenhuma fase de iniciativas em andamento.
- **F2 — FECHADO (revisão de segurança, 2026-07-31):** um único Code Owner
  nunca aprova o próprio PR, o que forçaria bypass em toda mudança de rotina
  e inviabilizaria a exigência de ≥2 aprovações. Migrado o dono para o time
  `@Healsyn/sq-infra`, agora com **2 membros com write access** neste
  repositório (@almirjuniordev, @afernandes97 — este último já era admin do
  `Healsyn/.github`, nenhum acesso novo foi concedido). PRs de rotina de
  qualquer um dos dois já têm um segundo revisor real, sem depender de
  bypass.

## Corte de tag (`v*`)

**Status da configuração (2026-08-01): adiado por decisão do usuário**, mesmo
adiamento registrado na seção "Perfil de mudança" acima. O Ruleset de tags
para `v*` bloqueando `update` e `delete` por qualquer ator ainda precisa ser
criado na UI do GitHub (Settings → Rules → Rulesets → New tag ruleset) — esta
seção descreve o comportamento pretendido, não um estado já em vigor. Até lá,
`update`/`delete` de qualquer tag `v*` são **tecnicamente possíveis** para
quem tiver write access — o processo abaixo é a única salvaguarda. Atualizar
esta nota para "em vigor" assim que o Ruleset for criado e confirmado
(`git push --delete` de uma tag de teste `v*` deve ser rejeitado).

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
