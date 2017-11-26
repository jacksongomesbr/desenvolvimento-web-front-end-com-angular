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

O componente **ListaDeDisciplinas** é um tipo de componente diferente do que vimos até agora: ele não funciona sozinho. Isso quer dizer que ele precisa ser inserido em outro componente para funcionar. No nosso contexto, o componente **App** inclui e utiliza o componente **ListaDeDisciplinas**. Vamos chamar o componente **App** de **host** \(hospedeiro\) e o componente **ListaDeDisciplinas** de **guest** \(hóspede\). Nessa comunicação o **host** deve fornecer recursos necessários para o **guest** funcionar, o que chamamos **input** \(entrada\). Por outro lado, quando o **guest** notifica o **host** sempre que fizer qualquer coisa importante, o que chamamos **output **\(saída\).

A figura a seguir ilustra o componente **ListaDeDisciplinas**.

![Ilustração do componente ListaDeDisciplinas no browser](/assets/intermediario-componente-listadedisciplinas-browser.png)

O componente apresenta uma lista de disciplinas. Cada item da lista contém:

* o nome da disciplina
* um botão para excluir a disciplina
* um botão para editar a disciplina

Os dois botões não executam as ações correspondentes \(excluir ou editar\) porque isso não é responsabilidade desse componente. O que o componente faz é notificar o **host** que o usuário deseja excluir ou editar determinada disciplina. Além disso o componente, ao apresentar a lista das disciplinas, precisa saber qual disciplina está sendo editada, para que destaque essa disciplina na lista \(perceba, na figura, o ícone do lápis e o fundo da linha com a cor cinza na disciplina "Língua Portuguesa"\).

A lista das disciplinas e a disciplina que está sendo editada são requeridas como **input**.

A disciplina para ser excluída e a disciplina para ser editada são fornecidas como **output**.

Portanto, fica a cargo do host fornecer input e tratar output. A figura a seguir ilustra essa relação entre os dois componentes.

![Diagrama demonstrando a interação entre os componentes App e ListaDeDisciplinas](/assets/interacao-componentes-app-listadedisciplinas.png)

O diagrama apresentado pela figura demonstra as conexões entre os componentes:

* **App **fornece as entradas para **ListaDeDisciplinas**
* **ListaDeDisciplinas** notifica **App** quando o usuário desejar excluir ou editar uma disciplina

> **\[info\] O diagrama dos componentes**
>
> A UML possui um diagrama para representar componentes de um software e as relações entre eles. Entretanto, por ser uma representação gráfica mais formal, requer que entradas e saídas sejam interfaces. Aqui adotamos uma simplificação desse diagrama para suportar o conceito de Input e Output do Angular.



![](/assets/software-componentes-app-listadedisciplinas.png)

