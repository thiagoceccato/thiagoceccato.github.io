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

### Dimensões Globais:

**Dimensões Customizadas para todas as páginas:**<br />
Deve ser disparado um push de dataLayer no momento de carregamento de todas as páginas do site (Considerar também todas as trocas de Path, quando o conteúdo da página é alterado mas a página não recarrega).<br />

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'dimension1': '[[userid]]',
    'dimension2': '[[usersegment]]',
    'dimension4': '[[gaid]]',
    'dimension5': '[[gtm-id]]',
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[userid]] | &quot;01234&quot; | ID único de usuário definido após o cadastro |
| [[usersegment]] | &quot;Santander Select&quot;, &quot;Santander Private Baking&quot; | Tipo de usuário |
| [[gaid]] | XXXXXXXXXX:XXXX:XX | ClientID interno do Analytics |
| [[gtm-id]] | GTM-XXXXXX:XX | ID do container do GTM instalado no ambiente |

---


### General


**No clique dos links do menu superior (Deve retornar o tagueamento conforme o item clicado pelo usuário. Ex: no menu superior trazer somente a variável de [[menu-superior]], já quando for clicado no submenu, retornar a variável de [[menu-superior]]:[[submenu]] e etc)**<br />

- **Onde:** Em todas as páginas em que estiverem disponíveis
    - **Titulo ou nome do botão/link:** &quot;Esfera Facebook&quot;, &quot;Esfera Instagram&quot;, &quot;Esfera Linkedin&quot; e etc
    
```html
<div
   data-gtm-event-category='santander:esfera:geral'
   data-gtm-event-action='clique:link:header'
   data-gtm-event-label='[[menu-superior]]:[[submenu]]:[[item-submenu]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[menu-superior]] | &#039;produtos&#039;, &#039;viagens&#039;, &#039;vale-compras&#039;, &#039;entre&#039;, &#039;carrinho&#039; e etc | Deve retornar o nome do menu clicado. |
| [[submenu]] | &#039;tv-audio-video&#039;, &#039;casa-decoracao&#039;, &#039;passagens&#039;, &#039;pacotes&#039;, &#039;diversao-lazer&#039; e etc | Deve retornar o nome do submenu clicado. |
| [[item-submenu]] | &#039;tv&#039;, &#039;fone-de-ouvido&#039;, &#039;camera-instantanea&#039;, &#039;drone&#039;, &#039;exercicios-e-academia&#039; e etc | Deve retornar o nome do item dentro do submenu. |

<br />


**No clique dos links de rede social do footer**<br />

- **Onde:** Em todas as páginas em que estiverem disponíveis
    - **Titulo ou nome do botão/link:** &quot;Esfera Facebook&quot;, &quot;Esfera Instagram&quot;, &quot;Esfera Linkedin&quot; e etc
    
```html
<div
   data-gtm-event-category='santander:esfera:geral'
   data-gtm-event-action='clique:link:footer'
   data-gtm-event-label='rede-social:[[nome-rede]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-rede]] | &#039;esfera_facebook&#039;, &#039;esfera_instagram&#039; e &#039;esfera_linkedin&#039; | Deve retornar o nome da rede social clicada. |

<br />


**No clique dos links do footer**<br />

- **Onde:** Em todas as páginas em que estiverem disponíveis
    - **Titulo ou nome do botão/link:** &quot;Produtos&quot;, &#039;Vale Compras&quot;, &#039;Quem Somos&quot; e etc
    
```html
<div
   data-gtm-event-category='santander:esfera:geral'
   data-gtm-event-action='clique:link:footer'
   data-gtm-event-label='[[nome-link]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-link]] | &#039;produtos&#039;, &#039;vale-comrpas&#039;, &#039;quem-somos&#039; e etc | Deve retornar o nome do link clicado. |

<br />


### Home

**Na interação com os ícones de mais ou menos na seção "Cashback pra vc"**<br />

- **Onde:** Na página home
    - **Titulo ou nome do botão/link:** "mais", "menos"
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'santander:esfera:home',
    'eventAction': 'interacao:icone:cashback',
    'eventLabel': '[[nome-icone]]:[[qtd-pontos]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-icone]] | 'mais' ou 'menos' | Deve retornar o nome do icone clicado. |
| [[qtd-pontos]] | '1100', '1200' e etc | Deve retornar a quantidade de pontos. |

<br />


**No clique do botão "Simular" na seção "Cashback pra vc"**<br />

- **Onde:** Na página home
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'santander:esfera:home',
    'eventAction': 'clique:botao:cashback',
    'eventLabel': 'simular:[[qtd-pontos]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[qtd-pontos]] | '1100', '1200' e etc | Deve retornar a quantidade de pontos. |

<br />


**No clique do botão ou link após clicar em  "Simular" na seção "Cashback pra vc"**<br />

- **Onde:** Na página home
    - **Titulo ou nome do botão/link:** "Confira aqui o passo a passo", "fazer-login-e-trocar"
    
```html
<div
   data-gtm-event-category='santander:esfera:home'
   data-gtm-event-action='clique:[[botao ou link]]:cashback'
   data-gtm-event-label='[[nome-item]]:[[qtd-pontos]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao ou link]] | 'botao' ou 'link' | Deve retornar o tipo de elemento clicado. |
| [[nome-item]] | 'confira-aqui-o-passo-a-passo', 'fazer-login-e-trocar' | Deve retornar o nome do item clicado. |
| [[qtd-pontos]] | '1100', '1200' e etc | Deve retornar a quantidade de pontos. |

<br />


**No callback de sucesso ou erro após clicar no botão para "Adicionar ao carrinho", depois selecionar o cartão para abater o valor**<br />

- **Onde:** Na página home logada
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'santander:esfera:home',
    'eventAction': 'clique:botao:cashback:callback',
    'eventLabel': 'adicionar-carrinho:[[status]]:[[qtd-pontos]]:[[cartao-selecionado]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | 'sucesso', 'erro' e etc | Deve retornar o status do callback. |
| [[qtd-pontos]] | '1100', '1200' e etc | Deve retornar a quantidade de pontos. |
| [[cartao-selecionado]] | 'cartao1', 'cartao2' e etc | Deve retornar a posição que o cartão estava no campo de seleção. |

<br />


### Parceiros

**No clique no botão de &quot;Ir para o site do parceiro&quot;**<br />

- **Onde:** Nas páginas de parceiros
    - **Titulo ou nome do botão/link:** &quot;Ir para o site do parceiro&quot;
    
```html
<div
   data-gtm-event-category='santander:esfera:parceiro'
   data-gtm-event-action='clique:botao:[[parceiro]]'
   data-gtm-event-label='ir-para-site-parceiro'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[parceiro]] | &#039;fast-shop&#039;, &#039;radio-taxi-vermelho-branco&#039; e etc | Deve retornar o nome do parceiro clicado. |

<br />


**No clique nos cards, dentro das seções abaixo de &quot;Cartões Elegíveis&quot;**<br />

- **Onde:** Nas páginas de parceiros
    - **Titulo ou nome do botão/link:** &quot;Crédito combustível Shell Box&quot;, &quot;Uber-crédito para viagens&quot; e etc
    
```html
<div
   data-gtm-event-category='santander:esfera:parceiro'
   data-gtm-event-action='clique:card:[[parceiro]]'
   data-gtm-event-label='[[nome-secao]]:[[nome-card]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[parceiro]] | &#039;fast-shop&#039;, &#039;radio-taxi-vermelho-branco&#039; e etc | Deve retornar o nome do parceiro clicado. |
| [[nome-secao]] | &#039;pessoas-que-compraram-este-produto-tambem-compraram&#039;, &#039;ganhe-mais-pontos-com-nossos-parceiros&#039; e etc | Deve retornar o nome da seção clicada pelo usuário. |
| [[nome-card]] | &#039;ponto-frio-ate-25%-desconto&#039;, &#039;extra-ate-25%-desconto&#039;, &#039;credito-combustivel-shell-box&#039;, &#039;reservecar-2pts-cada-real&#039; e etc | Deve retornar o nome do card clicado. |

<br />


**Na interação com opçoes de &quot;Sobre o desconto&quot;, &quot;Como funciona&quot;, &quot;Cartões elegíveis&quot; e etc**<br />

- **Onde:** Nas páginas de parceiros
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'santander:esfera:parceiro',
    'eventAction': 'clique:opcao:[[nome-opcao]]:[[parceiro]]',
    'eventLabel': '[[abriu-ou-fechou]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-opcao]] | &#039;como-funciona&#039;, &#039;sobre-beneficio&#039;, &#039;cartoes-elegiveis&#039; e etc | Deve retornar o nome da opção clicada. |
| [[parceiro]] | &#039;fast-shop&#039;, &#039;radio-taxi-vermelho-branco&#039; e etc | Deve retornar o nome do parceiro clicado. |
| [[abriu-ou-fechou]] | &#039;abriu&#039;, &#039;fechou&#039; | Deve retornar o status da opção, se está aberta ou fechada. |

<br />


**No clique nos links dentro da opções**<br />

- **Onde:** Nas páginas de parceiros
    - **Titulo ou nome do botão/link:** &quot;Acesse aqui&quot;, &quot;clique aqui&quot; e etc
    
```html
<div
   data-gtm-event-category='santander:esfera:parceiro'
   data-gtm-event-action='clique:link:[[nome-opcao]]:[[parceiro]]'
   data-gtm-event-label='[[nome-link]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-opcao]] | &#039;como-funciona&#039;, &#039;sobre-beneficio&#039;, &#039;cartoes-elegiveis&#039; e etc | Deve retornar o nome da opção clicada. |
| [[parceiro]] | &#039;fast-shop&#039;, &#039;radio-taxi-vermelho-branco&#039; e etc | Deve retornar o nome do parceiro clicado. |
| [[nome-link]] | &#039;clique-aqui&#039;, &#039;acesse-aqui&#039; e etc | Deve retornar o nome do link clicado. |

<br />


**No clique nos banners**<br />

- **Onde:** Nas páginas de parceiros
    - **Titulo ou nome do botão/link:** &quot;Acelere sua pontuação com parceiros Esfera&quot;
    
```html
<div
   data-gtm-event-category='santander:esfera:parceiro'
   data-gtm-event-action='clique:banner:[[parceiro]]'
   data-gtm-event-label='[[nome-banner]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[parceiro]] | &#039;fast-shop&#039;, &#039;radio-taxi-vermelho-branco&#039; e etc | Deve retornar o nome do parceiro clicado. |
| [[nome-banner]] | &#039;acelere-sua-pontuacao-com-parceiros-esfera&#039; e etc | Deve retornar o nome do banner clicado. |

<br />


**No clique no link de &quot;Ver todos&quot;**<br />

- **Onde:** Nas páginas de parceiros
    - **Titulo ou nome do botão/link:** &quot;Ver todos&quot;
    
```html
<div
   data-gtm-event-category='santander:esfera:parceiro'
   data-gtm-event-action='clique:link:[[parceiro]]'
   data-gtm-event-label='ver-todos'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[parceiro]] | &#039;fast-shop&#039;, &#039;radio-taxi-vermelho-branco&#039; e etc | Deve retornar o nome do parceiro clicado. |

<br />


### Clube Esfera

**Na rolagem de página**<br />

- **Onde:** Na página Clube Esfera
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'santander:esfera:clube-esfera',
    'eventAction': 'scroll-tracking',
    'eventLabel': '[[valor-porcentagem]]%'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[valor-porcentagem]] | &#039;10&#039;, &#039;20&#039;, &#039;40&#039; | Deve retornar percentual de scroll. |

<br />


**No clique das opções da seção de &quot;Conheça o Clube Esfera&quot;**<br />

- **Onde:** Na página de Clube Esfera
    - **Titulo ou nome do botão/link:** &quot;escolha um pacote de pontos&quot;, &quot;Escolha 3 lojas ou serviços&quot;, &quot;Aproveite descontos e promoções&quot; e etc
    
```html
<div
   data-gtm-event-category='santander:esfera:clube-esfera'
   data-gtm-event-action='clique:opcoes:clube-esfera'
   data-gtm-event-label='[[nome-opcao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-opcao]] | &#039;escolha-um-pacote-de-pontos-que-nao-expiram&#039;, &#039;escolha-3-lojas-ou-servicos&#039;,  &#039;aproveite-descontos-promocoes&#039; e etc | Deve retornar o nome da opção clicada. |

