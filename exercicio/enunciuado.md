# Atividade 1 — Auditoria e Reforma WCAG: Cardápio Digital Acessível

**Eixo da disciplina:** Aula 3 — WCAG (Web Content Accessibility Guidelines) e princípios POUR
**Formato:** individual ou em dupla
**Ferramentas:** navegador + editor de código (sem build, sem instalação)

---

## 1. Introdução

O "Café Ponto" contratou vocês para revisar o cardápio digital do site. A página **funciona** abre,
mostra os pratos, tem um formulário de reserva mas foi construída sem nenhuma preocupação com
acessibilidade. Ela vai reprovar em qualquer auditoria WCAG.

O trabalho de vocês tem duas partes:

1. **Auditar** o código em `starter/index.html` e `starter/styles.css`, encontrando violações dos
   princípios **POUR** (Perceptível, Operável, Compreensível, Robusto).
2. **Reformar** o código para que a página atenda, no mínimo, ao **Nível AA** da WCAG 2.1/2.2.

Este guia não contém o código de solução. Ele explica o que vocês precisam observar, testar e decidir
a implementação exata (como nomear uma classe CSS, que abordagem de HTML usar) é escolha de vocês, desde
que o resultado final seja acessível.

## 2. Objetivos de aprendizagem

Ao final desta atividade, vocês devem ser capazes de:

- Relacionar um problema concreto de código a um dos quatro princípios POUR.
- Diferenciar, na prática, o que é exigido no Nível A do que é exigido no Nível AA.
- Corrigir problemas comuns de acessibilidade em HTML e CSS sem depender de bibliotecas ou frameworks.
- Testar uma página **sem usar o mouse** e **sem depender só da visão** para validar acessibilidade.

## 3. Recapitulando (rápido)

- **Perceptível**: a informação precisa poder ser percebida por mais de um sentido (texto alternativo,
  contraste, não depender só de cor).
- **Operável**: tudo que se faz com mouse precisa também ser possível com teclado; o usuário precisa de
  tempo e não pode ser exposto a conteúdo perigoso (piscar, etc.).
- **Compreensível**: texto legível, comportamento prev­isível, ajuda para evitar e corrigir erros.
- **Robusto**: o código precisa ser interpretado corretamente por diferentes agentes de usuário, incluindo
  tecnologia assistiva.
- **Nível A** = mínimo aceitável. **Nível AA** = o padrão que a maioria das leis e políticas exige (é a
  meta desta atividade). **Nível AAA** = ótimo, mas não é exigido de um site inteiro.

Se alguma dessas ideias não estiver clara, volte aos slides da Aula 3 antes de continuar a atividade
pressupõe que vocês reconhecem esses termos.

## 4. Parte 1 — Auditoria

Abram `starter/index.html` no navegador e no editor. Antes de mudar qualquer linha de código, façam uma
auditoria por escrito. Criem uma tabela (pode ser no início do próprio `index.html`, como comentário, ou em
um arquivo `auditoria.md` separado decisão de vocês) com estas colunas:

| # | Onde está (elemento/trecho) | O que está errado | Qual princípio POUR viola | Critério(s) WCAG (nº) | Por que isso importa para um usuário real |
|---|------------------------------|--------------------|-----------------------------|--------------------------|----------------------------------------------|

Ao preencher a coluna de princípio POUR, tentem também anotar o **número do critério de sucesso da WCAG**
que parece se aplicar (ex.: 1.1.1, 2.4.7). Vocês não precisam decorar os números no final deste guia
(Seção 9, "Anexo") há uma lista com os critérios relevantes para esta atividade e o link oficial de cada
um. Consultem essa lista, leiam a definição e as técnicas sugeridas pelo W3C, e usem isso para decidir como
corrigir cada problema.

Para encontrar os problemas, não leiam só o código **usem a página** das seguintes formas:

- Tentem navegar por toda a página **usando apenas a tecla Tab** (e Shift+Tab para voltar), sem tocar no
  mouse. Anotem tudo que não conseguirem alcançar ou que "some" visualmente.
- Aumentem o zoom do navegador para 200% (Ctrl/Cmd + "+"). O texto continua legível? Algo quebra?
- Olhem o formulário de reserva: cliquem em cada campo e perguntem "se eu não estivesse vendo a tela,
  como eu saberia o que preencher aqui?"
- Reparem nas cores: o texto cinza-claro sobre fundo branco é confortável de ler? E a etiqueta
  "vegetariano" — como alguém que não distingue verde de vermelho saberia que um prato é vegetariano?

Esperamos entre **10 e 15 problemas** encontrados. Se vocês pararem em 4 ou 5, olhem de novo com mais
atenção eles estão distribuídos por HTML e CSS, em diferentes partes da página.

## 5. Parte 2 — Reforma guiada

Trabalhem etapa por etapa. Cada etapa tem uma pergunta norteadora — respondam a ela antes de escrever
código, mesmo que mentalmente.

### Etapa 1 — Idioma e estrutura do documento

*Pergunta norteadora:* como um leitor de tela sabe em qual idioma pronunciar o conteúdo da página?

- Verifiquem a tag `<html>`. Falta um atributo obrigatório para acessibilidade e SEO.
- Verifiquem se existe uma estrutura de landmarks (`<header>`, `<nav>`, `<main>`, `<footer>`) ou se tudo
  está dentro de `<div>` genéricas. Elementos semânticos ajudam tecnologia assistiva a "pular" para a
  parte da página que interessa ao usuário.

### Etapa 2 — Hierarquia de cabeçalhos

*Pergunta norteadora:* um usuário de leitor de tela costuma navegar pulando de heading em heading (tecla H
no NVDA/JAWS, VO+Cmd+H no VoiceOver). O que ele encontraria se a hierarquia estiver fora de ordem?

- Percorram todos os `<h1>` a `<h6>` da página na ordem em que aparecem no código. Existe exatamente um
  `<h1>`? A hierarquia pula níveis (ex.: de `<h1>` direto para `<h4>`)?
- Corrijam a hierarquia. Lembrem-se: nível de heading é sobre **estrutura do documento**, não sobre
  "tamanho da fonte que eu quero". Se um texto precisa parecer menor ou maior, isso é CSS, não é motivo
  para pular um nível de heading.

### Etapa 3 — Alternativas textuais para conteúdo não textual

*Pergunta norteadora:* se as imagens não carregassem, o visitante entenderia o que está perdendo?

- Cada `<img>` do cardápio precisa de um atributo `alt`. Decidam: essa imagem é **informativa** (o texto
  alternativo deve descrever o conteúdo relevante) ou é puramente **decorativa** (nesse caso, `alt=""`
  vazio é o correto, para que o leitor de tela a ignore)?
- Evitem texto alternativo redundante como `alt="imagem"` ou que apenas repita o nome do arquivo.

### Etapa 4 — Contraste de cores

*Pergunta norteadora:* qual é a razão de contraste mínima exigida pela WCAG AA para texto normal? E para
texto grande?

- Usem uma ferramenta de verificação de contraste (ex.: WebAIM Contrast Checker, ou o próprio inspetor de
  cores do DevTools do navegador, que já mostra a razão de contraste ao inspecionar um elemento de texto).
- Encontrem todos os pares de cor texto/fundo que não atingem 4.5:1 (texto normal) ou 3:1 (texto grande,
  ≥18pt ou ≥14pt em negrito) e ajustem os valores no CSS.
- Atenção: aumentar o contraste não pode "descaracterizar" a identidade visual a ponto de virar preto
  sobre branco em tudo o objetivo é achar tons que sejam esteticamente aceitáveis **e** cumpram a razão
  mínima.

### Etapa 5 — Foco visível e operação por teclado

