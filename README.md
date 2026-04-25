# skill-creator

Uma skill para Claude Code e agentes de IA que cria outras skills automaticamente.

VocÃª descreve um processo. O agente transforma em uma skill estruturada, passa por QA automÃ¡tico, e faz o deploy em `~/.claude/skills/` â€” pronta para usar.

---

## O que Ã© uma skill?

Uma skill Ã© um arquivo de instruÃ§Ã£o (SKILL.md) que vocÃª coloca no Claude Code para automatizar tarefas repetitivas. Em vez de explicar o que fazer toda vez, vocÃª escreve uma vez e o Claude executa sempre que vocÃª acionar.

Exemplos de skills que vocÃª pode criar com esse repositÃ³rio:
- Skill de geraÃ§Ã£o de posts LinkedIn a partir de vÃ­deos
- Skill de emissÃ£o de notas fiscais
- Skill de anÃ¡lise de concorrentes
- Skill de responder comentÃ¡rios com seu tom de voz
- Qualquer processo que vocÃª repete toda semana

---

## Como instalar

### Uma linha

```bash
curl -fsSL https://raw.githubusercontent.com/okjpg/skill-creator/main/install.sh | bash
```

O script detecta automaticamente se vocÃª tem Claude Code, OpenClaw, ou ambos â€” e instala em todos os ambientes encontrados.

Depois da instalaÃ§Ã£o, abra seu agente e digite:
```
/criar-skill
```

### Telegram (via agente de IA)

Copie o conteÃºdo de `prompt-instalacao.md` e envie para o seu agente no Telegram. O agente vai executar a instalaÃ§Ã£o automaticamente.

---

## Como usar

### Wizard visual (recomendado para comeÃ§ar)

ApÃ³s instalar, o wizard abre automaticamente no browser. Se precisar abrir de novo:

```bash
open ~/.claude/skills/criar-skill/wizard.html
```

O wizard guia vocÃª por 4 passos e gera o SKILL.md + evals.json prontos para copiar. TambÃ©m tem uma [biblioteca com 24 exemplos de skills](examples.html) para usar como base.

### 3 formas de criar uma skill (via agente)

**Modo 1 â€” Capturar o que vocÃª acabou de fazer**

VocÃª acabou de executar um processo no Claude. Digita:
```
/criar-skill
```
O agente lÃª o histÃ³rico da sessÃ£o, identifica os passos, e gera a skill automaticamente.

**Modo 2 â€” Colar um workflow existente**

VocÃª tem um processo descrito em texto (notas, Notion, papel). Cola no chat:
```
/criar-skill

Todo dia eu: 1) acesso o painel, 2) exporto o relatÃ³rio, 3) formato e mando por email
```
O agente extrai os passos e gera a skill.

**Modo 3 â€” Descrever uma ideia**

VocÃª tem uma ideia vaga do que quer automatizar:
```
/criar-skill quero algo que me ajude a responder comentÃ¡rios no Instagram
```
O agente faz perguntas para entender o que entra, o que sai, e como Ã© um resultado perfeito. Depois gera.

---

## O que acontece depois que vocÃª digita /criar-skill

```
VocÃª descreve â†’  Claude estrutura o workflow
                    â†“
              QA automÃ¡tico (10 checks)
                    â†“
              VocÃª revisa e aprova
                    â†“
              Deploy em ~/.claude/skills/
              (SKILL.md + evals/evals.json)
                    â†“
              Skill pronta para usar
```

O QA automÃ¡tico verifica:
- Nome no formato correto (kebab-case)
- DescriÃ§Ã£o com triggers suficientes para o Claude entender quando ativar
- Cada passo do workflow Ã© claro e imperativo
- Exemplos reais de input e output
- Edge cases cobertos
- Sem credenciais expostas no arquivo
- Evals preparados para teste (prompt + output esperado)

---

## Estrutura de uma skill gerada

Toda skill criada pelo `/criar-skill` gera no mÃ­nimo:

```
skill-name/
â”œâ”€â”€ SKILL.md              # Workflow, regras, edge cases
â””â”€â”€ evals/
    â””â”€â”€ evals.json        # Casos de teste (prompt + expected_output)
```

O `evals.json` permite testar se a skill estÃ¡ funcionando corretamente apÃ³s refinamentos e serve como documentaÃ§Ã£o de uso para quem instalar a skill.

Opcionais (quando necessÃ¡rio):
- `references/` â€” docs de apoio, specs, guias de estilo
- `scripts/` â€” cÃ³digo executÃ¡vel (ex: geraÃ§Ã£o de imagem, PDF)
- `assets/` â€” fontes, templates, arquivos estÃ¡ticos

---

## Skills nÃ£o saem perfeitas na primeira vez

Isso Ã© esperado e normal.

Depois do deploy, rode a skill com um input real. Quando algo sair errado, identifique a causa e corrija a instruÃ§Ã£o no SKILL.md. Skills melhoram com uso â€” cada refinamento que vocÃª documenta torna a skill mais robusta para sempre.

Guia completo: `references/guia-refinamento.md`

---

## Estrutura do repositÃ³rio

```
skill-creator/
â”œâ”€â”€ SKILL.md                        # A skill em si (instalar aqui)
â”œâ”€â”€ wizard.html                     # Wizard visual (abrir no browser)
â”œâ”€â”€ examples.html                   # Biblioteca com 24 exemplos de skills
â”œâ”€â”€ bruno-photo.jpeg                # Foto do autor
â”œâ”€â”€ README.md                       # Este arquivo
â”œâ”€â”€ LICENSE                         # MIT
â”œâ”€â”€ install.sh                      # Instalador automÃ¡tico
â”œâ”€â”€ prompt-instalacao.md            # Prompt para configurar via agente
â”œâ”€â”€ evals/
â”‚   â””â”€â”€ evals.json                  # Casos de teste do prÃ³prio skill-creator
â””â”€â”€ references/
    â”œâ”€â”€ skill-anatomy.md            # Template de anatomia de uma skill
    â””â”€â”€ guia-refinamento.md         # Guia de refinamento pÃ³s-deploy
```

### Wizard visual

Se preferir uma interface visual, abra o `wizard.html` no browser:

```bash
open wizard.html
```

O wizard guia vocÃª por 4 passos e gera o SKILL.md + evals.json prontos para copiar ou baixar. Funciona 100% offline, sem servidor.

---

## Requisitos

- [Claude Code](https://claude.ai/code) instalado
- Opcional: [GitHub CLI](https://cli.github.com/) para backup das skills no GitHub
- Opcional: [1Password CLI](https://developer.1password.com/docs/cli/) para proteger credenciais

---

## Feito por

Bruno Okamoto Â· SaaS, IA & Empreendedorismo

[LinkedIn](https://linkedin.com/in/obrunookamoto) Â· [YouTube](https://youtube.com/@obrunookamoto) Â· [Instagram](https://instagram.com/obrunookamoto)


---
*Créditos originais da metodologia: [Bruno Okamoto](https://github.com/okjpg)*
