# Iniciando com o Angular

> \[info\] **Objetivos do capítulo**:
>
> * demonstrar os elementos da Arquitetura do Angular
> * demonstrar a relação entre os elementos da Arquitetura do Angular e arquivos do projeto

O Angular é um framework para o desenvolvimento de software front-end. Isso quer dizer que utiliza tecnologias padrão do contexto web como HTML, CSS e uma linguagem de programação como JavaScript ou TypeScript.

Um software desenvolvido em Angular é composto por diversos elementos como: módulos, componentes, templates e serviços. Esses elementos fazem parte da arquitetura do Angular, que é ilustrada pela figura a seguir.

![](/assets/visao-geral-da-arquitetura-do-angular.png)

Essa arquitetura de software orientada a componentes implementa conceitos de dois padrões de arquitetura de software:

* **MVC **\(Model, View, Controller\) é um padrão de software que separa a representação da informação \(Model\) da interação do usuário com ele \(View-Controller\). Geralmente, Model e Controller são representados por código em linguagem de programação \(classes e/ou funções\) e View é representado por HTML e CSS.
* **MVVM** \(Model, View, View-Model\) é um padrão de software semelhante ao MVC, com a diferença de que o View-Model utiliza recurso de **data binding** \(mais sobre isso depois\) para fazer com que a View seja atualizada automaticamente quando ocorrer uma modificação no Model.

No contexto do Angular esses elementos são descritos conforme as seções a seguir.

## Elementos da Arquitetura do Angular

### Módulos

Módulos representam a forma principal de modularização de código. Isso significa que um módulo é um elemento de mais alto nível da arquitetura do Angular e é composto por outros elementos, como componentes e serviços.

Um software desenvolvido em Angular possui pelo menos um módulo, chamado **root module** \(módulo raiz\). Os demais módulos são chamados **feature modules** \(módulos de funcionalidades\).

### Bibliotecas

Bibliotecas funcionam como um agrupador de elementos de software desenvolvido em Angular. Bibliotecas oficiais têm o prefixo `@angular`. Geralmente é possível instalar bibliotecas utilizando o **npm** \(gerenciador de pacotes do NodeJs\).

Uma biblioteca pode conter módulos, componentes, diretivas e serviços.

### Componentes

Um componente está, geralmente, relacionado a algo visual, ou seja, uma tela ou parte dela. Nesse sentido, um componente possui código \(**Controller**\) que determina ou controla o comportamento da interação com o usuário \(**View** ou **Template**\).

O **Template** determina a parte visual do componente e é definido por código HTML e CSS, além de recursos específicos do Angular, como outros componentes e diretivas.

### Metadados

Metadados são um recurso do Angular para adicionar detalhes a classes. Isso é utilizado para que o Angular interprete uma classe como um Módulo ou como um Componente, por exemplo.

### Data binding

Data binding \(que seria algo como "vinculação de dados" em português\) é um reucrso do Angular que representa um componente importante da sua arquitetura. Considere os seguintes elementos:

* um Model define dados que serão apresentados no Template
* um Template apresenta os dados do Model
* um Controller ou um View-Model determina o comportamento do Template

Se o Controller atualiza o Model, então o Template tem que ser atualizado automaticamente. Se o usuário atualiza o Model por meio do Template \(usando um formulário, por exemplo\) o Controller também precisa ter acesso ao Model atualizado. O Data Binding atua garantindo que esse processo ocorra dessa forma.

### Diretivas

Diretivas representam um conceito do Angular que é um pouco confuso. Na prática, um Componente é uma Diretiva com um Template. Assim, um Componente é um tipo de Diretiva, que nem sempre está relacionada a algo visual. Angular define dois tipos de diretivas:

* **Diretivas Estruturais**: modificam o Template dinamicamente por meio de manipulação do DOM \(Document Object Model\), adicionando ou removendo elementos HTML
* **Diretivas de Atributos**: também modificam o Template, mas operam sobre elementos HTML já existentes

### Serviços

Um Serviço é uma abstração do Angular utilizado para isolar a lógica de negócio de Componentes. Na prática, um Serviço é representado por uma classe com métodos que podem ser utilizados em Componentes. Para isso, para que um Componente utilize um serviço, o Angular utiliza o conceito de **Injeção de Dependência** \(DI, do inglês **Dependency Injection**\). DI é um padrão de software que faz com que dependências sejam fornecidas para quem precisar. Na prática, o Angular identifica as dependências de um Componente e cria automaticamente instâncias delas, para que sejam utilizadas posteriormente no Componente.

Enquanto esses elementos da Arquitetura do Angular representam conceitos, é importante visualizar a relação deles com elementos práticos do software, ou seja, código. Para isso, a seção a seguir apresenta a estrutura padrão de um software desenvolvido em Angular.

## Estrutura padrão de um software desenvolvido em Angular

Um software desenvolvido em Angular é representado por vários arquivos HTML, CSS, TypeScript e de configuração \(geralmente arquivos em formato JSON\).

```
+ src
  + app
    - app.component.css
    - app.component.html
    - app.component.ts
    - app.module.ts
  + assets
  + environments
    - environment.prod.ts
    - environment.ts
  - index.html
  - maint.ts
  - polyfills.ts
  - styles.css
  - tsconfig.app.json
  - typings.d.ts
- .angular-cli.json
- package.json
- tsconfig.json
- tslint.json
```

No diretório raiz do software:

* `src`: contém o código-fonte do software \(módulos, componentes etc.\)
* `.angular-cli.json`: contém configurações do projeto Angular \(nome, scripts etc.\)
* `package.json`:  contém configurações do projeto NodeJS \(um projeto Angular utiliza NodeJS para gerenciamento de pacotes e bibliotecas, por exemplo\)
* `tsconfig.json` e `tslint.json`:  contêm configurações do processo de tradução de código TypeScript para JavaScript

No diretório `src`:

* `app`: contém o **root module** e os demais **feature modules** do projeto
* `assets`: contém arquivos CSS, JSON e scripts, por exemplo
* `environments`: contém arquivos de configuração do ambiente de desenvolvimento \(`environment.ts`\) e de produção \(`environment.prod.ts`\)
* `index.html`: contém o código HTML inicial para o projeto e inclui o componente principal do **root module**
* `main.ts`: contém o código TypeScript necessário para iniciar o software \(processo chamado de **Bootstrap**\)
* `polyfills.ts`: contém código TypeScript que indica scripts adicionais a serem carregados pelo Browser para funcionamento do software como um todo e para questões de compatibilidade com versões antigas de Browsers
* `style.css`: contém o código CSS para definir estilos globais para o software
* `tsconfig.app.json` e `typings.d.ts`: complementam configurações do arquivo `../tsconfig.json` específicas para o software em questão

No diretório `app`:

* `app.component.css`, `app.component.html` e `app.component.ts`: definem o component AppComponent, respectivamente: apresentação por meio de CSS, Template e Controller. Basicamente, estes três arquivos formam a base de todo componente
* `app.module.ts`: código TypeScript que define o **root module**

Esse capítulo incial do livro apresentou conceitos importantes do Angular. A maior parte dos conceitos está diretamente relacionada a código-fonte \(HTML, CSS, TypeScript e JSON\) que estão em arquivos do diretório onde encontra-se o projeto.

Sempre que necessário, volte a esse capítulo para revisar conceitos do Angular. Muito provavelmente, mesmo desenvolvedores experientes precisem, de tempos em tempos, rever essas definições da arquitetura do Angular.

