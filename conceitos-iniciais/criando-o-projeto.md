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



