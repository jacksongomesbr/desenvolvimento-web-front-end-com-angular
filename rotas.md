# Rotas

> Objetivos do capítulo:
>
> * demonstrar a utilização de rotas para apresentação de componentes

Os capítulos anteriores demonstraram como trabalhar com elementos principais da arquitetura do Angular, principalmente componentes e serviços. Esses elementos têm o objetivo de compor a arquitetura de um software desenvolvido em Angular e, como visto, há vários recursos que permitem a interação entre eles.

Este capítulo apresenta outro recurso de arquitetura de software desenvolvido em Angular que, na verdade, aumenta a usabilidade do software.

Antes de apresentar a parte técnica é importante lidar com alguns conceitos.

## URI, URL e URN

**URI** \(_Uniform Resource Identifier_\) é um identificador de um recurso. A sintaxe desse identificador é:

```
scheme:[//[user[:password]@]host[:port]][/path][?query][#fragment]
```

Onde os elementos entre **\[\]** são opcionais e:

* **scheme**: representa o protocolo ou o contexto;
* **user** e **password**: representam as credenciais do usuário;
* **host**: representa o identificador do dispositivo que contém o recurso;
* **port**: representa o número da porta do dispositivo;
* **path**: representa o caminho do recurso;
* **query**: uma cadeia de caracteres que representa parâmetros de URL, um conjunto de pares `chave=valor` separados por `&`; e
* **fragment**: uma cadeia de caracteres com formato dependente do contexto.

A figura a seguir ilustra um URI e a sua composição:

![](/assets/ilustracao-uri-url.png)

O URI identifica um recurso disponível na internet, mais especificamente um repositório do Github. Provavelmente você já conhece isso e pode ser que use os termos **endereço** e **URL**. Você não está errado. URL é a forma mais comum de URI na internet. **URL** \(_Uniform Resource Locator_\) é um endereço de um recurso na internet. Além disso, URL é a forma mais comum de criar \_links \_entre páginas web e incorporar uma imagem em uma página web, por exemplo.

**URN** \(_Uniform Resource Name_\) é o nome de um recurso em um contexto específico. Por exemplo: o ISBN \(_International Standard Book Number_\) de uma edição de "Romeu e Julieta", de William Shakespeare, é 0-486-27557-4; seu URN poderia ser **urn:isbn:0-486-2557-4**.

Voltando para URL e o contexto da internet, os browsers geralmente fornecem uma **barra de endereço**, por meio da qual o usuário indica qual URL deseja acessar, por exemplo a URL de uma página web. A partir do momento que o browser acessa uma página web ele passa a armazenar o primeiro endereço acessado e os demais endereços que forem acessados por meio de cliques em _links_.

Esse é, provavelmente, o formato mais intuitivo e o mais utilizado para acessar páginas web. Justamente por isso é necessário repensar a forma como o aplicativo desenvolvido em Angular não apenas entrega conteúdo para o usuário mas também como permite que o usuário o acesse.

## Rotas

Uma rota está diretamente relacionada a URL, ou seja, também funciona como um localizador de um recurso. A diferença é que acrescenta a possibilidade de utilizar **parâmetros de rota**. Por exemplo: considere um site de notícias **noticias.to** que permite acessar a notícia "Governo paga salarios de servidores", cujo identificador é 7899, está na categoria "política" e foi publicada em 20/12/2017, por meio do URL:

```
https://noticias.to/noticias/politica/2017/12/20/governo-paga-salarios-de-servidores/7899
```

Há informações no URL que pertencem à notícia e mudam de uma notíca para outra:

* **categoria:** politica
* **ano:** 2017
* **mês:** 12
* **dia:** 20
* **titulo:** governo-paga-salarios-de-servidores
* **identificador:** 7899

Analisando URLs de outras notícias alguém poderia chegar à conclusão de que há um padrão:

```
/noticias/categoria/ano/mes/dia/titulo/identificador
```

Independentemente de possuir parâmetros de rota, uma rota é um **padrão**. Cada uma dessas informações \(categoria, ano, mes, dia, titulo, identificador\), que muda de uma notícia para outra, pode ser representada como um **parâmetro de rota**.

A implementação desse conceito pode variar entre frameworks, mas provavelmente as mesmas funcionalidades estão disponíveis:

* definir uma rota \(e, opcionalmente, usar parâmetros de rota\)
* identificar valores dos parâmetros de rota

Além disso, como URLs são localizadores de recursos, rotas também servem para esse propósito, ou seja, uma rota está associada algum recurso e é uma forma de acessá-lo.

## Rotas no Angular

Controlar a visibilidade de componentes é uma tarefa simples. Isso pode ser consideguido, por exemplo, utilizando as diretivas `ngIf` e `hidden`. Entretanto, a complexidade aumenta na proporção da quantidade de componentes ou da complexidade das interações com o usuário. Por isso o Angular implementa o conceito de rotas de uma maneira bastante útil.

