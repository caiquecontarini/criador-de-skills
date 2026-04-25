# Guia de Refinamento PÃ³s-Deploy

**Uma skill deployada nÃ£o Ã© uma skill pronta. Ã‰ uma skill que precisa ser testada.**

O `/criar-skill` gera o melhor SKILL.md possÃ­vel com as informaÃ§Ãµes disponÃ­veis. Mas algumas coisas sÃ³ aparecem quando a skill roda de verdade â€” respostas de API inesperadas, tom que fica levemente errado, comportamento que faz sentido no papel mas falha na prÃ¡tica.

Isso Ã© normal. Ã‰ o processo.

---

## O que testar apÃ³s o deploy

Rodar a skill com um input real e avaliar cada etapa:

1. **A entrada funcionou?** A skill recebeu os dados do jeito esperado (URL, texto, arquivo)?
2. **O processamento gerou o resultado certo?** A saÃ­da tem o conteÃºdo correto, no formato correto, no tom correto?
3. **As integraÃ§Ãµes funcionaram?** APIs responderam como esperado? Os dados foram salvos onde deveriam?
4. **O resultado parece genÃ©rico?** Se qualquer pessoa poderia ter produzido aquela saÃ­da, a skill nÃ£o estÃ¡ usando os diferenciais que vocÃª definiu.

---

## Como refinar quando algo sair errado

Quando um resultado ficar ruim, nÃ£o descarte â€” **documente o erro e corrija a instruÃ§Ã£o**:

- **Resultado genÃ©rico/abstrato:** Adicionar exemplos concretos na seÃ§Ã£o de geraÃ§Ã£o. A skill precisa de Ã¢ncoras reais, nÃ£o sÃ³ regras.
- **Tom errado:** Incluir referÃªncia a exemplos reais do seu conteÃºdo publicado. O guia analÃ­tico descreve o estilo â€” os exemplos demonstram.
- **IntegraÃ§Ã£o com formato inesperado:** Testar a API, documentar o campo correto no arquivo de configuraÃ§Ã£o, corrigir o cÃ³digo.
- **Comportamento incompleto:** Adicionar um edge case especÃ­fico para aquela situaÃ§Ã£o.

---

## Exemplo real: refinamento da skill /linkedin-posts

Essa skill passou por 3 ciclos de refinamento apÃ³s o deploy:

**Ciclo 1 â€” Tom genÃ©rico**
Posts gerados soavam como conteÃºdo de consultor de marketing, nÃ£o como o autor.
Causa: a skill lia o guia analÃ­tico de voz, mas nÃ£o os posts reais como exemplos.
CorreÃ§Ã£o: incluir instruÃ§Ã£o explÃ­cita de ler exemplos reais (posts publicados) como calibraÃ§Ã£o antes de gerar.

**Ciclo 2 â€” Posts curtos, sem storytelling**
Posts tinham as regras de formataÃ§Ã£o certas (frases curtas, setas, sem emojis) mas faltava a narrativa.
Causa: as regras diziam O QUE evitar, mas nÃ£o descreviam a estrutura narrativa esperada.
CorreÃ§Ã£o: adicionar instruÃ§Ã£o explÃ­cita de estrutura: hook â†’ contexto pessoal â†’ jornada â†’ insight â†’ fechamento.

**Ciclo 3 â€” API com formato diferente do documentado**
O campo para extrair o ID do bloco criado era `items[0].id`, nÃ£o `blocks[0].pageId` como estava documentado.
Causa: documentaÃ§Ã£o escrita antes de testar a API em produÃ§Ã£o.
CorreÃ§Ã£o: testar a chamada real, documentar o campo correto no arquivo de configuraÃ§Ã£o.

**ConclusÃ£o:** a skill ficou boa depois de 3 rodadas de teste â†’ erro â†’ correÃ§Ã£o â†’ reteste. NÃ£o em uma.

---

## Mentalidade certa

Skills sÃ£o como receitas: na primeira vez que vocÃª tenta, descobre o que precisa ajustar. Na segunda, vocÃª sabe exatamente o que fazer. Na terceira, estÃ¡ no automÃ¡tico.

Cada correÃ§Ã£o que vocÃª documenta no SKILL.md torna a skill mais robusta para sempre â€” nÃ£o sÃ³ para a prÃ³xima vez que vocÃª usar, mas para qualquer pessoa da equipe que usar depois.


---
*Créditos originais da metodologia: [Bruno Okamoto](https://github.com/okjpg)*
