# Criando o projeto

> **\[info\] Objetivos do capítulo:**
>
> * demonstrar como utilizar o Angular CLI para criar o projeto **angular-escola**
> * apresentar os requisitos de software que serão implementados no capítulo
>
> **Branch**: [iniciando](https://github.com/jacksongomesbr/angular-escola/tree/iniciando)

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
    "build": "ng build --prod",
    ...
  },
  "private": true,
  "dependencies": {
    "@angular/animations": "^5.2.0",
    "@angular/common": "^5.2.0",
    "@angular/compiler": "^5.2.0",
    "@angular/core": "^5.2.0",
    "@angular/forms": "^5.2.0",
    "@angular/http": "^5.2.0",
    "@angular/platform-browser": "^5.2.0",
    "@angular/platform-browser-dynamic": "^5.2.0",
    "@angular/router": "^5.2.0",
    "core-js": "^2.4.1",
    "rxjs": "^5.5.6",
    "zone.js": "^0.8.19"
  },
  "devDependencies": {
    "@angular/cli": "1.6.6",
    "@angular/compiler-cli": "^5.2.0",
    "@angular/language-service": "^5.2.0",
    ...
  }
}
```

Por ser um conteúdo em formato JSON, o interpretamos da seguinte forma:

* `name`: é o atributo que define o nome do projeto para o NodeJS \(nesse ponto, não tem a ver com o Angular\)
* `scripts`: é o atributo que contém scripts que podem ser executados no prompt \(exemplos: `npm start` e `npm run build`\). Nesse caso esses scripts usam o Angular CLI \(veja os valores dos atributos `start` e `build`\)
* `dependencies`: é o atributo que define as dependências do projeto. Basicamente, as dependências atuais são padrão para software desenvolvido em Angular. Importante notar que o nome de cada atributo determina o pacote \(como `@angular/core`\) e seu valor determina a versão \(como `^5.2.0`, indicando a versão desses pacotes do Angular\)
* `devDependencies`: é o atributo que define as dependências de desenvolvimeto. São ferramentas que não geram código-fonte que será distribuído junto com o software que está sendo desenvolvido

## Examinando o código-fonte\#0

Agora que você já conhece um pouco mais da estrutura do software Angular, do Angular CLI e do npm, vamos dar uma olhada no código-fonte que está gerando o software atual, começando pelo `AppComponent` e seu **Template**, que está no arquivo `./src/app/app.component.html`, cujo trecho de conteúdo é apresentado a seguir.

```html
<div style="text-align:center">
  <h1>
    Welcome to {{ title }}!
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

Antes de prosseguirmos, perceba que há um atributo da classe `AppComponent` chamado `title` e tem o valor `'app'` \(onde já vimos isso?\).

A figura a seguir ilustra a estrutura do `AppComponent`.

![Estrutura do component AppComponent](/assets/projeto-original-appcomponent.png)

Como mostra a figura o componente `AppComponent` é composto por **Template**, **Controller** e **Model**. O Template está definido no arquivo `./src/app/app.component.html`, enquanto o Controller e o Model estão definidos no arquivo `./src/app/app.component.ts`. Template e Controller são mais fáceis de identificar nessa arquitetura. Enquanto o primeiro está no arquivo HTML o segundo é a classe no arquivo TypeScript. Entretanto, onde está o Model? Antes de responder essa pergunta faça um alteração no Controller: altere o valor do atributo `title` para, por exemplo, `'Angular'`.

Se você não tiver interrompido a execução do script `npm start` acontecerá uma recompilação incremental automaticamente e a janela do Browser será recarregada. Na página, o que está logo depois de `Welcome to` ? Isso mesmo, a palavra "Angular". Que mágica é essa? Nenhuma mágica! Veja novamente o Template, mais de perto:

```html
<h1>
Welcome to {{ title }}!
</h1>
```

Nesse momento você já deve ter aprendido que o trecho `{{ title }}` é responsável por fazer com que o valor do atributo `title` to Controller apareça no Browser. Isso é resultado do processo de **Data binding**:

* analisa o Controller e identifica métodos e atributos
* interpreta e analisa o Template e procura por, por exemplo, conteúdo que esteja entre `\{\{` e `\}\}`
* utiliza o recurso de **interpolação**, que faz com a expressão dentro de `\{\{` e `\}\}` seja interpretada e gere uma alteração no conteúdo do Template ao ser apresentado no Browser. Nesse caso `{{title}}` significa: mostre o valor de `title`. 

A figura a seguir ilustra os elementos desse processo.

![Relação entre Model-Template-Controller no AppComponent](/assets/angular-inicial-app-component-template-model.png)

Aqui aprendemos algo um conceito importante: o **Angular interpreta o Template**. Isso significa que todo o seu conteúdo, HTML e CSS, é interpretado pelo Angular antes de ser entregue para o Browser. É isso que faz com que o recurso de interpolação funcione. É por isso, também, que Data binding é tão importante no Angular. Ele é responsável por fazer com que o atributo `title`, que compõe o Model \(ah, agora sim!\), esteja disponível para ser usado no Template. Além disso, qualquer alteração no valor desse atributo representará uma alteração na visualização do componente no Browser.

