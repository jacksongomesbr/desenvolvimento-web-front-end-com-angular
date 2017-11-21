# Editando dados de uma disciplina

> \[info\] Objetivos do capítulo:
>
> * demonstrar a implementação do requisito "Editar dados de uma disciplina"
> * reforçar os conceitos anteriores
> * demonstrar a implementação mais completa de um CRUD

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

```
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

* atributo `editando`: é inicializado com null e manterá uma referência para a disciplina que está sendo editada \(quando houver alguma\)
* método `salvar()`: 