Para usar rotas é possível definí-las das seguintes formas:

* no **root module**
* no **feature module**
* usando um **módulo de rotas**

Nesse momento vamos usar a primeira abordagem. Modifique o arquivo `./src/app/app.module.ts`:

```typescript
...
import {RouterModule, Routes} from '@angular/router';

... (imports dos componentes)

const appRoutes: Routes = [
  {path: 'disciplinas', component: ListaDeDisciplinasComponent},
  {path: 'disciplinas/:id', component: EditorDeDisciplinaComponent},
  {path: '', component: HomeComponent,},
  {path: '**', component: PaginaNaoEncontradaComponent}
];

@NgModule({
  declarations: [
    AppComponent,
    ListaDeDisciplinasComponent,
    EditorDeDisciplinaComponent,
    HomeComponent,
    PaginaNaoEncontradaComponent
  ],
  imports: [
    RouterModule.forRoot(
      appRoutes,
      {enableTracing: true} 
    ),
    ...
  ],
  providers: [DisciplinasService],
  bootstrap: [AppComponent]
})
export class AppModule {
}
```

A primeira coisa a destacar é a variável `appRoutes`, do tipo `Routes`. O pacote `@angular/router` fornece o módulo `RouterModule` e a classe `Routes`. Na prática a variável `appRoutes` recebe um array no qual cada item é um objeto com dois atributos:

* `path`: representa a rota para o componente; e
* `component`: representa o componente associado à rota.

A primeira rota, `disciplinas`, está associada ao componente `ListaDeDisciplinasComponent`.

A segunda rota, `disciplinas/:id`, está usando um **parâmetro de rota**. A sintaxe do Angular para parâmetro de rotas é usar o sinal de dois pontos seguido do nome do parâmetro. Nesse caso o parâmetro chama-se `id`. A rota está associada ao componente `EditorDeDisciplinaComponent`.

A terceira e a quarta rotas têm um comportamento especial. A terceira rota é uma string vazia e está associada ao componente `HomeComponent`. Isso significa que essa é a **rota padrão**. A quarta rota é `**`, associada ao componente `PaginaNaoEncontradaComponent`. Isso significa um atalho para o seguinte comportamento: se o usuário fornecer uma rota não definida, leve até o componente `PaginaNaoEncontradaComponent`.

Quando o software é executado o Angular busca uma combinação entre a URL fornecida no browser e as rotas definidas no módulo. Isso é feito de cima para baixo, procurando no array `appRoutes`. Ao encontrar uma rota correspondente, o Angular cria uma instância do componente e a apresenta. A figura a seguir ilustra esse processo.

![](/assets/ilustracao-procurando-rota.png)

O modo de comparação entre a URL no browser e o caminho da rota é muito interessante. A forma de comparação considera se o caminho da rota combina com o final da URL do browser. A figura ilustra a busca por uma rota correspondente à URL `http://localhost:4200` e interrompe o processo quando encontra a primeira correspondência. Nesse mesmo sentido se a URL no browser for `http://localhost:4200/disciplinas` o Angular encontra a rota `disciplinas`. E assim por diante.

Voltando à figura a URL indica o que é conhecido como **raiz do site**. É opcional que a URL termine com `/`.

Outra característica importante desse processo é a ordem das rotas. Como disse, o Angular procura pela rota que combina com o final da URL, assim, se a rota padrão estiver no início da lista das rotas, então o Angular sempre a encontrará. Da mesma forma, se não encontrar uma rota correspondente o Angular vai até a rota `**`.

Ok, agora falta concluir essa seção falando sobre o **shell component**. 

### Shell component

Quando o Angular identifica a rota e o componente associado a ela é necessário apresentá-lo \(lembre-se que o componente é, provavelmente, visual\). Assim, ao utilizar rotas o Angular requer que o módulo \(nesse caso o `AppModule`\) utilize um componente como shell, o que significa que o Template do componente precisa indicar onde outros componentes serão apresentados. Para isso o Angular utiliza duas informações:

* qual o componente o módulo declara como `bootstrap` \(o `AppComponent` é o **shell component**\)
* onde está o elemento `router-outlet` no Template do `AppComponent`

A figura a seguir ilustra esse processo.

![](/assets/ilustracao-rotas-router-outlet-combinacao.png)

Como mostra a figura, o Angular combina o Template do `AppComponent` com o Template do `HomeComponent` para gerar uma saída HTML \[unificada\] para o browser. Importante perceber que tudo o que está antes e depois do elemento `router-outlet` é mantido na saída. Por isso o Angular usa, em conjunto, a descoberta da rota e do componente associado e o conteúdo do **shell component**.

## Estrutura do software

A figura a seguir apresentra as classes principais do software até este capítulo.

![](/assets/rotas-estrutura-classes.png)

## Padrão de trabalho

* criar componente
* definir rota
* implementar a lógica de negócio em um serviço
* implementar lógica de interface usar o serviço