<br />


**No clique do botão de &quot;Montar meu Clube&quot;**<br />

- **Onde:** Na página de Clube Esfera
    - **Titulo ou nome do botão/link:** &quot;Montar meu Clube&quot;
    
```html
<div
   data-gtm-event-category='santander:esfera:clube-esfera'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='montar-meu-clube'
>Botão</div>
```



<br />


**No clique do ícone de informações (i) das lojas favoritas**<br />

- **Onde:** Na página de Clube Esfera
    
```html
<div
   data-gtm-event-category='santander:esfera:clube-esfera'
   data-gtm-event-action='clique:icone:lojas-favoritas'
   data-gtm-event-label='infomacoes'
>Botão</div>
```

<br />

**Na visualização dos cards de produtos de &quot;Monte o clube do seu jeito&quot;**<br />

- **Onde:** Em todas as páginas que listarem cards de &quot;Montar meu clube&quot;
    

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'productImpressions',
  'eventCategory':'santander:esfera:clube-esfera',
  'eventAction': 'product-impression',
  'eventLabel': '[[secao]]',
  'noInteraction': '1',
  'ecommerce': {
    'impressions': [{
        'name': '[[nome-produto]]',
        'id': '[[id-produto]]',
        'list': '[[lista-produto]]',
        'price': '[[preco-produto]]',
        'brand': '[[marca-produto]]',
        'category': '[[categoria-produto]]'
    }]
  }
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[secao]] | &#039;quantos-pontos-deseja-ganhar&#039;, &#039;escolha-3-lojas-ou-servicos&#039;, &#039;aproveite-descontos-e-promocoes&#039; e etc | Deve retornar o nome da seção que o produto foi apresentado. |
| [[nome-produto]] | &#039;200pts&#039;, &#039;netflix&#039;, &#039;spotify&#039;, &#039;10%-desconto&#039; e etc | Nome do produto |
| [[id-produto]] |  &#039;pts123&#039;, &#039;netflix1234&#039;, &#039;desc-10&#039; e etc e etc | SKU do produto |
| [[preco-produto]] | &#039;3500&#039; e etc | Preço do produto em pontos |
| [[marca-produto]] | &#039;centauro&#039;, &#039;magalu&#039;, &#039;viagens&#039; e etc | Marca do produto |
| [[categoria-produto]] | &#039;montar-meu-clube&#039; | Arvore de categorias do produto no site |
| [[lista-produto]] | &#039;pontos-fixos-mes&#039;, &#039;lojas-servicos&#039;, &#039;descontos-promocoes&#039; e etc | Nome da lista que o produto aparece |

<br />



**No clique das opções de escolhas do cliente para sugerir um plano**<br />

- **Onde:** Na página de Clube Esfera
    - **Titulo ou nome do botão/link:** &quot;200 pts&quot;, &#039;Netflix&quot;, &quot;10% de desconto&quot; e etc
    
```html
<div
   data-gtm-event-category='santander:esfera:clube-esfera'
   data-gtm-event-action='clique:botao:[[nome-secao]]'
   data-gtm-event-label='[[opcao-escolhida]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-secao]] | &#039;quantos-pontos-deseja-ganhar&#039;, &#039;escolha-3-lojas-ou-servicos&#039;, &#039;aproveite-descontos-e-promocoes&#039; e etc | Deve retornar o nome da seção. |
| [[opcao-escolhida]] | &#039;200pts&#039;, &#039;netflix&#039;, &#039;netflix/spotify/centauro&#039;, &#039;10%-de-desconto&#039; e etc | Deve retornar a opção escolhida pelo usuário. |

<br />


**No clique do botão de &quot;Ver plano&quot; para ver a sugestão de um plano**<br />

- **Onde:** Na página de Clube Esfera
    
```html
<div
   data-gtm-event-category='santander:esfera:clube-esfera'
   data-gtm-event-action='clique:botao:plano-sugerido'
   data-gtm-event-label='ver-plano'
>Botão</div>
```



<br />


**Após clicar no botão de &quot;Selecionar Plano&quot;**<br />

- **Onde:** No lightbox de &quot;Sugestão para vc&quot;, na página de Clube Esfera
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'santander:esfera:clube-esfera',
    'eventAction': 'clique:botao:plano-sugerido',
    'eventLabel': 'selecionar-plano:[[opcoes-sugeridas]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[opcoes-sugeridas]] | &#039;200pts;netflix/spotify/centauro;10%-de-desconto&#039; e etc | Deve retornar as opções sugeridas, separadas por ponto e vírgula (;). |

<br />


**No clique das perguntas de FAQ, na seção de &quot;Continua com dúvida?&quot;**<br />

- **Onde:** Na página de Clube Esfera
    - **Titulo ou nome do botão/link:** &quot;Como Funciona&quot;, &quot;Como me cadastrar no Clube Esfera&quot; e etc
    
```html
<div
   data-gtm-event-category='santander:esfera:clube-esfera'
   data-gtm-event-action='clique:botao:continua-com-duvida'
   data-gtm-event-label='faq:[[nome-pergunta]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-pergunta]] | &#039;como-funciona&#039;, &#039;como-me-cadastrar-no-clube-esfera&#039;, &#039;troca-de-pontos&#039; e etc | Deve retornar o nome da pergunta clicada. |

<br />


### Clube Esfera - Montar Plano

**No clique do link de &quot;Alterar&quot; para trocar uma das opções**<br />

- **Onde:** Na página de Monte seu clube esfera
    - **Titulo ou nome do botão/link:** &quot;alterar&quot;
    
```html
<div
   data-gtm-event-category='santander:esfera:clube-esfera'
   data-gtm-event-action='clique:link:monte-seu-clube'
   data-gtm-event-label='[[nome-secao]]:alterar'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-secao]] | &#039;pacote-mensal-de-pontos&#039;, &#039;lojas-servicos-favoritos&#039; e etc | Deve retornar o nome da seção. |

<br />


**No clique das opções dentro dos menus de Pacote Mensal, Lojas e serviços e Por quanto quer multiplicar seus pontos**<br />

- **Onde:** Na página de Monte seu clube esfera
    - **Titulo ou nome do botão/link:** &quot;200 pts&quot;, &quot;Netflix&quot;, &quot;Spotify&quot;, &quot;x5&quot; e etc
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'santander:esfera:clube-esfera',
    'eventAction': 'clique:link:monte-seu-clube',
    'eventLabel': '[[nome-secao]]:[[opcao-escolhida]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-secao]] | &#039;pacote-mensal-de-pontos&#039;, &#039;lojas-servicos-favoritos&#039;, &#039;por-quanto-quer-multiplicar-pontos-gerados&#039; e etc | Deve retornar o nome da seção. |
| [[opcao-escolhida]] | &#039;200pts&#039;, &#039;netflix/spotify/centauro&#039;, &#039;x5&#039; e etc | Deve retornar a opção escolhida. |

<br />


**No clique das perguntas de FAQ, na seção de &quot;Continua com dúvida?&quot;**<br />

- **Onde:** Na página de Monte seu clube esfera
    - **Titulo ou nome do botão/link:** &quot;Como Funciona&quot;, &quot;Como me cadastrar no Clube Esfera&quot; e etc
    
```html
<div
   data-gtm-event-category='santander:esfera:clube-esfera'
   data-gtm-event-action='clique:botao:monte-seu-clube:continua-com-duvida'
   data-gtm-event-label='faq:[[nome-pergunta]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-pergunta]] | &#039;como-funciona&#039;, &#039;como-me-cadastrar-no-clube-esfera&#039;, &#039;troca-de-pontos&#039; e etc | Deve retornar o nome da pergunta clicada. |

<br />


**No clique do ícone de informações (i) das lojas favoritas**<br />

- **Onde:** Na página de Monte seu clube esfera
    
```html
<div
   data-gtm-event-category='santander:esfera:clube-esfera'
   data-gtm-event-action='clique:icone:monte-seu-clube:por-quanto-quer-multiplicar-pontos'
   data-gtm-event-label='infomacoes'
>Botão</div>
```



<br />


**Ao selecionar o checkbox para afirmar que leu o &#039;Termo &amp; Condições&quot;**<br />

- **Onde:** Na página de Monte seu clube esfera
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'santander:esfera:clube-esfera',
    'eventAction': 'select:checkbox:monte-seu-clube',
    'eventLabel': 'li-concordo-termo-condicoes'
  });
</script>
```




<br />


**Após clicar no botão de &quot;Confirmar meu clube&quot;**<br />

- **Onde:** Na página de Monte seu clube esfera
    
```html
<div
   data-gtm-event-category='santander:esfera:clube-esfera'
   data-gtm-event-action='clique:botao:monte-seu-clube'
   data-gtm-event-label='confirmar-meu-clube:[[pacote]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[pacote]] | &#039;pacote-mensal:200pts;pontos-x5-em:centauro/magalu/ifood&#039; e etc | Deve retornar o pacote escolhido pelo usuário. |

<br />


**Após clicar no botão de &quot;Usar outro cartão&quot;**<br />

- **Onde:** Na página de Checkout Pagamento
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'santander:esfera:clube-esfera:checkout',
    'eventAction': 'clique:botao:forma-pagamento',
    'eventLabel': '[[modo-pagamento]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[modo-pagamento]] | &#039;cartao-1&#039;, &#039;cartao-2&#039;, &#039;usar-outro-cartao&#039; e etc | Deve retornar o modo de pagamento. |

<br />


**No clique no botão de &quot;Continuar&quot;**<br />

- **Onde:** Na página de Checkout Pagamento
    
```html
<div
   data-gtm-event-category='santander:esfera:clube-esfera:checkout'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='continuar'
>Botão</div>
```



<br />


### Clube Esfera - Gestão da Conta

**No clique nos links de &quot;Alterar&quot; e &quot;Saiba como usar seu clube&quot;**<br />

- **Onde:** Na Página de Meu clube Esfera
    
```html
<div
   data-gtm-event-category='santander:esfera:meu-clube-esfera'
   data-gtm-event-action='clique:link:lojas-servicos-favoritos'
   data-gtm-event-label='[[nome-link]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-link]] | &#039;alterar&#039;, &#039;saiba-como-usar-seu-clube&#039; e etc | Deve retornar o nome do link clicado. |

<br />


**No clique nos botões, da seção de &quot;Gerenciar Assinatura&quot;**<br />

- **Onde:** Na Página de Meu clube Esfera
    
```html
<div
   data-gtm-event-category='santander:esfera:meu-clube-esfera'
   data-gtm-event-action='clique:botao:gerenciar-assinatura'
   data-gtm-event-label='[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;saiba-como-suar-seu-clube&#039;, &#039;ver-informacoes-de-pagamento&#039;, &#039;cancelar-assinatura&#039; e etc | Deve retornar o nome do botão clicado. |

<br />


**Ao selecionar o checkbox para afirmar que leu o &#039;Termo &amp; Condições&quot;**<br />

