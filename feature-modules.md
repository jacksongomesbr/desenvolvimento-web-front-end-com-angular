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

O `AppModule`, que é o **root module** do projeto, é inserido na interface gráfica \(representada pelo arquivo `./src/index.html`\). O `AppModule` importa os demais módulos \(`AdminModule`, `SharedModule` e `PublicoModule`\). O módulos `AdminModule` e `PublicoModule` importam o `SharedModule` e comunicam-se com a API HTTP REST.

Na prática, o `AppModule` funciona como um concentrador dos demais módulos, já que ele tem poucas funcionalidades específicas. O `AdminModule` é responsável pelas funcionalidades administrativas \(requerem acesso por login e senha\). O `PublicoModule` contém funcionalidades que não requererem acesso por login e senha. O `SharedModule` contém funcionalidades que são utilizadas por outros módulos.



