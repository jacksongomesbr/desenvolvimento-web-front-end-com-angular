# Primeiro teste

> **\[info\] Objetivos do capítulo:**
>
> * apresentar o conceito de teste de software
> * demonstrar como escrever testes com o Angular utilizando Karma e Jasmine
>
> **Branch**: [iniciando](https://github.com/jacksongomesbr/angular-escola/tree/iniciando)

Teste de software é uma área da Engenharia de Software que envolve um conjunto de práticas para garantir a qualidade de software conforme requisitos e expectativas dos interessados, clientes ou desenvolvedores. Durante esse processo há geração de informação e conhecimento que permite entender o comportamento do software em situações simuladas ou com dados fictícios.

Uma forma de avaliar a qualidade do software utilizando teste de software é utilizar mecanismos que permitam identificar se o software está operando como esperado, fazendo o que deveria fazer.

Ao criar o projeto com o Angular CLI, junto com o componente `AppComponent` também é criado o arquivo `src/app/app.component.spec.ts`. Esse arquivo TypeScript descreve testes utilizando [Jasmine](https://jasmine.github.io/index.html). Por definição Jasmine é um framework de teste de software que segue a técnica BDD \(_Behaviour Driven Development_\). Embora o Jasmine possa ser utilizado diretamente para testar JavaScript o Angular utiliza duas outras tecnologias:

* [Karma](https://karma-runner.github.io/1.0/index.html), uma ferramenta utilizada para executar os testes escritos em Jasmine \(por definição, Karma é um **test runner**\); e
* [Protractor](http://www.protractortest.org), um framework para testes **end-to-end** \(e2e\).

Então, um trecho do arquivo `src/app/app.component.spec.ts`:

```typescript
import { TestBed, async } from '@angular/core/testing';
import { AppComponent } from './app.component';
describe('AppComponent', () => {
  ...
  }));
  it('should create the app', async(() => {
  ...
  }));
  it(`should have as title 'Angular'`, async(() => {
  ...
  }));
  it('should render title in a h1 tag', async(() => {
  ...
  }));
});
```

Testes escritos em Jasmine são chamados de **specs** e, por padrão, precisam estar em arquivos com nomes que terminem em `.spec.ts` \(isso está definido no arquivo `src/tsconfig.spec.json`\).

Depois das primeiras linhas com import há a primeira utilização de um recurso do Jasmine: a chamada do método `describe()`. Esse método cria uma **suíte de testes** \(um grupo de testes\). Os parâmetros para o método são:

* o nome da suíte de testes; e
* uma função que define suítes internas e specs.

No exemplo, o nome da suíte é 'AppComponent' e a função que define as suítes internas e as specs está definindo três testes.

As specs \(testes\) são definidas por meio de uma chamada ao método it\(\). Esse método cria uma spec. Os parâmetros do método são:

* o nome da spec \(na prática, uma descrição do que a spec está testando\);
* uma função que descreve o código do teste; e
* um número que representa o tempo limite de execução do teste \(**timeout**\).

No caso do exemplo há três specs:

* `should create the app` \(deveria criar o app -- ou o app deveria ser criado\)
* `should have title as 'Angular'` \(deveria ter o título como 'Angular' -- ou o título deveria ser 'Angular'\)
* `should render title in a h1 tag` \(deveria renderizar o título em uma tag h1 -- ou o título deveria estar em uma tag h1\)

Percebeu o "should" \(deveria\) no nome de cada spec? Isso faz parte da filosofia BDD que indica que cada spec checa se as **expectativas** sobre o teste são válidas \(**passam**\) ou não \(**falham**\). Antes de ver mais detalhes, vamos testar o software. Execute o comando:

```
npm test
```

Esse comando executa o script **"test"** definido no arquivo `package.json`. Na prática, ele executa o comando `ng test`.

Dentro de instantes você deveria ver uma janela do browser mostrando algo semelhante ao da figura a seguir.

![Janela do browser demonstrando o resultado dos testes iniciais](/assets/primeiro-teste-browser-inicial.png)

Note que a URL do browser é `http://localhost:9876`. Essa é a URL padrão que apresenta os resultados do teste do software. A tela apresenta várias informações importantes:

* versão do sistema operacional \(Windows 10\)
* versão do browser \(Chrome 63\)
* versão do Karma \(1.7.1\)
* versão do Jasmine \(2.6.4\)
* tempo total de execução dos testes \(0.187s\)
* quantidade de specs \(3\)
* quantidade de falhas é zero \(0\), o que indica que todos os testes passaram \(ou que não houve falha nos testes\)

Além disso a tela apresenta o próprio componente \(`AppComponent`\) em execução. Ainda, também há informações no prompt. Você deveria ver algo como o seguinte:

```
15 12 2017 01:56:31.937:WARN [karma]: No captured browser, open http://localhost:9876/
15 12 2017 01:56:32.381:INFO [Chrome 63.0.3239 (Windows 10 0.0.0)]: Connected on socket ...
Chrome 63.0.3239 (Windows 10 0.0.0): Executed 0 of 3 SUCCESS (0 secs / 0 secs)
Chrome 63.0.3239 (Windows 10 0.0.0): Executed 1 of 3 SUCCESS (0 secs / 0.101 secs)
Chrome 63.0.3239 (Windows 10 0.0.0): Executed 2 of 3 SUCCESS (0 secs / 0.143 secs)
Chrome 63.0.3239 (Windows 10 0.0.0): Executed 3 of 3 SUCCESS (0 secs / 0.184 secs)
Chrome 63.0.3239 (Windows 10 0.0.0): Executed 3 of 3 SUCCESS (0.092 secs / 0.184 secs)
```

Essas informações indicam dados adicionais:

* o primeiro teste executou em 0.101s
* o segundo teste executou em 0.143 - 0.101 = 0.042s
* o terceiro teste executou em 0.184 - 0.143 = 0.041s

Ok, agora vamos a detalhes dos testes mas, antes de ver cada teste individualmente, o segundo parâmetro do método `describe()`  chama o método `beforeEach()`, que é utilizada para realizar uma espécie de **setup dos testes**, executando um código antes deles:

```typescript
import { TestBed, async } from '@angular/core/testing';
import { AppComponent } from './app.component';
describe('AppComponent', () => {
  beforeEach(async(() => {
    TestBed.configureTestingModule({
      declarations: [
        AppComponent
      ],
    }).compileComponents();
  }));
  ...
```

A classe `TestBed` é um utilitário do Angular para testes de software. Seu método `configureTestingModule()` cria um módulo específico para executar os testes que serão descritos a seguir. Pense nisso como um módulo mais enxuto que os utilizados no restante do software, voltando apenas para esses testes. Por isso você deveria entender que o parâmetro fornecido para o método é muito similar aos metadados de um módulo e, nesse caso, indicam os componentes do módulo \(atributo `declarations`\). Portanto, o setup permite que cada teste utilize o componente `AppComponent`.

## Primeiro teste

O trecho de código a seguir apresenta o primeiro teste.

```typescript
it('should create the app', async(() => {
  const fixture = TestBed.createComponent(AppComponent);
  const app = fixture.debugElement.componentInstance;
  expect(app).toBeTruthy();
}));
```

Esse primeiro teste verifica se foi possível criar uma instância do componente `AppComponent`, o que é feito por meio de uma sequência de passos:

1. criar uma instância de `AppComponent`: isso é feito por meio do método `TestBed.createComponent()` \(armazenado na variável `fixture`\) que retorna uma instância de `ComponentFixture`, referência ao ambiente de execução dos testes. O ambiente de execução dos testes dá acesso ao `DebugElement`, que representa o elemento do DOM que representa o componente
2. acessar a instância do componente \(`fixture.debugElement.componentInstance`\), armazenada na variável `app`;
3. criar uma expectativa para o teste

Uma expectativa é criada por meio do método `expect()`. Uma expectativa representa o comportamento esperado do software: que `app` \(uma instância do `AppComponent`\) não seja `null`. O parâmetro de `expect()` indica a expressão que será analisada, comparada, com outra expressão ou valor. Nesse caso a comparação é realizada por meio do método `toBeTruthy()`. Se a expectativa não for satisfeita o teste falhará. Isso seria o comportamento obtido, por exemplo, por causa de um erro de tempo de execução \(**runtime**\).

## Segundo teste

O trecho de código a seguir apresenta o segundo teste.

```typescript
it(`should have as title 'Angular'`, async(() => {
  const fixture = TestBed.createComponent(AppComponent);
  const app = fixture.debugElement.componentInstance;
  expect(app.title).toEqual('Angular');
}));
```

A sequência de passos é semelhante à do primeiro teste. A diferença está na expectativa: o teste deseja verificar se o atributo `title` da instância do `AppComponent` \(`app`\) tem valor igual a `'Angular'`. Novamente, a expectativa é criada por meio do método `expect()`. Dessa vez a comparação entre o valor esperado \(`app.title`\) e o valor obtido \(`'Angular'`\) é feita por meio do método `toEqual()`.

Com isso aprendemos que é possível testar membros de um componente \(atributos e métodos\), o que é muito valioso.

## Terceiro teste

O trecho de código a seguir apresenta o terceiro teste.

```typescript
it('should render title in a h1 tag', async(() => {
  const fixture = TestBed.createComponent(AppComponent);
  fixture.detectChanges();
  const compiled = fixture.debugElement.nativeElement;
  expect(compiled.querySelector('h1').textContent).toContain('Welcome to Angular!');
}));
```

Em relação aos testes anteriores a sequência de passos inclui uma chamada ao método `ComponentFixture.detectChanges()`. Você deveria aprender, nos capítulos seguintes, que o Angular utiliza, muito, um recurso chamado **data binding**. Esse recurso faz com que o valor do atributo `title` seja apresentado no Template. Para checar se esse é o comportamento obtido é necessário, primeiro, identificar que ocorreu data binding. Uma forma de fazer isso é usando o método `detectChanges()`.

Outra diferença é que o teste lida com o DOM depois de ter sido transformado pelo Angular, ou seja, na prática, em como o usuário está vendo o componente no momento. Por causa disso não é utilizada uma instância do componente, mas uma referência ao DOM compilado \(o elemento DOM na raiz do componente, na verdade\). Isso é feito por meio de `fixture.debugElement.nativeElement` \(armazenado na variável `compiled`\).

Por fim, a expectativa é um pouco mais complexa, pois envolve lidar com o DOM. Para isso ocorre o seguinte:

1. encontrar o elemento `h1` no DOM: isso é feito por meio do método `querySelector()`;
2. comparar o conteúdo textual do elemento `h1` com a string `Welcome to Angular!`: isso é feito por meio de uma expectativa sobre o valor do atributo `textConent` do objeto que representa o elemento `h1` conter a string desejada, comparação implementada pelo método `toContain()`.

## You shall not pass! Ou provocando uma falha nos testes

Suponha que você modifique o valor do atributo `title` do `AppComponent` para `'Web'`. O que os testes diriam? Se os testes ainda estiverem em execução, apenas observe a janela do browser que mostra o resultado dos testes \(ilustrada pela figura a seguir\).

![](/assets/primeiro-teste-com-falha.png)

A saída dos testes mostra claramente que algo errado aconteceu: dos três testes, dois falharam. A saída indica cada teste que falhou \(detalhe em vermelho\):

* o segundo teste falhou porque a string `'Web'` não igual a `'Angular'`; e
* o terceiro teste falhou porque a string `'Welcome to Web!'` não contém `'Welcome to Angular!'`.

Percebeu que os testes são executados novamente sempre que há uma mudança no código do software? Esse é o comportamento padrão, mas se você precisar desligá-lo precisará de uma mudança no arquivo `package.json`, modificando o script de teste de \(`ng test`\):

```typescript
...
"scripts": {
...
  "test": "ng test",
...
},
...
```

para \(`ng test --single-run`\):

```typescript
...
"scripts": {
...
  "test": "ng test --single-run",
...
},
...
```

O problema disso é que, ao executar o comando `npm test` você verá a janela do browser aparecer e sumir logo depois de alguns instantes. Assim, precisará lidar com a saída do comando no prompt para identificar o que ocorreu \(testes passaram ou falharam\).

> **\[info\] Resumo do capítulo:**
>
> * testes de software permitem avaliar se o comportamento do software está como o esperado
> * Jasmine é um framework utilizado para escrever testes usando a técnica BDD
> * Karma e Protractor são ferramentas do Angular para executar testes
> * o comando `npm test` é usado para executar testes
> * testes são definidos dentro de suítes
> * uma suíte é criada usando o método `describe()`
> * um teste é criado usando o método `it()`
> * um teste pode validar diversos comportamentos do software, mas cada comportamento é validado usando o método `expect()`, que compara um valor esperado com um valor obtido
> * testes do Angular envolvem verificar membros do componente \(atributos e método\) e o DOM compilado, que é visualizado pelo cliente no browser



