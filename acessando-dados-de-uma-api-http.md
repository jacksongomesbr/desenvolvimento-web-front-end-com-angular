# Acessando dados de uma API HTTP

> **\[info\] Objetivos do capítulo:**
>
> * reforçar o conceito de requisição assícrona utilizando a classe HttpClient
> * demonstrar a utilização da classe HttpClient para acessar dados de uma API HTTP REST
> * demonstrar como lidar com situações de sucesso e erro ao lidar com requisições assíncronas
>
> **Branch**: [livro-dados-externos-api-http-rest](https://github.com/jacksongomesbr/angular-escola/tree/livro-dados-externos-api-http-rest)

O capítulo [Acessando dados externos de forma assíncrona](/acessando-dados-externos-de-forma-assincrona.md) apresentou como utilizar recursos do Angular para acessar dados de um arquivo JSON. Entretanto, a limitação daquela abordagem, embora seja suficiente para várias situações, precisa de uma alternativa mais completa, que permita a persistência dos dados. Uma solução para essa questão é utilizar uma API HTTP. Vamos utilizar uma API HTTP REST disponível no repositório [angular-escola-api](https://github.com/jacksongomesbr/angular-escola-api). Este capítulo não apresenta detalhes de como fazer essa API funcionar, mas vez ou outras algumas informações serão apresentadas.

## Iniciando o servidor web da API

Depois de clonar o repositório da API execute o comando:

```
npm start
```

Após alguns instantes o servidor estará disponível para aceitar requisições. Se quiser testar a API diretamente pelo browser acesse o endereço da API, indicado na mensagem de saída do comando anterior.

## Modificando o DisciplinasService

Como você já sabe o serviço DisciplinasService do capítulo anterior está fazendo requisições para o servidor web de desenvolvimento em busca do arquivo JSON que contém a lista das disciplinas. Para consultar a API a primeira alteração está na estrutura do serviço:

* remover todos os atributos
* adicionar o atributo `API_URL` com o valor `"http://localhost:3000"` \(que é o endereço padrão do servidor web da API\)
* remover o método `carregarDados()`
* alterar o código dos métodos `todas()`, `salvar()`, `excluir()` e `encontrar()`.

Assim, o serviço DisciplinasService fica dessa maneira:

```
...
@Injectable()
export class DisciplinasService {
  API_URL = 'http://localhost:3000';

  constructor(private http: HttpClient) { }

  todas() {
    return this.http.get(this.API_URL + '/disciplinas');
  }

  salvar(id: number, nome: string, descricao: string) {
    const disciplina = {nome: nome, descricao: descricao};
    if (id) {
      return this.http.patch(this.API_URL + '/disciplinas/' + id, disciplina);
    } else {
      return this.http.post(this.API_URL + '/disciplinas', disciplina);
    }
  }

  excluir(disciplina: number | Disciplina) {
    let id;
    if (typeof(disciplina) === 'number') {
      id = disciplina;
    } else {
      id = disciplina.id;
    }
    return this.http.delete(this.API_URL + '/disciplinas/' + id);
  }

  encontrar(arg: number) {
    return this.http.get(this.API_URL + '/disciplinas/' + arg);
  }
}
```

O método `todas()` utiliza o método `get()` do `HttpClient` para realizar uma requisição **GET** para a URL `/disciplinas` \(a partir da URL da API\) para retornar a lista das disciplinas. A API retorna um array de objetos com os atributos: `id`, `codigo`, `nome` e `descricao`. Da mesma forma como antes \(no antigo método `carregarDados()`\)  os dados são convertidos para objetos automaticamente quando a requisição HTTP for realizada -- lembre-se que é preciso chamar o método `subscribe()` para isso.

O método `salvar()` recebe os dados de uma disciplina como parâmetro \(`id`, `nome`, `descricao`\), define um objeto chamado `disciplina` com os valores dos parâmetros e checa o valor do parâmetro `id` para saber se é necessário:

* **alterar**: nesse caso o `id` possui algum valor e é feita uma requisição **PATCH **para a URL da disciplina na API, por exemplo `/disciplinas/1`, por meio do método `patch()` da classe `HttpClient`; ou
* **cadastrar**: nesse caso o `id` não possui valor \(ex: `null`\) e é feita uma requisição **POST **para a URL `/disciplinas` da API por meio do método `post()` da classe `HttpClient`.

Perceba que nos dois casos o segundo parâmetro da chamada dos métodos `patch()` e `post()`é o objeto `disciplina`. Isso é necessário porque a API espera que os dados estejam em formato JSON, o que também é feito automaticamente pelo Angular \(converte o objeto `disciplina` para sua representação em JSON ao enviar os dados para a API\). Além disso, da mesma forma como acontece com o método `get()` da classe `HttpClient`, a requisição HTTP só acontece, efetivamente, quando é chamado o método `subscribe()`.

O método `excluir()` recebe o parâmetro `disciplina`, que pode ser um número \(o identificador da disciplina\) ou o objeto que representa a disciplina. Com base no tipo do parâmetro o código obtém o identificador da disciplina e o utiliza para fazer uma requisição **DELETE** para a URL da API que representa a disciplina \(ex: `/disciplinas/1`\). Isso é feito por meio de uma chamada para o método `delete()` da classe `HttpClient`.

Por último, o método `encontrar()` recebe o identificador da disciplina como parâmetro e faz uma requisição **GET** para a URL da API que representa a disciplina por meio do método `get()` da classe `HttpClient`.

## Modificando o AppComponent

As alterações mais expressivas acontecem no Controller do `AppComponent` para usar o serviço `DisciplinasService`. Primeiro, o `constructor()` chama o novo método `atualizarLista()`:

```typescript
constructor(private disciplinasService: DisciplinasService) {
  this.atualizarLista();
}

atualizarLista() {
  this.disciplinasService.todas()
    .subscribe(disciplinas => {
      this.disciplinas = disciplinas;
      this.status_lista = true;
    }, () => this.status_lista = false);
}
```

O método `atualizarLista()` chama o método `todas()` do `DisciplinasService`. Perceba que, porque o retorno do método é um `Observable`, é possível chamar o método `subscribe()` logo em seguida. Como de costume, uma figura para explicar isso em partes:

![Ilustração da estrutura de callback de sucesso e erro](/assets/acessando-dados-api-exemplo-subscribe-sucesso-erro.png)

A figura destaca, na cor verde, o código da callback de sucesso. Lembre-se que a callback de sucesso executa se a requisição para a API for bem sucedida \(código de retorno igual 200\). Nesse caso a **arrow function** tem o parâmetro disciplinas e o corpo da função faz:

* atribui `disciplinas` para `this.disciplinas` \(a lista das disciplinas, que o Template utiliza\)
* atribui `true` para `this.status_lista` \(um valor de controle utilizado no Template\)

Ainda na figura, a cor vermelha destaca a callback de erro. A callback de erro executa se a requisição para a API não for bem sucedida \(código de retorno diferente de 200, como 404 ou 500\). A **arrow function** não tem parâmetro e seu corpo atribui `false` para `this.status_lista`.

O método `salvar()` chama o método `salvar()` do `DisciplinasService`:

```
salvar() {
  this.disciplinasService.salvar(this.editando.id, 
     this.editando.nome, this.editando.descricao)
    .subscribe(disciplina => {
        this.redefinir();
        this.salvar_ok = true;
        this.atualizarLista();
      },
      () => {
        this.redefinir();
        this.salvar_erro = true;
      }
    );
}
```

Nesse caso a callback de sucesso, que indica que os dados foram salvos na API, faz com que a tela apresente uma mensagem de sucesso e a lista de disciplinas é atualizada \(para mostrar os novos dados das disciplinas\). Note também que a callback recebe o parâmetro `disciplina`, que não é utilizado, mas que é retornado pela API contendo os dados do objeto. No caso da API que estamos utilizando, se for feita uma requisição POST o objeto retornado pela API contém um atributo id cujo valor é gerado automaticamente, de forma incremental \(lembra que já precisamos controlar isso manualmente?\). No caso da callback de erro, o código faz com que a tela apresente uma mensagem de erro.

Os demais métodos, `excluir()` e `editar()`, utilizam, respectivamente, os métodos `excluir()` e `encontrar()` da classe `DisciplinasService`. Fica como exercício interpretar o código.

A execução do software no browser não é muito diferente do capítulo anterior. Entretanto, como estamos utilizando uma API HTTP REST, é importante tomar certos cuidados. Por exemplo, se, por algum motivo, não for possível acessar a API, é interessante apresentar uma mensagem de erro na tela. A figura a seguir ilustra essa situação.

![Execução do software no browser demonstrando mensagem de erro de conexão com a API HTTP REST](/assets/software-dados-api-http-demonstracao-erro.png)

Caso contrário, se a API estiver acessível e tudo estiver ocorrendo como esperado, a interface continua mostrando os dados e permitindo interação com o usuário normalmente.

Para finalizar o capítulo, um diagrama de sequência demonstrando a interação entre `AppComponent`, `DisciplinasService` e o **Servidor HTTP da API** quando é executado o método `atualizarLista()` do `AppComponent`.

![Diagrama de sequência demonstrando a interação entre AppComponent, DisciplinasService e Servidor HTTP quando é chamado o método atualizarLista() do AppComponent](https://www.planttext.com/plantuml/img/fPFVIiCm5CRlynJdRYtCMcJUz48svj0x1NSfZ3Gvpc3QXFmPnTVn4No4lPWdLMQrJ1GBeIJvlY-_SqBcFd0NOgFPmjgbQQnfiGrmSW6NoWjbjMgvlqEtKm8h24Pod-Lil7VCyHY2lM-BBOPiSYe_1PDZ8KEC2cvgJrkyrZZYEoOiVAozSAh6J72jQowUDZuAzDvCuR22pfbybDbpIECsr-lrRGLNgpKCeLbH5B3OHkua1uV1kDQ0DE0_R91if65S1rZkNwNQ6ZWhImRqVSaU5w0LtGH8XE5voVTptTYXY6JyNYf3xH7wW26CL0_eo8Zf92A33BiAPkLi2kTbcVVwNy3k-BCo1_4V2LFhUs-FkA9PWX4ax_OlT4TH1ySjfo8Y1AGSwasZtf4TUcjIexAW6ZGnvF-dQ2LBWx4v_UWbl040)

O diagrama de sequência demonstra as interações desde a chamada do método `todas\(\)` do `DisciplinasService` até o tratamento do retorno do Servidor HTTP \(ou erro\).

> **\[info\] Resumo do capítulo:**
>
> * A classe `HttpClient` é muito útil para acessar dados via requisições HTTP
> * A classe `HttpClient` pode ser utilizada para comunicação com uma API HTTP REST
> * A utilização de serviços para organização do código é um recurso para melhoria da arquitetura do software



