# Acessando dados externos de forma assíncrona

> Objetivos do capítulo:
>
> * demonstrar o uso do HttpClient para acessar dados externos \(arquivo JSON, API REST\)

Até o capítulo anterior \([Serviços](/servicos.md)\) a estrutura do software já começa a ficar mais modular. Para prosseguir nessa direção uma modificação importante é trabalhar com fontes de dados. Por enquanto, vamos utilizar um mecanismo simples, mas muito eficiente.

O serviço DisciplinasService possui um atributo disciplinas que contém um array de objetos. Embora o software esteja funcional o array de objetos é definido diretamente no código-fonte. Isso faz com que, toda vez que houver uma modificação na lista, seja necessário compilar o software novamente, o que não é interessante.

A prática mais adequada é separar as coisas: de um lado o software, do outro a fonte de dados.

Nesse capítulo vamos usar duas estratégias para armazenar a lista das disciplinas. Primeiro, utilizando JSON.

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

Para acessar os dados de arquivos JSON o Angular disponibiliza o módulo `HttpClientModule`, que precisa, primeiro, ser importado no **root module**:

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

A primeira modificação no código do serviço é a injeção de dependência de um `HttpClient`, uma classe do módulo `HttpClientModule` que fornece métodos apropriados para realizar requisições HTTP assíncronas. A classe `HttpClient` fornece o método `get()`, que é utilizado para realizar uma requisição GET. O parâmetro informado para o método `get()` é uma URL que, nesse caso, corresponde ao caminho para o arquivo `disciplinas.json`.

A classe HttpClient é usada efetivamente no método `carregarDados()` que aceita como parâmetro uma função callback que deverá ser executada quando o carregamento dos dados for concluído. Bem, antes de prosseguirmos com a leitura do código, é importante detalhar o conceito de requisição assíncrona. A figura a seguir ilustra esse processo.

@startuml
Alice -> Bob: Authentication Request
Bob --> Alice: Authentication Response

Alice -> Bob: Another authentication Request
Alice <-- Bob: another authentication Response
@enduml





