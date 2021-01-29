![Pearson Next](https://skola.sfo2.cdn.digitaloceanspaces.com/pearson/pearson_next.png)

> Documento de Especificação Técnica

<br />

## Implementação da Camada de dados - Pearson Next
Última atualização: 27/01/2021 <br />
Em caso de dúvidas, entrar em contato com: [thiago.ceccato@pearson.com](mailto:thiago.ceccato@pearson.com)

<br />

## Objetivo
Este documento tem como objetivo instruir a implementação da camada de dados e de data attributes para utilização de recursos de monitoramento do Google Analytics referentes ao ambiente de [URL Produção: https://pearsonnext.com.br/](https://pearsonnext.com.br/).

<br />

### **Descrição Geral**

O `snippet` do Google Tag Manager é um pequeno trecho de código javascript ou non-javascript, através do uso de um iframe quando o javascript não está disponível, que é inserido nas páginas do site, tornando possível que a configuração das tags sejam realizadas via interface.


### **Posicionamento do Código - Google Tag Manager**

#### 1. Copie o seguinte JavaScript e cole-o o mais próximo da tag `<head>` de abertura possível em todas as páginas do seu site.

```html

<html>
  <head>
    <!-- Google Tag Manager -->
<script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','GTM-K2Q4ZNN');</script>
<!-- End Google Tag Manager -->
    //...
  </head>
```

#### 2. Copie o seguinte trecho e cole-o imediatamente após a marcação `<body>` de abertura em cada página do seu site.

```html
<body>
 <!-- Google Tag Manager (noscript) -->
<noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-K2Q4ZNN"
height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
<!-- End Google Tag Manager (noscript) -->
  //...
  </body>
</html>
```

Link de referência: [Documentação Oficial Google Tag Manager](https://developers.google.com/tag-manager/quickstart)


## Observações
> Os valores especificados entre colchetes `[[ ]]` são variáveis dinâmicas e devem ser substituídas por seus respectivos valores.<br />

> Todos os valores enviados ao Google Analytics devem estar sanitizados, ou seja, sem espaços, acentuação ou caracteres especiais. <br />


---

## Overview e Descrições Técnicas

### Camada de dados (DataLayer)

> É um array de objetos javascript utilizado pelo Google Tag Manager para receber em seus atributos, dados importantes do site.
Para implementar o dataLayer no site, o desenvolvedor pode utilizar formas diferentes para preencher os dados. Essas formas são dependentes da ação estabelecida na documentação e também do nível da interação.

**Instalação**<br />
Inserir a camada de dados antes do snippet de instalação do Google Tag Manager. Exemplo:

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer = [{
    'atributo1': '[[valor1]]',
    'atributo2': '[[valor2]]'
  }];
</script>
```

OU

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'atributo1': '[[valor1]]',
    'atributo2': '[[valor2]]'
  });
</script>
```

### Atributos HTML (Data Attributes)

> São atributos customizados inseridos nos elementos HTML da página, permitindo a inclusão de dados adicionais.

**Instalação**
1. Elementos: ```<div>Elemento</div>``` <br />
Todos os elementos do html que serão clicados, deverão ser mapeados recebendo os atributos com sua estrutura no item.

```html
<div
  data-gtm-event-category='[[exemplo:valor-categoria]]'
  data-gtm-event-action='[[exemplo:valor-acao]]'
  data-gtm-event-label='[[exemplo:valor-rotulo]]'
 >
  Texto do elemento
</div>
```

#### Importante:
> Também devem ter os data-attributes `data-gtm-event-category`, `data-gtm-event-action` e `data-gtm-event-label`. Preenchidos conforme instruções específicas.

<br />

## Implementação

A documentação foi descrita para algumas áreas especificas do ambiente do site ou aplicativo.


### Especificações Globais:

**Itens Gerais:**<br />
Todas as informações entre colchetes `[[  ]]` são variáveis dinâmicas que devem ser preenchidas com seus respectivos valores; <br />
Todos os valores enviados ao Google Analytics devem estar sanitizados, ou seja, sem espaços, acentuação ou caracteres especiais; <br />
Caso a informação solicitada não estiver disponivel retornar o valor com tipagem undefined.

---


### General


**No clique dos links do menu superior (Deve retornar o tagueamento conforme o item clicado pelo usuário. Ex: no menu superior trazer somente a variável de [[menu-superior]], já quando for clicado no submenu, retornar a variável de [[menu-superior]]:[[submenu]] e etc)**<br />

- **Onde:** Em todas as páginas em que estiverem disponíveis
    - **Titulo ou nome do botão/link:** &quot;pearsonnext Facebook&quot;, &quot;pearsonnext Instagram&quot;, &quot;pearsonnext Linkedin&quot; e etc
    
```html
<div
   data-gtm-event-category='pearsonnext:geral'
   data-gtm-event-action='clique:link:header'
   data-gtm-event-label='[[menu]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[menu]] | &#039;home&#039;, &#039; encontre_seu_curso&#039;, &#039;sobre_a_pearson&#039;, e etc | Deve retornar o nome do menu clicado. |

<br />


**No clique dos links do footer**<br />

- **Onde:** Em todas as páginas em que estiverem disponíveis
    - **Titulo ou nome do botão/link:** &quot;Sobre a Pearson&quot;, &quot;Ajuda e Suporte&quot;, &quot;Fale Conosco&quot; e etc
    
```html
<div
   data-gtm-event-category='pearsonnext:geral'
   data-gtm-event-action='clique:link:footer'
   data-gtm-event-label='[[link-footer]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[link-footer]] | &#039;sobre_a_pearson&#039;, &#039;ajuda_&_suporte&#039; e &#039;pearsonnext_linkedin&#039; | Deve retornar o nome do footer clicado |

<br />


**Na rolagem de página**<br />

- **Onde:** Em todas as páginas
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'pearsonnext:geral',
    'eventAction': 'scroll-tracking',
    'eventLabel': '[[valor-porcentagem]]%',
    'noInteraction': '1',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[valor-porcentagem]] | &#039;10&#039;, &#039;20&#039;, &#039;40&#039; | Deve retornar percentual de scroll. |

<br />

**No clique nos banners de tipos de cursos**<br />

- **Onde:** Home
    - **Titulo ou nome do botão/link:** &quot;Carreiras&quot;, &quot;Soft Skills&quot;, &quot;Tecnologia&quot; e etc
    
```html
<div
   data-gtm-event-category='pearsonnext:home'
   data-gtm-event-action='clique:banner:categoria_curso'
   data-gtm-event-label='[[banner_clicado]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[banner_clicado]] | &#039;carreiras&#039;, &#039;soft_skills&#039; e &#039;tecnologia&#039; | Deve retornar o nome do banner clicado. |


<br />

**No clique dos links dos parceiros**<br />

- **Onde:** Home
    - **Titulo ou nome do botão/link:** &quot;Holberton&quot;, &quot;Alura&quot;, &quot;Impacta&quot; e etc
    
```html
<div
   data-gtm-event-category='pearsonnext:home'
   data-gtm-event-action='clique:link:parceiros'
   data-gtm-event-label='[[link-parceiro]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[link-parceiro]] | &#039;holberton&#039;, &#039;alura&#039; e &#039;impacta&#039; | Deve retornar o nome dos parceiros clicados. |


<br />



## Enhanced E-commerce

### Product Impression:

**Na visualização de uma vitrine de produtos - considerar a página de resultado de busca** <br />

- **Onde:** O objeto javascript (dataLayer) abaixo deve ser disparado uma única vez no carregamento de uma página de lista de produtos.

```html
<script>
dataLayer.push({
  'event': 'productImpression',
  'eventCategory': 'enhanced-ecommerce',
  'eventAction': 'product-impression',
  'eventLabel': '',
  'ecommerce': {
    'impressions': [
      {
        'name': '[[nome-curso]]',  // Nome ou ID do produto é obrigatório
        'id': '[[id-curso]]',
        'price': '[[preco-curso]]',
        'brand': '[[marca-curso]]',
        'category': '[[categoria-curso]]',
        'variant': '[[variacao-curso]]',
        'list': '[[lista-curso]]',
        'position': '[[posicao-curso]]'
      }, {
        'name': '[[nome-curso2]]',  // Nome ou ID do produto é obrigatório
        'id': '[[id-curso2]]',
        'price': '[[preco-curso2]]',
        'brand': '[[marca-curso2]]',
        'category': '[[categoria-curso2]]',
        'variant': '[[variacao-curso2]]',
        'list': '[[lista-curso2]]',
        'position': '[[posicao-curso2]]'
      }
    ]
  }
});
</script>
```

<br />

**Catálogo - Produtos**

<br />

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-curso]] | &#039;excel_modulo_1&#039;, &#039;lideranca_corporativa&#039; e etc | Nome do Curso |
| [[id-curso]] |  &#039;pts123&#039;, &#039;netflix1234&#039;, &#039;desc-10&#039; e etc e etc | SKU do Curso |
| [[preco-curso]] | &#039;3500&#039; e etc | Preço do curso |
| [[marca-curso]] | &#039;centauro&#039;, &#039;magalu&#039;, &#039;viagens&#039; e etc | Quem está oferencedo o curso |
| [[categoria-curso]] | &#039;montar-meu-clube&#039; | Categoria do curso |
| [[lista-curso]] | &#039;mais_procurados&#039;, &#039;novos_cursos&#039; e etc | Nome da lista que o curso aparece |
| [[posicao-curso]]     | '1','2'           | Posição que o produto aparece na lista  |
| [[variacao-curso]]    | '24_horas/curso_online', '3_horas/curso_online'           | Carga Horária do curso             |


