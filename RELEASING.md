# Releasing — Healsyn/.github

Este documento descreve o processo de mudança e de release deste repositório, que
hospeda os workflows reusáveis consumidos pelos serviços da Healsyn e o perfil
público da organização.

## Perfil de mudança

Todo Pull Request que toque em `.github/workflows/**`, `.github/dependabot.yml`,
`/CODEOWNERS` ou este arquivo (`RELEASING.md`) é tratado como perfil **`critical`**
(ver `automation/profiles.yaml` em `engineering-control-plane`): exige revisão de
Code Owner (branch protection de `main` tem "Require review from Code Owners"
habilitado) e não pode ser mesclado por push direto ou force-push.

## Dono e SLA

- **Dono nomeado (2026-07-31):** @almirjuniordev.
- **SLA:** best-effort até um valor numérico formal ser decidido e registrado aqui.
  Não bloqueia nenhuma fase de iniciativas em andamento.

## Corte de tag (`v*`)

O Ruleset de tags cobre `v*` e bloqueia `update` e `delete` por qualquer ator.

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
repositório (não uma omissão): a identidade de release será nomeada antes do
próximo corte de tag que dependa dela. Até lá, o processo acima (item anterior)
é a única salvaguarda para criação de tags, e é responsabilidade de quem cortar a
tag segui-lo integralmente.

## Isolamento do runner self-hosted contra fork PRs

`Organization Settings → Actions → General → "Approval for running fork pull
request workflows from contributors"` está configurado como **"Require approval
for all external contributors"** (corrigido em 2026-07-31 — o valor anterior,
"first-time contributors", permitia que um colaborador externo rodasse workflow
sem aprovação assim que tivesse qualquer PR anterior mergeado).

Esta configuração **não pode regredir** para "first-time contributors" ou para
"never require approval" sem revisão do DevSecOps Lead — o runner é self-hosted e
este repositório é público.
