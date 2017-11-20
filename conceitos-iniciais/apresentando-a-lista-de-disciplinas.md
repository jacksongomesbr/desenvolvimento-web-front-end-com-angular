# Apresentando a lista de disciplinas

> **\[info\] Objetivos do capítulo:**
>
> * demonstrar a implementação do requisito "apresentar as disciplinas" de forma iterativa
> * demonstrar o uso das diretivas `ngFor` e `ngIf`
> * reforçar o entendimento do Data binding

Começando pelo Controller do `AppComponent`, remova o atributo `title` e adicione o atributo `disciplinas`. Esse atributo é um array de string, contendo os nomes das disciplinas. O conteúdo do Controller deve ficar mais ou menos assim \(trecho\):

```typescript
export class AppComponent {
  disciplinas = [
    'Língua Portuguesa',
    'Arte',
    'Educação Física',
    'Matemática',
    'História',
    'Geografia',
    'Ciências',
    'Redação',
    'Língua Estrangeira Moderna - Inglês',
    'Ensino Religioso'
  ];
}
```

Na sequência altere o Template do `AppComponent` para o seguinte conteúdo:

```html
<h1>Disciplinas</h1>
<hr>
<ul>
  <li *ngFor="let disciplina of disciplinas">
    {{disciplina}}
  </li>
</ul>
```

Parte importante do Template está no elemento `li`: o atributo `*ngFor` é, na verdade, uma **diretiva estrutural **que modifica o Template no browser, gerando novos elementos com base em uma repetição.

> A diretiva, na verdade mesmo, é chamada `NgForOf`. Usamos uma espécie de atalho por meio do atributo `*ngFor` \(sem mais explicações sobre isso, por enquanto\). Então, ao ler `NgForOf` ou `*ngFor` saiba que estamos falando da mesma coisa.

A diretiva `*ngFor` é utilizada para repetir um elemento do Template. Nesse caso, é justamente essa a sua aplicação: repetir o elemento `li` para apresentar os nomes das disciplinas. A repetição é realizada por uma expressão: `let disciplina of disciplinas` significa que o `*ngFor` vai repetir o elemento `li` para cada item do array `disciplinas` e cada item recebe o nome de `disciplina`. Explicando a mesma coisa de outra forma:

* a repetição sempre é baseada em uma coleção de itens \(ou em algo que possa ser interpretado como tal\)
* o Angular cria uma variável de template \(existe apenas dentro do elemento `li`\) com o nome de `disciplina`
* essa variável de template é usada como uma referência para cada item da repetição

É por isso que a interpolação funciona dentro do elemento `li` para apresentar o nome de cada disciplina.

> A documentação do Angular afirma que `let disciplina of disciplinas` é uma **microssintaxe **e isso não é a mesma coisa que uma expressão usada em uma interpolação.

Abra o aplicativo no browser. O resultado deve ser semelhante à figura a seguir.

![Apresentando a lista das disciplinas](/assets/browser-listando-disciplinas.png)

## Melhorando o modelo de dados usando objetos

A utilização de um array de strings pode ser suficiente em vários casos. Entretanto, quando se deseja apresentar mais informações é necessário utilizar outra abordagem. Suponha, portanto, que precisemos apresentar dois dados de cada disciplina: **nome** e **descrição**. Uma boa forma de fazer isso é utilizar objetos.

Modifique o Controller do `AppComponent` para que cada item do array `disciplinas` seja um objeto com dois atributos: `nome` e `descricao`, como mostra o trecho de código a seguir.

```typescript
export class AppComponent {
  disciplinas = [
    {
      nome: 'Língua Portuguesa',
      descricao: 'O objetivo norteador da BNCC de Língua Portuguesa é garantir a todos os alunos o acesso aos saberes ' +
      'linguísticos necessários para a participação social e o exercício da cidadania, pois é por meio da língua que ' +
      'o ser humano pensa, comunica-se, tem acesso à informação, expressa e defende pontos de vista, partilha ou ' +
      'constrói visões de mundo e produz conhecimento.'
    },
    {
      nome: 'Arte',
      descricao: 'A Arte é uma área do conhecimento e patrimônio histórico e cultural da humanidade. No Ensino ' +
      'Fundamental, o componente curricular está centrado em algumas de suas linguagens: as Artes visuais, a Dança, ' +
      'a Música e o Teatro. Essas linguagens articulam saberes referentes a produtos e fenômenos artísticos e ' +
      'envolvem as práticas de criar, ler, produzir, construir, exteriorizar e refletir sobre formas artísticas. ' +
      'A sensibilidade, a intuição, o pensamento, as emoções e as subjetividades se manifestam como formas de ' +
      'expressão no processo de aprendizagem em Arte. '
    },
  ...
}
```