- **Onde:** Na Página de Meu clube Esfera
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'santander:esfera:meu-clube-esfera',
    'eventAction': 'select:checkbox:alterar-ljas-servicos-favoritos',
    'eventLabel': 'li-concordo-termo-condicoes'
  });
</script>
```




<br />


**Após clicar no botão de &quot;Confirmar alteração&quot;**<br />

- **Onde:** Na Página de Meu clube Esfera
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'santander:esfera:meu-clube-esfera',
    'eventAction': 'clique:botao',
    'eventLabel': 'confirmar-alteracao:[[opcoes-selecionadas]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[opcoes-selecionadas]] | &#039;magalu/ifood/rappi&#039; e etc | Deve retornar as opções selecionadas pelo usuário. |

<br />


**Após clicar no botão de &quot;Alterar&quot;**<br />

- **Onde:** No lightbox de Confirmar Alteração
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'santander:esfera:meu-clube-esfera',
    'eventAction': 'clique:botao:lightbox:confirmar-alteracao',
    'eventLabel': 'alterar:[[opcoes-selecionadas]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[opcoes-selecionadas]] | &#039;magalu/ifood/rappi&#039; e etc | Deve retornar as opções selecionadas pelo usuário. |

<br />


**No clique nos links**<br />

- **Onde:** Na Página de Como usar seu clube
    
```html
<div
   data-gtm-event-category='santander:esfera:como-usar-seu-clube'
   data-gtm-event-action='clique:link'
   data-gtm-event-label='[[nome-secao]]:[[nome-link]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-secao]] | &#039;meu-pacote-mensal-de-pontos-extras&#039;, &#039;descontos-exclusivos&#039; e etc | Deve retornar o nome da seção que o usuário está interagindo. |
| [[nome-link]] | &#039;trocar-por-produtos-ou-servicos&#039;, &#039;consultar-no-extrato&#039; e etc | Deve retornar o nome do link clicado. |

<br />


**No clique do ícone de informações (i)**<br />

- **Onde:** Na página de Como usar seu clube
    
```html
<div
   data-gtm-event-category='santander:esfera:como-usar-seu-clube'
   data-gtm-event-action='clique:icone'
   data-gtm-event-label='infomacoes:[[nome-secao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-secao]] | &#039;meu-pacote-mensal-de-pontos-extras&#039;, &#039;descontos-exclusivos&#039; e etc | Deve retornar o nome da seção que o usuário está interagindo. |

<br />


### LP App

**No clique dos botões**<br />

- **Onde:** Na página de app.
    
```html
<div
   data-gtm-event-category='santander:esfera:app'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='[[nome-botao]]:[[nome-secao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;baixe-o-app-e-aproveite&#039;, &#039;comece-a-usar&#039; e etc | Deve retornar o nome do botão clicado. |
| [[nome-secao]] | &#039;tenha-uma-experiencia-esfera-completa&#039; e etc | Deve retornar o nome da seção. |

<br />


### Enhanced E-commerce

## Enhanced Ecommerce

### Promotion Impression:

**Na visualização dos banners e cards** <br /> 

- **Onde:** O objeto javascript (dataLayer) abaixo deve ser disparado uma única vez na visualização de um banner ou card.
 
```html
<script>
dataLayer.push({
  'event': 'promotionImpression',
  'eventCategory': 'enhanced-ecommerce',
  'eventAction': 'promotion-impression',
  'eventLabel': '',
  'ecommerce': {
    'promoView': {
      'promotions': [{
        'id': '[[id-banner]]',        //Nome ou ID do banner é obrigatório
        'name': '[[nome-banner]]',
        'creative': '[[arte-banner]]',
        'position': '[[posicao-banner]]'
      }]
    }
  }
});
</script>
```

<br />

| Nome      | Variável          | Exemplo           | Descrição                 |
| :------------ | :------------------------ | :------------------------ | :---------------------------------------- |
| id      | [[id-banner]]       | 'banner123'           | ID do banner                |
| name      | [[nome-banner]]       | 'seja-bem-vindo-e-hora-de-explorar-a-esfera", "viagens-ha-sempre-um-novo-lugar-para-voce-conhecer'    | Nome do amigável do banner        |
| creative    | [[arte-banner]]       | 'banner-bem-vindo.jpg'            | Nome do arquivo de mídia          |
| position    | [[posicao-banner]]    | '1'           | Posição que o banner aparece na lista   |
 
<br />

### Promotion Click:

**No clique de um banner e cards** <br />

- **Onde:** O objeto javascript (dataLayer) abaixo deve ser disparado uma única vez no clique de um banner.
 
```html
<script>
dataLayer.push({
  'event': 'promotionClick',
  'eventCategory': 'enhanced-ecommerce',
  'eventAction': 'promotion-click',
  'eventLabel': '',
  'ecommerce': {
    'promoClick': {
      'promotions': [{
        'id': '[[id-banner]]',        //Nome ou ID do produto é obrigatório
        'name': '[[nome-banner]]',
        'creative': '[[arte-banner]]',
        'position': '[[posicao-banner]]'
      }]
    }
  }
});
</script>
```

<br />
 
| Nome      | Variável          | Exemplo           | Descrição                 |
| :------------ | :------------------------ | :------------------------ | :---------------------------------------- |
| id      | [[id-banner]]       | 'banner123'           | ID do banner                |
| name      | [[nome-banner]]       | 'seja-bem-vindo-e-hora-de-explorar-a-esfera", "viagens-ha-sempre-um-novo-lugar-para-voce-conhecer'    | Nome do amigável do banner        |
| creative    | [[arte-banner]]       | 'banner-bem-vindo.jpg'            | Nome do arquivo de mídia          |
| position    | [[posicao-banner]]    | '1'           | Posição que o banner aparece na lista   |
 
<br />

### Product Impression:

**Na visualização de uma vitrine de produtos - No caso de recursos de viagem, considerar a página de resultado de busca** <br />

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
        'name': '[[nome-produto]]',  // Nome ou ID do produto é obrigatório
        'id': '[[id-produto]]',
        'price': '[[preco-produto]]',
        'brand': '[[marca-produto]]',
        'category': '[[categoria-produto]]',
        'variant': '[[variacao-produto]]',
        'list': '[[lista-produto]]',
        'position': '[[posicao-produto]]',
        'dimension16': '[[loja]]',
        'dimension17': '[[tipo-produto]]',
        'dimension18': '[[cidade-retirada]]',
        'dimension19': '[[cidade-devolucao]]',
        'dimension20': '[[modelo-veiculo]]',
        'metric2': '[[valor-real]]'
      }, {
        'name': '[[nome-produto2]]',  // Nome ou ID do produto é obrigatório
        'id': '[[id-produto2]]',
        'price': '[[preco-produto2]]',
        'brand': '[[marca-produto2]]',
        'category': '[[categoria-produto2]]',
        'variant': '[[variacao-produto2]]',
        'list': '[[lista-produto2]]',
        'position': '[[posicao-produto2]]',
        'dimension16': '[[loja2]]',
        'dimension17': '[[tipo-produto]]',
        'dimension18': '[[cidade-retirada]]',
        'dimension19': '[[cidade-devolucao]]',
        'dimension20': '[[modelo-veiculo]]',
        'metric2': '[[valor-real2]]'
      }
    ]
  }
});
</script>
```

<br />

**Catálogo - Produtos**

<br />

| Nome      | Variável          | Exemplo           | Descrição                 |
| :------------ | :------------------------ | :------------------------ | :---------------------------------------- |
| name      | [[nome-produto]]      | 'jogo-de-panelas-tramontina-antiaderente'         | Nome do produto               |
| id      | [[id-produto]]      | '10649879'            | ID do produto               |
| price     | [[preco-produto]]     | '13999'           | Preço do produto em pontos            |
| brand     | [[marca-produto]]     | 'sony', 'samsung'           | Marca do produto              |
| category    | [[categoria-produto]]   | 'produto', 'milhas'           | Categoria do produto            |
| variant     | [[variacao-produto]]    | '110v'            | Variação do produto             |
| list      | [[lista-produto]]     | 'catalogo-produto'            | Nome da lista de produtos         |
| position    | [[posicao-produto]]     | '1'           | Posição que o produto aparece na lista  |
| dimension16   | [[loja]]    | 'ponto-frio', 'fast-shop'           | Nome da loja do market place oferecida  |
| dimension17   | [[tipo-produto]]  | 'transferencia-para-parceiro', 'viagem', 'cartao-presente', 'beneficio', 'produto-digital', 'acumulacao', 'produto-fisico', 'transferencia-de-pontos', 'compra-de-pontos'     | Tipo do produto disponibilizado na página |
| metric2     |[[valor-real]]|'129.50'|Valor total do produto em reais|


<br />

**Passagens**

**Considerar cada card de passagens como um único produto**

<br />

| Nome      | Variável          | Exemplo           | Descrição                 |
| :------------ | :------------------------ | :------------------------ | :---------------------------------------- |
| name      | [[nome-produto]]      | 'iv:gru-sdu/rec-vcp' (ida e volta), 'i:sdu-gru' (somente ida) e etc| Nome do voo considerando ida (i), ida e volta (iv), origem/destino (gru-sdu) |
| id      | [[id-produto]]      |  'cvcfligth'            | ID do produto               |
| price     | [[preco-produto]]     | '13999'           | Preço da passagem em pontos           |
| brand     | [[marca-produto]]     |  'avianca'          | Marca do compania aérea               |
| category    | [[categoria-produto]]   |  'passagens'            | Categoria do produto            |
| variant     | [[variacao-produto]]    |  'voo-direto', 'direto:bagagem-2', '1-parada:bagagem-1' e etc     | Tipo do voo e bagagem       |
| list      | [[lista-produto]]     | 'viagens:passagens', 'viagens:pacotes'            | Nome da lista de voos         |
| position    | [[posicao-produto]]     | '1'           | Posição que o produto aparece na lista  |
| dimension17   | [[tipo-produto]]  | 'transferencia-para-parceiro', 'viagem', 'cartao-presente', 'beneficio', 'produto-digital', 'acumulacao', 'produto-fisico', 'transferencia-de-pontos', 'compra-de-pontos'     | Tipo do produto disponibilizado na página |
| metric2     |[[valor-real]]|'129.50'|Valor total do produto em reais|

<br />

**Hotéis**

<br />

| Nome      | Variável          | Exemplo           | Descrição                 |
| :------------ | :------------------------ | :------------------------ | :---------------------------------------- |
| name      | [[nome-produto]]      | 'hotel-panamby-sao-paulo' (somente ida) e etc| Nome do hotel  |
| id      | [[id-produto]]      |  'cvchotel'           | ID do produto               |
| price     | [[preco-produto]]     | '20000'           | Preço do hotel em pontos            |
| brand     | [[marca-produto]]     |  'hotel', 'pousada' etc           | Tipo da hospedagem              |
| category    | [[categoria-produto]]   |  'hoteis'           | Categoria do produto            |
| variant     | [[variacao-produto]]    |  'apto-duplo', 'apto-casal', 'superior' etc     | Tipo de quarto        |
| list      | [[lista-produto]]     | 'viagens:hoteis', 'viagens:pacotes'           | Nome da lista de hotéis         |
| position    | [[posicao-produto]]     | '1'           | Posição que o produto aparece na lista  |
| dimension17   | [[tipo-produto]]  | 'transferencia-para-parceiro', 'viagem', 'cartao-presente', 'beneficio', 'produto-digital', 'acumulacao', 'produto-fisico', 'transferencia-de-pontos', 'compra-de-pontos'     | Tipo do produto disponibilizado na página |
| metric2     |[[valor-real]]|'129.50'|Valor total do produto em reais|


<br />

**Carros**

<br />

| Nome      | Variável          | Exemplo           | Descrição                 |
| :------------ | :------------------------ | :------------------------ | :---------------------------------------- |
| name      | [[nome-produto]]      | 'localiza:localiza-hertz-centro-faria-lima' etc| Nome da locação de carro considerando nome da locadora e nome da unidade ou loja da locadora   |
| id      | [[id-produto]]      |   'cvcvehicle'            | ID do produto               |
| price     | [[preco-produto]]     | '3500'            | Preço da locacao em pontos            |
| brand     | [[marca-produto]]     |   'movida', 'localiza'          | Nome da locadora              |
| category    | [[categoria-produto]]   |  'carros'           | Categoria do produto            |
| list      | [[lista-produto]]     | 'viagens:carros'            | Nome da lista de veículos         |
| position    | [[posicao-produto]]     | '1'           | Posição que o produto aparece na lista  |
| dimension17   | [[tipo-produto]]  | 'transferencia-para-parceiro', 'viagem', 'cartao-presente', 'beneficio', 'produto-digital', 'acumulacao', 'produto-fisico', 'transferencia-de-pontos', 'compra-de-pontos'     | Tipo do produto disponibilizado na página |
| dimension18   | [[cidade-retirada]]   | 'sao-paulo-sp'      | Nome da cidade selecionada para retirada do veículo |
| dimension19   | [[cidade-devolucao]]  | 'rio-de-janeiro-rj'     | Nome da cidade selecionada para devolução do veículo  |
| dimension20   | [[modelo-veiculo]]  | 'toyota-yaris'      | Nome do modelo do veículo |
| metric2     |[[valor-real]]|'129.50'|Valor total do produto em reais|


<br />

### Product Click:

**No clique em um produto (também considerar recursos de "viagens" - clique botão "Trocar", "Ver detalhes", "Selecionar" )**<br /> 

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
      'actionField': {'list': '[[lista-produto]]'},
      'products': [{
        'name': '[[nome-produto]]',       //Nome ou ID do produto é obrigatório
        'id': '[[id-produto]]',
        'price': '[[preco-produto]]',
        'brand': '[[marca-produto]]',
        'category': '[[categoria-produto]]',
        'variant': '[[variacao-produto]]',
        'position': '[[posicao-produto]]',
        'dimension16': '[[loja]]',
        'dimension17': '[[tipo-produto]]',
        'dimension18': '[[cidade-retirada]]',
              'dimension19': '[[cidade-devolucao]]',
              'dimension20': '[[modelo-veiculo]]',
        'metric2': '[[valor-real]]'
      }]
    }
  }
});
</script>
```

