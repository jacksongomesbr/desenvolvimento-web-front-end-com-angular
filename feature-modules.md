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

Na sequência o texto demonstra a construção parcial dessa arquitetura. 

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

