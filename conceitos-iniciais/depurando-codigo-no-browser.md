# Depurando código no browser

**\[info\] Objetivos do capítulo:**

> * demonstrar como utilizar o recurso de depuração de código do Browser
>
> **Branch**: [iniciando](https://github.com/jacksongomesbr/angular-escola/tree/iniciando)

Devido ao processo de compilação/tradução do código-fonte para que o browser execute o software os recursos de inspeção do código-fonte/elementos apresenta o que o browser interpreta, ou seja, o resultado do processo de compilação/tradução. A figura a seguir ilustra a tela de um browser com o painel das ferramentas de desenvolvedor.

![](/assets/browser-com-ferramentas-desenvolvedor.png)

Enquanto isso é útil para o browser, não permite que o desenvolver possa acompanhar a execução de partes do software observando o código-fonte original. 

Entretanto, a mesma ferramenta dá acesso à importante funcionalidade de depuração de código. Para isso, basta acessar o painel **Sources** e, na aba **Network**, à esquerda, acessar o nó `webpack://` e seu filho que corresponde ao caminho do projeto do software no ambiente local \(ex: `D:/developer/angular-escola`\), como mostra a figura a seguir.

![](/assets/depuracao-browser-sources-webpack-breakpoint.png)

A figura demonstra que o arquivo `/src/app/app.component.ts` está selecionado. Do lado direito da lista dos arquivos a tela apresenta o código-fonte original do arquivo. No painel que mostra o código-fonte há um **breakpoint** \(ponto de parada\) na linha 9. Isso significa que a execução do software no browser terá uma pausa quando da sua execução. Ainda mais à direita da tela há uma barra de ferramentas que permitem controlar a depuração:

![](/assets/browser-debug-barra-de-ferramentas.png)

Os botões representam, nesta ordem:

* pausar/continuar a execução do código
* executar a próxima linha e não entrar no código da função/método
* executar a próxima linha e entrar no código da função/método
* executar a próxima linha e sair do código da função/método
* desativar os breakpoints
* pausar em exceções

A figura a seguir ilustra a tela durante o processo de depuração.

![](/assets/browser-depuracao-ocorrendo.png)

Por fim, no painel mais à direita é possível acessar funcionalidades como inspecionar variáveis ou expressões \(**Watch**\), ver a pilha de chamadas \(**Call stack**\) e inspecionar as variáveis do escopo \(**Scope**\).