<br />

**Catálogo - Produtos**

<br />

| Nome      | Variável          | Exemplo           | Descrição                 |
| :------------ | :------------------------ | :------------------------ | :---------------------------------------- |
| name      | [[nome-produto]]      | 'jogo-de-panelas-tramontina-antiaderente'         | Nome do produto               |
| id      | [[id-produto]]      | '10649879'            | ID do produto               |
| price     | [[preco-produto]]     | '13999'           | Preço do produto em pontos            |
| brand     | [[marca-produto]]     | 'sony', 'samsung'           | Marca do produto              |
| category    | [[categoria-produto]]   | 'produto', 'milhas'           | Categoria do produto            |
| variant     | [[variacao-produto]]    | '110v'            | Variação do produto             |
| list      | [[lista-produto]]     | 'catalogo-produto'            | Nome da lista de produtos         |
| position    | [[posicao-produto]]     | '1'           | Posição que o produto aparece na lista  |
| dimension16     | [[loja]]    | 'ponto-frio', 'fast-shop'           | Nome da loja do market place oferecida  |
| dimension17   | [[tipo-produto]]  | 'transferencia-para-parceiro', 'viagem', 'cartao-presente', 'beneficio', 'produto-digital', 'acumulacao', 'produto-fisico', 'transferencia-de-pontos', 'compra-de-pontos'     | Tipo do produto disponibilizado na página |
| metric2     |[[valor-real]]|'129.50'|Valor total do produto em reais|

<br />

**Passagens**

**Considerar cada card de passagens como um único produto**

<br />

| Nome      | Variável          | Exemplo           | Descrição                 |
| :------------ | :------------------------ | :------------------------ | :---------------------------------------- |
| name      | [[nome-produto]]      | 'iv:gru-sdu/rec-vcp' (ida e volta), 'i:sdu-gru' (somente ida) e etc| Nome do voo considerando ida (i), ida e volta (iv), origem/destino (gru-sdu) |
| id      | [[id-produto]]      |  'cvcfligth'            | ID do produto               |
| price     | [[preco-produto]]     | '13999'           | Preço da passagem em pontos           |
| brand     | [[marca-produto]]     |  'avianca'          | Marca do compania aérea               |
| category    | [[categoria-produto]]   |  'passagens'            | Categoria do produto            |
| variant     | [[variacao-produto]]    |  'voo-direto', 'direto:bagagem-2', '1-parada:bagagem-1' e etc     | Tipo do voo e bagagem       |
| list      | [[lista-produto]]     | 'viagens:passagens', 'viagens:pacotes'            | Nome da lista de voos         |
| position    | [[posicao-produto]]     | '1'           | Posição que o produto aparece na lista  |
| dimension17   | [[tipo-produto]]  | 'transferencia-para-parceiro', 'viagem', 'cartao-presente', 'beneficio', 'produto-digital', 'acumulacao', 'produto-fisico', 'transferencia-de-pontos', 'compra-de-pontos'     | Tipo do produto disponibilizado na página |
| metric2     |[[valor-real]]|'129.50'|Valor total do produto em reais|


<br />

**Hotéis**

<br />

| Nome      | Variável          | Exemplo           | Descrição                 |
| :------------ | :------------------------ | :------------------------ | :---------------------------------------- |
| name      | [[nome-produto]]      | 'hotel-panamby-sao-paulo' (somente ida) e etc| Nome do hotel  |
| id      | [[id-produto]]      |  'cvchotel'           | ID do produto               |
| price     | [[preco-produto]]     | '20000'           | Preço do hotel em pontos            |
| brand     | [[marca-produto]]     |  'hotel', 'pousada' etc           | Tipo da hospedagem              |
| category    | [[categoria-produto]]   |  'hoteis'           | Categoria do produto            |
| variant     | [[variacao-produto]]    |  'apto-duplo', 'apto-casal', 'superior' etc     | Tipo de quarto        |
| list      | [[lista-produto]]     | 'viagens:hoteis', 'viagens:pacotes'           | Nome da lista de hotéis         |
| position    | [[posicao-produto]]     | '1'           | Posição que o produto aparece na lista  |
| dimension17   | [[tipo-produto]]  | 'transferencia-para-parceiro', 'viagem', 'cartao-presente', 'beneficio', 'produto-digital', 'acumulacao', 'produto-fisico', 'transferencia-de-pontos', 'compra-de-pontos'     | Tipo do produto disponibilizado na página |
| metric2     |[[valor-real]]|'129.50'|Valor total do produto em reais|

<br />

**Carros**

<br />

| Nome      | Variável          | Exemplo           | Descrição                 |
| :------------ | :------------------------ | :------------------------ | :---------------------------------------- |
| name      | [[nome-produto]]      | 'localiza:localiza-hertz-centro-faria-lima' etc| Nome da locação de carro considerando nome da locadora e nome da unidade ou loja da locadora   |
| id      | [[id-produto]]      |   'cvcvehicle'            | ID do produto               |
| price     | [[preco-produto]]     | '3500'            | Preço da locacao em pontos            |
| brand     | [[marca-produto]]     |   'movida', 'localiza'          | Nome da locadora              |
| category    | [[categoria-produto]]   |  'carros'           | Categoria do produto            |
| list      | [[lista-produto]]     | 'viagens:carros'            | Nome da lista de veículos         |
| position    | [[posicao-produto]]     | '1'           | Posição que o produto aparece na lista  |
| dimension17   | [[tipo-produto]]  | 'transferencia-para-parceiro', 'viagem', 'cartao-presente', 'beneficio', 'produto-digital', 'acumulacao', 'produto-fisico', 'transferencia-de-pontos', 'compra-de-pontos'     | Tipo do produto disponibilizado na página |
| dimension18   | [[cidade-retirada]]   | 'sao-paulo-sp'      | Nome da cidade selecionada para retirada do veículo |
| dimension19   | [[cidade-devolucao]]  | 'rio-de-janeiro-rj'     | Nome da cidade selecionada para devolução do veículo  |
| dimension20   | [[modelo-veiculo]]  | 'toyota-yaris'      | Nome do modelo do veículo |
| metric2     |[[valor-real]]|'129.50'|Valor total do produto em reais|

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
      'actionField': {'list': '[[lista-produto]]'},
      'products': [{
        'name': '[[nome-produto]]',       //Nome ou ID do produto é obrigatório
        'id': '[[id-produto]]',
        'price': '[[preco-produto]]',
        'brand': '[[marca-produto]]',
        'category': '[[categoria-produto]]',
        'variant': '[[variacao-produto]]',
        'dimension6':'[[viagem-origem]]',
        'dimension7':'[[viagem-destino]]',
        'dimension8':'[[viagem-data-ida]]',
        'dimension9':'[[viagem-data-volta]',
        'dimension10':'[[passageiro-adulto]]',
        'dimension11':'[[passageiro-crianca]]',
        'dimension12':'[[passageiro-bebe]]',
        'dimension13':'[[tipo-viagem]',
        'dimension14':'[[voo-direto]]',
        'dimension15':'[[classe-executiva]]',
        'dimension16':'[[loja]]',
        'dimension17': '[[tipo-produto]]',
        'dimension18': '[[cidade-retirada]]',
        'dimension19': '[[cidade-devolucao]]',
        'dimension20': '[[modelo-veiculo]]',
        'metric2': '[[valor-real]]'

      }]
    }
  }
 });
