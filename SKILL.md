---
name: criar-skill
description: >
  Cria um SKILL.md completo para Claude Code a partir de qualquer workflow, sessÃ£o
  executada ou ideia nova. Acionar quando o usuÃ¡rio pedir para criar, montar, construir
  ou salvar uma skill â€” mesmo que nÃ£o use a palavra "skill". Triggers: "cria uma skill",
  "transforma isso em skill", "quero automatizar esse processo", "salva o que vocÃª
  acabou de fazer como skill", "faz uma skill de...", "como eu crio uma skill para...",
  "/criar-skill", "transforma essa sessÃ£o em skill", "quero repetir isso automaticamente".
  TambÃ©m ativa ao final de sessÃµes longas quando o usuÃ¡rio pede para capturar o que foi
  feito. NÃƒO use para executar skills existentes, criar outros tipos de arquivo,
  refatorar cÃ³digo, ou tarefas que nÃ£o sejam construÃ§Ã£o de novas skills.
---

# Skill: criar-skill

Cria um SKILL.md completo a partir de workflows existentes ou ideias novas. Roda
QA automÃ¡tico, detecta credenciais expostas, e deploya em `~/.claude/skills/` com
backup opcional no GitHub.

---

## Antes de ComeÃ§ar

Esta skill requer:
- **Claude Code** instalado e rodando (`claude` no terminal)
- Pasta `~/.claude/skills/` existindo (criada automaticamente pelo Claude Code)
- **Opcional:** GitHub CLI (`gh`) autenticado, para backup no GitHub
- **Opcional:** 1Password CLI (`op`) autenticado, para proteger credenciais

Se vocÃª nÃ£o tem `~/.claude/skills/`, rode `claude` uma vez â€” ele cria a pasta.

---

## DetecÃ§Ã£o de Modo

Antes de qualquer aÃ§Ã£o, identificar o modo com base no contexto:

| CondiÃ§Ã£o | Modo |
|----------|------|
| Invocada sem input E hÃ¡ histÃ³rico de tarefa executada na sessÃ£o | Modo 1: Captura de SessÃ£o |
| Input contÃ©m etapas numeradas, bullets de aÃ§Ã£o, ou palavras como "primeiro/depois/se/quando" | Modo 2: AnÃ¡lise de Workflow |
| Input Ã© descriÃ§Ã£o vaga, ideia ou intenÃ§Ã£o sem etapas definidas | Modo 3: Entrevista |
| AmbÃ­guo | Perguntar: "VocÃª quer capturar o que fizemos nessa sessÃ£o, analisar um workflow que vai colar, ou criar algo novo do zero?" |

---

## Modo 1: Captura de SessÃ£o

**Quando:** Invocada sem input apÃ³s execuÃ§Ã£o de tarefa longa na mesma sessÃ£o.

1. Ler o histÃ³rico completo da conversa atual.
2. Extrair trÃªs camadas:
   - **Fluxo principal:** todas as aÃ§Ãµes executadas em ordem sequencial
   - **Decision points:** condicionais identificadas (ex: "SE login expirado â†’ renovar sessÃ£o antes de prosseguir")
   - **Edge cases:** erros que ocorreram + como foram resolvidos
3. Montar rascunho do SKILL.md seguindo o template em `references/skill-anatomy.md`.
4. Apresentar o rascunho: "Identifiquei esses passos. EstÃ¡ correto? Posso ajustar qualquer etapa antes de prosseguir."
5. Aguardar confirmaÃ§Ã£o ou correÃ§Ãµes.
6. ApÃ³s confirmaÃ§Ã£o, ir para **QA AutomÃ¡tico**.

---

## Modo 2: AnÃ¡lise de Workflow

**Quando:** O usuÃ¡rio cola texto com etapas, notas ou processo descrito.

