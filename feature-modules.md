# Feature-modules

> **\[info\] Objetivos do capítulo:**
>
> * demonstrar a modularização do software utilizando módulos
> * criando componentes em módulos
> * importando módulos
> * descrevendo rotas em módulos
>
> **Branch**: [livro-modulos-admin-publico-shared](https://github.com/jacksongomesbr/angular-escola/tree/livro-modulos-admin-publico-shared)

Todos os capítulos anteriores estão convergindo para um ponto: utilizar recursos do Angular para melhorar a arquitetura e estrutura do software. A figura a seguir dá uma ideia mais visual disso.

![Arquitetura do software demonstrando interações entre módulos e API](/assets/software-arquitetura-modulos-api.png)

O funcionamento esperado é o seguinte:

* a interface gráfica é representada pelo arquivo `src/index.html`
* `AppModule`, que é o **root module**, é apresentado na interface gráfica 
* `AppModule` importa os módulos `AdminModule` e `PublicoModule`. 
* `AdminModule` e `PublicoModule` importam o `SharedModule` e comunicam-se com a API HTTP REST.

Na prática, o `AppModule` funciona como um concentrador dos demais módulos, já que ele tem poucas funcionalidades específicas, por isso os demais módulos, que fornecem funcionalidades, são chamados **feature modules:**

* `AdminModule` é responsável pelas funcionalidades administrativas \(requerem acesso por login e senha\); 
* `PublicoModule` contém funcionalidades que não requererem acesso por login e senha. 
* `SharedModule` contém funcionalidades que são utilizadas por outros módulos.

Na sequência o texto demonstra os elementos dessa arquitetura.

## Estrutura do software

Apresentar uma figura ilustrando o diagrama de classes do software até aqui seria inútil por causa da quantidade de arquivos e artefatos do software até então. Entretanto, para ter uma ideia do tamanho e da complexidade do software como criado neste capítulo, as tabelas a seguir apresentam a relação de componentes em cada **feature module **e uma breve descrição de cada um deles.

**Módulo Admin**

| Componente | Descrição |
| :--- | :--- |
| `AdminComponent` | o **shell component** do módulo `Admin` |
| `CadastroDeDisciplinaComponent` | fornece um formulário para cadastro de uma disciplina |
| `CadastroDeTurmaComponent` | fornece um formulário para cadastro de uma turma |
| `DisciplinaComponent` | fornece a página inicial de uma disciplina, com dados para leitura ou consulta |
| `HomeComponent` | fornece a página inicial do módulo `Admin` |
| `ListaDeDisciplinasComponent` | fornece uma lista das disciplinas e dá acesso a outras funcionalidades: cadastrar, excluir e editar |
| `ListaDeTurmasComponent` | fornece uma lista das turmas e dá acesso a outras funcionalidades: cadastrar, excluir e editar |
| `PaginaNaoEncontradaComponent` | fornece uma página para ser apresentada quando uma URL não combinar com alguma das rotas do módulo `Admin` |
| `TurmaComponent` | fornece a página inicial de uma turma, com dados para leitura ou consulta |
| `AdminModule` | o módulo, em si |
| `AdminRoutingModule` | o módulo de rotas |
| `Disciplina` | o model de disciplina |
| `DisciplinasService` | o serviço com a lógica de negócio para o gerenciamento de disciplinas |
| `Turma` | o model de turma |
| `TurmasService` | o serviço com a lógica de negócio para o gerenciamento de turmas |

**Módulo Público**

| Componente | Descrição |
| :--- | :--- |
| LoginComponent | fornece um formulário para autenticação e acesso ao sistema |
| PublicoComponent | o **shell component** do módulo `Publico` |
| LogonService | o serviço com a lógica para autenticação do usuário |
| PublicoModule | o módulo, em si |
| PublicoRoutingModule | o módulo de rotas |

**Módulo Shared**

| Component | Descrição |
| :--- | :--- |
| AuthService | o serviço com a lógica para autenticação do usuário |
| SharedModule | o módulo, em si |



## Criando módulos

Para criar os elementos da arquitetura vamos utilizar o Angular CLI. Para começar, vamos criar os **feature modules**. Para fazer isso execute a linha de comando a seguir.

```
ng g m Admin --routing true
```

O comando cria o módulo `Admin`, na pasta `src/app/admin`, e os arquivos:

* `src/app/admin/admin.module.ts`: representa o módulo em si
* `src/app/admin/admin-routing.module.ts`: representa o **módulo de rotas**

O capítulo anterior demonstrou como trabalhar com o recurso de rotas no Angular. Entretanto o capítulo utilizou o formato de descrever as rotas no próprio módulo `AppModule`. Aqui neste capítulo, como há mais módulos, cada módulo possui, também, um **módulo de rotas**. Um módulo de rotas é um módulo que tem uma única responsabilidade: definir as rotas do módulo. Esse módulo será importado no **feature module** para que o Angular identifique corretamente as rotas e os seus componentes associados. O comando anterior especifica que deve ser criado um módulo de rotas por meio da opção `--routing true`.

## Criando componentes nos módulos

Para criar componentes em um módulo utilizamos o Angular CLI, usando a linha de comando a seguir:

```
ng g m Admin/Admin -m Admin --spec false
```

O comando cria o componente `Admin`, na pasta `src/app/admin/admin` e os arquivos do componente. Usar o `Admin/Admin` é uma forma de indicar para o Angular CLI que o componente `Admin` pertence ao módulo `Admin`. A opção `-m` é uma forma de garantir isso ainda mais. Além disso o comando também modifica o `AdminModule`, inserindo o AdminComponent no array `declarations`.

No padrão adotado neste livro todo **feature module** possui um componente de mesmo nome que é usado como **shell component** -- porque continuamos a usar o recurso de rotas.

Os demais componentes são criados de forma semelhante. Por exemplo, a criação do componente CadastroDeDisciplina:

```
ng g m Admin/CadastroDeDisciplina -m Admin --spec false
```

A linha de comando cria o componente `CadastroDeDisciplina` na pasta `src/app/admin/cadastro-de-disciplina` e modifica o `AppModule` para incluir esse componente no array `declarations`.



## Definindo rotas

Com a utilização de feature modules o recurso de rotas ganha uma utilidade ainda mais marcante e evidente. O fundamento adotado pelo Angular é que cada **feature module** pode possuir seu **módulo de rotas**. Posteriormente, cada módulo