<br />

### Product Click:

**No clique em um produto **<br /> 

- **Onde:** O objeto javascript (dataLayer) abaixo deve ser disparado uma única vez no clique em algum produto.

```html
<script>
dataLayer.push({
  'event': 'productClick',
  'eventCategory': 'enhanced-ecommerce',
  'eventAction': 'product-click',
  'eventLabel': '',
  'ecommerce': {
    'click': {
      'actionField': {'list': '[[lista-curso]]'},
      'products': [{
        'name': '[[nome-curso]]',       //Nome ou ID do produto é obrigatório
        'id': '[[id-curso]]',
        'price': '[[preco-curso]]',
        'brand': '[[marca-curso]]',
        'category': '[[categoria-curso]]',
        'variant': '[[variacao-curso]]',
        'position': '[[posicao-curso]]'
      }]
    }
  }
});
</script>
```

<br />

**Catálogo - Produtos**

<br />

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-curso]] | &#039;excel_modulo_1&#039;, &#039;lideranca_corporativa&#039; e etc | Nome do Curso |
| [[id-curso]] |  &#039;pts123&#039;, &#039;netflix1234&#039;, &#039;desc-10&#039; e etc e etc | SKU do Curso |
| [[preco-curso]] | &#039;3500&#039; e etc | Preço do curso |
| [[marca-curso]] | &#039;centauro&#039;, &#039;magalu&#039;, &#039;viagens&#039; e etc | Quem está oferencedo o curso |
| [[categoria-curso]] | &#039;montar-meu-clube&#039; | Categoria do curso |
| [[lista-curso]] | &#039;mais_procurados&#039;, &#039;novos_cursos&#039; e etc | Nome da lista que o curso aparece |
| [[posicao-curso]]     | '1','2'           | Posição que o produto aparece na lista  |
| [[variacao-curso]]    | '24_horas/curso_online', '3_horas/curso_online'           | Carga Horária do curso             |

