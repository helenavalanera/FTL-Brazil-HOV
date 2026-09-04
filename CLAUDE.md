# CLAUDE.md — Workflow de Co-Work FTL Brazil HOV

Este repositório integra o material oficial do professor (`upstream` = `ftl-brazil-2026/ebook`,
somente leitura — push desabilitado) com a pasta de estudos pessoal da Helena (`/aprendizado`),
versionada no `origin` (`helenavalanera/ftl-brazil-hov`).

Perfil: Engenharia de Produção, foco em otimização de processos (Lean/PDCA) e Back-End.
Toda análise deve conectar código/teoria da aula com esse repertório.

## Convenções

- **Marco zero**: cada sincronização com o upstream gera uma tag local `marco-zero-AAAA-MM-DD`
  no commit de `upstream/main` recém-puxado. É a referência usada no diff incremental do
  Comando 2 (`git diff <última tag marco-zero>..upstream/main`). Ver tags com `git tag -l "marco-zero-*"`.
- **Branch de trabalho pessoal**: a Helena trabalha em branches próprios (ex: `remote-sensing-0109`,
  `week1-helenavalanera`), nunca commita direto em `main`.
- **Reviews de aula**: HTML autocontido (CSS/estilos embutidos ou Tailwind via CDN), salvo em
  `/aprendizado/reviews/`, nomeado `AAAA-MM-DD-tema-da-aula.html`.
- **Autenticação de push**: este ambiente roda em VM isolada sem acesso ao gitconfig/SSH reais da
  Helena. O push usa um Personal Access Token do GitHub fornecido pela Helena por sessão — nunca
  fica salvo em texto puro no repositório.

## COMANDO 1 — Pré-Aula (Sincronização)

**Gatilho:** "Atualize o repositório do FTL_Brasil" (ou equivalente).

**Passos:**
1. `git fetch upstream`
2. Comparar `upstream/main` com a última tag `marco-zero-*` existente — listar arquivos/commits novos.
3. `git pull upstream main --rebase` (ou, se a Helena estiver num branch de trabalho, avaliar se o
   pull deve ir para `main` local e depois ela decide se traz para o branch de trabalho — não fazer
   merge/rebase automático no branch de trabalho sem perguntar).
4. Criar a tag `marco-zero-AAAA-MM-DD` no commit resultante.
5. Reportar de forma resumida: o que mudou (arquivos, notebooks, conteúdo teórico) desde o marco
   zero anterior.

## COMANDO 2 — Pós-Aula (Review em HTML e Versionamento)

**Gatilho:** a Helena cola a transcrição da aula e pede o review/módulo.

**Passos:**
1. **Análise incremental**: `git diff <marco-zero-anterior>..upstream/main` (ou equivalente) para
   ver o que mudou nos códigos do professor desde o marco zero atual.
2. **Cruzamento**: cruzar essas mudanças de código com o conteúdo da transcrição colada.
3. **Geração HTML**: montar o review com Resumo executivo, Análise técnica (código vs. teoria),
   Insights práticos (Engenharia de Produção / Back-End) e Checklist. Salvar em
   `/aprendizado/reviews/AAAA-MM-DD-tema.html`.
4. **Git**: `git add .` → commit descritivo com o tema da aula → `git push origin <branch atual>`.
   Nunca commitar/push direto em `main` sem confirmação explícita.

## Pendências conhecidas (revisar a cada sessão nova)

- Confirmar se o push precisa de um novo token (tokens de sessão da VM não persistem entre sessões).
- Confirmar branch de trabalho atual antes do Comando 1, caso a Helena tenha trocado de branch.
