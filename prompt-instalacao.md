# Prompt de InstalaÃ§Ã£o â€” skill-creator

Copie e cole este prompt no seu agente (Claude Code, Telegram, ou qualquer interface de IA) para instalar a skill-creator automaticamente.

---

## Prompt de instalaÃ§Ã£o completa

```
Quero instalar a skill-creator no meu Claude Code. Siga os passos abaixo:

1. Verificar se Claude Code estÃ¡ instalado
   - Rodar: claude --version
   - Se nÃ£o estiver instalado: informar que preciso instalar em claude.ai/code antes de continuar

2. Criar a estrutura de pastas da skill
   - Criar: ~/.claude/skills/criar-skill/
   - Criar: ~/.claude/skills/criar-skill/references/
   - Criar: ~/.claude/skills/criar-skill/evals/

3. Baixar os arquivos do repositÃ³rio okjpg/skill-creator
   - OpÃ§Ã£o A (com git): git clone https://github.com/okjpg/skill-creator /tmp/skill-creator-tmp
   - OpÃ§Ã£o B (com curl, arquivo por arquivo):
     curl -s https://raw.githubusercontent.com/okjpg/skill-creator/main/SKILL.md -o ~/.claude/skills/criar-skill/SKILL.md
     curl -s https://raw.githubusercontent.com/okjpg/skill-creator/main/references/skill-anatomy.md -o ~/.claude/skills/criar-skill/references/skill-anatomy.md
     curl -s https://raw.githubusercontent.com/okjpg/skill-creator/main/references/guia-refinamento.md -o ~/.claude/skills/criar-skill/references/guia-refinamento.md
     curl -s https://raw.githubusercontent.com/okjpg/skill-creator/main/evals/evals.json -o ~/.claude/skills/criar-skill/evals/evals.json

4. Se usou OpÃ§Ã£o A (git clone), copiar os arquivos para o lugar certo:
   cp /tmp/skill-creator-tmp/SKILL.md ~/.claude/skills/criar-skill/SKILL.md
   cp /tmp/skill-creator-tmp/references/skill-anatomy.md ~/.claude/skills/criar-skill/references/
   cp /tmp/skill-creator-tmp/references/guia-refinamento.md ~/.claude/skills/criar-skill/references/
   cp /tmp/skill-creator-tmp/evals/evals.json ~/.claude/skills/criar-skill/evals/
   rm -rf /tmp/skill-creator-tmp

5. Verificar que os arquivos estÃ£o no lugar:
   ls ~/.claude/skills/criar-skill/
   ls ~/.claude/skills/criar-skill/references/
   ls ~/.claude/skills/criar-skill/evals/

6. Confirmar a instalaÃ§Ã£o rodando:
   cat ~/.claude/skills/criar-skill/SKILL.md | head -5

7. Informar: "âœ“ skill-creator instalada em ~/.claude/skills/criar-skill/ â€” use /criar-skill para comeÃ§ar"

Se qualquer passo falhar: mostrar o erro exato e sugerir o que fazer.
```

---

## Prompt de teste pÃ³s-instalaÃ§Ã£o

Depois de instalar, use este prompt para testar se a skill estÃ¡ funcionando:

```
Quero testar a skill-creator.

Crie uma skill simples chamada "ola-mundo" que, quando eu digitar "oi" ou "olÃ¡", responda com uma saudaÃ§Ã£o personalizada usando meu nome.

Use o modo entrevista (/criar-skill) para criar essa skill do zero.
```

---

## Prompt de criaÃ§Ã£o rÃ¡pida (Modo 2 â€” workflow colado)

Use quando vocÃª jÃ¡ tem um processo descrito:

```
/criar-skill

Meu processo toda semana:
1. Acesso o [FERRAMENTA] e exporto o relatÃ³rio de [TIPO]
2. Abro no Excel e filtro por [CRITÃ‰RIO]
3. Copio os dados e formato num email no padrÃ£o: assunto "[ASSUNTO]", destinatÃ¡rios [LISTA]
4. Reviso e envio

Quero automatizar os passos 2 e 3.
```

Substitua os valores entre colchetes pelo seu processo real antes de enviar.

---

## Prompt de captura de sessÃ£o (Modo 1 â€” pÃ³s-execuÃ§Ã£o)

Use depois de executar qualquer tarefa longa no Claude Code:

```
/criar-skill

Acabei de executar esse processo com vocÃª. Captura tudo que fizemos nessa sessÃ£o e transforma em uma skill para eu poder repetir sempre que precisar.
```

---

## Prompt para Telegram (agente configurado com Claude)

Se vocÃª usa um bot no Telegram conectado ao Claude, envie:

```
Instala a skill skill-creator no meu Claude Code.
RepositÃ³rio: https://github.com/okjpg/skill-creator
Segue as instruÃ§Ãµes do arquivo prompt-instalacao.md
```

O agente vai executar os passos de instalaÃ§Ã£o via terminal no seu computador (requer que o agente tenha acesso ao shell local ou via SSH).

---

## SoluÃ§Ã£o de problemas comuns

**"claude: command not found"**
Claude Code nÃ£o estÃ¡ instalado. Acesse claude.ai/code para instalar.

**"Permission denied" ao criar ~/.claude/skills/"**
Rodar com permissÃ£o correta:
```bash
mkdir -p ~/.claude/skills/criar-skill/references
chmod 755 ~/.claude/skills/criar-skill
```

**"curl: command not found"**
Usar wget como alternativa:
```bash
wget -q https://raw.githubusercontent.com/okjpg/skill-creator/main/SKILL.md -O ~/.claude/skills/criar-skill/SKILL.md
```

**A skill nÃ£o ativa quando digito /criar-skill**
Fechar e reabrir o Claude Code â€” skills sÃ£o carregadas na inicializaÃ§Ã£o.


---
*Créditos originais da metodologia: [Bruno Okamoto](https://github.com/okjpg)*