1. Identificar estrutura de processo no texto colado.
2. Extrair:
   - AÃ§Ãµes sequenciais â†’ seÃ§Ã£o Workflow
   - Condicionais â†’ decision points dentro do Workflow
   - SituaÃ§Ãµes de falha mencionadas â†’ Edge Cases
3. Se algum passo ficou ambÃ­guo, fazer no mÃ¡ximo 2 perguntas de esclarecimento antes de gerar.
4. Montar SKILL.md seguindo `references/skill-anatomy.md`.
5. Ir para **QA AutomÃ¡tico**.

---

## Modo 3: Entrevista

**Quando:** O usuÃ¡rio descreve uma ideia sem estrutura de processo definida.

### Fase 1 â€” DefiniÃ§Ã£o da tarefa
- Perguntar: "O que essa skill faz? Descreva o input e o output especÃ­fico."
- Se a resposta for vaga (ex: "ajuda com emails"), pedir: "Pode descrever um exemplo concreto? O que entra exatamente, o que sai?"
- Confirmar em 1 frase: "EntÃ£o essa skill recebe [X] e produz [Y]. Correto?"
- NÃ£o avanÃ§ar atÃ© ter confirmaÃ§Ã£o explÃ­cita.

### Fase 2 â€” Triggers
- Perguntar: "Quais sÃ£o as 5 formas diferentes que vocÃª digitaria para ativar essa skill?"
- Sugerir 3-5 frases adicionais que o usuÃ¡rio provavelmente nÃ£o mencionou.
- Perguntar: "O que NÃƒO deve ativar essa skill? Quais pedidos parecidos mas diferentes ela deve ignorar?"

### Fase 3 â€” PadrÃ£o de qualidade
- Perguntar: "Como Ã© um output perfeito? Se puder, mostre um exemplo real."
- Perguntar: "Como Ã© um output que falhou? O que vocÃª nÃ£o quer ver?"
- Perguntar: "Qual o input mais quebrado ou incompleto que essa skill poderia receber? Como deve reagir?"

### Fase 4 â€” Confirmar brief
Compilar e apresentar:
```
Nome sugerido: [kebab-case]
PropÃ³sito: [1 frase]
Triggers: [lista]
Negative boundaries: [lista]
Input: [descriÃ§Ã£o]
Output: [descriÃ§Ã£o]
Edge cases: [lista]
```
Perguntar: "EstÃ¡ correto? Posso ajustar qualquer campo antes de gerar."
ApÃ³s confirmaÃ§Ã£o, gerar SKILL.md com `references/skill-anatomy.md` e ir para **QA AutomÃ¡tico**.

---

## QA AutomÃ¡tico

Executar TODOS os 10 checks antes de mostrar o SKILL.md gerado. NÃ£o pular nenhum.

```
â–¡ 1. Nome em kebab-case? (sem espaÃ§os, sem underscores, sem maiÃºsculas)
â–¡ 2. Description com 50+ palavras, terceira pessoa, 5+ trigger phrases, negative boundaries?
â–¡ 3. Cada etapa do Workflow Ã© uma aÃ§Ã£o Ãºnica, imperativa e nÃ£o-ambÃ­gua?
â–¡ 4. Pelo menos 2 exemplos concretos com input real â†’ output real?
â–¡ 5. Edge Cases cobertos? (input faltando, ambÃ­guo, formato errado)
â–¡ 6. Output Format explicitamente definido?
â–¡ 7. Sem linguagem vaga? ("handle appropriately", "format nicely", "as needed" sÃ£o proibidos)
â–¡ 8. Negative boundaries definidos no YAML?
â–¡ 9. SKILL.md contÃ©m credencial, senha, token ou API key hardcoded?
â–¡ 10. Pelo menos 2 evals preparados para evals/evals.json? (prompt + expected_output)
```

**Score < 7:** corrigir os itens com falha automaticamente, sem mostrar ao usuÃ¡rio.
**Score â‰¥ 7:** mostrar SKILL.md com score "QA: X/10 âœ“" e listar o que ainda poderia melhorar.