Se você se lembrar da discussão sobre Angular ser MVC ou MVVM é aqui que o entendimento pesa a favor do segundo. O Controller não está desassociado do Model e, por causa do Data binding, sua função inclui informar o Template de que ocorreu uma alteração no Model, de forma que ele seja atualizado.

É isso, o Model é um conceito abstrato e está, geralmente, no Controller, representado por atributos e métodos -- sim, também é possível mostrar o valor retornado por um método no template \(mais sobre isso depois\).

Para finalizar essa seção falta entender como o `AppComponent` é apresentado e qual a relação dele com o `AppModule` ou outros componentes do projeto. Para isso, veja o arquivo `./src/index.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Angular Escola</title>
  <base href="/">

  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="icon" type="image/x-icon" href="favicon.ico">
</head>
<body>
  <app-root></app-root>
</body>
</html>
```

Ele é o primeiro arquivo que o Browser acessa, embora não somente com esse conteúdo. Quando o Angular compila o código-fonte ele entrega um pouco mais, veja:

```html
<!doctype html>
<html lang="en">
...
</head>
<body>
  <app-root></app-root>
  <script type="text/javascript" src="inline.bundle.js"></script>
  <script type="text/javascript" src="polyfills.bundle.js"></script>
  <script type="text/javascript" src="styles.bundle.js"></script>
  <script type="text/javascript" src="vendor.bundle.js"></script>
  <script type="text/javascript" src="main.bundle.js"></script>
</body>
</html>
```

Esse é um trecho do código-fonte visualizado pelo Browser. Perceba as linhas com elementos `script` antes da tag `</body>`. Esses elementos não estão no código-fonte original, eles foram embutidos aí pelo processo de compilação do Angular \(que foi iniciado quando o script `npm start` foi executado\). Não precisa fazer isso agora \(sério!\) mas em algum momento você vai perceber que o conteúdo daqueles arquivos indicados nos elementos `script` é realmente muito importante porque eles representam o resultado da **tradução de todo o código-fonte do projeto em conteúdo que o Browser consegue interpretar**. Destaque para o arquivo `main.bundle.js`, que contém o conteúdo do arquivo `./src/main.ts` cujo trecho é:

```
import { enableProdMode } from '@angular/core';
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
import { AppModule } from './app/app.module';
...
platformBrowserDynamic().bootstrapModule(AppModule)
  .catch(err => console.log(err));
```

O método `platformBrowserDynamic()` retorna um objeto que contém o método `boostrapModule()` que, ao ser executado, recebe como parâmetro a classe `AppModule`, que será utilizada para representar o **root module**. Lembra disso? No capítulo [Iniciando com o Angular](/conceitos-iniciais/iniciando-com-o-angular.md) vimos que o **root module** é o módulo mais importante de todo projeto Angular, porque é o primeiro a ser executado \(agora você deve estar entendendo isso\).

Certo, então o Angular carrega primeiro o `index.html`, depois usa o `AppModule` como **root module**, mas isso ainda não explica como o `AppComponent` passou a aparecer no Browser. Para entender isso, veja o arquivo `./src/app/app.module.ts`, que define o `AppModule`:

```
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

As três primeiras linhas importam os módulos `BrowserModule` e `NgModule` dos seus respectivos pacotes, bem como a classe `AppComponent`. Como você já sabe, o Angular utiliza **annotation functions** para adicionar metadados a classes. Aqui está sendo utilizada a função `NgModule()` que transforma a classe `AppModule` em um módulo para o Angular. O objeto passado como parâmetro para a função possui características importantes a destacar:

* `declarations`: é um vetor que contém componentes do módulo \(isso mesmo, `AppComponent` está contido no `AppModule`\);
* `imports`: é um vetor que contém as dependências \(outros módulos\);
* `bootstrap`: é um vetor que contém os componentes que devem ser utilizados quando uma instância do `AppModule` passar a existir \(isso, indicado ali no arquivo `./src/main.ts`\).

As coisas estão ficando mais conectadas agora e a figura a seguir ilustra isso.

![](/assets/angular-inicial-estrutura-appmodule-appcomponent-index.png)

É isso! O elemento `app-root` é interpretado corretamente pelo Browser quando recebe o arquivo `index.html` porque ele está indicado como `selector` do `AppComponent`, que é o componente executado pelo `AppModule`, qué o **root module**. O `selector` do `AppComponent` é utilizado pelo Angular para saber onde apresentar o component no Template. O valor `'app-root'` indica que o Angular deve procurar um elemento com esse nome. E é o que acontece.

**Viu? Não é mágica. É software!**

## O que vem a seguir?

Como agora você já deve estar entendendo melhor o funcionamento do Angular e o código-fonte do projeto, é hora de sujar as mãos =\) No capítulo a seguir você vai implementar o requisito: **ver as disciplinas**.

