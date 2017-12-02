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

O URI identifica um recurso disponível na internet, mais especificamente um repositório do Github. Provavelmente você já conhece isso e pode ser que use os termos **endereço** e **URL**. Você não está errado. URL é a forma mais comum de URI na internet. 

**URL** \(_Uniform Resource Locator_\) é um endereço de um recurso na internet.