<br />

### Product Detail:

**Na visualização de detalhes de produto e da página de "Detalhes do resgate"**<br />

- **Onde:** O objeto javascript (dataLayer) abaixo deve ser disparado uma única vez no carregamento da página de um produto.

```html
<script>
dataLayer.push({
  'event': 'productDetail',
  'eventCategory': 'enhanced-ecommerce',
  'eventAction': 'product-detail',
  'eventLabel': '',
  'ecommerce': {
    'detail': {
      'actionField': {'list': '[[lista-curso]]'},
      'products': [{
        'name': '[[nome-curso]]',       //Nome ou ID do produto é obrigatório
        'id': '[[id-curso]]',
        'price': '[[preco-curso]]',
        'brand': '[[marca-curso]]',
        'category': '[[categoria-curso]]',
        'variant': '[[variacao-curso]]'
      }]
    }
  }
 });
</script>
```
<br />

**Catálogo - Produtos**

<br />

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-curso]] | &#039;excel_modulo_1&#039;, &#039;lideranca_corporativa&#039; e etc | Nome do Curso |
| [[id-curso]] |  &#039;pts123&#039;, &#039;netflix1234&#039;, &#039;desc-10&#039; e etc e etc | SKU do Curso |
| [[preco-curso]] | &#039;3500&#039; e etc | Preço do curso |
| [[marca-curso]] | &#039;centauro&#039;, &#039;magalu&#039;, &#039;viagens&#039; e etc | Quem está oferencedo o curso |
| [[categoria-curso]] | &#039;montar-meu-clube&#039; | Categoria do curso |
| [[lista-curso]] | &#039;mais_procurados&#039;, &#039;novos_cursos&#039; e etc | Nome da lista que o curso aparece |
| [[posicao-curso]]     | '1','2'           | Posição que o produto aparece na lista  |
| [[variacao-curso]]    | '24_horas/curso_online', '3_horas/curso_online'           | Carga Horária do curso             |

