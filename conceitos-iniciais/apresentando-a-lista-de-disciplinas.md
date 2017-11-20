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

A diretiva `*ngFor` é utilizada para repetir um elemento do Template. Nesse caso, é justamente essa a sua aplicação: repetir o elemento `li` para apresentar os nomes das disciplinas. A repetição é realizada por uma expressão: `let disciplina of disciplinas` significa que o `*ngFor` vai repetir o elemento `li` para cada item do array `disciplinas` e cada item recebe o nome de `disciplina`, o que demonstra outra utilização do **Data binding**. Explicando a mesma coisa de outra forma:

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

Para implementar esses requisitos vamos usar outros recursos do Angular.

Começando pelo Controller do `AppComponent` são acrescentados:

* o atributo `selecionado`
* o método `selecionar()`

```
export class AppComponent {
  selecionado = null;
  disciplinas = [
    new Disciplina('Língua Portuguesa', 'O objetivo norteador da BNCC de Língua Portuguesa ' +
      'é garantir a todos os alunos o acesso aos saberes linguísticos necessários para a ' +
      'participação social e o exercício da cidadania, pois é por meio da língua que o ser ' +
      'humano pensa, comunica-se, tem acesso à informação, expressa e defende pontos de ' +
      'vista, partilha ou constrói visões de mundo e produz conhecimento.'),
  ...
  ];

  selecionar(disciplina) {
    this.selecionado = disciplina;
  }
}
```

O atributo `selecionado` será utilizado para que o Controller saiba qual disciplina o usuário selecionou. O usuário fará isso por meio de uma chamada para o método `selecionar()`, que recebe o parâmetro `disciplina` e o atribui para o atributo `selecionado`. Quando isso acontecer \(o usuário selecionar uma disciplina\) o software apresentará a sua descrição.

Para utilizar mais recursos do desenvolvimento front-end, dessa vez também alteramos o arquivo `./src/app/app.component.css`:

```css
.selecionado {
    font-weight: bold;
}

li p:nth-child(1) {
    cursor: pointer;
    font-size: 12pt;
}

li p:nth-child(1):hover {
    font-weight: bold;
}

li p:nth-child(2) {
    font-size: 10pt;
}
```

O arquivo CSS do `AppComponent` possui regras de formatação:

* seletor `.selecionado`: para deixar o nome da disciplina selecionada em negrito
* seletor `li p:nth-child(1)`: para que o cursor do mouse sobre o nome da disciplina seja `pointer` \(semelhante ao comportamento do cursor em links\) e tenha fonte de tamanho 12pt
* seletor `li p:nth-child(1):hover`: para que o nome da disciplina sob o mouse fique em negrito
* seletor `li p:nth-child(2)`: para que a fonte da descrição seja de tamanho 10pt

Agora, o Template:

```html
<h1>Disciplinas</h1>
<hr>
<ul>
  <li *ngFor="let disciplina of disciplinas" (click)="selecionar(disciplina)">
      <p [class.selecionado]="selecionado == disciplina">{{disciplina.nome}}</p>
      <p *ngIf="selecionado == disciplina">{{disciplina.descricao}}</p>
  </li>
</ul>
```

A primeira coisa a destacar é o recurso que proporciona a interação com o usuário. Como já comentado, o requisito determina que o usuário possa clicar no nome da disciplina. Isso é um ação do usuário para a interface. O Angular implementa isso por meio de **eventos** e de uma sintaxe própria: o atributo `(click)` no elemento `li` indica que estamos fornecendo um **tratador para o evento** `click`; o tratador é o valor do atributo, ou seja, uma expressão que representa a chamada do método `selecionar(disciplina)`, definido no Controller.

Outra característica importante é a chamada do método `selecionar()` fornece como parâmetro `disciplina`, que é o item da repetição realizada pelo `*ngFor`. Se você voltar no código do Controller vai ver que esse parâmetro é atribuído para o atributo `selecionado`, que faz justamente isso: guarda uma  referência para a disciplina selecionada.

