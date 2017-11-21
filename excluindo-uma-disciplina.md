# Excluindo uma disciplina

> **\[info\] Objetivos do capítulo:**
>
> * demonstrar a implementação do requisito "Excluir uma disciplina"

Este é um capítulo mais simples. Basicamente os recursos utilizados já foram apresentados, então ele é mais curto.

Primeiro, um trecho do Controller do `AppComponent`:

```typescript
...
export class AppComponent {
  selecionado = null;
  nome = null;
  descricao = null;
  disciplinas = [
    new Disciplina('Língua Portuguesa', '...'),
    ...
  ];

  salvar() {
  ...
  }

  excluir(disciplina) {
    if (confirm('Tem certeza que deseja excluir a disciplina "'
        + disciplina.nome + '"?')) {
      const i = this.disciplinas.indexOf(disciplina);
      this.disciplinas.splice(i, 1);
    }
  }
}
```

Com exceção do código omitido, que você já conheceu nos capítulos anteriores, o método `excluir()` recebe como parâmetro um objeto, chamado `disciplina`. Esse método exclui uma disciplina da lista, mas, antes, solicita uma confirmação do usuário \(por isso está usando a função `confirm()`\). Se o usuário confirmar que deseja excluir a disciplina, então o código prossegue:

* encontra o índice da disciplina na lista usando o método `indexOf()`
* remove um elemento da lista usando o método `splice()`

Agora, então, um trecho do código do Template:

```html
<h1>Disciplinas</h1>
<hr>
<ul>
    <li *ngFor="let disciplina of disciplinas">
        {{disciplina.nome}}
        <button (click)="excluir(disciplina)">
            Excluir
        </button>
    </li>
</ul>

<h2>Cadastrar</h2>
<p>Use este formulário para cadastrar uma disciplina.</p>
<form #cadastro="ngForm" (submit)="salvar()">
...
</form>
```

A parte importante do template é o elemento `button` ao lado do nome da disciplina. Há um tratador do evento `(click)` que chama o método `excluir()` passando como parâmetro a `disciplina` atual da repetição do `*ngFor`.

O resultado da execução do software no browser é ilustrado pela figura a seguir.

![](/assets/software-excluindo-disciplina-browser.png)

Esse capitulo é bem curto e não há muito o que resumir, mas é bom notar que os conceitos vistos nos capítulos anteriores continuam funcionando aqui, mesmo que em aplicações diferentes.



