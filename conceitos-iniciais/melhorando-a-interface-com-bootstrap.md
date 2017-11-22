# Melhorando a interface gráfica do CRUD de Disciplinas

> **\[info\] Objetivos do capítulo:**
>
> * demonstrar a melhoria da lista de disciplinas utilizando tabelas
> * apresentar o Bootstrap como framework front-end
> * demonstrar a utilização do Bootstrap para melhorar o aspecto visual geral do software

Não se preocupe. Eu concordo contigo. Um software pode até funcionar muito bem, mas o visual descuidado certamente tira alguns pontos de qualquer avaliação. Por isso, vamos começar a melhorar o aspecto visual do CRUD de disciplinas construído até aqui em dois passos. Primeiro, vamos usar tabelas para melhorar a apresentação da lista de disciplinas.

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

O diferencial até aqui é a utilização de tabelas \(elemento `table`\) para a apresentação da lista de tabelas. Perceba que a diretiva `*ngFor` foi aplicada no elemento `tr` do `tbody`.

