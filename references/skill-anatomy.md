# Anatomia de um SKILL.md (Claude Code)

## Estrutura obrigatÃ³ria

```
---
name: kebab-case-sem-espacos
description: >
  [50+ palavras. Terceira pessoa ("Processa...", nÃ£o "Eu processo...").
  5+ trigger phrases explÃ­citas que o usuÃ¡rio poderia digitar.
  Negative boundaries: "NÃƒO use para X, Y, Z."
  Quanto mais especÃ­fico, melhor.]
---

# [Nome da Skill]

[ParÃ¡grafo de Overview: o que a skill faz e quando ativa â€” escrito para o Claude, nÃ£o para humano]

## Workflow
1. [AÃ§Ã£o imperativa especÃ­fica â€” "Ler o arquivo X" nÃ£o "O arquivo X deve ser lido"]
2. [SE condiÃ§Ã£o â†’ aÃ§Ã£o alternativa]
3. [PrÃ³xima aÃ§Ã£o]
...

## Output Format
[Estrutura exata do output: tipo de arquivo, seÃ§Ãµes, formataÃ§Ã£o, tom]

## Edge Cases
- Se [condiÃ§Ã£o especÃ­fica]: [aÃ§Ã£o especÃ­fica]
- Se [input faltando]: [o que fazer]
- Se [formato errado]: [o que fazer]

## Examples

### Exemplo 1 (happy path)
Input: [exemplo real e concreto]
Output: [exemplo real e concreto do que Claude deve produzir]

### Exemplo 2 (edge case)
Input: [exemplo de input quebrado ou incomum]
Output: [como Claude deve reagir]
```

## Regras de qualidade

**YAML description:**
- Terceira pessoa obrigatÃ³ria
- MÃ­nimo 5 trigger phrases explÃ­citas
- Negative boundaries sempre presentes
- "Pushy": quanto mais especÃ­fico, maior a chance de ativar corretamente

**Workflow:**
- Cada passo = uma Ãºnica aÃ§Ã£o
- Voz imperativa: "Ler", "Extrair", "Perguntar" â€” nÃ£o "Deve ser lido", "Ã‰ necessÃ¡rio extrair"
- Condicionais explÃ­citas: "SE [condiÃ§Ã£o] â†’ [aÃ§Ã£o]"
- Sem linguagem vaga: banido "handle appropriately", "format nicely", "as needed", "quando relevante"

**Exemplos:**
- Input e output REAIS, nÃ£o descriÃ§Ãµes abstratas
- Pelo menos 1 edge case
- Quanto mais concretos, melhor o comportamento

## Tamanho ideal
- 100 a 300 linhas
- Acima de 300: considerar dividir em mÃºltiplas skills ou mover conteÃºdo para references/

## Estrutura de pastas

Uma skill completa tem no mÃ­nimo:

```
skill-name/
  â”œâ”€â”€ SKILL.md              â† workflow, regras, edge cases
  â””â”€â”€ evals/
      â””â”€â”€ evals.json        â† casos de teste (prompt + expected_output)
```

O `evals/evals.json` segue este formato:

```json
{
  "skill_name": "nome-da-skill",
  "evals": [
    {
      "id": 1,
      "prompt": "input real que o usuÃ¡rio digitaria",
      "expected_output": "descriÃ§Ã£o concreta do que a skill deve produzir"
    }
  ]
}
```

MÃ­nimo 2 evals por skill: 1 happy path + 1 edge case.

Opcionais (sÃ³ quando necessÃ¡rio):
- `references/` â€” docs de apoio, specs, guias de estilo
- `scripts/` â€” cÃ³digo executÃ¡vel (ex: geraÃ§Ã£o de imagem, PDF)
- `assets/` â€” fontes, templates, arquivos estÃ¡ticos


---
*Créditos originais da metodologia: [Bruno Okamoto](https://github.com/okjpg)*
