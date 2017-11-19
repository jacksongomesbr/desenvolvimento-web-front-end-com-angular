# Apresentando a lista de disciplinas

> \[info\] **Objetivo do capítulo:**
>
> * demonstrar a implementação do requisito "apresentar as disciplinas" de forma iterativa
> * demonstrar o uso das diretivas `ngFor` e `ngIf`
> * reforçar o entendimento do Data binding

Começando pelo Controller do `AppComponent`, remova o atributo `title` e adicione o atributo `disciplinas`. Esse atributo é um array de string, contendo os nomes das disciplinas. O conteúdo do Controller deve ficar mais ou menos assim (trecho):

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