</script>
```
<br />

**Catálogo - Produtos**

<br />

| Nome      | Variável          | Exemplo           | Descrição                 |
| :------------ | :------------------------ | :------------------------ | :---------------------------------------- |
| name      | [[nome-produto]]      | 'jogo-de-panelas-tramontina-antiaderente'         | Nome do produto               |
| id      | [[id-produto]]      | '10649879'            | ID do produto               |
| price     | [[preco-produto]]     | '13999'           | Preço do produto em pontos            |
| brand     | [[marca-produto]]     | 'sony', 'samsung'           | Marca do produto              |
| category    | [[categoria-produto]]   | 'produto', 'milhas'           | Categoria do produto            |
| variant     | [[variacao-produto]]    | '110v'            | Variação do produto             |
| list      | [[lista-produto]]     | 'catalogo-produto'            | Nome da lista de produtos         |
| dimension16   | [[loja]]          | 'ponto-frio', 'fast-shop'           | Nome da loja do market place oferecida  |
| dimension17   | [[tipo-produto]]  | 'transferencia-para-parceiro', 'viagem', 'cartao-presente', 'beneficio', 'produto-digital', 'acumulacao', 'produto-fisico', 'transferencia-de-pontos', 'compra-de-pontos'     | Tipo do produto disponibilizado na página |
| metric2     |[[valor-real]]|'129.50'|Valor total do produto em reais|

<br />

**Passagens**

<br />

| Nome      | Variável          | Exemplo           | Descrição                 |
| :------------ | :------------------------ | :------------------------ | :---------------------------------------- |
| name      | [[nome-produto]]      | 'iv:gru-sdu/rec-vcp:adulto' (ida e volta), 'i:sdu-gru:crianca' (somente ida) e etc| Nome do voo considerando ida (i), ida e volta (iv), origem/destino (gru-sdu) e passageiros (adulto, crianca)  |
| id      | [[id-produto]]      |  'cvcfligth'            | ID do produto               |
| price     | [[preco-produto]]     | '13999'           | Preço da passagem em pontos           |
| brand     | [[marca-produto]]     |  'avianca'          | Marca do compania aérea               |
| category    | [[categoria-produto]]   |  'passagens'            | Categoria do produto            |
| variant     | [[variacao-produto]]    |  'voo-direto', 'direto:bagagem-2', '1-parada:bagagem-1' e etc     | Tipo do voo e bagagem       |
| list      | [[lista-produto]]     | 'viagens:passagens', 'viagens:pacotes'            | Nome da lista de voos         |
| dimension6    | [[viagem-origem]]     | 'gru', e etc      |Origem da viagem|
| dimension7    | [[viagem-destino]]    | 'sdu' e etc       |Destino da viagem|
| dimension8  | [[viagem-data-ida]]   | '01/10/2019' e etc      |Data de Ida da viagem|
| dimension9  | [[viagem-data-volta]    | '10/10/2019' e etc      |Data de Volta da viagem|
| dimension10 | [[passageiro-adulto]]   | '1'     |Quantidade de passageiros Adultos|
| dimension11 | [[passageiro-crianca]]  | '2'     |Quantidade de passageiros Crianças|
| dimension12 | [[passageiro-bebe]]   | '1'       |Quantidade de passageiros Bebês|
| dimension13 | [[tipo-viagem]      | 'ida-e-volta', 'somente-ida' e etc      |Deve retornar o tipo de viagem selecionado pelo usuário|
| dimension14 | [[voo-direto]]      | 'true' ou 'false'     |Opção de voos direto|
| dimension15 | [[classe-executiva]]    | 'true' ou 'false'     |Opção de classe executiva|
| dimension17   | [[tipo-produto]]  | 'transferencia-para-parceiro', 'viagem', 'cartao-presente', 'beneficio', 'produto-digital', 'acumulacao', 'produto-fisico', 'transferencia-de-pontos', 'compra-de-pontos'     | Tipo do produto disponibilizado na página |
| metric2     |[[valor-real]]|'129.50'|Valor total do produto em reais|


<br />

**Hotéis**

<br />

| Nome      | Variável          | Exemplo           | Descrição                 |
| :------------ | :------------------------ | :------------------------ | :---------------------------------------- |
| name      | [[nome-produto]]      | 'hotel-panamby-sao-paulo' (somente ida) e etc| Nome do hotel  |
| id      | [[id-produto]]      |  'cvchotel'           | ID do produto               |
| price     | [[preco-produto]]     | '20000'           | Preço do hotel em pontos            |
| brand     | [[marca-produto]]     |  'hotel', 'pousada' etc           | Tipo da hospedagem              |
| category    | [[categoria-produto]]   |  'hoteis'           | Categoria do produto            |
| variant     | [[variacao-produto]]    |  'apto-duplo', 'apto-casal', 'superior' etc     | Tipo de quarto        |
| list      | [[lista-produto]]     | 'viagens:hoteis', 'viagens:pacotes'           | Nome da lista de hotéis         |
| dimension6    | [[viagem-origem]]     | 'gru', e etc      |Origem da viagem|
| dimension7    | [[viagem-destino]]    | 'sdu' e etc       |Destino da viagem|
| dimension8  | [[viagem-data-ida]]   | '01/10/2019' e etc      |Data de Ida da viagem|
| dimension9  | [[viagem-data-volta]    | '10/10/2019' e etc      |Data de Volta da viagem|
| dimension10 | [[passageiro-adulto]]   | '1'     |Quantidade de passageiros Adultos|
| dimension11 | [[passageiro-crianca]]  | '2'     |Quantidade de passageiros Crianças|
| dimension12 | [[passageiro-bebe]]   | '1'       |Quantidade de passageiros Bebês|
| dimension13 | [[tipo-viagem]      | 'ida-e-volta', 'somente-ida' e etc      |Deve retornar o tipo de viagem selecionado pelo usuário|
| dimension14 | [[voo-direto]]      | 'true' ou 'false'     |Opção de voos direto|
| dimension15 | [[classe-executiva]]    | 'true' ou 'false'     |Opção de classe executiva|
| dimension17   | [[tipo-produto]]  | 'transferencia-para-parceiro', 'viagem', 'cartao-presente', 'beneficio', 'produto-digital', 'acumulacao', 'produto-fisico', 'transferencia-de-pontos', 'compra-de-pontos'         | Tipo do produto disponibilizado na página |
| metric2     |[[valor-real]]|'129.50'|Valor total do produto em reais|

<br />

**Carros**

<br />

| Nome      | Variável          | Exemplo           | Descrição                 |
| :------------ | :------------------------ | :------------------------ | :---------------------------------------- |
| name      | [[nome-produto]]      | 'localiza:localiza-hertz-centro-faria-lima' etc| Nome da locação de carro considerando nome da locadora e nome da unidade ou loja da locadora   |
| id      | [[id-produto]]      |   'cvcvehicle'            | ID do produto               |
| price     | [[preco-produto]]     | '3500'            | Preço da locacao em pontos            |
| brand     | [[marca-produto]]     |   'movida', 'localiza'          | Nome da locadora              |
| category    | [[categoria-produto]]   |  'carros'           | Categoria do produto            |
| list      | [[lista-produto]]     | 'viagens:carros'            | Nome da lista de veículos         |
| dimension17   | [[tipo-produto]]  | 'transferencia-para-parceiro', 'viagem', 'cartao-presente', 'beneficio', 'produto-digital', 'acumulacao', 'produto-fisico', 'transferencia-de-pontos', 'compra-de-pontos'     | Tipo do produto disponibilizado na página |
| dimension18   | [[cidade-retirada]]   | 'sao-paulo-sp'      | Nome da cidade selecionada para retirada do veículo |
| dimension19   | [[cidade-devolucao]]  | 'rio-de-janeiro-rj'     | Nome da cidade selecionada para devolução do veículo  |
| dimension20   | [[modelo-veiculo]]  | 'toyota-yaris'      | Nome do modelo do veículo |
| metric2     |[[valor-real]]|'129.50'|Valor total do produto em reais|

<br />

### AddToCart:

**No clique para adicionar um produto (considerar recursos de "viagens") ao carrinho - Em recursos de "viagens" considerar o clique do botão "Continuar" no modal de confirmação de pedido na página de detalhes do resgate** <br />
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
        'name': '[[nome-produto]]',       //Nome ou ID do produto é obrigatório
        'id': '[[id-produto]]',
        'price': '[[preco-produto]]',
        'brand': '[[marca-produto]]',
        'category': '[[categoria-produto]]',
        'variant': '[[variacao-produto]]',
        'quantity': '[[quantidade-produto]]',
        'dimension6':'[[viagem-origem]]',
        'dimension7':'[[viagem-destino]]',
        'dimension8':'[[viagem-data-ida]]',
        'dimension9':'[[viagem-data-volta]',
        'dimension10':'[[passageiro-adulto]]',
        'dimension11':'[[passageiro-crianca]]',
        'dimension12':'[[passageiro-bebe]]',
        'dimension13':'[[tipo-viagem]',
        'dimension14':'[[voo-direto]]',
        'dimension15':'[[classe-executiva]]',
        'dimension16':'[[loja]]',
        'dimension17': '[[tipo-produto]]',
        'dimension18': '[[cidade-retirada]]',
        'dimension19': '[[cidade-devolucao]]',
        'dimension20': '[[modelo-veiculo]]',
        'metric2': '[[valor-real]]'
      }]
    }
  }
});
</script>
```

<br />

**Catálogo - Produtos**

<br />

| Nome      | Variável          | Exemplo           | Descrição                 |
| :------------ | :------------------------ | :------------------------ | :---------------------------------------- |
| name      | [[nome-produto]]      | 'jogo-de-panelas-tramontina-antiaderente'         | Nome do produto               |
| id      | [[id-produto]]      | '10649879'            | ID do produto               |
| price     | [[preco-produto]]     | '13999'           | Preço do produto em pontos            |
| brand     | [[marca-produto]]     | 'sony', 'samsung'           | Marca do produto              |
| category    | [[categoria-produto]]   | 'produto', 'milhas'           | Categoria do produto            |
| variant     | [[variacao-produto]]    | '110v'            | Variação do produto             |
| list      | [[lista-produto]]     | 'catalogo-produto'            | Nome da lista de produtos         |
| dimension16   | [[loja]]          | 'ponto-frio', 'fast-shop'           | Nome da loja do market place oferecida  |
| dimension17   | [[tipo-produto]]  | 'transferencia-para-parceiro', 'viagem', 'cartao-presente', 'beneficio', 'produto-digital', 'acumulacao', 'produto-fisico', 'transferencia-de-pontos', 'compra-de-pontos'     | Tipo do produto disponibilizado na página |
| metric2     |[[valor-real]]|'129.50'|Valor total do produto em reais|

<br />

**Passagens**

<br />

