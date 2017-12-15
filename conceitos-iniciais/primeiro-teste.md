# Primeiro teste

> **\[info\] Objetivos do capítulo:**
>
> * apresentar o conceito de teste de software
> * demonstrar como escrever testes com o Angular utilizando Karma e Jasmine

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

Primeiro teste:

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

Uma expectativa é criada por meio do método `expect()`. Uma expectativa representa o comportamento esperado do software: que `app` \(uma instância do `AppComponent`\) não seja `null`. Isso é representado em código por meio de: `expect(app).toBeTruthy()`. Se expectativa não for satisfeita o teste falha. Nesse caso pode acontecer, por exemplo, um erro de tempo de execução \(**runtime**\).



