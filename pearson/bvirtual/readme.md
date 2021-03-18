![BVirtual](https://www.foa.unesp.br/Home/instituicao/biblioteca108/pearson.png)

> Documento de Especificação Técnica

<br />

## Implementação da Camada de dados - BV
Última atualização: 17/03/2021 <br />
Em caso de dúvidas, entrar em contato com: [thiago.ceccato@pearson.com](mailto:thiago.ceccato@pearson.com)

<br />

## Objetivo
Este documento tem como objetivo instruir a implementação da camada de dados e de data attributes para utilização de recursos de monitoramento do Google Analytics referentes ao ambiente de [URL Produção: https://plataforma.bvirtual.com.br/](https://plataforma.bvirtual.com.br/).

<br />

### **Descrição Geral**

O `snippet` do Google Tag Manager é um pequeno trecho de código javascript ou non-javascript, através do uso de um iframe quando o javascript não está disponível, que é inserido nas páginas do site, tornando possível que a configuração das tags sejam realizadas via interface.


### **Posicionamento do Código - Google Tag Manager**

#### 1. Copie o seguinte JavaScript e cole-o o mais próximo da tag `<head>` de abertura possível em todas as páginas do seu site.

```html

<html>
  <head>
    <!-- Google Tag Manager -->
    <script>
        (function (w, d, s, l, i) {
            w[l] = w[l] || []; w[l].push({
                'gtm.start':
                    new Date().getTime(), event: 'gtm.js'
            }); var f = d.getElementsByTagName(s)[0],
                j = d.createElement(s), dl = l != 'dataLayer' ? '&l=' + l : ''; j.async = true; j.src =
                    'https://www.googletagmanager.com/gtm.js?id=' + i + dl; f.parentNode.insertBefore(j, f);
        })(window, document, 'script', 'dataLayer', 'GTM-KTG9WHQ');
    </script>
    <!-- End Google Tag Manager -->
    //...
  </head>
```

#### 2. Copie o seguinte trecho e cole-o imediatamente após a marcação `<body>` de abertura em cada página do seu site.

```html
<body>
  <!-- Google Tag Manager (noscript) -->
    <noscript>
        <iframe src="https://www.googletagmanager.com/ns.html?id=GTM-KTG9WHQ"
                height="0" width="0" style="display:none;visibility:hidden"></iframe>
    </noscript>
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


**No clique dos links do Header **<br />

- **Onde:** Em todas as páginas em que estiverem disponíveis
    - **Titulo ou nome do botão/link:** &quot;Ir para conteudo&quot;, &quot;ir para menu&quot; e etc
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:geral',
    'eventAction': 'clique:header_topo',
    'eventLabel': '[[nome-menu]]%',
    'noInteraction': '1',
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-menu]] | &#039;ir_para_conteudo&#039;, &#039; ir_para_menu&#039;, &#039;sobre_a_pearir_para-buscason&#039;, e etc | Deve retornar o nome do menu clicado. |

<br />


**No clique dos links do Menu Lateral**<br />

- **Onde:** Em todas as páginas em que estiverem disponíveis
    - **Titulo ou nome do botão/link:** &quot;Inicio&quot;, Expert Header&quot;Acervo&quot; e etc
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:geral',
    'eventAction': 'clique:menu_lateral',
    'eventLabel': '[[nome-menu]]%',
    'noInteraction': '1',
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-menu]] | &#039;inicio&#039;, &#039;expert_header&#039; e &#039;Acervo&#039; | Deve retornar o nome clicado |

<br />


**No Clique do botao Home**<br />

- **Onde:** No Inicio
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:home',
    'eventAction': 'clique:botao',
    'eventLabel': '[[sugestoes_de_leitura]]%',
    'noInteraction': '1',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sugestoes_de_leitura]] | &#Introducao_a_bioquimica&#039;, &#Soltando_as_amarras&#039; | Deve retornar ao botao home. |

<br />

**No clique do link Expert Reader**<br />

- **Onde:** Expert Reader
    - **Titulo ou nome do botão/link:** &quot;Expert Reader&quot; e etc
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:expert_reader',
    'eventAction': 'clique:link',
    'eventLabel': '[[nome-link]]%',
    'noInteraction': '1',
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-link]] | &#039;Todas_as_materias&#039;, &#039;Reviews&#039; e &#039;Dicas_de_leitura&#039; | Deve retornar o nome do link clicado. |


<br />

**No clique dos link Acervo**<br />

- **Onde:** Acervo
    - **Titulo ou nome do botão/link:** &quot;Acervo&quot; e etc
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:acervo',
    'eventAction': 'clique:categoria',
    'eventLabel': '[[nome-categoria]]%',
    'noInteraction': '1',
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-categoria]] | &#039;Categoria&#039;, &#039;Subcategoria&#039; e &#039;Editora&#039; | Deve retornar o nome do link clicados. |


<br />





### Product Click:Minhas Listas

**No clique em Menu **<br /> 

- **Onde:** Minhas Listas

```html
<script>
dataLayer.push({
  'event': 'productClick',
  'eventCategory': 'bvirtual:minhas_listas',
  'eventAction': 'clique:link',
  'eventLabel': '[[nome-link]]',
  'ecommerce': {
    'click': {
      'actionField': {'list': '[[lista-livro]]'},
      'products': [{
        'name': '[[nome-livro]]',       //Nome ou ID do produto é obrigatório
        'id': '[[id-livro]]',
        'price': '[[preco-livro]]',
        'brand': '[[marca-livro]]',
        'category': '[[categoria-livro]]',
        'variant': '[[variacao-livro]]',
        'position': '[[posicao-livro]]'
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
| [[nome-link]] | &#039;Minhas_listasvejo&#039;, &#039;Continuar_lendo&#039; e etc | Nome da Lista |
| [[nome-lista]]| &#039;Crias_nova_lista&#039; e etc | Direciona seua lista |
| [[nome-lista]] | &#039;Onze-tons_de_felicidade&#039;e etc | Direciona livros selecionados |
| [[nome-lista]]| &#039;Escola&#039;,&#Iphone&#039; | Modo de leitura |

<br />



**No clique do menu"**<br />

- **Onde:** Em Continuar lendo

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:continuar_lendo',
    'eventAction': 'clique:link',
    'eventLabel': '[[nome-link]]%',
    'noInteraction': '1',
  });
</script>
```
<br />

**Catálogo - Produtos**

<br />

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-link]] | &#039;minhas_listas&#039;, &#039;continuar_lendo&#039; e etc | lista de livros |
| [[nome-livro]]|  &#039;Espirito_empreendedor&#039;, &#039;Quimica_geral&#039; e etc e etc | Nome do livro |
| [[nome-autor]] | &#039;Giselle_Dziura&#039; e etc | Nome do Autor |
| [[nome-imagem]]| &#039;Espirito_empreendedor&#039;, &#039;Quimica_geral&#039; e etc | Nome do livro |


<br />



**No clique em filtrar ** <br />

- **Onde:** Cartoes de estudo.

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:cartoes',
    'eventAction': 'clique:filtrar',
    'eventLabel': '[[valor-digitado]]%',
    'noInteraction': '1',
  });
</script>
```

<br />

**Catálogo - Produtos**

<br />

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[valor-digitado]]| &#039;Filtrar_por_palavra_chave&#039;e etc | Filtro por palavra chave |
| [[nome-cartao]] |  &#039;Nome_do_cartao_criado&#039; e etc e etc | Nome do cartao |



<br />
 
### No clique em Titulo do Livro Adicionado
 
- **Onde:** Destaque e Notas.

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:destaques',
    'eventAction': 'clique:titulo',
    'eventLabel': '[[nome-titulo]]%',
    'noInteraction': '1',
  });
</script>
```


**Catálogo - Produtos**

<br />

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-titulo]]| &#039;Destaque&#039;, &#039;Notas&#039; e etc | Referencia sobre a leitura  |
| [[nome-livro]] &#039;Sobre_gatos&#039;, &#039;Alcool_e_drogas&#039; e etc e etc | Nome do livro |
| [[nome-autor]] | &#039;Lessing_Doris&#039; &#039;Guilherme_Messas&#039; e etc | Nome do autor|
| [[nome-notas]] | &#039;Paginas_marcadas&#039;, &#039;Destaques&#039; , &#039;Notas&#039; e etc | LIvro acionado |


<br />
 
### No clique do clique do menu
 
- **Onde:** sugestoes de leitura
 
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:sugestoes',
    'eventAction': 'clique:sugestao_de_leitura',
    'eventLabel': '[[nome-link]]%',
    'noInteraction': '1',
  });
</script>
```

<br />
 

**Catálogo - Produtos**

<br />

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-link]] | &#039;COntinuar_lendo&#039;, &#039;Livros_lidos&#039; e etc | Controle da leitura |
| [[nome-livro]] |  &#039;Financas&#039;, &#039;Fundamentos_de_quimica&#039; e etc e etc | Nome do livro |
| [[nome-autor]] | &#039;Valter_Pereira&#039; e etc | Nome do autor |
| [[nome-livro]]| &#039;Adicionar_a_uma_lista&#039;, &#039;comprar_esse_livro&#039; e etc | Direcionamento do livro |


<br />
 
---

<br />
 
### No clique do menu
 
- **Onde:** Livros lidos
 
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:livros_lidos',
    'eventAction': 'clique:livros_lidos',
    'eventLabel': '[[nome-link]]%',
    'noInteraction': '1',
  });
