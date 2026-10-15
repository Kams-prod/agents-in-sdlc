---
title: "Exercício 1 - Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente de nuvem do Copilot →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica — ou se há informações importantes que o Copilot deve conhecer —, garanta que esse contexto esteja acessível. Os [arquivos de instruções][instruction-files] fornecem essa orientação para que o Copilot entenda não apenas *o que* você quer, mas também *como* o trabalho deve ser feito.

Neste exercício, você vai:

- explorar como contexto específico do projeto, diretrizes de codificação e padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e arquivos de instrução com escopo definido por caminho.
- adicionar um novo padrão geral ao arquivo `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos roteiros do VS Code e do CLI, aqui você não fará uma solicitação *antes e depois*. O agente de nuvem do Copilot trabalha de forma assíncrona com issues do GitHub, então é mais difícil demonstrar o impacto lado a lado em tempo real. Você verá a influência do arquivo de instruções mais adiante neste roteiro, ao revisar os pull requests criados pelo agente de nuvem.

## Arquivos de instruções

### Cenário

Como qualquer boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- a camada de dados sempre precisa de testes de unidade.
- a interface do usuário deve usar o modo escuro e ter um visual moderno.
- o código deve ser documentado com comentários TSDoc.
- um bloco de comentários no início de cada arquivo deve descrever sua finalidade.

Com arquivos de instruções, você fornecerá ao Copilot as informações necessárias para executar tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de codificação e seus requisitos. Esse recurso ajuda a orientar o Copilot para gerar sugestões e trechos de código mais relevantes. Você pode especificar as convenções de codificação, bibliotecas e até os tipos de comentários que prefere. É possível criar instruções para todo o repositório ou para tipos específicos de arquivo, fornecendo contexto para determinadas tarefas.

Há dois tipos de arquivo de instruções:

- `.github/copilot-instructions.md`, um único arquivo enviado ao Copilot em **todas** as solicitações feitas no repositório. Ele deve conter orientações para o projeto, como contexto relevante para a maioria das solicitações de chat ou CLI enviadas ao Copilot. Isso pode incluir a stack tecnológica, uma visão geral do que está sendo desenvolvido, práticas recomendadas e orientações globais.
- Arquivos `.github/instructions/*.instructions.md`, que podem ser criados para tarefas ou tipos de arquivo específicos. Eles podem conter diretrizes para linguagens específicas, como TypeScript ou Astro, ou para tarefas como criar componentes de interface ou conjuntos de testes.

> [!NOTE]
> Ao trabalhar no IDE, os arquivos de instruções são usados para gerar código no Copilot Chat — não para conclusões de código ou sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam tanto os arquivos de instruções do repositório quanto os arquivos `*.instructions.md` com frontmatter `applyTo` quando geram código.
>
> O Copilot também [oferece suporte a arquivos de instruções em outros padrões][custom-instructions-support], incluindo AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Uma discussão completa sobre a criação de arquivos de instruções está além do escopo deste workshop. No entanto, os exemplos do projeto mostram uma abordagem representativa. De forma geral:

- Mantenha as instruções em `copilot-instructions.md` focadas nas orientações gerais do projeto, como uma descrição do que está sendo criado, a estrutura do projeto e os padrões globais de codificação.
- Use arquivos `*.instructions.md` para dar orientações específicas a tipos de arquivo (testes de unidade, componentes Astro ou camada de dados) ou tarefas.
- Use linguagem natural. Mantenha as orientações claras e inclua exemplos de como o código deve (e não deve) ser.

Não há uma única maneira de criar arquivos de instruções, assim como não há uma única maneira de usar IA. A experimentação ajudará você a descobrir o que funciona no seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto sólido de arquivos de instruções. Ao explorar os arquivos deste projeto, você verá orientações para vários tipos de tarefa, incluindo [atualizações da interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface oferece essa opção de maneira diferente (por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI) — a lição correspondente explicará como usá-la quando for relevante.
>
> Quer encontrar modelos ou um ponto de partida? Explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instrução, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizadas deste projeto

Leia os arquivos de instruções fornecidos com este repositório. Há um arquivo principal `copilot-instructions.md` e vários arquivos `*.instructions.md` para diferentes tarefas. Abra-os no editor ou na interface web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Essas orientações se aplicam a qualquer interação com o Copilot.
3. Abra a pasta `.github/instructions` e examine seu conteúdo. Observe as instruções para arquivos Astro, a camada de dados Drizzle, testes e outros temas.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo: ele define um glob (relativo à raiz do repositório) que determina a quais arquivos as instruções se aplicam. Neste caso, qualquer arquivo de teste TypeScript (por exemplo, um que corresponda a `**/*.test.ts`) será incluído.
5. Observe as instruções específicas para criar testes de unidade neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instrução, como `unit-tests.instructions.md`, e para arquivos existentes no projeto. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** de `copilot-instructions.md` documenta os padrões de codificação do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentário nos arquivos.

## Adicionar um novo padrão ao repositório

Agora você fará a única edição deste exercício: adicionar uma regra para que a documentação fique no código, em comentários TSDoc, e um cabeçalho de comentário no nível do arquivo. O agente de nuvem usará essa regra ao trabalhar nas issues atribuídas a ele mais adiante neste roteiro.

Antes de editar, crie uma branch para o trabalho. O agente de nuvem lerá as instruções da branch de destino da issue, mas você fará commit das alterações separadamente:

1. No terminal do codespace, crie uma branch e mude para ela:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como destacado anteriormente, `.github/copilot-instructions.md` fornece informações gerais do projeto ao Copilot. Vamos documentar os padrões de codificação do repositório para aprimorar as sugestões.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar próxima da linha 27. Observe como ela documenta os padrões de codificação do projeto — mas ainda não define regras de documentação no código, por isso a função auxiliar gerada não tinha comentários de documentação.
3. Adicione as linhas Markdown a seguir logo depois dos padrões existentes, instruindo o Copilot a adicionar cabeçalhos de comentário aos arquivos e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem ser criados no nível do repositório (`.github/copilot-instructions.md`) para orientações globais ou como arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo de instruções do repositório é o lugar certo para padrões gerais, como a regra de comentários de documentação que você acabou de adicionar.

## Fazer commit, push e merge da atualização de instruções

O agente de nuvem lê os arquivos de instrução da branch de destino da issue. Ao atribuir uma issue ao Copilot na próxima lição, ele criará uma branch a partir de `main` — portanto, suas alterações de instruções precisam estar em `main` para que o agente as use.

1. Prepare e faça commit:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request da branch `custom-instructions-cloud` para `main` e faça o merge. Assim, o agente de nuvem lerá essas instruções ao trabalhar nas issues atribuídas a ele na próxima lição.

> [!TIP]
> Se preferir trabalhar diretamente em `main` durante este workshop, você pode ignorar a etapa da branch e fazer commit diretamente em `main`. A etapa da branch foi incluída para que o workshop reflita uma prática comum em projetos reais.

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções deste projeto e adicionou um novo padrão geral ao repositório. Você verá esse padrão nos pull requests gerados pelo agente de nuvem nas próximas etapas deste roteiro.

Em seguida, você [atribuirá sua primeira issue ao agente de nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
