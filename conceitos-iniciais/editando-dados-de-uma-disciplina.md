# Editando dados de uma disciplina

> **\[info\] Objetivos do capítulo:**
>
> * demonstrar a implementação do requisito "Editar dados de uma disciplina"
> * reforçar os conceitos anteriores
> * demonstrar a implementação mais completa de um CRUD
>
> **Branch:** [livro-crud-disciplina](https://github.com/jacksongomesbr/angular-escola/tree/livro-crud-disciplina)

Como o capítulo anterior, esse capítulo não é muito extenso em termos de conceitos novos. Entretanto, ele reúne os capítulos anteriores para implementar um CRUD, que significa:

* **C**reate \(cadastrar\)
* **R**etrieve \(recuperar, listar, consultar\)
* **U**pdate \(atualizar, editar\)
* **D**elete \(excluir\)

A figura a seguir ilustra a interface do software como construída ao final do capítulo.

![Software com o CRUD de Disciplinas](/assets/software-inicial-crud-disciplinas.png)

A interface continua com duas partes: uma apresentando uma lista e outra apresentando um formulário. Na lista, para cada item, há dois botões: "Excluir" e "Editar". Ao clicar no botão "Excluir" o software solicita uma confirmação do usuário. Ao clicar no botão "Editar" os dados da disciplina em questão são apresentados no formulário \(nome e descrição\). Nesse momento o formulário entra no modo de edição e:

* se o usuário clicar em "Salvar" os dados da disciplina serão atualizados
* se o usuário clicar em "Cancelar" qualquer alteração não será armazenada

O modo padrão do formulário é o cadastro. Nesse sentido se o usuário preencher os campos \(nome e descrição\) e:

* clicar em "Salvar", os dados serão armazenados em uma nova disciplina, apresentando-a na lista
* clicar em "Cancelar", os dados não serão armazenados

Vamos ao código-fonte. Primeiro, um trecho do Controller:

```typescript
...
export class AppComponent {
  editando = null;
  nome = null;
  descricao = null;
  disciplinas = [
    new Disciplina('Língua Portuguesa', 'O objetivo norteador da BNCC...'),
    ...
  ];

  salvar() {
    if (this.editando) {
      this.editando.nome = this.nome;
      this.editando.descricao = this.descricao;
    } else {
      const d = new Disciplina(this.nome, this.descricao);
      this.disciplinas.push(d);
    }
    this.nome = null;
    this.descricao = null;
    this.editando = null;
  }

  excluir(disciplina) {
    if (this.editando == disciplina) {
      alert('Você não pode excluir uma disciplina que está editando');
    } else {
      if (confirm('Tem certeza que deseja excluir a disciplina "'
          + disciplina.nome + '"?')) {
        const i = this.disciplinas.indexOf(disciplina);
        this.disciplinas.splice(i, 1);
      }
    }
  }

  editar(disciplina) {
    this.nome = disciplina.nome;
    this.descricao = disciplina.descricao;
    this.editando = disciplina;
  }

  cancelar() {
    this.nome = null;
    this.descricao = null;
    this.editando = null;
  }
}
```

As novidades são:

* adição do atributo `editando`: é inicializado com null e manterá uma referência para a disciplina que está sendo editada \(quando houver alguma\)
* atualização no método`salvar()`: para tratar a situação do formulário \(cadastrando ou editando disciplina\)
* atualização no método `excluir()`: para tratar a situação em que o usuário quer excluir uma disciplina que está sendo editada
* adição do método `editar()`

O método `salvar()` tem um novo comportamento: se o usuário estiver editando uma disciplina \(verifica o valor do atributo `editando`\), então usa os dados do formulário e atualiza os dados da disciplina que está sendo editada. Caso contrário, continua com o comportamento anterior: usa os dados do formulário para criar uma instância de `Disciplina` e adiciona no vetor `disciplinas`.

O método `excluir()` tem um novo comportamento: se o usuário estiver editando uma disciplina \(verifica o valor do atributo editando\), então informa o usuário que não é possível excluir a disciplina que está sendo editada. Caso contrário, continua com o comportamento normal: solicita uma confirmação do usuário e exclui a disciplina.

O método `editar()` recebe como parâmetro um objeto que representa a disciplina a ser editada, atualiza os campos do formulário com os atributos de `disciplina` e faz com que o atributo `editando` receba `disciplina` para indicar que o formulário está no modo de edição \(há uma disciplina sendo editada\).

O método `cancelar()` tem a função de reiniciar o formulário para os valores-padrão, atributo o valor `null` para os atributos `nome`, `descricao` e `editando`. 

Para concluir, o Template:

```html
<h1>Disciplinas</h1>
<hr>
<ul>
    <li *ngFor="let disciplina of disciplinas">
        {{disciplina.nome}}
        <button (click)="excluir(disciplina)">
            Excluir
        </button>
        <button (click)="editar(disciplina)">
            Editar
        </button>
    </li>
</ul>

<h2>Cadastrar</h2>
<p>Use este formulário para cadastrar ou editar uma disciplina.</p>
<form #cadastro="ngForm" (submit)="salvar()">
    <div>
        <label for="nome">Nome</label><br>
        <input type="text" id="nome" name="nome" [(ngModel)]="nome" required>
    </div>
    <div>
        <label for="descricao">Descrição</label><br>
        <textarea id="descricao" name="descricao" [(ngModel)]="descricao"
                  cols="70" rows="5"></textarea>
    </div>
    <button type="submit" [disabled]="!cadastro.valid">Salvar</button>
    <button type="reset" (click)="cancelar()">Cancelar</button>
</form>
```

Para ilustrar a interface completa, a figura a seguir ilustra o processo da interface CRUD para Disciplina.

![Relações entre os elementos do Template e do Controller](/assets/software-modelo-crud-template-component.png)


A figura demonstra as várias relações entre os elementos do Template e do Controller. Como ela resume os conceitos vistos neste e nos quatro capítulos anteriores, se você chegou aqui e está um pouco perdido, não se preocupe, pode acontecer. Reserve um pouco mais de tempo para estudar a figura e o código-fonte e, se necessário, volte nos capítulos anteriores.

O conjunto de funcionalidades implementadas até aqui demonstram vários recursos do Angular, mas a interface gráfica não é das melhores. No próximo capítulo vamos aprender como melhorar o aspecto visual do software.

