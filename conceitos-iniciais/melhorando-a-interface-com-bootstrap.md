# Melhorando a interface gráfica do CRUD de Disciplinas

> **\[info\] Objetivos do capítulo:**
>
> * demonstrar a melhoria da lista de disciplinas utilizando tabelas
> * apresentar o Bootstrap como framework front-end
> * demonstrar a utilização do Bootstrap para melhorar o aspecto visual geral do software
>
> **Branches**: [livro-crud-disciplinas-tabelas](https://github.com/jacksongomesbr/angular-escola/tree/livro-crud-disciplinas-tabelas) e [livro-crud-disciplinas-bootstrap](https://github.com/jacksongomesbr/angular-escola/tree/livro-crud-disciplinas-bootstrap)

Eu concordo contigo. Um software pode até funcionar muito bem, mas o visual descuidado certamente tira alguns pontos de qualquer avaliação. Por isso, vamos começar a melhorar o aspecto visual do CRUD de disciplinas construído até aqui em dois passos. Primeiro, vamos usar tabelas para melhorar a apresentação da lista de disciplinas.

O código-fonte do Controller não precisa ser alterado, apenas do Template. Vamos aos trechos mais importantes:

```html
<h1>Disciplinas</h1>
<hr>
<table>
    <thead>
        <tr>
            <th>Disciplina</th>
            <th>Ações</th>
        </tr>
    </thead>
    <tbody>
        <tr *ngFor="let disciplina of disciplinas">
            <td>{{disciplina.nome}}</td>
            <td>
                <button (click)="excluir(disciplina)">
                    Excluir
                </button>
                <button (click)="editar(disciplina)">
                    Editar
                </button>
            </td>
        </tr>
    </tbody>
</table>

<h2>Cadastrar</h2>
...
```

O diferencial até aqui é a utilização de tabelas \(elemento `table`\) para a apresentação da lista de tabelas. Perceba que a diretiva `*ngFor` foi aplicada no elemento `tr` do `tbody`. O resultado da execução do software é ilustrado pela figura a seguir.

![Execução do software no browser demonstrando o uso de tabelas para apresentar a lista das disciplinas](/assets/software-crud-disciplinas-com-tables-browser.png)

Perceba que o fato de utilizar tabelas torna a exibição da lista de disciplinas mais visualmente confortável, já que os elementos estão alinhados adequadamente.

## Bootstrap e amigos

Se, no mínimo, você já tiver feito algum trabalho de desenvolvimento web muito provavelmente já ouviu falar do Bootstrap, framework CSS que se tornou muito popular nos últimos anos. Para melhorar a aparência visual da interface gráfica do software vamos utilizar um conjunto de ferramentas:

* [**ng-bootstrap**](https://ng-bootstrap.github.io/#/home): pacote Angular que implementa comportamento do Bootstrap no formato do Angular
* **bootstrap**: framework CSS e interface gráfica para front-end
* **font-awesome**: biblioteca de ícones

O primeiro passo é adicionar essas bibliotecas ao projeto, para fazer isso utilize o npm:

```
npm install --save @ng-bootstrap/ng-bootstrap bootstrap@4.0.0-beta.2 font-awesome
```

Quando a execução dessa linha de comando concluir o arquivo `./package.json` terá sido modificado para incluir essas dependências.

A próxima etapa é configurar o arquivo `./.angular-cli.json` para incluir os dessas dependências:

```js
{
  "$schema": "./node_modules/@angular/cli/lib/config/schema.json",
  "project": {
    "name": "Angular Escola"
  },
  "apps": [
    {
      "root": "src",
      ...
      "styles": [
        "styles.css",
        "../node_modules/bootstrap/dist/css/bootstrap.css",
        "../node_modules/font-awesome/css/font-awesome.css"
      ],
      ...
      }
    }
  ],
  ...
}
```

Entendendo que esse arquivo representa um objeto JSON, a modificação está em incluir duas linhas no array `apps.styles` indicando os arquivos de estilos do **bootstrap** e do **font-awesome**, respectivamente.

Na sequência, antes de mostrar algumas das alterações realizadas no Template, a figura a seguir ilustra o software em execução no browser.

![Resultado das melhorias na interface em execução no browser](/assets/software-interface-bootstrap-browser.png)

As modificações estéticas são perceptíveis: utilização de fontes, cores, ícones, utilização de mais espaço de tela, distribuindo melhor os elementos visuais. 

São várias as modificações no Template, começando pela inclusão de todo o conteúdo dentro de um elemento `div` com a classe CSS `container`, que é uma das mais básicas do **bootstrap**.

```html
<div class="container">
...
</div>
```

Na sequência, o elemento table também passa por melhorias:

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

As classes CSS `table`, `table-hover` e `table-bordered` são aplicadas no elemento `table`.

A classe `thead-light` é aplicada no elemento `thead`.

A classe `table-active` é aplicada no elemento `tr` filho de `tbody` quando a disciplina sendo editada \(atributo `editando`\) é igual à `disciplina` da iteração do `*ngFor`.

São adicionados ícones aos botões utilizando classes CSS do **font-awesome**:

* no botão "Excluir": o ícone da leixeira é adicionado por meio de `<i class="fa fa-trash"></i>`
* no botão "Editar": o ícone do lápis é adicionado por meio de `<i class="fa fa-pencil-square"></i>`

Há outras melhorias, como a apresentação de notificações das operações de excluir, cadastrar e editar, que também usam recursos do bootstrap e requerem uma alteração na lógica do Controller, mas vou deixar como exercício para você mesmo perceber isso.

Esse capítulo finaliza a seção "Conceitos iniciais". Para concluir, um resumão dos conceitos vistos até agora:

* o Angular é um **Framework** front-end
* há vários elementos na Arquitetura do Angular que determinam a forma de desenvolver software Angular, dentre eles os conceitos de **Model, View \(Template\) e Controller \(View-Model\)**
* o **Angular CLI** é uma ferramenta de linha de comando que auxilia e torna mais rápido o desenvolvimento com Angular
* o Angular possui uma sintaxe própria para interpretar os conteúdos dos Templates \(**é como um HTML++**\)
* o recurso de **interpolação** em Templates é muito útil e provavelmente um dos mais utilizados
* as diretivas `*ngFor` e `*ngIf` são fundamentais em Templates para gerar conteúdo para o browser
* a integração entre Template e Controller fornecida pelo **Data binding** é um recurso essencial
* o recurso de **eventos** permite interação com o usuário
* utilizar **bootstrap e font-awesome** permite melhorar a estética da interface gráfica

É isso aí! Até a próxima!

