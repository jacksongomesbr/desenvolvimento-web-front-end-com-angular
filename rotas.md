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

Controlar a visibilidade de componentes não é uma tarefa complexa. Isso pode ser consideguido, por exemplo, utilizando as diretivas `ngIf` e `hidden`. Entretanto, a complexidade aumenta na proporção da quantidade de componentes ou da complexidade das interações com o usuário.