Essa notação do TypeScript para representar objetos de forma literal é muito útil quando não se tem uma classe para descrever objetos.

Na sequência ocorre uma pequena alteração no Template:

```html
<h1>Disciplinas</h1>
<hr>
<ul>
  <li *ngFor="let disciplina of disciplinas">
      <strong>{{disciplina.nome}}</strong>: {{disciplina.descricao}}
  </li>
</ul>
```

A principal mudança está no fato de que as interpolações usam expressões baseadas nos atributos do objeto `disciplina`.

Para ver o resultado, abra o aplicativo no browser.

## Melhorando o modelo de dados usando classes

Enquanto a utilização de objetos literais é bastante útil, também é interessante expressar o modelo de dados utilizando classes. Para isso, crie o arquivo `./src/app/disciplina.model.ts` com o conteúdo a seguir:

```typescript
export class Disciplina {
  nome: string;
  descricao: string;

  constructor(nome: string, descricao?: string) {
    this.nome = nome;
    this.descricao = descricao;
  }
}
```

Para utilizar mais recursos do TypeScript, a classe `Disciplina` possui dois atributos: `nome` e `descricao`, ambos do tipo `string`. O método `constructor()` recebe dois parâmetros, ambos do tipo `string`: `nome` e `descricao` \(que é opcional -- note o uso de `?`\). Para concluir, a classe é disponibilizada para uso externo por meio da instrução `export`.

Para usar a classe `Disciplina`, modifique o Controller do `AppComponent`:

```
import {Component} from '@angular/core';
import {Disciplina} from './disciplina.model';
...
export class AppComponent {
  disciplinas = [
    new Disciplina('Língua Portuguesa', 'O objetivo norteador da BNCC de Língua Portuguesa ' +
      'é garantir a todos os alunos o acesso aos saberes linguísticos necessários para a ' +
      'participação social e o exercício da cidadania, pois é por meio da língua que o ser ' +
      'humano pensa, comunica-se, tem acesso à informação, expressa e defende pontos de ' +
      'vista, partilha ou constrói visões de mundo e produz conhecimento.'),
    new Disciplina('Educação Física', 'A Educação Física é o componente curricular ' +
      'que tematiza as práticas corporais em suas diversas formas de codificação e ' +
      'significação social, entendidas como manifestações das possibilidades ' +
      'expressivas dos sujeitos e patrimônio cultural da humanidade. Nessa concepção, ' +
      'o movimento humano está sempre inserido no âmbito da cultura e não se limita a ' +
      'um deslocamento espaço-temporal de um segmento corporal ou de um corpo todo. ' +
      'Logo, as práticas corporais são textos culturais passíveis de leitura e produção.'),
    ...
  ];
}
```

Perceba que os elementos do atributo `disciplinas` agora são instâncias da classe `Disciplina`. O mais interessante é que o Template não precisa ser alterado, já que está utilizando objetos com a mesma estrutura de antes, ou seja, espera que os itens do atributo `disciplinas` sejam objetos que tenham os atributos `nome` e `descricao`.

## Adicionando interação com o usuário

A apresentação de todos os dados das disciplinas na tela pode ser melhorada usando interação com o usuário. Suponha que o usuário veja a lista das disciplinas contendo apenas seus nomes; ele deve clicar no nome de uma disciplina para ver sua descrição. Além disso, para dar uma resposta visual para o usuário quando ele clicar no nome da disciplina, considere que será usada uma formatação em negrito.

Para implementar esses requisitos vamos usar outras recursos do Angular.