### Se check 9 falhar (credencial detectada):

1. Executar `op --version` para verificar se 1Password CLI estÃ¡ disponÃ­vel.
2. **Se op disponÃ­vel:**
   - Descobrir vaults: `op vault list --format=json`
   - Se hÃ¡ mais de 1 vault: perguntar "Qual vault usar?" e listar as opÃ§Ãµes.
   - Se hÃ¡ apenas 1 vault: usar automaticamente.
   - Se credencial jÃ¡ existe no vault: substituir no SKILL.md por:
     `op item get "{NOME_DO_ITEM}" --vault "{VAULT_ESCOLHIDO}" --field credential --reveal`
   - Se nÃ£o existe: criar com `op item create --category login --vault "{VAULT_ESCOLHIDO}"`,
     depois referenciar conforme acima.
3. **Se op ausente:**
   - Salvar credencial no `.env` do projeto atual (ou `~/.claude/.env` se nÃ£o houver projeto).
   - Substituir no SKILL.md pela variÃ¡vel de ambiente: `$NOME_DA_CREDENCIAL`
   - Informar: "Salvei a credencial em .env como $NOME_DA_CREDENCIAL. NÃ£o commite esse arquivo."

---

## ApresentaÃ§Ã£o e RevisÃ£o

ApÃ³s QA aprovado:

1. Mostrar o SKILL.md completo.
2. Exibir: "QA: X/10 âœ“" e lista dos itens que poderiam melhorar (se houver).
3. Perguntar: "Quer ajustar algo antes do deploy?"
4. Aguardar aprovaÃ§Ã£o explÃ­cita antes de prosseguir.

---

## Deploy

ApÃ³s aprovaÃ§Ã£o do SKILL.md:

### Deploy Claude Code

1. Criar pasta `~/.claude/skills/{nome-da-skill}/`
2. Salvar o SKILL.md nessa pasta.
3. Criar `evals/evals.json` com os exemplos coletados durante o QA (check 10):
   ```json
   {
     "skill_name": "{nome-da-skill}",
     "evals": [
       {
         "id": 1,
         "prompt": "{input real do exemplo 1}",
         "expected_output": "{output esperado do exemplo 1}"
       },
       {
         "id": 2,
         "prompt": "{input real do exemplo 2}",
         "expected_output": "{output esperado do exemplo 2}"
       }
     ]
   }
   ```
4. Confirmar: "Deployado em ~/.claude/skills/{nome}/ (SKILL.md + evals/evals.json)"

### GitHub (opcional)

Perguntar: "Quer salvar no GitHub como backup?"

Se sim:
1. Verificar autenticaÃ§Ã£o: `gh auth status`
   - Se nÃ£o autenticado: "Execute `gh auth login` e tente novamente."
2. Perguntar: "Qual o nome do seu repositÃ³rio? (ex: {SEU_USUARIO}/skills)"
3. Verificar se repo existe: `gh repo view {REPO}`
   - Se nÃ£o existe: `gh repo create {REPO} --public`
4. Gerar SKILL-publico.md: copiar o SKILL.md removendo:
   - Paths absolutos internos (ex: `~/.claude/`, `/root/...`)
   - ReferÃªncias a vaults ou credentials especÃ­ficos
   - LÃ³gica de deploy especÃ­fica de ambiente
   - Qualquer menÃ§Ã£o a contexto pessoal (nomes de pessoas, equipe interna)
   E mantendo intactos: workflow, edge cases, QA checklist, exemplos, lÃ³gica da skill.
5. Subir SKILL-publico.md (nÃ£o o SKILL.md interno) + evals/evals.json.
6. Commit e push com mensagem: `feat: add {nome} skill`
7. Confirmar com URL do arquivo no GitHub.

---

## Refinamento PÃ³s-Deploy

