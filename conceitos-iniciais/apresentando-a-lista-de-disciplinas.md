# Apresentando a lista de disciplinas

> **\[info\] Objetivos do capítulo:**
>
> * demonstrar a implementação do requisito "apresentar as disciplinas" de forma iterativa
> * demonstrar o uso das diretivas `ngFor` e `ngIf`
> * reforçar o entendimento do Data binding

Começando pelo Controller do `AppComponent`, remova o atributo `title` e adicione o atributo `disciplinas`. Esse atributo é um array de string, contendo os nomes das disciplinas. O conteúdo do Controller deve ficar mais ou menos assim \(trecho\):

```typescript
export class AppComponent {
  disciplinas = [
    'Língua Portuguesa',
    'Arte',
    'Educação Física',
    'Matemática',
    'História',
    'Geografia',
    'Ciências',
    'Redação',
    'Língua Estrangeira Moderna - Inglês',
    'Ensino Religioso'
  ];
}
```

Na sequência altere o Template do `AppComponent` para o seguinte conteúdo:

```html
<h1>Disciplinas</h1>
<hr>
<ul>
  <li *ngFor="let disciplina of disciplinas">
    {{disciplina}}
  </li>
</ul>
```

Parte importante do Template está no elemento `li`: o `*ngFor`, que parece um atributo, é uma **diretiva estrutural **que modifica o Template no browser, gerando novos elementos. A diretiva `*ngFor` é utilizada para repetir um elemento do Template. Nesse caso, é justamente essa a sua aplicação: repetir o elemento `li` para apresentar os nomes das disciplinas. A repetição é realizada por uma repetição: `let disciplina of disciplinas` significa que o `*ngFor` vai repetir o elemento `li` para cada item do array `disciplinas` e cada item recebe o nome de `disciplina`. O conteúdo do elemento `li` é uma interpolação para apresentar o valor de `disciplina`.

Abra o aplicativo no browser. O resultado deve ser semelhante à figura a seguir.

![Apresentando a lista das disciplinas](/assets/browser-listando-disciplinas.png)