*Pergunta norteadora:* como alguém que não usa mouse sabe **onde está** na página?

- Procurem no CSS por alguma regra que remova o contorno de foco (algo que zera o indicador visual de
  foco em elementos interativos). Por que isso é um problema grave de acessibilidade, mesmo que deixe a
  página "mais bonita"?
- O botão "Adicionar ao carrinho" foi implementado com uma `<div>` e um evento de clique. Um `<div>` não
  recebe foco de teclado nem é anunciado como botão por um leitor de tela. Substituam por um elemento
  HTML que já nasce com essa semântica e esse comportamento — vocês não precisam adicionar `tabindex` nem
  `role` manualmente se escolherem o elemento certo.
- Depois de corrigir, testem: dá para "clicar" nesse botão só com o teclado (Tab até ele, Enter ou
  Espaço)?

### Etapa 6 — Formulários acessíveis

*Pergunta norteadora:* um placeholder desaparece assim que o usuário começa a digitar. Isso é suficiente
como identificação do campo?

- O formulário de reserva usa apenas `placeholder` como pista do que preencher, sem `<label>`. Associem
  cada campo a um rótulo persistente e programaticamente ligado a ele (existe mais de uma forma correta de
  fazer essa associação em HTML pesquisem as opções e escolham uma, justificando na auditoria por quê).

### Etapa 7 — Não dependam só de cor

*Pergunta norteadora:* peguem o asterisco vermelho ao lado de "Seu nome*" no formulário. Se a página fosse
impressa em preto e branco, ou vista por alguém com daltonismo, ainda ficaria claro que aquele campo é
obrigatório?

- O campo obrigatório hoje é indicado só pela cor vermelha do asterisco. Adicionem um segundo indicador que
  não dependa de cor (um texto persistente como "obrigatório", por exemplo) para qualquer informação que
  hoje é comunicada só por cor.
- Aproveitem e revisem também a etiqueta "vegetariano" dos pratos: ela já tem texto (então não é um
  problema de "só cor"), mas o tom de verde usado tem contraste suficiente contra o fundo branco? Testem
  com a ferramenta de contraste da Etapa 4.

### Etapa 8 — Textos de link e propósito claro

*Pergunta norteadora:* se alguém usasse um leitor de tela para listar **todos os links da página** fora de
contexto (isso é uma funcionalidade real de leitores de tela), os textos fariam sentido sozinhos?

- Encontrem o link com texto "clique aqui" e reescrevam para que o texto do link, isolado, já diga para
  onde ele leva ou o que ele faz.

### Etapa 9 — Desafio opcional (rumo ao AAA)

Sem ser exigido, tentem melhorar mais um destes pontos e documentem a decisão na auditoria:

- Textos muito pequenos (verifiquem se algum `font-size` está fixo em pixels muito baixos e se o texto
  continua confortável de ler ao redimensionar a janela).
- Uso de unidades relativas (`rem`/`em`) em vez de `px` fixo para tamanho de fonte, permitindo que o
  usuário ajuste o tamanho do texto pelas configurações do navegador/SO sem quebrar o layout.

## 6. Como testar o resultado final

Antes de considerar a atividade pronta, rodem este checklist:

- [ ] Naveguei a página inteira só com o teclado (Tab, Shift+Tab, Enter, Espaço) e nunca "perdi" o foco.
- [ ] Todo elemento com foco tem um indicador visual claro.
- [ ] Testei com zoom de 200% e nada quebrou ou ficou ilegível.
- [ ] Rodei um verificador de contraste em todos os pares texto/fundo relevantes.
- [ ] Toda imagem informativa tem `alt` descritivo; toda imagem decorativa tem `alt=""`.
- [ ] Todo campo de formulário tem um `<label>` associado.
- [ ] Nenhuma informação depende só de cor.
- [ ] Rodei a aba **Lighthouse** (DevTools do Chrome/Edge, categoria Accessibility) ou a extensão **axe
      DevTools** e revisei os apontamentos não precisa zerar 100%, mas expliquem na entrega qualquer
      apontamento que decidiram não corrigir e por quê.
