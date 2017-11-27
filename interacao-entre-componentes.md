# Interação entre componentes

> Objetivos do capítulo:
>
> * demonstrar a modularização do código utilizando componentes
> * demonstrar a integração entre componentes usando input e output
>
> **Branches**: [livro-componentes-lista](https://github.com/jacksongomesbr/angular-escola/tree/livro-componentes-lista) e [livro-componentes-editor](https://github.com/jacksongomesbr/angular-escola/tree/livro-componentes-editor).

Uma vez que a quantidade de funcionalidades em um componente começa a aumentar, prejudicando, por exemplo, a legibilidade e a organização do código-fonte, o componente torna-se um candidato a passar por um processo de modularização.

O `AppComponent`, no momento, fornece as funcionalidades de CRUD de disciplinas, ou seja:

* Listar disciplinas
* Cadastrar disciplinas
* Editar disciplinas
* Excluir disciplinas

Como alternativa ao fato de haver um só componente realizando todas as essas funcionalidades vamos utilizar o conceito de componentes do Angular e criar níveis de interações entre eles.

## Criando componentes

O **Angular CLI** é a ferramenta ideal para criar conteúdo para o software, já vimos isso quando iniciamos o próprio projeto a partir de uma linha de código. Outro comando importante é o **generate** \(cujo atalho é **g**\). Ele é usado para criar conteúdo como componentes, módulos e serviços. Para cada tipo de comando há uma opção, como **c**, atalho para **component**.

Vamos começar pelo componente `ListaDeDisciplinas`, que implementa a funcionalidade de listar disciplinas. Para criá-lo use a linha de comando:

```
ng g c ListaDeDisciplinas --spec false
```

O comando cria o diretório `./src/app/lista-de-disciplinas` e, dentro dele, os arquivos necessários para o componente, ou seja CSS, Template e Controller.

## Componente lista de disciplinas

O componente **ListaDeDisciplinas** é um tipo de componente diferente do que vimos até agora: ele não funciona sozinho. Isso quer dizer que ele precisa ser inserido em outro componente para funcionar. No nosso contexto, o componente **App** inclui e utiliza o componente **ListaDeDisciplinas**. Vamos chamar o componente **App** de **host** \(hospedeiro\) e o componente **ListaDeDisciplinas** de **guest** \(hóspede\). Nessa comunicação o **host** deve fornecer recursos necessários para o **guest** funcionar, o que chamamos **input** \(entrada\). Por outro lado, quando o **guest** notifica o **host** sempre que fizer qualquer coisa importante, o que chamamos **output **\(saída ou evento\).

Pode ser que não tenha te ocorrido, mas é uma analogia bem interessante. Imagine que você está hospedando um convidado na sua casa. Você é o host, enquanto seu convidado é o guest. Sua casa é onde você irá receber seu convidado. O convidado precisa de um lugar para dormir, então você fornece para ele uma cama \(input\). Seu convidado te notifica quer ir embora \(output\), então você se despede dele e o encaminha até a saída. Pensou naquele convidado exigente \(chato\)? Com um componente guest é a mesma coisa: muita necessidade e muita notificação!

A figura a seguir ilustra o componente **ListaDeDisciplinas**.

![Ilustração do componente ListaDeDisciplinas no browser](/assets/intermediario-componente-listadedisciplinas-browser.png)

O componente apresenta uma lista de disciplinas. Cada item da lista contém:

* o nome da disciplina
* um botão para excluir a disciplina
* um botão para editar a disciplina

Os dois botões não executam as ações correspondentes \(excluir ou editar\) porque isso não é responsabilidade desse componente. O que o componente faz é **notificar o** **host** que o usuário deseja excluir ou editar determinada disciplina. Além disso o componente, ao apresentar a lista das disciplinas, precisa saber qual disciplina está sendo editada, para que destaque essa disciplina na lista \(perceba, na figura, o ícone do lápis e o fundo da linha com a cor cinza na disciplina "Língua Portuguesa"\). Assim:

* **input**: a lista das disciplinas e a disciplina que está sendo editada; e
* **output**: a disciplina que o usuário deseja excluir ou editar.

O **host** utiliza \(ou recebe\) **guest**, então ele deve fornecer os inputs requeridos. Além disso, o host também pode realizar alguma ação quando for notificado de que o usuário deseja realizar uma exclusão ou uma edição de uma disciplina.

A figura a seguir ilustra essa relação entre os dois componentes.

![Diagrama demonstrando a interação entre os componentes App e ListaDeDisciplinas](/assets/interacao-componentes-app-listadedisciplinas.png)

O diagrama apresentado pela figura demonstra as conexões entre os componentes:

* **App **fornece as entradas para **ListaDeDisciplinas**
  * os atributos `disciplinas` e `editando` de `AppComponent` são vinculados aos **input** `disciplinas` e `editando` do `ListaDeDisciplinasComponent`
* **ListaDeDisciplinas** notifica **App** quando o usuário desejar excluir ou editar uma disciplina
  * o método `editar()` do `AppComponent` é executado quando ocorrer o output `onEditar`
  * o método `excluir()` do `AppComponent` é executado quando ocorrer o output `onExcluir`

> **\[info\] O diagrama dos componentes**
>
> A UML possui um diagrama para representar componentes de um software e as relações entre eles. Entretanto, por ser uma representação gráfica mais formal, requer que entradas e saídas sejam interfaces. Aqui adotamos uma simplificação desse diagrama para suportar o conceito de Input e Output do Angular.

Uma observação importante: os nomes dos atributos das classes \(host e guest\) não precisam ser os mesmos.

Antes de utilizar o componente **ListaDeDisciplinas** no **App**, vamos ver detalhes da sua composição. Primeiro, o Controller:

```typescript
import {Component, EventEmitter, Input, OnInit, Output} from '@angular/core';

@Component({
  selector: 'app-lista-de-disciplinas',
  ...
})
export class ListaDeDisciplinasComponent implements OnInit {
  @Input()
  disciplinas = null;

  @Input()
  editando = null;

  @Output()
  onEditar = new EventEmitter<any>();

  @Output()
  onExcluir = new EventEmitter<any>();

  constructor() {
  }

  ngOnInit() {
  }

  excluir(disciplina) {
    this.onExcluir.emit(disciplina);
  }

  editar(disciplina) {
    this.onEditar.emit(disciplina);
  }
}
```

Atenção para os atributos `disciplinas` e `editando`. Ambos são anotados com `@Input()`, o que indica para o Angular tratá-los como **input**. Os atributos `onEditar` e `onExcluir` são anotados com `@Output()`, o que indica para o Angular tratá-los como **output**. Além disso, perceba também mudanças na linha do `import`.

Na prática, os atributos `onEditar` e `onExcluir` definem eventos, por isso são inicializados com uma instância de `EventEmitter`, a classe do Angular para representar eventos. Note, na instanciação de `EventEmitter`, que o código define o tipo de dados do evento utilizando a notação de diamante: `<any>`. Isso é melhor interpretado assim: um evento pode carregar consigo alguma informação \(dados\) e, para isso, é importante indicar o tipo desses dados.

Os métodos `excluir(disciplina)` e `editar(disciplina)` são utilizados, respectivamente, como tratadores de eventos para quando o usuário clicar nos botões correspondentes, escolhendo excluir ou editar uma disciplina. O importante aqui é que, como estamos utilizando eventos \(**output**\), não queremos que seja responsabilidade desse componente **ListaDeDisciplinas** realizar essas tarefas. Pelo contrário, o esperado é notificar o **host** de que isso aconteceu. Para indicar para o **host** que uma disciplina deve ser excluída é usada a chamada para o método `this.onExcluir.emit(disciplina)`. Isso fará um "disparo" do evento, notificando o **host** de que ele ocorreu, e incluirá, como dado do evento, a disciplina em questão \(o objeto `disciplina`\). Da mesma forma, para notificar o host que deve editar uma disciplina é usada uma chamada para o método `this.onEditar.emit(disciplina)`.

O Template do **ListaDeDisciplinas** contém:

```html
<table class="table table-hover table-bordered">
  <thead class="thead-light">
  <tr>
    <th>Disciplina</th>
    <th width="200">Ações</th>
  </tr>
  </thead>
  <tbody>
  <tr *ngFor="let disciplina of disciplinas"
      [class.table-active]="editando == disciplina">
    <td>
      <i *ngIf="editando == disciplina"
         class="fa fa-pencil-square"></i>
      {{disciplina.nome}}
    </td>
    <td>
      <button class="btn btn-sm btn-danger"
              (click)="excluir(disciplina)">
        <i class="fa fa-trash"></i> Excluir
      </button>
      <button class="btn btn-sm btn-primary"
              (click)="editar(disciplina)">
        <i class="fa fa-pencil-square"></i> Editar
      </button>
    </td>
  </tr>
  </tbody>
</table>
```

Percebeu semelhança com o Template do capítulo anterior? Isso não é coincidência. Foi realmente isso que aconteceu. Tanto é que a melhor dica para se trabalhar com múltiplos componentes em Angular, pelo menos enquanto você for um dev iniciante, é usar apenas um componente e implementar nele todas as funcionalidades desejadas. Depois, a tarefa é "recortar" partes desse componente e usar em outros componentes.

A forma pela qual o host incorpora o guest e informa o vínculo com input e output é por meio do Template. Veja o trecho a seguir, do Template do componente **App**:

```html
<app-lista-de-disciplinas 
    [disciplinas]="disciplinas" 
    [editando]="editando"
    (onEditar)="editar($event)" 
    (onExcluir)="excluir($event)">
</app-lista-de-disciplinas>
```

Primeiro, o elemento `app-lista-de-disciplinas` é utilizado para indicar para o Angular que nesse local deve ser apresentado o componente **ListaDeDisciplinas**. Isso acontece por causa do `selector` do componente **ListaDeDisciplinas** \(veja o Controller desse componente\).

Segundo, a sintaxe que tem um atributo HTML com nome entre colchetes você já conhece. É a mesma que já utilizamos para definir, por exemplo, a classe CSS de um elemento do HTML com base em uma condição \(ex: o fundo da linha da tabela na cor cinza quando a disciplina está sendo editada\). Então o trecho `[disciplinas]="disciplinas"` indica o uso de Data Binding: o input chamado `disciplinas` está vinculado ao atributo `disciplinas`. Da mesma forma o trecho `[editando]="editando"` usa Data binding para vincular o input `editando` ao atributo `editando`.

Por fim, a sintaxe que tem um atributo HTML com nome entre parênteses você também já conhece. É a mesma que já utilizamos para tratar eventos \(ex: excluir uma disciplina quando o usuário clicar em um botão associado a ela\). Então o trecho `(onExcluir)="excluir($event)"` indica o uso de um tratador de eventos: o método `excluir()` deve ser chamado passando `$event` como parâmetro quando ocorrer o evento `onExcluir` do componente **ListaDeDisciplinas**. É importante observar que `$event` é uma forma de acessar os dados do evento quando disparado no componente **ListaDeDisciplinas**. Nesse caso, do evento `onEditar`, o dado é a disciplina que deve ser excluída. O mesmo comportamento acontece para o evento `onEditar`.

A figura a seguir procura resumir todas as informações apresentadas até aqui e procura demonstrar a interação entre os componentes **App** e **ListaDeDisciplinas**.

![Interações entre os componentes App e ListaDeDisciplinas](/assets/software-componentes-app-listadedisciplinas.png)

O repositório [livro-componentes-editor](#) ainda fornece mais um nível de criação de componentes, demonstrando o código para o componente EditorDeDisciplina, que fornece um formulário para cadastro e para edição de uma disciplina. Fica como exercício para fixação da aprendizagem o estudo do código desse repositório.

> **\[info\] Resumo do capítulo:**
>
> * Utilizamos Angular CLI para criar um componente
> * Um **host** é um componente que utiliza ou inclui um outro, chamado de **guest**
> * Um **guest** não funciona sem ser utilizado em um host
> * O conceito de **Input** é utilizado para o host fornecer entrada para o guest
> * O conceito de **Output** é utilizado para o guest notificar o host de que algo \(um evento\) aconteceu
> * A interação entre componentes é mais um recurso que usa Data binding para vincular entradas do guest a atributos do host



