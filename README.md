# Desenvolvimento web front-end com Angular

Este é um livro open-source, tanto no conteúdo quanto no código-fonte associado. O conteúdo é resultado de algumas práticas com o Framework Angular e segue uma abordagem prática, com foco no entendimento de conceitos e tecnologias no contexto do desenvolvimento de um software de gerenciamento escolar.

Com o passar dos capítulos será possível perceber a utilização iterativa e incremental de recursos até chegar à conclusão do desenvolvimento do gerenciador escolar.

Parte do conteúdo do livro é baseada na documentação oficial do Angular, disponível em [https://angular.io](https://angular.io).

Como o Angular é um projeto em constante desenvolvimento \(pelo menos até agora\) serão publicadas atualizações no conteúdo do livro sempre que possível, para refletir novos recursos e funcionalidades. No momento, o conteúdo do livro é baseado na versão **5.0.0**.

## Conhecimentos desejáveis

Este livro aborda o desenvolvimento de software front-end para a web do ponto-de-vista do Angular. Isso quer dizer que não trata de conceitos iniciais de HTML, CSS, JavaScript, TypeScript e Bootstrap. Entretanto, os conceitos fundamentais dessas tecnologias vão sendo apresentados no decorrer dos capítulos, conforme surge a necessidade deles.

Para aprender mais sobre essas tecnologias recomendo essas fontes:

* **TypeScript**: [Documentação oficial do TypeScript - Microsoft](https://www.typescriptlang.org/docs/home.html), [TypeScript Deep Dive](https://www.gitbook.com/book/basarat/typescript/details)
* **HTML, CSS e JavaScript:** [W3Schools](https://www.w3schools.com/)
* **Boostrap**: [Documentação oficial do Bootstrap](http://getbootstrap.com/docs/4.0/getting-started/introduction/)

## Código-fonte

O software _Angular Escola_, que serve como apoio para este livro está disponível gratuitamente no Github: [https://github.com/jacksongomesbr/angular-escola](https://github.com/jacksongomesbr/angular-escola). O repositório provê branches que demonstram a evolução do desenvolvimento do software na mesma ordem dos capítulos do livro. Cada capítulo, se necessário, indica qual branch utilizar.

## Dependências

Para utilizar o código-fonte do livro e desenvolver em Angular você precisa, antes, montar seu ambiente de desenvolvimento com algumas ferramentas:

1. **Git**: disponível em [https://git-scm.com/](https://git-scm.com/)
2. **Node.js**: disponível em [https://nodejs.org](https://nodejs.org) representa um ambiente de execução do JavaScript fora do browser e também inclui o **npm**, um gerenciador de pacotes
3. **Editor de textos ou IDE**: atualmente há muitas opções, mas destaco estas:
   1. **VisualStudioCode**: disponível em [https://code.visualstudio.com/](https://code.visualstudio.com/)
   2. **Nodepad++**: disponível em [https://notepad-plus-plus.org/](https://notepad-plus-plus.org/) 
   3. **PHPStorm **ou **WebStorm**: disponíveis em [https://www.jetbrains.com](https://www.jetbrains.com)

## Visão geral

A tabela a seguir apresenta uma visão geral dos conceitos apresentados ou discutidos em cada capítulo do livro.

| Capítulo | Conteúdos |
| :--- | :--- |
| [Iniciando com o Angular](/conceitos-iniciais/iniciando-com-o-angular.md) | Arquitetura do Angular; Elementos da Arquitetura do Angular \(ex.: módulos e componentes\) |
| [Angular CLI](/conceitos-iniciais/angular-cli.md) | Ferramenta Angular CLI; Comandos |
| [Criando o projeto](/conceitos-iniciais/criando-o-projeto.md) | Ferramenta Angular CLI; Comando new |
| [Apresentando a lista de disciplinas](/conceitos-iniciais/apresentando-a-lista-de-disciplinas.md) | Data binding; Diretivas; Eventos |
| [Cadastrando uma disciplina](/cadastrando-uma-disciplina.md) | Formulários; Two-way Data binding; Eventos |
| [Excluindo uma disciplina](/excluindo-uma-disciplina.md) | Two-way Data Binding; Eventos |
| [Editando dados de uma disciplina](/conceitos-iniciais/editando-dados-de-uma-disciplina.md) | Formulários; Two-way Data Binding; Eventos |
| [Melhorando a interface gráfica do CRUD de disciplinas](/conceitos-iniciais/melhorando-a-interface-com-bootstrap.md) | Formulários; Data Binding; Eventos; Bootstrap; Font Awesome |
| [Interação entre componentes](/interacao-entre-componentes.md) | Criação de componentes; Input e Output para interação entre componentes |