O próximo passo é garantir o determinado no requisito no sentido da apresentação. Para fazer com que a disciplina selecionada fique em negrito vamos aplicar uma classe CSS chamada `selecionado` em um elemento `p` que contém o nome da disciplina sempre que a `disciplina` da repetição do `*ngFor` for igual à disciplina selecionada. O Angular fornece um recurso para modificar características de elementos do HTML por meio de expressões. Esse recurso está sendo usado na tag `<p>`: `[class.selecionado]` parece  como um atributo do HTML, mas é tratado pelo Angular antes de o HTML ser fornecido para o browser, modificando o atributo `class` e inserindo nele a classe CSS chamada `selecionado`. Nesse sentido, o valor do atributo é uma expressão e, nesse caso, uma expressão booleana: `selecionado == disciplina` determina, então, que o elemento `p` terá o atributo `class` com valor `selecionado` se a `disciplina` atual da iteração do `*ngFor` for igual à disciplina selecionada.

Para finalizar, o software deve apresentar a descrição da disciplina selecionada. Para isso vamos usar outra diretiva, a `NgIf`. Veja o trecho:

```html
<li *ngFor="let disciplina of disciplinas" ...>
    ...
    <p *ngIf="selecionado == disciplina">{{disciplina.descricao}}</p>
</li>
```

A diretiva `*ngIf` é aplicada no elemento `p` usado para apresentar a descrição da disciplina. Como o propósito dessa diretiva é funcionar como um condicional o valor que o atributo recebe é uma expressão booleanda: `selecionado == disciplina`. Se a expressão for verdadeira \(se a `disciplina` da repetição do `*ngFor` for igual à disciplina selecionada\), o elemento `p` estará presente no HTML, caso contrário, não estará presente. Perceba que usei o termo "estar presente" ao invés de "visível" porque é isso que acontece. Se a expressão booleanda for falsa, o elemento no qual a diretiva `*ngIf` é aplicada nem está presente no HTML.

Para ilustrar isso, veja um trecho da estrutura HTML no browser:

```html
<ul _ngcontent-c0="">
  <!--bindings={"ng-reflect-ng-for-of": "[object Object],[object Object"}-->
  <li _ngcontent-c0="">
      <p _ngcontent-c0="" class="selecionado">Língua Portuguesa</p>
      <!--bindings={"ng-reflect-ng-if": "true"}-->
      <p _ngcontent-c0="">O objetivo norteador da BNCC de Língua Portuguesa ...</p>
  </li>
  <li _ngcontent-c0="">
      <p _ngcontent-c0="" class="">Educação Física</p>
      <!--bindings={"ng-reflect-ng-if": "false"}-->
  </li>
  ...
</ul>
```

Os vários comentários presentes no HTML parecem ser algo que o Angular utiliza para tratar o comportamento da interface? É justamente isso que acontece \(não precisa tentar interpretar os comentários agora\). No caso desse exemplo, o usuário clicou na disciplina "Língua Portuguesa". Características importantes:

* para o elemento `li` correspondente à disciplina selecionada:
  * o elemento `p` que mostra o nome da disciplina possui `class="selecionado"`
  * o elemento `p` que mostra a descrição da disciplina está presente no HTML
* para os demais elementos `li`:
  * o elemento `p` que mostra o nome da disciplina não possui o atributo `class`
  * o elemento `p` que mostra a descrição da disciplina não existe no HTML

Se você já tiver programado para front-end anteriormente vai entender melhor a complexidade do que está acontecendo aqui, que o Angular não passa para o programador. O Angular está realizando a chamada **manipulação do DOM** para que o HTML entregue para o browser tenha o comportamento esperado \(ou explícito no software\).

Por fim, a figura a seguir ilustra o funcionando do software no browser.

![](/assets/software-listando-disciplinas-interacao-browser.png)

Pronto, esse capítulo mostrou a utilização de vários recursos do Angular para implementar o requisito "Apresentar a lista das disciplinas".

> **\[info\] Para resumir:**
>
> * utilizar recursos da Programação Orientada a Objetos é muito bom para representação do modelo de dados
> * a diretiva `NgForOf` é utilizada para repetir elementos no Template com base em uma coleção de itens \(um array\)
> * a diretiva `NgIf` é utilizada para incluir ou excluir elementos do Template com base em um condicional
> * `(click)` representa a sintaxe do Angular para criar tratadores de eventos
> * `[class.selecionado]` representa a sintaxe do Angular para adicionar ou remover atributos no HTML



