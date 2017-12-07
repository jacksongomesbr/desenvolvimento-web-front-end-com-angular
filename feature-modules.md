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
| PublicoModule | o módulo, em si |
| PublicoRoutingModule | o módulo de rotas |

**Módulo Shared**

| Component | Descrição |
| :--- | :--- |
| AuthService | o serviço com a lógica para autenticação do usuário |
| SharedModule | o módulo, em si |

## Estrutura de Navegação

Conforme o software aumenta na quantidade de componentes e funcionalidades torna-se interessante uma visão geral da navegação -- até mesmo porque estamos utilizando rotas. A figura a seguir ilustra a navegação entre as telas \(ou páginas\) do software.

![Estrutura do site na forma de um mapa de navegação](/assets/feature-modules-navegacao.png)

Essa figura permite compreender que a primeira tela acessada é a tela de login. A partir dela o usuário tem acesso à tela home e às demais telas, como disciplinas e turmas. A partir dessas telas outras podem ser acessdas, como a página da disciplina ou a página da turma. As setas são bidirecionais para indicar os caminhos são de ida e volta.

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

Com a utilização de feature modules o recurso de rotas ganha uma utilidade ainda mais marcante e evidente. O fundamento adotado pelo Angular é que cada **feature module** pode possuir seu **módulo de rotas**. Posteriormente, o **root module** poderá importar o **feature module** que desejar.

O **módulo de rotas** para o módulo `Admin`, chamado `AdminRouting`, contém um conteúdo semelhante ao seguinte \(omitidas linhas com `import`\):

```typescript
...
const routes: Routes = [
  {
    path: 'admin', component: AdminComponent, children: [
      {path: 'disciplinas', component: ListaDeDisciplinasComponent},
      {path: 'disciplinas/:id', component: DisciplinaComponent},
      {path: 'disciplinas/:id/novo', component: CadastroDeDisciplinaComponent},
      {path: 'disciplinas/:id/editar', component: CadastroDeDisciplinaComponent},
      {path: 'cadastrar-turma', component: CadastroDeTurmaComponent},
      {path: 'turmas', component: ListaDeTurmasComponent},
      {path: 'turmas/:id', component: TurmaComponent},
      {path: '', component: HomeComponent},
      {path: '**', component: PaginaNaoEncontradaComponent}
  ]}
];

@NgModule({
  imports: [RouterModule.forChild(routes)],
  exports: [RouterModule]
})
export class AdminRoutingModule {
}
```

Aqui o array `routes` ganha uma característica diferenciada: o primeiro elemento, cujo atributo `path` tem valor `'admin'`, está relacionado ao componente `AdminComponent` e possui o atributo `children`, que é um array de rotas. O recurso que permite definir esses tipos de rotas é chamado de **rotas filhas**. Assim, a rota `'admin'` possui várias **rotas filhas**. Além disso essa é a forma de indicar para o Angular que o `AdminComponent` deve ser usado como **shell component**, ou seja, o Template dele contém o elemento `router-outlet`.

Note também que há uma mudança na forma de indicar as rotas para os metadados do módulo: o atributo `imports` contém uma chamada para `RouterModule.forChild()`, que recebe como parâmetro o array `routes`. Esse é uma diferença marcante para a forma de indicar as rotas para o **root module**, quando é utilizado o método `RouterModule.forRoot()`.

Outro passo necessário é importar o módulo de rotas no feature module. A seguir, um trecho do AdminModule:

```typescript
...
@NgModule({
  imports: [
    CommonModule,
    FormsModule,
    HttpClientModule,
    NgbModule,
    AdminRoutingModule,
    SharedModule
  ],
  ...
})
export class AdminModule {
}
```

O penúltimo elemento do array `imports` é o módulo de rotas \(`AdminRoutingModule`\).

O módulo de rotas para o módulo `Publico`, chamado `PublicoRouting`, contém um conteúdo semelhante ao seguinte:

```typescript
...
const routes: Routes = [
  {
    path: '', component: PublicoComponent, children: [
      {path: '', component: LoginComponent}
    ]
  }
];

@NgModule({
  imports: [RouterModule.forChild(routes)],
  exports: [RouterModule]
})
export class PublicoRoutingModule {
}
```

O recurso de rotas filhas também é utilizado aqui. `PublicoComponent` é o shell component. O fato interessante é que o atributo `path` contém o mesmo valor `''` nas duas rotas. Como o Angular considera `''` a rota padrão, isso quer dizer que o `PublicoModule` tem maior prioridade no momento em o Angular procurar encontrar uma rota com base na URL.

## Incluindo as rotas dos feature modules no root module

O último passo desse processo é importar os **feature modules** no **root module**. A seguir, um trecho do `AppModule`:

```typescript
...
@NgModule({
  imports: [
    BrowserModule,
    NgbModule.forRoot(),
    FormsModule,
    HttpClientModule,
    AdminModule,
    PublicoModule,
    AppRoutingModule
  ],
...
})
export class AppModule {
}
```

Os módulos `AdminModule`, `PublicoModule` e `AppRoutingModule` são importados por meio do atributo `imports` dos metadados da classe `AppModule`. Como os dois primeiros importam seus respectivos módulos de rotas, o root module também tem conhecimento delas e, assim, consegue aplicar o mesmo processo anterior de procura de rotas quando o browser fornece uma URL.

Um aspecto importante é que a importação segue uma ordem. Veja que, primeiro, é importado o módulo `AdminModule`, depois o `PublicoModule` e, por fim, o `AppRoutingModule`. Como você viu no capítulo anterior o processo de procura de rotas percorre uma lista de rotas do início até encontrar uma rota correspondente. Nesse caso a ordem das importações dos módulos quer dizer para o Angular considerar, primeiro, as rotas do `AdminModule`. Isso é muito importante porque as rotas do `PublicoModule`, por serem a rota padrão, devem estar no final do processo de procura de rotas.

## Shared modules

O Angular utiliza o conceito de **shared modules** para indicar módulos que fornecem recursos que são usados por outros. Conforme a arquitetura do software até o momento o serviço `AuthService` implementa a lógica de negócio de autenticação. Para ele poder ser usado nos módulos `AdminModule` e `PublicoModule` esse elemento do software deve estar em um módulo compartilhado. Se ele estivesse, por exemplo, no módulo `PublicoModule` e o `AdminModule` o importasse isso geraria problemas de ordem ou ciclos de importação.

Assim, a solução é isolar o AuthService em outro módulo, no caso o **SharedModule**. Na prática, um **shared module** é também um **feature module**, mas, provavelmente, ele apenas fornecerá recursos para serem utilizados em outros **feature modules**.

Posteriormente, cada **feature module** importa o **shared module** quando precisar utilizar seus recursos.

## Fluxo de trabalho

O fluxo de trabalho do capítulo anterior acaba de ser atualizado -- e também aumentou em complexidade. Da mesma forma como antes, seguir os passos do processo também ajuda a compreendê-lo:

1. **criar feature module**: essa agora é a primeira coisa a fazer;
2. **criar componente**: crie os componentes do **feature module**. Continue conduzido esse processo de forma iterativa, não se preocupe em construir o componente por inteiro, deixe-o como criado pelo Angular CLI;
3. **definir rotas**: depois, defina as rotas no **módulo de routas**;
4. **implementar a lógica de negócio em um serviço**: utilize serviços para implementar a lógica de negócio; e
5. **implementar lógica do componente e usar o serviço**: volte a trabalhar com cada compomente.