| Nome      | Variável          | Exemplo           | Descrição                 |
| :------------ | :------------------------ | :------------------------ | :---------------------------------------- |
| name      | [[nome-produto]]      | 'iv:gru-sdu/rec-vcp:adulto' (ida e volta), 'i:sdu-gru:crianca' (somente ida) e etc| Nome do voo considerando ida (i), ida e volta (iv), origem/destino (gru-sdu) e passageiros (adulto, crianca)  |
| id      | [[id-produto]]      |  'cvcfligth'            | ID do produto               |
| price     | [[preco-produto]]     | '13999'           | Preço da passagem em pontos           |
| brand     | [[marca-produto]]     |  'avianca'          | Marca do compania aérea               |
| category    | [[categoria-produto]]   |  'passagens'            | Categoria do produto            |
| variant     | [[variacao-produto]]    |  'voo-direto', 'direto:bagagem-2', '1-parada:bagagem-1' e etc     | Tipo do voo e bagagem       |
| list      | [[lista-produto]]     | 'viagens:passagens', 'viagens:pacotes'            | Nome da lista de voos         |
| dimension6    | [[viagem-origem]]     | 'gru', e etc      |Origem da viagem|
| dimension7    | [[viagem-destino]]    | 'sdu' e etc       |Destino da viagem|
| dimension8  | [[viagem-data-ida]]   | '01/10/2019' e etc      |Data de Ida da viagem|
| dimension9  | [[viagem-data-volta]    | '10/10/2019' e etc      |Data de Volta da viagem|
| dimension10 | [[passageiro-adulto]]   | '1'     |Quantidade de passageiros Adultos|
| dimension11 | [[passageiro-crianca]]  | '2'     |Quantidade de passageiros Crianças|
| dimension12 | [[passageiro-bebe]]   | '1'       |Quantidade de passageiros Bebês|
| dimension13 | [[tipo-viagem]      | 'ida-e-volta', 'somente-ida' e etc      |Deve retornar o tipo de viagem selecionado pelo usuário|
| dimension14 | [[voo-direto]]      | 'true' ou 'false'     |Opção de voos direto|
| dimension15 | [[classe-executiva]]    | 'true' ou 'false'     |Opção de classe executiva|
| dimension17   | [[tipo-produto]]  | 'transferencia-para-parceiro', 'viagem', 'cartao-presente', 'beneficio', 'produto-digital', 'acumulacao', 'produto-fisico', 'transferencia-de-pontos', 'compra-de-pontos'     | Tipo do produto disponibilizado na página |
| metric2     |[[valor-real]]|'129.50'|Valor total do produto em reais|


<br />

**Hotéis**

<br />

| Nome      | Variável          | Exemplo           | Descrição                 |
| :------------ | :------------------------ | :------------------------ | :---------------------------------------- |
| name      | [[nome-produto]]      | 'hotel-panamby-sao-paulo' (somente ida) e etc| Nome do hotel  |
| id      | [[id-produto]]      |  'cvchotel'           | ID do produto               |
| price     | [[preco-produto]]     | '20000'           | Preço do hotel em pontos            |
| brand     | [[marca-produto]]     |  'hotel', 'pousada' etc           | Tipo da hospedagem              |
| category    | [[categoria-produto]]   |  'hoteis'           | Categoria do produto            |
| variant     | [[variacao-produto]]    |  'apto-duplo', 'apto-casal', 'superior' etc     | Tipo de quarto        |
| list      | [[lista-produto]]     | 'viagens:hoteis', 'viagens:pacotes'           | Nome da lista de hotéis         |
| dimension6    | [[viagem-origem]]     | 'gru', e etc      |Origem da viagem|
| dimension7    | [[viagem-destino]]    | 'sdu' e etc       |Destino da viagem|
| dimension8  | [[viagem-data-ida]]   | '01/10/2019' e etc      |Data de Ida da viagem|
| dimension9  | [[viagem-data-volta]    | '10/10/2019' e etc      |Data de Volta da viagem|
| dimension10 | [[passageiro-adulto]]   | '1'     |Quantidade de passageiros Adultos|
| dimension11 | [[passageiro-crianca]]  | '2'     |Quantidade de passageiros Crianças|
| dimension12 | [[passageiro-bebe]]   | '1'       |Quantidade de passageiros Bebês|
| dimension13 | [[tipo-viagem]      | 'ida-e-volta', 'somente-ida' e etc      |Deve retornar o tipo de viagem selecionado pelo usuário|
| dimension14 | [[voo-direto]]      | 'true' ou 'false'     |Opção de voos direto|
| dimension15 | [[classe-executiva]]    | 'true' ou 'false'     |Opção de classe executiva|
| dimension17   | [[tipo-produto]]  | 'transferencia-para-parceiro', 'viagem', 'cartao-presente', 'beneficio', 'produto-digital', 'acumulacao', 'produto-fisico', 'transferencia-de-pontos', 'compra-de-pontos'     | Tipo do produto disponibilizado na página |
| metric2     |[[valor-real]]|'129.50'|Valor total do produto em reais|


<br />

**Carros**

<br />

| Nome      | Variável          | Exemplo           | Descrição                 |
| :------------ | :------------------------ | :------------------------ | :---------------------------------------- |
| name      | [[nome-produto]]      | 'localiza:localiza-hertz-centro-faria-lima' etc| Nome da locação de carro considerando nome da locadora e nome da unidade ou loja da locadora   |
| id      | [[id-produto]]      |   'cvcvehicle'            | ID do produto               |
| price     | [[preco-produto]]     | '3500'            | Preço da locacao em pontos            |
| brand     | [[marca-produto]]     |   'movida', 'localiza'          | Nome da locadora              |
| category    | [[categoria-produto]]   |  'carros'           | Categoria do produto            |
| list      | [[lista-produto]]     | 'viagens:carros'            | Nome da lista de veículos         |
| dimension17   | [[tipo-produto]]  | 'transferencia-para-parceiro', 'viagem', 'cartao-presente', 'beneficio', 'produto-digital', 'acumulacao', 'produto-fisico', 'transferencia-de-pontos', 'compra-de-pontos'         | Tipo do produto disponibilizado na página |
| dimension18   | [[cidade-retirada]]   | 'sao-paulo-sp'      | Nome da cidade selecionada para retirada do veículo |
| dimension19   | [[cidade-devolucao]]  | 'rio-de-janeiro-rj'     | Nome da cidade selecionada para devolução do veículo  |
| dimension20   | [[modelo-veiculo]]  | 'toyota-yaris'      | Nome do modelo do veículo |
| metric2     |[[valor-real]]|'129.50'|Valor total do produto em reais|

<br />

### Remove From Cart:
**No clique para remover um produto (também considerar recursos de "viagens")  do carrinho <br />**
- **Onde:** O objeto javascript (dataLayer) abaixo deve ser disparado uma única vez no clique do botão para remover um produto do carrinho.

```html
<script>
dataLayer.push({
  'event': 'removeFromCart',
  'eventCategory': 'enhanced-ecommerce',
  'eventAction': 'remove-from-cart',
  'eventLabel': '',
  'ecommerce': {
    'remove': {
      'products': [{
        'name': '[[nome-produto]]',       //Nome ou ID do produto é obrigatório
        'id': '[[id-produto]]',
        'price': '[[preco-produto]]',
        'brand': '[[marca-produto]]',
        'category': '[[categoria-produto]]',
        'variant': '[[variacao-produto]]',
        'quantity': '[[quantidade-produto]]',
        'dimension6':'[[viagem-origem]]',
        'dimension7':'[[viagem-destino]]',
        'dimension8':'[[viagem-data-ida]]',
        'dimension9':'[[viagem-data-volta]',
        'dimension10':'[[passageiro-adulto]]',
        'dimension11':'[[passageiro-crianca]]',
        'dimension12':'[[passageiro-bebe]]',
        'dimension13':'[[tipo-viagem]',
        'dimension14':'[[voo-direto]]',
        'dimension15':'[[classe-executiva]]',
        'dimension16':'[[loja]]',
        'dimension17': '[[tipo-produto]]',
        'dimension18': '[[cidade-retirada]]',
        'dimension19': '[[cidade-devolucao]]',
        'dimension20': '[[modelo-veiculo]]',
        'metric2': '[[valor-real]]'
      }]
    }
  }
});
</script>
```

<br />


**Catálogo - Produtos**

<br />

| Nome      | Variável          | Exemplo           | Descrição                 |
| :------------ | :------------------------ | :------------------------ | :---------------------------------------- |
| name      | [[nome-produto]]      | 'jogo-de-panelas-tramontina-antiaderente'         | Nome do produto               |
| id      | [[id-produto]]      | '10649879'            | ID do produto               |
| price     | [[preco-produto]]     | '13999'           | Preço do produto em pontos            |
| brand     | [[marca-produto]]     | 'sony', 'samsung'           | Marca do produto              |
| category    | [[categoria-produto]]   | 'produto', 'milhas'           | Categoria do produto            |
| variant     | [[variacao-produto]]    | '110v'            | Variação do produto             |
| list      | [[lista-produto]]     | 'catalogo-produto'            | Nome da lista de produtos         |
| dimension16   | [[loja]]          | 'ponto-frio', 'fast-shop'           | Nome da loja do market place oferecida  |
| dimension17   | [[tipo-produto]]  | 'transferencia-para-parceiro', 'viagem', 'cartao-presente', 'beneficio', 'produto-digital', 'acumulacao', 'produto-fisico', 'transferencia-de-pontos', 'compra-de-pontos'         | Tipo do produto disponibilizado na página |
| metric2     |[[valor-real]]|'129.50'|Valor total do produto em reais|

<br />

**Passagens**

<br />

| Nome      | Variável          | Exemplo           | Descrição                 |
| :------------ | :------------------------ | :------------------------ | :---------------------------------------- |
| name      | [[nome-produto]]      | 'iv:gru-sdu/rec-vcp:adulto' (ida e volta), 'i:sdu-gru:crianca' (somente ida) e etc| Nome do voo considerando ida (i), ida e volta (iv), origem/destino (gru-sdu) e passageiros (adulto, crianca)  |
| id      | [[id-produto]]      |  'cvcfligth'            | ID do produto               |
| price     | [[preco-produto]]     | '13999'           | Preço da passagem em pontos           |
| brand     | [[marca-produto]]     |  'avianca'          | Marca do compania aérea               |
| category    | [[categoria-produto]]   |  'passagens'            | Categoria do produto            |
| variant     | [[variacao-produto]]    |  'voo-direto', 'direto:bagagem-2', '1-parada:bagagem-1' e etc     | Tipo do voo e bagagem       |
| list      | [[lista-produto]]     | 'viagens:passagens', 'viagens:pacotes'            | Nome da lista de voos         |
| dimension6    | [[viagem-origem]]     | 'gru', e etc      |Origem da viagem|
| dimension7    | [[viagem-destino]]    | 'sdu' e etc       |Destino da viagem|
| dimension8  | [[viagem-data-ida]]   | '01/10/2019' e etc      |Data de Ida da viagem|
| dimension9  | [[viagem-data-volta]    | '10/10/2019' e etc      |Data de Volta da viagem|
| dimension10 | [[passageiro-adulto]]   | '1'     |Quantidade de passageiros Adultos|
| dimension11 | [[passageiro-crianca]]  | '2'     |Quantidade de passageiros Crianças|
| dimension12 | [[passageiro-bebe]]   | '1'       |Quantidade de passageiros Bebês|
| dimension13 | [[tipo-viagem]      | 'ida-e-volta', 'somente-ida' e etc      |Deve retornar o tipo de viagem selecionado pelo usuário|
| dimension14 | [[voo-direto]]      | 'true' ou 'false'     |Opção de voos direto|
| dimension15 | [[classe-executiva]]    | 'true' ou 'false'     |Opção de classe executiva|
| dimension17   | [[tipo-produto]]  | 'transferencia-para-parceiro', 'viagem', 'cartao-presente', 'beneficio', 'produto-digital', 'acumulacao', 'produto-fisico', 'transferencia-de-pontos', 'compra-de-pontos'     | Tipo do produto disponibilizado na página |
| metric2     |[[valor-real]]|'129.50'|Valor total do produto em reais|


<br />

**Hotéis**

<br />

| Nome      | Variável          | Exemplo           | Descrição                 |
| :------------ | :------------------------ | :------------------------ | :---------------------------------------- |
| name      | [[nome-produto]]      | 'hotel-panamby-sao-paulo' (somente ida) e etc| Nome do hotel  |
| id      | [[id-produto]]      |  'cvchotel'           | ID do produto               |
| price     | [[preco-produto]]     | '20000'           | Preço do hotel em pontos            |
| brand     | [[marca-produto]]     |  'hotel', 'pousada' etc           | Tipo da hospedagem              |
| category    | [[categoria-produto]]   |  'hoteis'           | Categoria do produto            |
| variant     | [[variacao-produto]]    |  'apto-duplo', 'apto-casal', 'superior' etc     | Tipo de quarto        |
| list      | [[lista-produto]]     | 'viagens:hoteis', 'viagens:pacotes'           | Nome da lista de hotéis         |
| dimension6    | [[viagem-origem]]     | 'gru', e etc      |Origem da viagem|
| dimension7    | [[viagem-destino]]    | 'sdu' e etc       |Destino da viagem|
| dimension8  | [[viagem-data-ida]]   | '01/10/2019' e etc      |Data de Ida da viagem|
| dimension9  | [[viagem-data-volta]    | '10/10/2019' e etc      |Data de Volta da viagem|
| dimension10 | [[passageiro-adulto]]   | '1'     |Quantidade de passageiros Adultos|
| dimension11 | [[passageiro-crianca]]  | '2'     |Quantidade de passageiros Crianças|
| dimension12 | [[passageiro-bebe]]   | '1'       |Quantidade de passageiros Bebês|
| dimension13 | [[tipo-viagem]      | 'ida-e-volta', 'somente-ida' e etc      |Deve retornar o tipo de viagem selecionado pelo usuário|
| dimension14 | [[voo-direto]]      | 'true' ou 'false'     |Opção de voos direto|
| dimension15 | [[classe-executiva]]    | 'true' ou 'false'     |Opção de classe executiva|
| dimension17   | [[tipo-produto]]  | 'transferencia-para-parceiro', 'viagem', 'cartao-presente', 'beneficio', 'produto-digital', 'acumulacao', 'produto-fisico', 'transferencia-de-pontos', 'compra-de-pontos'     | Tipo do produto disponibilizado na página |
| metric2     |[[valor-real]]|'129.50'|Valor total do produto em reais|


<br />

**Carros**

<br />

| Nome      | Variável          | Exemplo           | Descrição                 |
| :------------ | :------------------------ | :------------------------ | :---------------------------------------- |
| name      | [[nome-produto]]      | 'localiza:localiza-hertz-centro-faria-lima' etc| Nome da locação de carro considerando nome da locadora e nome da unidade ou loja da locadora   |
| id      | [[id-produto]]      |   'cvcvehicle'            | ID do produto               |
| price     | [[preco-produto]]     | '3500'            | Preço da locacao em pontos            |
| brand     | [[marca-produto]]     |   'movida', 'localiza'          | Nome da locadora              |
| category    | [[categoria-produto]]   |  'carros'           | Categoria do produto            |
| list      | [[lista-produto]]     | 'viagens:carros'            | Nome da lista de veículos         |
| dimension17   | [[tipo-produto]]  | 'transferencia-para-parceiro', 'viagem', 'cartao-presente', 'beneficio', 'produto-digital', 'acumulacao', 'produto-fisico', 'transferencia-de-pontos', 'compra-de-pontos'     | Tipo do produto disponibilizado na página |
| dimension18   | [[cidade-retirada]]   | 'sao-paulo-sp'      | Nome da cidade selecionada para retirada do veículo |
| dimension19   | [[cidade-devolucao]]  | 'rio-de-janeiro-rj'     | Nome da cidade selecionada para devolução do veículo  |
| dimension20   | [[modelo-veiculo]]  | 'toyota-yaris'      | Nome do modelo do veículo |
| metric2     |[[valor-real]]|'129.50'|Valor total do produto em reais|

<br />
 
### Checkout:
 
- **Onde:** O objeto javascript (dataLayer) abaixo deve ser disparado uma única vez no carregamento das páginas do checkout.

```html
<script>
dataLayer.push({
  'event': 'checkout',
  'eventCategory': 'enhanced-ecommerce',
  'eventAction': 'checkout-[[passo-checkout]]',
  'eventLabel': '[[tipo-carrinho]]',
  'ecommerce': {
    'checkout': {
      'actionField': {'step': '[[passo-checkout]]'},
      'products': [{
        'name': '[[nome-produto]]',       //Nome ou ID do produto é obrigatório
        'id': '[[id-produto]]',
        'price': '[[preco-produto]]',
        'brand': '[[marca-produto]]',
        'category': '[[categoria-produto]]',
        'variant': '[[variacao-produto]]',
        'quantity': '[[quantidade-produto]]',
        'dimension6':'[[viagem-origem]]',
        'dimension7':'[[viagem-destino]]',
        'dimension8':'[[viagem-data-ida]]',
        'dimension9':'[[viagem-data-volta]',
        'dimension10':'[[passageiro-adulto]]',
        'dimension11':'[[passageiro-crianca]]',
        'dimension12':'[[passageiro-bebe]]',
        'dimension13':'[[tipo-viagem]',
        'dimension14':'[[voo-direto]]',
        'dimension15':'[[classe-executiva]]',
        'dimension16':'[[loja]]',
        'dimension17': '[[tipo-produto]]',
        'dimension18': '[[cidade-retirada]]',
        'dimension19': '[[cidade-devolucao]]',
        'dimension20': '[[modelo-veiculo]]',
        'metric2': '[[valor-real]]'
      }]
    }
  }
});
</script>
```
 
<br />
 
| Variável          | Exemplo           | Descrição                 |
| :------------------------ | :------------------------ | :---------------------------------------- |
| [[passo-checkout]]      | Carrinho = 1/ Login = 2 / Entrega = 3/ Pagamento = 4        | Retornar '1', '2', '3' ou '4' de acordo com a página que o usuário está.                |
| [[tipo-carrinho]]       | 'carrinho-pagina' ou 'carrinho-lateral'           | Considerar eventLabel somente no carrinho - Deve retornar o tipo do carrinho      |

<br />

**Catálogo - Produtos**

<br />

| Nome      | Variável          | Exemplo           | Descrição                 |
| :------------ | :------------------------ | :------------------------ | :---------------------------------------- |
| name      | [[nome-produto]]      | 'jogo-de-panelas-tramontina-antiaderente'         | Nome do produto               |
| id      | [[id-produto]]      | '10649879'            | ID do produto               |
| price     | [[preco-produto]]     | '13999'           | Preço do produto em pontos            |
| brand     | [[marca-produto]]     | 'sony', 'samsung'           | Marca do produto              |
| category    | [[categoria-produto]]   | 'produto', 'milhas'           | Categoria do produto            |
| variant     | [[variacao-produto]]    | '110v'            | Variação do produto             |
| list      | [[lista-produto]]     | 'catalogo-produto'            | Nome da lista de produtos         |
| dimension16   | [[loja]]          | 'ponto-frio', 'fast-shop'           | Nome da loja do market place oferecida  |
| dimension17   | [[tipo-produto]]  | 'transferencia-para-parceiro', 'viagem', 'cartao-presente', 'beneficio', 'produto-digital', 'acumulacao', 'produto-fisico', 'transferencia-de-pontos', 'compra-de-pontos' etc       | Tipo do produto disponibilizado na página |
| metric2     |[[valor-real]]|'129.50'|Valor total do produto em reais|

<br />

**Passagens**

<br />

| Nome      | Variável          | Exemplo           | Descrição                 |
| :------------ | :------------------------ | :------------------------ | :---------------------------------------- |
| name      | [[nome-produto]]      | 'iv:gru-sdu/rec-vcp:adulto' (ida e volta), 'i:sdu-gru:crianca' (somente ida) e etc| Nome do voo considerando ida (i), ida e volta (iv), origem/destino (gru-sdu) e passageiros (adulto, crianca)  |
| id      | [[id-produto]]      |  'cvcfligth'            | ID do produto               |
| price     | [[preco-produto]]     | '13999'           | Preço da passagem em pontos           |
| brand     | [[marca-produto]]     |  'avianca'          | Marca do compania aérea               |
| category    | [[categoria-produto]]   |  'passagens'            | Categoria do produto            |
| variant     | [[variacao-produto]]    |  'voo-direto', 'direto:bagagem-2', '1-parada:bagagem-1' e etc     | Tipo do voo e bagagem       |
| list      | [[lista-produto]]     | 'viagens:passagens', 'viagens:pacotes'            | Nome da lista de voos         |
| dimension6    | [[viagem-origem]]     | 'gru', e etc      |Origem da viagem|
| dimension7    | [[viagem-destino]]    | 'sdu' e etc       |Destino da viagem|
| dimension8  | [[viagem-data-ida]]   | '01/10/2019' e etc      |Data de Ida da viagem|
| dimension9  | [[viagem-data-volta]    | '10/10/2019' e etc      |Data de Volta da viagem|
| dimension10 | [[passageiro-adulto]]   | '1'     |Quantidade de passageiros Adultos|
| dimension11 | [[passageiro-crianca]]  | '2'     |Quantidade de passageiros Crianças|
| dimension12 | [[passageiro-bebe]]   | '1'       |Quantidade de passageiros Bebês|
| dimension13 | [[tipo-viagem]      | 'ida-e-volta', 'somente-ida' e etc      |Deve retornar o tipo de viagem selecionado pelo usuário|
| dimension14 | [[voo-direto]]      | 'true' ou 'false'     |Opção de voos direto|
| dimension15 | [[classe-executiva]]    | 'true' ou 'false'     |Opção de classe executiva|
| dimension17   | [[tipo-produto]]  | 'transferencia-para-parceiro', 'viagem', 'cartao-presente', 'beneficio', 'produto-digital', 'acumulacao', 'produto-fisico', 'transferencia-de-pontos', 'compra-de-pontos'         | Tipo do produto disponibilizado na página |
| metric2     |[[valor-real]]|'129.50'|Valor total do produto em reais|


<br />

**Hotéis**

<br />

| Nome      | Variável          | Exemplo           | Descrição                 |
| :------------ | :------------------------ | :------------------------ | :---------------------------------------- |
| name      | [[nome-produto]]      | 'hotel-panamby-sao-paulo' (somente ida) e etc| Nome do hotel  |
| id      | [[id-produto]]      |  'cvchotel'           | ID do produto               |
| price     | [[preco-produto]]     | '20000'           | Preço do hotel em pontos            |
| brand     | [[marca-produto]]     |  'hotel', 'pousada' etc           | Tipo da hospedagem              |
| category    | [[categoria-produto]]   |  'hoteis'           | Categoria do produto            |
| variant     | [[variacao-produto]]    |  'apto-duplo', 'apto-casal', 'superior' etc     | Tipo de quarto        |
| list      | [[lista-produto]]     | 'viagens:hoteis', 'viagens:pacotes'           | Nome da lista de hotéis         |
| dimension6    | [[viagem-origem]]     | 'gru', e etc      |Origem da viagem|
| dimension7    | [[viagem-destino]]    | 'sdu' e etc       |Destino da viagem|
| dimension8  | [[viagem-data-ida]]   | '01/10/2019' e etc      |Data de Ida da viagem|
| dimension9  | [[viagem-data-volta]    | '10/10/2019' e etc      |Data de Volta da viagem|
| dimension10 | [[passageiro-adulto]]   | '1'     |Quantidade de passageiros Adultos|
| dimension11 | [[passageiro-crianca]]  | '2'     |Quantidade de passageiros Crianças|
| dimension12 | [[passageiro-bebe]]   | '1'       |Quantidade de passageiros Bebês|
| dimension13 | [[tipo-viagem]      | 'ida-e-volta', 'somente-ida' e etc      |Deve retornar o tipo de viagem selecionado pelo usuário|
| dimension14 | [[voo-direto]]      | 'true' ou 'false'     |Opção de voos direto|
| dimension15 | [[classe-executiva]]    | 'true' ou 'false'     |Opção de classe executiva|
| dimension17   | [[tipo-produto]]  | 'transferencia-para-parceiro', 'viagem', 'cartao-presente', 'beneficio', 'produto-digital', 'acumulacao', 'produto-fisico', 'transferencia-de-pontos', 'compra-de-pontos'     | Tipo do produto disponibilizado na página |
| metric2     |[[valor-real]]|'129.50'|Valor total do produto em reais|


<br />

**Carros**

<br />

| Nome      | Variável          | Exemplo           | Descrição                 |
| :------------ | :------------------------ | :------------------------ | :---------------------------------------- |
| name      | [[nome-produto]]      | 'localiza:localiza-hertz-centro-faria-lima' etc| Nome da locação de carro considerando nome da locadora e nome da unidade ou loja da locadora   |
| id      | [[id-produto]]      |   'cvcvehicle'            | ID do produto               |
| price     | [[preco-produto]]     | '3500'            | Preço da locacao em pontos            |
| brand     | [[marca-produto]]     |   'movida', 'localiza'          | Nome da locadora              |
| category    | [[categoria-produto]]   |  'carros'           | Categoria do produto            |
| list      | [[lista-produto]]     | 'viagens:carros'            | Nome da lista de veículos         |
| dimension17   | [[tipo-produto]]  | 'transferencia-para-parceiro', 'viagem', 'cartao-presente', 'beneficio', 'produto-digital', 'acumulacao', 'produto-fisico', 'transferencia-de-pontos', 'compra-de-pontos'     | Tipo do produto disponibilizado na página |
| dimension18   | [[cidade-retirada]]   | 'sao-paulo-sp'      | Nome da cidade selecionada para retirada do veículo |
| dimension19   | [[cidade-devolucao]]  | 'rio-de-janeiro-rj'     | Nome da cidade selecionada para devolução do veículo  |
| dimension20   | [[modelo-veiculo]]  | 'toyota-yaris'      | Nome do modelo do veículo |
| metric2     |[[valor-real]]|'129.50'|Valor total do produto em reais|

<br />

### Checkout Option:

- **Onde:** O objeto javascript (dataLayer) abaixo deve ser disparado na seleção de um tipo de pagamento.

```html
<script>
dataLayer.push({
    'event': 'checkoutOption',
  'eventCategory': 'enhanced-ecommerce',
  'eventAction': 'checkout-option',
  'eventLabel': 'tipo-pagamento',
    'ecommerce': {
      'checkout_option': {
        'actionField': {'step':'4', 'option': 'pagamento:[[opcao-selecionada]]'}
      }
    }
  });
</script>
```

<br />

| Variável          | Exemplo               | Descrição                                   |
| :------------------------ | :-------------------------------- | :---------------------------------------------------------------------------- |
| [[opcao-selecionada]] | : 'apenas-pontos', 'pontos+dinheiro' etc  | Deve retornar o item selecionado  |

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
        'affiliation': '[[loja-transacao]]',
        'revenue': '[[valor-total-transacao]]',
        'tax': '[[taxa-transacao]]',
        'shipping': '[[frete-transacao]]',
        'coupon': '[[cupom-transacao]]',
        'metric1': '[[valor-transacao-dinheiro]]',
        'metric3': '[[taxa-embarque]]',
        'dimension21':'[[tipo-pagamento]]',
      },
      'products': [{
        'name': '[[nome-produto]]',       //Nome ou ID do produto é obrigatório
        'id': '[[id-produto]]',
        'price': '[[preco-produto]]',
        'brand': '[[marca-produto]]',
        'category': '[[categoria-produto]]',
        'variant': '[[variacao-produto]]',
        'quantity': '[[quantidade-produto]]',
        'dimension6':'[[viagem-origem]]',
        'dimension7':'[[viagem-destino]]',
        'dimension8':'[[viagem-data-ida]]',
        'dimension9':'[[viagem-data-volta]',
        'dimension10':'[[passageiro-adulto]]',
        'dimension11':'[[passageiro-crianca]]',
        'dimension12':'[[passageiro-bebe]]',
        'dimension13':'[[tipo-viagem]',
        'dimension14':'[[voo-direto]]',
        'dimension15':'[[classe-executiva]]',
        'dimension16':'[[loja]]',
        'dimension17': '[[tipo-produto]]',
        'dimension18': '[[cidade-retirada]]',
        'dimension19': '[[cidade-devolucao]]',
        'dimension20': '[[modelo-veiculo]]',
        'metric2': '[[valor-real]]'
      }]
    }
  }
});
</script>
```
 
*Descrição Purchase:*
 
| Nome      | Variável          | Exemplo       | Descrição                     |
| :------------ | :------------------------ | :---------------- | :------------------------------------------------ |
| id      | [[id-transacao]]      | '000011652'       | ID da Transação                 |
| revenue     | [[valor-total-transacao]] | '13999'       | Valor total da transação incluindo frete e taxas - em pontos  |
| tax       | [[taxa-transacao]]    | '70.00'       | Taxas da transação, embarque - em pontos                |
| shipping    | [[frete-transacao]]     | '15.00'       | Frete da transação                |
| coupon    | [[cupom-transacao]]   | 'cupom-2019'        | Cupom de desconto usado na transação        |
| metric1     | [[valor-transacao-dinheiro]]    | '123.10'      | Valor total da transação em dinheiro      |
| metric3   | [[taxa-embarque]]   |'17.10'        | Valor total de taxas em dinheiro    |
| dimension21 | [[tipo-pagamento]]    | 'cash', 'points' e 'cash+points'        | Opções de pagamento utilizadas na compra    |
 


<br />
 
*Descrição Produtos:*
 

**Catálogo - Produtos**

<br />

| Nome      | Variável          | Exemplo           | Descrição                 |
| :------------ | :------------------------ | :------------------------ | :---------------------------------------- |
| name      | [[nome-produto]]      | 'jogo-de-panelas-tramontina-antiaderente'         | Nome do produto               |
| id      | [[id-produto]]      | '10649879'            | ID do produto               |
| price     | [[preco-produto]]     | '13999'           | Preço do produto em pontos            |
| brand     | [[marca-produto]]     | 'sony', 'samsung'           | Marca do produto              |
| category    | [[categoria-produto]]   | 'produto', 'milhas'           | Categoria do produto            |
| variant     | [[variacao-produto]]    | '110v'            | Variação do produto             |
| list      | [[lista-produto]]     | 'catalogo-produto'            | Nome da lista de produtos         |
| dimension16   | [[loja]]          | 'ponto-frio', 'fast-shop'           | Nome da loja do market place oferecida  |
| dimension17   | [[tipo-produto]]  | 'transferencia-para-parceiro', 'viagem', 'cartao-presente', 'beneficio', 'produto-digital', 'acumulacao', 'produto-fisico', 'transferencia-de-pontos', 'compra-de-pontos'         | Tipo do produto disponibilizado na página |
| metric2     |[[valor-real]]|'129.50'|Valor total do produto em reais|

<br />

**Passagens**

<br />

| Nome      | Variável          | Exemplo           | Descrição                 |
| :------------ | :------------------------ | :------------------------ | :---------------------------------------- |
| name      | [[nome-produto]]      | 'iv:gru-sdu/rec-vcp:adulto' (ida e volta), 'i:sdu-gru:crianca' (somente ida) e etc| Nome do voo considerando ida (i), ida e volta (iv), origem/destino (gru-sdu) e passageiros (adulto, crianca)  |
| id      | [[id-produto]]      |  'cvcfligth'            | ID do produto               |
| price     | [[preco-produto]]     | '13999'           | Preço da passagem em pontos           |
| brand     | [[marca-produto]]     |  'avianca'          | Marca do compania aérea               |
| category    | [[categoria-produto]]   |  'passagens'            | Categoria do produto            |
| variant     | [[variacao-produto]]    |  'voo-direto', 'direto:bagagem-2', '1-parada:bagagem-1' e etc     | Tipo do voo e bagagem       |
| list      | [[lista-produto]]     | 'viagens:passagens', 'viagens:pacotes'            | Nome da lista de voos         |
| dimension6    | [[viagem-origem]]     | 'gru', e etc      |Origem da viagem|
| dimension7    | [[viagem-destino]]    | 'sdu' e etc       |Destino da viagem|
| dimension8  | [[viagem-data-ida]]   | '01/10/2019' e etc      |Data de Ida da viagem|
| dimension9  | [[viagem-data-volta]    | '10/10/2019' e etc      |Data de Volta da viagem|
| dimension10 | [[passageiro-adulto]]   | '1'     |Quantidade de passageiros Adultos|
| dimension11 | [[passageiro-crianca]]  | '2'     |Quantidade de passageiros Crianças|
| dimension12 | [[passageiro-bebe]]   | '1'       |Quantidade de passageiros Bebês|
| dimension13 | [[tipo-viagem]      | 'ida-e-volta', 'somente-ida' e etc      |Deve retornar o tipo de viagem selecionado pelo usuário|
| dimension14 | [[voo-direto]]      | 'true' ou 'false'     |Opção de voos direto|
| dimension15 | [[classe-executiva]]    | 'true' ou 'false'     |Opção de classe executiva|
| dimension17   | [[tipo-produto]]  | 'transferencia-para-parceiro', 'viagem', 'cartao-presente', 'beneficio', 'produto-digital', 'acumulacao', 'produto-fisico', 'transferencia-de-pontos', 'compra-de-pontos'       | Tipo do produto disponibilizado na página |
| metric2     |[[valor-real]]|'129.50'|Valor total do produto em reais|


<br />

**Hotéis**

<br />

| Nome      | Variável          | Exemplo           | Descrição                 |
| :------------ | :------------------------ | :------------------------ | :---------------------------------------- |
| name      | [[nome-produto]]      | 'hotel-panamby-sao-paulo' (somente ida) e etc| Nome do hotel  |
| id      | [[id-produto]]      |  'cvchotel'           | ID do produto               |
| price     | [[preco-produto]]     | '20000'           | Preço do hotel em pontos            |
| brand     | [[marca-produto]]     |  'hotel', 'pousada' etc           | Tipo da hospedagem              |
| category    | [[categoria-produto]]   |  'hoteis'           | Categoria do produto            |
| variant     | [[variacao-produto]]    |  'apto-duplo', 'apto-casal', 'superior' etc     | Tipo de quarto        |
| list      | [[lista-produto]]     | 'viagens:hoteis', 'viagens:pacotes'           | Nome da lista de hotéis         |
| dimension6    | [[viagem-origem]]     | 'gru', e etc      |Origem da viagem|
| dimension7    | [[viagem-destino]]    | 'sdu' e etc       |Destino da viagem|
| dimension8  | [[viagem-data-ida]]   | '01/10/2019' e etc      |Data de Ida da viagem|
| dimension9  | [[viagem-data-volta]    | '10/10/2019' e etc      |Data de Volta da viagem|
| dimension10 | [[passageiro-adulto]]   | '1'     |Quantidade de passageiros Adultos|
| dimension11 | [[passageiro-crianca]]  | '2'     |Quantidade de passageiros Crianças|
| dimension12 | [[passageiro-bebe]]   | '1'       |Quantidade de passageiros Bebês|
| dimension13 | [[tipo-viagem]      | 'ida-e-volta', 'somente-ida' e etc      |Deve retornar o tipo de viagem selecionado pelo usuário|
| dimension14 | [[voo-direto]]      | 'true' ou 'false'     |Opção de voos direto|
| dimension15 | [[classe-executiva]]    | 'true' ou 'false'     |Opção de classe executiva|
| dimension17   | [[tipo-produto]]  | 'transferencia-para-parceiro', 'viagem', 'cartao-presente', 'beneficio', 'produto-digital', 'acumulacao', 'produto-fisico', 'transferencia-de-pontos', 'compra-de-pontos'       | Tipo do produto disponibilizado na página |
| metric2     |[[valor-real]]|'129.50'|Valor total do produto em reais|


<br />

**Carros**

<br />

| Nome      | Variável          | Exemplo           | Descrição                 |
| :------------ | :------------------------ | :------------------------ | :---------------------------------------- |
| name      | [[nome-produto]]      | 'localiza:localiza-hertz-centro-faria-lima' etc| Nome da locação de carro considerando nome da locadora e nome da unidade ou loja da locadora   |
| id      | [[id-produto]]      |   'cvcvehicle'            | ID do produto               |
| price     | [[preco-produto]]     | '3500'            | Preço da locacao em pontos            |
| brand     | [[marca-produto]]     |   'movida', 'localiza'          | Nome da locadora              |
| category    | [[categoria-produto]]   |  'carros'           | Categoria do produto            |
| list      | [[lista-produto]]     | 'viagens:carros'            | Nome da lista de veículos         |
| dimension17   | [[tipo-produto]]  | 'transferencia-para-parceiro', 'viagem', 'cartao-presente', 'beneficio', 'produto-digital', 'acumulacao', 'produto-fisico', 'transferencia-de-pontos', 'compra-de-pontos' etc       | Tipo do produto disponibilizado na página |
| dimension18   | [[cidade-retirada]]   | 'sao-paulo-sp'      | Nome da cidade selecionada para retirada do veículo |
| dimension19   | [[cidade-devolucao]]  | 'rio-de-janeiro-rj'     | Nome da cidade selecionada para devolução do veículo  |
| dimension20   | [[modelo-veiculo]]  | 'toyota-yaris'      | Nome do modelo do veículo |
| metric2     |[[valor-real]]|'129.50'|Valor total do produto em reais|


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