<br />

### AddToCart:

**No clique para adicionar um produto ** <br />
- **Onde:** O objeto javascript (dataLayer) abaixo deve ser disparado uma única vez no clique do CTA de adição de produto ao carrinho.

```html
<script>
dataLayer.push({
  'event': 'addToCart',
  'eventCategory': 'enhanced-ecommerce',
  'eventAction': 'add-to-cart',
  'eventLabel': '',
  'ecommerce': {
    'add': {
      'products': [{
        'name': '[[nome-curso]]',       //Nome ou ID do produto é obrigatório
        'id': '[[id-curso]]',
        'price': '[[preco-curso]]',
        'brand': '[[marca-curso]]',
        'category': '[[categoria-curso]]',
        'variant': '[[variacao-curso]]',
        'quantity': '[[quantidade-curso]]'
      }]
    }
  }
});
</script>
```

<br />

**Catálogo - Produtos**

<br />

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-curso]] | &#039;excel_modulo_1&#039;, &#039;lideranca_corporativa&#039; e etc | Nome do Curso |
| [[id-curso]] |  &#039;pts123&#039;, &#039;netflix1234&#039;, &#039;desc-10&#039; e etc e etc | SKU do Curso |
| [[preco-curso]] | &#039;3500&#039; e etc | Preço do curso |
| [[marca-curso]] | &#039;centauro&#039;, &#039;magalu&#039;, &#039;viagens&#039; e etc | Quem está oferencedo o curso |
| [[categoria-curso]] | &#039;montar-meu-clube&#039; | Categoria do curso |
| [[lista-curso]] | &#039;mais_procurados&#039;, &#039;novos_cursos&#039; e etc | Nome da lista que o curso aparece |
| [[posicao-curso]]     | '1','2'           | Posição que o produto aparece na lista  |
| [[variacao-curso]]    | '24_horas/curso_online', '3_horas/curso_online'           | Carga Horária do curso             |


<br />
 
### Checkout:
 
- **Onde:** O objeto javascript (dataLayer) abaixo deve ser disparado uma única vez no carregamento das páginas do checkout.

```html
<script>
dataLayer.push({
  'event': 'checkout',
  'eventCategory': 'enhanced-ecommerce',
  'eventAction': 'checkout-[[passo-checkout]]',
  'ecommerce': {
    'checkout': {
      'actionField': {'step': '[[passo-checkout]]'},
      'products': [{
        'name': '[[nome-curso]]',       //Nome ou ID do produto é obrigatório
        'id': '[[id-curso]]',
        'price': '[[preco-curso]]',
        'brand': '[[marca-curso]]',
        'category': '[[categoria-curso]]',
        'variant': '[[variacao-curso]]',
        'quantity': '[[quantidade-curso]]'
      }]
    }
  }
});
</script>
```
 
<br />
 
| Variável          | Exemplo           | Descrição                 |
| :------------------------ | :------------------------ | :---------------------------------------- |
| [[passo-checkout]]      | Pagamento = 1/ Dados = 2 / Confirmação = 3        | Retornar '1', '2', '3' ou '4' de acordo com a página que o usuário está.                |

<br />

**Catálogo - Produtos**

<br />

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-curso]] | &#039;excel_modulo_1&#039;, &#039;lideranca_corporativa&#039; e etc | Nome do Curso |
| [[id-curso]] |  &#039;pts123&#039;, &#039;netflix1234&#039;, &#039;desc-10&#039; e etc e etc | SKU do Curso |
| [[preco-curso]] | &#039;3500&#039; e etc | Preço do curso |
| [[marca-curso]] | &#039;centauro&#039;, &#039;magalu&#039;, &#039;viagens&#039; e etc | Quem está oferencedo o curso |
| [[categoria-curso]] | &#039;montar-meu-clube&#039; | Categoria do curso |
| [[lista-curso]] | &#039;mais_procurados&#039;, &#039;novos_cursos&#039; e etc | Nome da lista que o curso aparece |
| [[posicao-curso]]     | '1','2'           | Posição que o produto aparece na lista  |
| [[variacao-curso]]    | '24_horas/curso_online', '3_horas/curso_online'           | Carga Horária do curso             |