</script>
```

<br />

**Catálogo - Produtos**

<br />

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-link]] | &#039;Continuar_lendo&#039;, &#039;Livros_lidos&#039; e etc | Situação do livro|
| [[nome-livro]]|  &#039;Financas&#039;, &#039;Fundamentos_de_quimica&#039; e etc e etc | Nome do livro |
| [[nome-autor]] | &#039;Valter_Pereira&#039; e etc | Nome do autor |
| [[nome-livro]] | &#039;Ler_agora&#039;, &#039;Adicionar_a_uma_lista&#039;, &#039;comprar_esse_livro&#039; e etc | direcionamento do livro |


<br />
 
---

<br />
 
### No clique em metas de leitura
 
- **Onde:** Metas de leitura
 
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:metas',
    'eventAction': 'clique:ativar',
    'eventLabel': '[[botao-ativar]]%',
    'noInteraction': '1',
  });
</script>
```

<br />
 

**Catálogo - Produtos**

<br />

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao-ativar]] | &#039;Ativar_meta_de_leitura&#039;, &#039;Desativar_meta_de_leitura&#039; e etc | Status da meta de leitura |
| [[valor-digitado]] |  &#039;Numero_de_paginas&#039; e etc e etc | Quantidade de paginas do livro |
| [[valor-botao]]| &#039;Por_dia&#039;, &#039;Por_mes&#039;  e etc | Frequencia da leitura|
| [[valor-botao]]| &#039;Segunda&#039;, &#039;Terca&#039;, &#039;Quarta&#039; e etc | Dias de folga de leitura |
| [[nome-salvar]] | &#039;Sucesso&#039; | Salvar preferencias |


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
