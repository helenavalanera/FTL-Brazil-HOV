# CLAUDE.md — Workflow de Co-Work FTL Brazil HOV

Este repositório integra o material oficial do professor (`upstream` = `ftl-brazil-2026/ebook`,
somente leitura — push desabilitado) com a pasta de estudos pessoal da Helena (`/aprendizado`),
versionada no `origin` (`helenavalanera/FTL-Brazil-HOV`).

Perfil: Engenharia de Produção, foco em otimização de processos (Lean/PDCA) e Back-End.
Toda análise deve conectar código/teoria da aula com esse repertório.

## Convenções

- **Marco zero**: cada sincronização com o upstream gera uma tag local `marco-zero-AAAA-MM-DD`
  no commit de `upstream/main` recém-puxado. É a referência usada no diff incremental do
  Comando 2 (`git diff <última tag marco-zero>..upstream/main`). Ver tags com `git tag -l "marco-zero-*"`.
- **Branch de trabalho pessoal**: a Helena trabalha em branches próprios (ex: `remote-sensing-0109`,
  `week1-helenavalanera`), nunca commita direto em `main`.
- **Reviews de aula**: HTML autocontido (CSS/JS embutidos, Tailwind via CDN aceitável), salvo em
  `/aprendizado/reviews/`, nomeado `AAAA-MM-DD-tema-da-aula.html`. Ver estilo/estrutura de
  referência em `/aprendizado/referencias/aula5sensoriamentoremoto.html` (visual escuro, sidebar
  de navegação, cards `<details>` por módulo, glossário, exercícios com solução colapsável).
- **Commits da Helena**: servem para registrar o avanço e as modificações dela mesma (não são
  1:1 com cada aula) — mensagens descritivas do que mudou, sem forçar um tema por commit.
- **Autenticação de push**: este ambiente roda em VM isolada sem acesso ao gitconfig/SSH reais da
  Helena. O push usa um Personal Access Token do GitHub fornecido pela Helena por sessão — nunca
  fica salvo em texto puro no repositório, nem em nenhum arquivo de memória.

## LIMITAÇÃO PERMANENTE — transcrições de aula do Emanuel Goulart

**Toda transcrição de áudio das aulas (Tactiq, Teams, ou similar) sai ilegível especificamente na
fala do Emanuel Goulart** — confirmado em 6 transcrições de aulas diferentes (Sensoriamento Remoto
I e II, Python GIS Session I e II, Aula 1 Ecossistema Geoespacial). Padrão observado:

- As falas curtas de outros participantes (colegas, mediadores) saem claras e corretas.
- A fala do Emanuel sai como palavras soltas em inglês, foneticamente parecidas com o que ele
  disse, sem sintaxe — não é um idioma errado, são palavras erradas. **Traduzir não resolve**
  (traduzir texto errado produz texto errado em outro idioma, não recupera a fala original).
- Termos técnicos ditos literalmente em inglês (STAC, bbox, NDVI, Python, nomes de bibliotecas)
  costumam sobreviver corretos — são a única pista de tópico aproveitável da transcrição.
- Causa provável: problema específico do áudio/microfone/conexão do Emanuel (não configuração de
  idioma da ferramenta, já que os outros participantes saem intactos). Só resolve na origem —
  ajuste técnico de captação de áudio antes da próxima aula — e está fora do escopo deste workflow.

**Regra permanente para o Comando 2**: nunca tentar reconstruir ou "traduzir" a fala do Emanuel a
partir da transcrição. Não fabricar paráfrase do que ele disse. Usar a transcrição só como pista
de qual tópico foi coberto (via termos técnicos reconhecíveis) e timestamp aproximado, e construir
o conteúdo teórico real a partir de: (1) o código/notebooks novos no upstream desde o marco zero,
e (2) conhecimento estabelecido da área de sensoriamento remoto/geoespacial — deixando sempre
explícito no review o que veio de cada fonte (ver `source-note` no HTML de referência).

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

**Atenção**: rodar cada comando git isoladamente e checar o resultado antes do próximo — nunca
encadear `checkout && reset --hard` (ou similar) num único bloco sem verificar. Um erro assim já
rodou `reset --hard` no branch errado numa sessão anterior (recuperado via `origin`, mas evitável).

## COMANDO 2 — Pós-Aula (Review em HTML e Versionamento)

**Gatilho:** a Helena cola a transcrição da aula (ou anexa docx/html) e pede o review/módulo.

**Passos:**
1. **Checar a transcrição primeiro**: ela vai vir com a fala do Emanuel ilegível (ver seção acima).
   Não tentar traduzir/reconstruir — extrair só os termos técnicos reconhecíveis como pista de tema.
2. **Análise incremental**: `git diff <marco-zero-anterior>..upstream/main` (ou equivalente) para
   ver o que mudou nos códigos/notebooks do professor desde o marco zero atual — essa é a fonte
   principal e confiável de conteúdo técnico real.
3. **Geração HTML**, seguindo o padrão de `/aprendizado/referencias/aula5sensoriamentoremoto.html`:
   - Conceitos fundamentais explicados de forma sucinta (o "recap" básico do tema, para quem não
     lembra a teoria de base — não assumir que a Helena decorou tudo)
   - Análise técnica cruzando código real com teoria
   - Insights práticos conectados a Engenharia de Produção / Back-End
   - Checklist de prática
   - Nota de transparência (`source-note`) explicando de onde veio cada parte do conteúdo
   Salvar em `/aprendizado/reviews/AAAA-MM-DD-tema.html`.
4. **Git**: `git add .` → commit descritivo com o tema da aula → `git push origin <branch atual>`.
   Nunca commitar/push direto em `main` sem confirmação explícita.

## Material de referência (contexto acumulado, não regerar do zero)

- `/aprendizado/referencias/aula5sensoriamentoremoto.html` — guia de preparação (modelo de
  formato/estilo a seguir nos próximos reviews: sidebar, cards por módulo, glossário, exercícios).
- `/aprendizado/referencias/cursocompleto.html` — compilado de todo o curso de Python p/ Dados
  Geoespaciais (Módulos I-IV: fundamentos Python, dados geoespaciais, análise espacial, casos reais).
  Consultar antes de reexplicar um conceito básico já coberto ali.
- Transcrições brutas (docx) **não são versionadas neste repositório** — contêm nomes de colegas
  de turma (dados de terceiros) e não têm conteúdo aproveitável da fala do Emanuel. Ficam só na
  conversa/anexos da sessão, não commitadas.

## Pendências conhecidas (revisar a cada sessão nova)

- Confirmar se o push precisa de um novo token (tokens configurados na VM/sessão anterior podem
  não estar disponíveis numa sessão nova — checar `git remote -v` e testar `git ls-remote origin`).
- Antes do Comando 1, sempre `git status` e `git branch --show-current` para saber em que branch
  de trabalho a Helena está, e voltar para esse branch depois de atualizar `main`.
- Se o problema de áudio do Emanuel for resolvido numa aula futura e a transcrição vier legível,
  atualizar esta seção — a limitação pode deixar de ser permanente.
