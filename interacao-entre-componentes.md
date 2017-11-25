# Interação entre componentes

> Objetivos do capítulo:
>
> * demonstrar a modularização do código utilizando componentes
> * demonstrar a integração entre componentes usando input e output
>
> Branches: livro-componentes-lista

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



![](/assets/software-componentes-app-listadedisciplinas.png)