**Uma skill deployada nÃ£o Ã© uma skill pronta. Ã‰ uma skill que precisa ser testada.**

ApÃ³s o deploy, rodar a skill com um input real. Quando algo sair errado, nÃ£o descarte â€” corrija a instruÃ§Ã£o no SKILL.md e teste de novo. Skills melhoram com uso.

Guia completo de refinamento com exemplo real: `references/guia-refinamento.md`

---

## Edge Cases

- **Modo 1 sem histÃ³rico relevante:** "NÃ£o encontrei um workflow executado nessa sessÃ£o. Quer colar um processo ou criar algo do zero?"
- **Skill com nome jÃ¡ existente:** "JÃ¡ existe uma skill chamada {nome}. Deseja sobrescrever, renomear ou cancelar?"
- **Workflow com mais de 50 etapas:** Sugerir dividir em 2-3 skills menores antes de gerar. Apresentar sugestÃ£o de como dividir.
- **UsuÃ¡rio cancela durante entrevista:** Criar pasta `~/.claude/skills/criar-skill/drafts/` se nÃ£o existir, salvar brief parcial em `drafts/{nome}-draft.md` e informar o caminho.
- **GitHub sem autenticaÃ§Ã£o gh CLI:** "gh CLI nÃ£o autenticado. Execute `gh auth login` e tente de novo."
- **op CLI disponÃ­vel mas nÃ£o autenticado:** "1Password CLI encontrado mas nÃ£o autenticado. Execute `op signin` ou use .env como alternativa."

---

## Exemplos

### Exemplo 1 â€” Captura de SessÃ£o

**Contexto:** Acabou de executar um workflow de emissÃ£o de NF na prefeitura.
**Input:** `/criar-skill` (sem texto adicional)

**Output esperado:**
```
Identifiquei esses passos na sessÃ£o:

## Workflow
1. Acessar portal da prefeitura
2. SE sessÃ£o expirada â†’ fazer login antes de prosseguir
3. Clicar em "Emitir NF"
4. Preencher campos: tomador (CNPJ), serviÃ§o (cÃ³digo), valor, descriÃ§Ã£o
5. Confirmar emissÃ£o â†’ aguardar processamento (5-10 segundos)
6. Baixar PDF gerado

## Edge Cases identificados
- Se portal retornar erro 503: aguardar 30s e tentar novamente
- Se CNPJ do tomador nÃ£o encontrado: verificar cadastro no sistema antes de emitir

EstÃ¡ correto? Posso ajustar qualquer etapa antes de prosseguir.
```

### Exemplo 2 â€” Modo Entrevista (ideia vaga)

**Input:** "quero criar uma skill que me ajuda a responder comentÃ¡rios no Instagram"

**Output esperado (Fase 1):**
```
Entendi que vocÃª quer automatizar respostas no Instagram.

Para montar essa skill corretamente, preciso entender o que entra e o que sai.

O que entra: vocÃª cola o comentÃ¡rio aqui e eu sugiro uma resposta?
Ou vocÃª quer que eu acesse o Instagram diretamente e responda?
```

### Exemplo 3 â€” Modo AnÃ¡lise de Workflow

**Input:** "toda semana eu: 1) acesso o notion, 2) pego os cards marcados como 'publicar', 3) exporto o texto, 4) formato pra linkedin, 5) agende no buffer"

**Output esperado:**
```
Identifiquei um workflow de publicaÃ§Ã£o semanal com 5 etapas.

Antes de gerar, 2 perguntas rÃ¡pidas:

1. No passo 4 (formatar pro LinkedIn): vocÃª segue algum padrÃ£o especÃ­fico de formato? (ex: tamanho mÃ¡ximo, emojis, estrutura de parÃ¡grafos)
2. O agendamento no Buffer Ã© manual ou vocÃª quer que a skill faÃ§a via API?
```


---
*Créditos originais da metodologia: [Bruno Okamoto](https://github.com/okjpg)*