<br />
 
### Purchase:
 
- **Onde:** O objeto javascript (dataLayer) abaixo deve ser disparado uma única vez no carregamento da página de transação, e se o usuário acessar novamente o link ou atualizar a página, o objeto não deve ser disparado novamente.
 
```html
<script>
dataLayer.push({
  'event': 'purchase',
  'eventCategory': 'enhanced-ecommerce',
  'eventAction': 'purchase',
  'eventLabel': '',
  'ecommerce': {
    'purchase': {
      'actionField': {
        'id': '[[id-transacao]]',       //ID da transação é obrigatório
        'affiliation': '[[loja-transacao]]',          //caso não tenha a informação, basta deixar a string vazia
        'revenue': '[[valor-total-transacao]]',       
        'tax': '[[taxa-transacao]]',                  //caso não tenha a informação, basta deixar a string vazia
        'shipping': '[[frete-transacao]]',            //caso não tenha a informação, basta deixar a string vazia
        'coupon': '[[cupom-transacao]]',              //caso não tenha a informação, basta deixar a string vazia
      },
      'products': [{
        'name': '[[nome-curso]]',       //Nome ou ID do produto é obrigatório
        'id': '[[id-curso]]',
        'price': '[[preco-curso]]',
        'brand': '[[marca-curso]]',
        'category': '[[categoria-curso]]',
        'variant': '[[variacao-curso]]'
      }]
    }
  }
});
</script>
```

<br />
 
*Descrição Produtos:*
 

**Catálogo - Produtos**

<br />

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-curso]] | &#039;excel_modulo_1&#039;, &#039;lideranca_corporativa&#039; e etc | Nome do Curso |
| [[id-curso]] |  &#039;pts123&#039;, &#039;netflix1234&#039;, &#039;desc-10&#039; e etc e etc | SKU do Curso |
| [[preco-curso]] | &#039;3500&#039; e etc | Preço do curso |
| [[marca-curso]] | &#039;centauro&#039;, &#039;magalu&#039;, &#039;viagens&#039; e etc | Quem está oferencedo o curso |
| [[categoria-curso]] | &#039;montar-meu-clube&#039; | Categoria do curso |
| [[lista-curso]] | &#039;mais_procurados&#039;, &#039;novos_cursos&#039; e etc | Nome da lista que o curso aparece |
| [[posicao-curso]]     | '1','2'           | Posição que o produto aparece na lista  |
| [[variacao-curso]]    | '24_horas/curso_online', '3_horas/curso_online'           | Carga Horária do curso             |

<br />
 
---
 
 
## Considerações Finais
 
> Recomendações do Google:
> 1. [Enhanced Ecommerce - Product Impressions](https://developers.google.com/tag-manager/enhanced-ecommerce#product-impressions)
> 2. [Enhanced Ecommerce - Product Clicks](https://developers.google.com/tag-manager/enhanced-ecommerce#product-clicks)
> 3. [Enhanced Ecommerce - Product Detail](https://developers.google.com/tag-manager/enhanced-ecommerce#details)
> 4. [Enhanced Ecommerce - AddToCart / Remove From Cart](https://developers.google.com/tag-manager/enhanced-ecommerce#cart)
> 5. [Enhanced Ecommerce - Promotion Impression](https://developers.google.com/tag-manager/enhanced-ecommerce#promo-impressions)
> 6. [Enhanced Ecommerce - Promotion Click](https://developers.google.com/tag-manager/enhanced-ecommerce#promo-clicks)
> 7. [Enhanced Ecommerce - Checkout](https://developers.google.com/tag-manager/enhanced-ecommerce#checkout)
> 8. [Enhanced Ecommerce - Purchase](https://developers.google.com/tag-manager/enhanced-ecommerce#purchases)

> Em caso de dúvidas, entrar em contato com: [](mailto:@zoly.com.br)
 
<script>
  document.addEventListener("DOMContentLoaded", function(event) {
    document.querySelectorAll("h1 a")[0].style.display = 'none';
  });
</script>
