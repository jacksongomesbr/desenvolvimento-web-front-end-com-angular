# Serviços

> Objetivos do capítulo:
>
> * demonstrar a modularização de código utilizando serviços
>
> **Branch**: [livro-services](https://github.com/jacksongomesbr/angular-escola/tree/livro-services)

Conforme a Arquitetura do Angular a utilização de **Serviços **tem o propósito de organizar o projeto de software Angular, isolando lógica de negócio e separando-a dos Controllers. Não é possível afirmar que seja obrigatório utilizar serviços, mas é muito desejável.

Na prática não há diferença para o usuário porque, provavelmente, utilizar serviços não afetará diretamente o comportamento da interface. Assim, os benefícios ficam por conta da melhor organização do projeto e da Engenharia de Software para o projeto que está sendo desenvolvido.

Um serviço é uma classe que pode ser utilizada por outros componentes do projeto. Dois detalhes importantes são: o que diferencia um serviço de outro tipo de componente e como um componente utiliza um serviço. Veremos isso a seguir.

Até o momento a lógica do CRUD de disciplinas está toda nos Controllers dos componentes. Isso não é uma boa prática, porque é indicado que a lógica dos Controllers seja voltada para gerenciar o comportamento da interface, ou seja, para controlar a interação entre o usuário e o software. Vamos começar criando o serviço DisciplinasService usando Angular CLI e o comando **generate** \(abreviado por **g**\) e a opção **service** \(abreviada por **s**\):

```
ng g s Disciplinas --spec false --module App
```

A linha de comando cria o arquivo `./src/app/disciplinas.service.ts` \(a opção **--spec false** é utilizada para não criar um arquivo de testes, que é o padrão\). A opção **--module App** é utilizada para que o módulo **App** seja modificado para prover o serviço. Um módulo precisa prover ou fornecer um serviço para que ele possa ser utilizado pelos seus componentes. Para isso ocorre uma alteração na declaração do módulo App, como mostra o trecho de código a seguir:

```typescript
...
import { DisciplinasService } from './disciplinas.service';

@NgModule({
  declarations: [
  ...
  ],
  imports: [
  ...
  ],
  providers: [DisciplinasService],
  bootstrap: [AppComponent]
})
export class AppModule {
}
```

O atributo `providers` está informando para os demais componentes do módulo **App** que o serviço `DisciplinasService` está disponível para uso.

O Angular CLI cria um serviço operacional, mas não funcional, ou seja, vazio. Entretanto, é bom dar uma olhada no código:

```typescript
import { Injectable } from '@angular/core';

@Injectable()
export class DisciplinasService {
  constructor() { }
}
```

Novamente, um serviço é uma classe. A diferença está na anotação `@Injectable()`. É ela que instrui o Angular a considerar a classe `DisciplinasService` como um serviço -- pois é, não há uma anotação com nome parecido com _@Service\(\)_ =\.

Então, vamos adicionar funcionalidades nesse serviço:

* retornar a lista de disciplinas
* salvar uma disciplina \(cadastrar ou excluir\)
* excluir uma disciplina
* encontrar uma disciplina

Na verdade, essas funcionalidades já estão disponíveis no Controller do `AppComponent`, então o trabalho agora é realocar código. Dessa forma, o código do `DisciplinasServices` fica mais ou menos assim:

```typescript
import {Injectable} from '@angular/core';
import {Disciplina} from './disciplina.model';

@Injectable()
export class DisciplinasService {
  private disciplinas = null;
  private novo_id = 1;

  constructor() {
    this.disciplinas = [
      new Disciplina(1, 'Língua Portuguesa', 'O ...'),
      ...
    ];
  }

  todas(): Array<Disciplina> {
    return this.disciplinas;
  }

  salvar(id: number, nome: string, descricao: string): Disciplina {
    if (id) {
      let d = this.encontrar(id);
      if (d) {
        d.nome = nome;
        d.descricao = descricao;
      } else {
        throw new Error('Não foi possível encontrar a disciplina');
      }
      return d;
    } else {
      const d = new Disciplina(this.novo_id, nome, descricao);
      this.disciplinas.push(d);
      this.novo_id++;
      return d;
    }
  }

  excluir(disciplina: number | Disciplina) {
    let d = null;
    if (typeof(disciplina) === 'number') {
      d = this.encontrar(disciplina);
    } else {
      d = this.encontrar(disciplina.id);
    }
    if (d) {
      const i = this.disciplinas.indexOf(d);
      this.disciplinas.splice(i, 1);
    } else {
      throw new Error('Não foi possível encontrar a disciplina');
    }
  }

  encontrar(arg: number | string) {
    let d = null;
    if (typeof(arg) === 'number') {
      d = this.disciplinas.filter(d => d.id === arg);
    } else {
      d = this.disciplinas.filter(d => d.nome === arg);
    }
    if (d != null && d.length > 0) {
      return d[0];
    } else {
      return null;
    }
  }
}
```

O método `todas()` retorna todas as disciplinas.

O método `encontrar()` recebe o id da disciplina ou o seu nome, procura o array `disciplinas` e, se encontrar, retorna a disciplina correspondente. Caso contrário retorna `null` para indicar que a disciplina não foi encontrada. Note a expressão usada para definir o tipo do parâmetro `arg`: `number | string`. Isso significa que o método aceita um parâmetro do tipo `number` ou `string`. Note também a utilização do método `filter()` para aplicar um predicado \(representado por uma função seta\) para encontrar um item do array.

A lógica dos métodos `salvar()` e `excluir()` é semelhante à utilizada anteriormente no `AppComponente`, com a diferença de que não há interação com o usuário. É importante ressaltar também que esses métodos estão disparando exceções \(veja as linhas com `throw new Error(...)`\) que devem ser devidamente tratadas nos componentes que utilizarem esse serviço.

Já no Controller do `AppComponent` a mudança começa no método construtor:

```typescript
constructor(private disciplinasService: DisciplinasService) {
  this.disciplinas = this.disciplinasService.todas();
}
```

Essa sintaxe `private disciplinasService: DisciplinasService` utiliza um recurso importante do Angular chamado _**Dependency Injection**_ \(DI\). Por meio de DI o Angular cria automaticamente uma instância de `DisciplinasService` e a atribui ao atributo privado `disciplinasService`, tornando o serviço disponível no componente. Com isso o código desse Controller fica como o seguinte:

```typescript
...
import {DisciplinasService} from "./disciplinas.service";
...
export class AppComponent {
  editando = {id: 0, nome: '', descricao: ''};
  ...
  disciplinas = null;

  constructor(private disciplinasService: DisciplinasService) {
    this.disciplinas = this.disciplinasService.todas();
  }

  salvar() {
    try {
      const d = this.disciplinasService.salvar(this.editando.id, 
        this.editando.nome, this.editando.descricao);
      this.salvar_ok = true;
      this.editando = {id: 0, nome: '', descricao: ''};
    } catch (e) {
      this.salvar_erro = true;
    }
  }

  excluir(disciplina) {
    if (this.editando === disciplina) {
      alert('Você não pode excluir uma disciplina que está editando');
    } else {
      if (confirm('Tem certeza que deseja excluir a disciplina "'
          + disciplina.nome + '"?')) {
        try {
          this.disciplinasService.excluir(disciplina);
          this.excluir_ok = true;
          this.redefinir();
        } catch (e) {
          this.excluir_erro = true;
        }
      }
    }
  }
...
}
```

Perceba como a lógica da interface foi mantida e o código não possui a lógica de manipular o array de disciplinas \(consultar, inserir, excluir e encontrar elementos\), já que isso fica a cargo do serviço `DisciplinasService`. Assim, o componente `AppComponent` utiliza o serviço `DisciplinasService` e vimos como utilizar esse curso para organizar melhor o código e utilizar os padrões de arquitetura do Angular.

Como o código-fonte do software está crescendo, tanto em termos de volume quanto de arquivos, é interessante adotar uma abordagem que auxilia ainda mais na organização de código: diagramas da UML. A figura a seguir apresenta o diagrama de classes do software até então.

![Diagrama de classes do software](/assets/software-diagrama-de-classes-servicos.png)

O diagrama de classes apresentado pela figura demonstra relações entre alguns elementos do software até então, principalmente com foco nos controllers dos componentes, no serviço `DisciplinasService` e nas relações \(usa\) entre eles.

