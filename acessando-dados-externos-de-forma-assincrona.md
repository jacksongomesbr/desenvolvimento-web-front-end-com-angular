# Acessando dados externos de forma assíncrona

> **\[info\] Objetivos do capítulo:**
>
> * demonstrar a representação de dados em arquivos JSON
> * demonstrar o uso do HttpClient para acessar dados externos \(arquivo JSON\)
> * apresentar o padrão Observer e explicar seu funcionamento
> * apresentar a comunicação entre componentes do software usando diagrama de sequência da UML
>
> **Branch**: [livro-dados-externos-json](https://github.com/jacksongomesbr/angular-escola/tree/livro-dados-externos-json)

Até o capítulo anterior \([Serviços](/servicos.md)\) a estrutura do software já começa a ficar mais modular. Para prosseguir nessa direção uma modificação importante é trabalhar com fontes de dados. Por enquanto, vamos utilizar um mecanismo simples, mas muito eficiente.

O serviço `DisciplinasService` possui um atributo `disciplinas`, que contém um array de objetos. Embora o software esteja funcional o array de objetos é definido diretamente no código-fonte, o que pode ser melhorado. A prática mais adequada é separar as coisas: de um lado o software, do outro a **fonte de dados**.

Uma fonte de dados é um conceito abstrato que pode ser representado por um arquivo de texto, JSON ou um banco de dados, por exemplo. Aqui, usaremos um arquivo em formato JSON.

O arquivo `./src/assets/dados/disciplinas.json` contém a lista das disciplinas:

```js
[
  {
    "id": 1,
    "nome": "Língua Portuguesa",
    "descricao": "..."
  },
  {
    "id": 2,
    "nome": "Inglês",
    "descricao": "..."
  },
  ...
]
```

Para acessar os dados de arquivos JSON é necessário realizar requisições HTTP para um servidor web. Se você estiver executando o projeto em modo de desenvolvimento, o servidor já disponibilizado pelo Angular CLI para desenvolvimento será utilizado tanto para servir o conteúdo do software quanto fornecer acesso ao arquivo JSON. Para isso o Angular disponibiliza o módulo `HttpClientModule`, que precisa, primeiro, ser importado no **root module**:

```typescript
...
import {HttpClientModule} from '@angular/common/http';

@NgModule({
  declarations: [
    ...
  ],
  imports: [
    NgbModule.forRoot(),
    BrowserModule,
    HttpClientModule,
    FormsModule
  ],
  ...
})
export class AppModule {
}
```

Depois, o serviço `DisciplinasService` precisa ser modificado:

```typescript
...
import {HttpClient} from '@angular/common/http';
import {Observable} from 'rxjs/Observable';

@Injectable()
export class DisciplinasService {
  private disciplinas = null;
  private novo_id = 1;

  constructor(private http: HttpClient) {
  }

  carregarDados(callback) {
    this.http.get('./assets/dados/disciplinas.json')
      .subscribe(disciplinas => this.disciplinas = disciplinas)
      .add(callback);
  }

  ...

}
```

A primeira modificação no código do serviço é a injeção de dependência de um `HttpClient`, uma classe do módulo `HttpClientModule` que fornece métodos apropriados para realizar requisições HTTP assíncronas. A classe `HttpClient` fornece o método `get()`, que é utilizado para realizar uma requisição HTTP GET. O parâmetro informado para o método `get()` é uma URL que, nesse caso, corresponde ao caminho para o arquivo `disciplinas.json`.

A classe `HttpClient` é usada efetivamente no método `carregarDados()`, que aceita como parâmetro uma **função callback** que deverá ser executada quando o carregamento dos dados for concluído. Esse comportamento é uma estratégia para carregar os dados de uma vez, como um array de disciplinas, mantendo uma referência para esse array no atributo `DisciplinasSservice.disciplinas` e também no atributo `AppComponent.disciplinas`. Isso acontece porque a forma de acesso à lista de disciplinas, até o momento, não permite alterar seu conteúdo de forma persistente, ou seja, não salva os dados.

Antes de prosseguirmos com a leitura do código, é importante detalhar o conceito de **requisição assíncrona**. A figura a seguir ilustra esse processo.

![Ilustração do processo de requisição assíncrona](https://www.planttext.com/plantuml/img/VP0z3i8m38NtdC8Z35GLwiI0g13Y07HaiHeFGQaDJjg1wt0BLYw6GVmAYSZAak-zb-T5ogYvxw9ponY8Cy5a3XlI8NZH6QnN3VYGsh2FWJ4LkoJidi_VQ6DgX2Wjnd141G6bjjSaiELV3umPQZtqOH0WReMpeXSOJSjoxC3EPyZZQpEuSNGv6sY33_cFDyL4BtE-dBuJghBwav2eUwSuOk-Aee3Qj5R4atMoQu-AztvPb0MCS6vXhEtn2W00)

A figura demonstra que o Cliente faz uma requisição HTTP para o Servidor \(usando o verbo GET do HTTP\) que busca pelo arquivo solicitado. Conforme a situação o Servidor retorna uma de duas respostas possíveis para o Cliente:

* **código 200**: o arquivo foi encontrado, então retorna o conteúdo dele
* **código 404**: o arquivo não foi encontrador, então não há conteúdo no retorno

De qualquer forma, um comportamento característico desse cenário acontece: quando o Cliente faz a requisição ele continua em funcionamento enquanto aguarda uma resposta do Servidor. Quando a resposta chega o Cliente sabe que isso aconteceu e executa uma ação específica para cada tipo de retorno \(conforme o código de retorno 200 ou 404, por exemplo\). É por isso que a requisição é assícrona.

Em termos de código o método `get()` da classe `HttpClient` não realiza a requisição. Ele cria um objeto do tipo `Observable` \(do pacote `rxjs`\) que implementa um padrão de projeto chamado **Observer**, que é ideal para essa situação das requisições assíncronas. Por causa desse padrão, vamos a uma analogia demonstada pela figura a seguir.

![Ilustração do processo de assinatura de uma revista](https://www.planttext.com/plantuml/img/TP313OCm34NlcS8Bm02ega28Mm_j26wILfOWGKx2GZrKwXeiLWjeeHAzsk_PNxyC4JcchbMgRicwQ24xGcCeUiO2Bico1mo1738Wi1r83BK0FspD98n6Wo6AP3pe-UAMNfuKK4qtOvAnzkv6t8cOGtLFoCQ2ymE2DJG-nuTNUIwRo1Wyz2W6Gf-kBMcSrY1ywbOSp3SeYBaRzZpx_FjeO-w6Rjn0-5_vD7Z8LkKqlZQzgT8w8ss_0G00)

A figura ilustra a comunicação entre Cliente e Editora. Depois de realizar a assinatura o cliente fica aguardando a Editora publicar uma nova edição da revista e enviar um exemplar para o Cliente, _observando_ esse processo. Enquanto isso acontece o Cliente não fica parado, mas vai realizando seus afazeres. Quando um exemplar da nova edição chega, então o Cliente inicia a sua leitura.

O padrão **Observer** tem dois elementos importantes: o **publisher** \(no caso, a Editora\) e o **subscriber** \(no caso, o Cliente\). Um terceiro elemento é importante é a revista, que representa o **dado** entregue pela Editora para o Cliente.

No contexto do código o método `get()` cria uma instância de `Observable`. Para realizar uma requisição e realizar uma ação quando a resposta chega do servidor é utilizado o método `subscribe()`. Por sua vez o método `subscribe()` pode receber dois parâmetros:

* **o primeiro parâmetro é uma função callback executada quando a requisição for bem sucedida.** Nesse caso, o retorno da requisição é convertido para JSON e tratado no código como um objeto, representado pelo parâmetro da função callback
* **o segundo parâmetro é uma função callback executada quando a requisição não for bem sucedida.** Nesse caso o retorno da requisição \(se houver\) também é convertido para JSON e tratado no código como um objeto, também representado pelo parâmetro da função callback.

No caso do código do serviço `DisciplinasService` apenas o primeiro parâmetro está sendo utilizado, o que ilustrado pela figura a seguir.

![Ilustração da composição de uma função callback para o método subscribe\(\)](/assets/httpclient-observer-subscribe-callback.png)

A sintaxe utilizada é conhecida como **arrow function** \(função seta\) e, por ser uma função, tem duas partes:

* parâmetros; e 
* corpo.

Se a lista de parâmetros for vazia, pode-se usar apenas `()`.

Voltando ao código do serviço `DisciplinasService` a função callback de sucesso para a chamada de `subscribe()` atribui o parâmetro disciplinas \(a lista de disciplinas retornada pelo servidor já em formato de objetos\) para o atributo da classe de mesmo nome.

Para finalizar a leitura do código do `DisciplinasService` há mais um recurso importante sendo utilizado. O método `carregarDados()` recebe como parâmetro uma função callback. O método cria uma instância de `Observable` e chama o método `subscribe()`. Como o objetivo desse método é executar a função callback informada como parâmetro quando a requisição HTTP for concluída e os dados estiverem disponíveis, é feita uma chamada para a função `add()`, informando como parâmetro a função callback.

Para usar o serviço `DisciplinasService` o `constructor()` do Controller `AppComponent` comporta-se da seguinte forma:

```typescript
constructor(private disciplinasService: DisciplinasService) {
  this.disciplinasService.carregarDados(
    () => this.disciplinas = this.disciplinasService.todas()
  );
}
```

Perceba que a chamada para `carregarDados()` informa uma **arrow function** como parâmetro \(a callback\).

Para finalizar esse capítulo, a figura a seguir resume e ilustra os conceitos apresentados, indicando a sequência de passos da comunicação entre os componentes.

![Diagrama de sequência do software, demonstrando a interação entre AppComponent, DisciplinasService e Servidor HTTP](https://www.planttext.com/plantuml/img/bLFDJW913BxFK_G6c-Wy05424QD74pcIcB9Jbz5bkdQw9BwEZ-6L5tCMGLX-8jncfkttVVtQ6KH5qNfUcc5LtV6yua11uReF8nzpNvK-O7mcMVYSUf2Z21Ke8tGSkpcMvHJpzymSvfv2cAbMas0Bqcx7RUFsBNBeP2aIwsdCnKzf2vzUqRb_wLP7n_BoE1u_zU3XVWpx3CPQ2yEYHd48GieI61n3N9T2KvfoJ0lhf1iSb9RVRWM1yb7x1HzIdk_DJYdSza5dFjhMssw62Qm4uekPDdFvjMRLov-1gP4CC6aKptA1ZWrQDpj9qioZ1Nyr2HNAycjEAia4sbkLx62zTkyNjhuMQfKTnet8abxtDBOLjCquPzatyCUDQ-iVYk3dpJQpRyo0Wal_wWS0)

Se necessário analise a sequência do diagrama mais de uma vez, acompanhado do código-fonte.