- [ ] (Bônus) Se tiverem acesso, ativem um leitor de tela nativo (Narrador no Windows, VoiceOver no Mac) e
      tentem entender o cardápio e preencher a reserva só ouvindo.

## 7. Autoavaliação

Antes de entregar, respondam por escrito (algumas frases bastam):

1. Qual foi o problema mais difícil de perceber sozinho, sem ferramenta? Por quê?
2. Escolham uma correção que vocês fizeram e expliquem a qual critério de conformidade (Nível A ou AA) ela
   provavelmente se relaciona.
3. Se tivessem mais tempo, o que fariam para chegar perto do Nível AAA?

## 8. Entrega

- `index.html` e `styles.css` reformados.
- A tabela de auditoria da Parte 1.
- As respostas da autoavaliação (Seção 7).

## 9. Anexo — Critérios WCAG desta atividade (para consulta)

Estes são os critérios de sucesso da WCAG 2.2 relacionados aos tipos de problema presentes nesta
atividade. Cliquem no link para ler a definição oficial e, principalmente, a aba **"How to Meet"** de cada
critério na página do W3C ela lista técnicas concretas de como corrigir. O critério exato que se aplica a
cada problema que vocês encontraram é algo que vocês precisam decidir; esta lista é o ponto de partida da
consulta, não o gabarito.

| Critério | Nome | Nível | Link |
|---|---|---|---|
| 1.1.1 | Conteúdo não textual (Non-text Content) | A | https://www.w3.org/WAI/WCAG22/quickref/#non-text-content |
| 1.3.1 | Informações e relações (Info and Relationships) | A | https://www.w3.org/WAI/WCAG22/quickref/#info-and-relationships |
| 1.4.1 | Uso de cor (Use of Color) | A | https://www.w3.org/WAI/WCAG22/quickref/#use-of-color |
| 1.4.3 | Contraste mínimo (Contrast Minimum) | AA | https://www.w3.org/WAI/WCAG22/quickref/#contrast-minimum |
| 1.4.4 | Redimensionar texto (Resize Text) | AA | https://www.w3.org/WAI/WCAG22/quickref/#resize-text |
| 2.1.1 | Teclado (Keyboard) | A | https://www.w3.org/WAI/WCAG22/quickref/#keyboard |
| 2.4.4 | Propósito do link, em contexto (Link Purpose In Context) | A | https://www.w3.org/WAI/WCAG22/quickref/#link-purpose-in-context |
| 2.4.6 | Cabeçalhos e rótulos (Headings and Labels) | AA | https://www.w3.org/WAI/WCAG22/quickref/#headings-and-labels |
| 2.4.7 | Foco visível (Focus Visible) | AA | https://www.w3.org/WAI/WCAG22/quickref/#focus-visible |
| 3.1.1 | Idioma da página (Language of Page) | A | https://www.w3.org/WAI/WCAG22/quickref/#language-of-page |
| 3.3.2 | Rótulos ou instruções (Labels or Instructions) | A | https://www.w3.org/WAI/WCAG22/quickref/#labels-or-instructions |
| 4.1.2 | Nome, função, valor (Name, Role, Value) | A | https://www.w3.org/WAI/WCAG22/quickref/#name-role-value |

## 10. Referências úteis

- WCAG 2.2 Quick Reference (lista completa, com filtro por nível): https://www.w3.org/WAI/WCAG22/quickref/
- WebAIM Contrast Checker: https://webaim.org/resources/contrastchecker/
- MDN — Acessibilidade: https://developer.mozilla.org/pt-BR/docs/Web/Accessibility
- MDN — Formulários acessíveis: https://developer.mozilla.org/pt-BR/docs/Learn/Forms/How_to_structure_a_web_form