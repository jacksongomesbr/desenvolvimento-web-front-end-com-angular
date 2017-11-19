# Criando o projeto

> \[info\] Objetivos do capítulo:
>
> * demonstrar como utilizar o Angular CLI para criar o projeto **angular-escola**
> * apresentar os requisitos de software que serão implementados no capítulo

O capítulo [Angular CLI](/conceitos-iniciais/angular-cli.md) apresentou essa ferramenta, comandos e opções. A primeira utilização da ferramenta está na criação do projeto com o comando:

```
ng new angular-escola
```

Isso fará com que seja criado um diretório `angular-escola` no local onde o comando foi executado. Como já informado, o diretório contém código-fonte de um software Angular funcional. Para vê-lo em execução acesse o diretório `angular-escola` e execute o comando:

```
npm start
```

Esse script executa o comando `ng serve` do Angular CLI e cria um servidor web local, por padrão na porta **4200**. Para ver o software em funcionamento, abra o endereço `http://localhost:4200` no seu browser de preferência. Mais para frente podemos discutir um pouco mais a saída desse comando, porque é muito interessante. O resultado deverá ser semelhante ao da figura a seguir.

![Versão inicial do software em execução no Browser](/assets/software-versao-inicial-browser.png)

Esse não é um comando do Angular CLI. Antes, é um comando \(script\) que está definido no arquivo `package.json`.

## O arquivo package.json

O arquivo `package.json` contém informações do projeto, scripts e dependências. O arquivo criado pelo Angular CLI tem mais ou menos o seguinte conteúdo:

```js
{
  "name": "angular-escola",
  ...
  "scripts": {
    "ng": "ng",
    "start": "ng serve",
    "build": "ng build",
    ...
  },
  "private": true,
  "dependencies": {
    "@angular/animations": "^5.0.0",
    "@angular/common": "^5.0.0",
    "@angular/compiler": "^5.0.0",
    "@angular/core": "^5.0.0",
    "@angular/forms": "^5.0.0",
    "@angular/http": "^5.0.0",
    "@angular/platform-browser": "^5.0.0",
    "@angular/platform-browser-dynamic": "^5.0.0",
    "@angular/router": "^5.0.0",
    ...
  },
  "devDependencies": {
    "@angular/cli": "1.5.2",
    "@angular/compiler-cli": "^5.0.0",
    "@angular/language-service": "^5.0.0",
    ...
  }
}
```

Por ser um conteúdo em formato JSON, o interpretamos da seguinte forma:

* `name`: é o atributo que define o nome do projeto para o NodeJS \(nesse ponto, não tem a ver com o Angular\)
* `scripts`: é o atributo que contém scripts que podem ser executados no prompt \(exemplos: `npm start` e `npm run build`\). Nesse caso esses scripts usam o Angular CLI \(veja os valores dos atributos `start` e `build`\)
* `dependencies`: é o atributo que define as dependências do projeto. Basicamente, as dependências atuais são padrão para software desenvolvido em Angular. Importante notar que o nome de cada atributo determina o pacote \(como `@angular/core`\) e seu valor determina a versão \(como `^5.0.0`, indicando a versão desses pacotes do Angular\)
* `devDependencies`: é o atributo que define as dependências de desenvolvimeto. São ferramentas que não geram código-fonte que será distribuído junto com o software que está sendo desenvolvido

## Examinando o código-fonte\#0

Agora que você já conhece um pouco mais da estrutura do software Angular, do Angular CLI e do npm, vamos dar uma olhada no código-fonte que está gerando o software atual, começando pelo `AppComponent` e seu **Template**, que está no arquivo `./src/app/app.component.html`, cujo trecho de conteúdo é apresentado a seguir.

```html
<div style="text-align:center">
  <h1>
    Welcome to {{title}}!
  </h1>
  <img width="300" alt="Angular Logo" src="data:image/svg+xml;base64,PHN...Pg==">
</div>
...
```

Se HTML for familiar pra você então não há problema para entender o código do Template. Na verdade, tem algo diferente ali perto de `Welcome to` e vamos falar disso daqui a pouco.

Agora, o Controller do mesmo componente, no arquivo `./src/app/app.component.ts`, com código-fonte a seguir:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'app';
}
```

Há pouco conteúdo, mas já são usados vários conceitos do Angular, vamos examinar um-a-um:

1. a primeira linha usa a instrução `import` para incluir algo chamado `Component` e que é disponibilizado pelo pacote `@angular/core`. 
2. `Component` é uma **annotation function** e é utilizada pelo Angular para adicionar metadados à classe `AppComponent`.
3. Os metadados são fornecidos como parâmetro para a função `Component`: 
   1. `selector`: indica que o **seletor **é `app-root`. E o que é seletor? Veremos daqui a pouco
   2. `templateUrl`: indica que o arquivo utilizado para o template do componente é `./app.component.html` \(que vimos há pouco\)
   3. `styleUrls`: é um array que indica quais são os arquivos CSS utilizados no componente. Nesse caso, está sendo utilizado apenas o arquivo `./app.component.css`
4. a instrução `export` é utilizada para que outros arquivos possam utilizar a classe `AppComponent`

Antes de prosseguirmos, perceba que há um atributo da classe `AppComponent` chamado `title` e tem o valor `'app'` (onde já vimos isso?).

A figura a seguir ilustra a estrutura do `AppComponent`.

![Estrutura do component AppComponent](/assets/projeto-original-appcomponent.png)

Como mostra a figura o component `AppComponent` é composto por **Template**, **Controller** e **Model**. O Template está definido no arquivo `./src/app/app.component.html`, enquanto o Controller e o Model estão definidos no arquivo './src/app/app.component.ts`.

