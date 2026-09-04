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
3. O pull SEMPRE vai para o branch `main` local, nunca direto no branch de trabalho atual da
   Helena (ex: `remote-sensing-0109`). `main` funciona só como espelho do upstream:
   - `git checkout main`
   - `git pull upstream main --rebase`
   - Se a Helena quiser trazer o conteúdo novo para o branch de trabalho, isso é um passo
     separado e explícito (merge/rebase de `main` para o branch), pedido por ela — nunca
     automático. Isolamento total: sync nunca gera conflito de rebase no trabalho em andamento.
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

- Confirmar se o push precisa de um novo token (tokens configurados na VM/sessão anterior podem
  não estar disponíveis numa sessão nova — checar `git remote -v` e testar `git ls-remote origin`).
- Antes do Comando 1, sempre `git status` e `git branch --show-current` para saber em que branch
  de trabalho a Helena está, e voltar para esse branch depois de atualizar `main`.
