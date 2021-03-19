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


### Geral


**No clique dos links do Header** <br />

- **Onde:** Em todas as páginas em que estiverem disponíveis
    - **Titulo ou nome do botão/link:** &quot;Ir para conteudo&quot;, &quot;ir para menu&quot; e etc
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:geral',
    'eventAction': 'clique:header_topo',
    'eventLabel': '[[nome-menu]]'
  
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-menu]] | &#039;ir_para_conteudo&#039;, &#039; ir_para_menu&#039;, &#039;sobre_a_pearir_para-buscason&#039;, e etc | Deve retornar o nome do menu clicado. |

<br />



**No clique dos links do Header** <br />

- **Onde:** Em todas as páginas em que estiverem disponíveis
    - **Titulo ou nome do botão/link:** caixa_de_entrada; meu_perfil; dados_de_pagamento; a_biblioteca_virtual; 
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:geral',
    'eventAction': 'clique:menu_perfil',
    'eventLabel': '[[nome-menu]]'
    
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-menu]] | caixa_de_entrada; meu_perfil; dados_de_pagamento; a_biblioteca_virtual;, e etc | Deve retornar o nome do menu clicado. |

<br />


**No clique dos links do Header** <br />

- **Onde:** Em todas as páginas em que estiverem disponíveis
    - **Titulo ou nome do botão/link:** abrir_menu;  
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:geral',
    'eventAction': 'clique:abrir_perfil',
    'eventLabel': '[[nome-clique]]'
    
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-clique]] | abrir_menu                            | Deve retornar o nome do botao clicado. |

<br />



**No clique dos links do Header** <br />

- **Onde:** Em todas as páginas em que estiverem disponíveis
    - **Titulo ou nome do botão/link:** fechar_menu;  
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:geral',
    'eventAction': 'clique:fechar_perfil',
    'eventLabel': '[[nome-clique]]'
    
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-clique]] | fechar_menu                           | Deve retornar o nome do botao clicado. |

<br />



**No clique dos links do Menu Lateral** <br />
 
- **Onde:** Em todas as páginas em que estiverem disponíveis
    - **Titulo ou nome do botão/link:** &quot;Inicio&quot;, Expert Header&quot;Acervo&quot; e etc
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:geral',
    'eventAction': 'clique:menu_lateral',
    'eventLabel': '[[nome-menu]]'
    
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-menu]] | &#039;inicio&#039;, &#039;expert_header&#039; e &#039;Acervo&#039; | Deve retornar o nome do menu clicado |

<br />


### HOME


**No Clique do botao no livro em Sugestões de Leitura** <br />

- **Onde:** No Inicio
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:home',
    'eventAction': 'clique:botao',
    'eventLabel': '[[sugestoes_de_leitura]]'
  
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sugestoes_de_leitura]] | &#Introducao_a_bioquimica&#039;, &#Soltando_as_amarras&#039; | Deve retornar o nome do livro clicado. |

<br />



**No clique das setas em Sugestões de Leitura** <br />

- **Onde:** No Inicio
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:home',
    'eventAction': 'clique:sugestoes_de_leitura',
    'eventLabel': 'setas:[direita|esquerda]'
    
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| setas:[direita|esquerda] | [direita|esquerda] = Inserir direita para quando a seta for clicada para na direita e esquerda quando for clicada na esquerda | Deve retornar o nome da seta clicada. |

<br />



**No clique das setas em Sugestões de Leitura** <br />

- **Onde:** No Inicio
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:home',
    'eventAction': 'clique:sugestoes_de_leitura',
    'eventLabel': '[[nome-livro]]',
    'dimension3': '[[ Titulo do Livro ]]',      
    'dimension4': '[[ Quantidade de paginas ]]',
    'dimension5': '[[ Nome da Editora ]]',
    'dimension6': '[[ Numero da Edicao ]]',
    'dimension7': '[[ Capitulo da leitura  ]]',
    'dimension8': '[[ Avaliar Leitura  ]]',
    'dimension9': '[[ Selecionar Categoria  ]]',
    'dimension10': '[[ Idioma da Leitura  ]]'  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-livro]] |  "introducao_a_bioquimica, matematica_financeira_aplicada, etc | Deve retornar o nome do livro clicado. |

<br />



**No clique dos botões dentro de um livro** <br />

- **Onde:** Após o clique inicial em um livro
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:home',
    'eventAction': 'clique:sugestoes_de_leitura',
    'eventLabel': '[[nome-botao]]',
    'dimension3': '[[ Titulo do Livro ]]',      
    'dimension4': '[[ Quantidade de paginas ]]',
    'dimension5': '[[ Nome da Editora ]]',
    'dimension6': '[[ Numero da Edicao ]]',
    'dimension7': '[[ Capitulo da leitura  ]]',
    'dimension8': '[[ Avaliar Leitura  ]]',
    'dimension9': '[[ Selecionar Categoria  ]]',
    'dimension10': '[[ Idioma da Leitura  ]]'  
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]]  |  ler_agora; adicionar_a_uma_lista; ver_mais_detalhes; fechar | Deve retornar o nome do botão clicado. |

<br />



**No clique informar dados** <br />

- **Onde:** Inicio
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:home',
    'eventAction': 'clique:botao',
    'eventLabel': 'informar_dados'
      
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| informar_dados |                                        | Deve retornar o nome do botão clicado. |

<br />



**No clique em Continue Lendo** <br />

- **Onde:** Inicio
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:home',
    'eventAction': 'clique:botao',
    'eventLabel': 'continue_lendo'
      
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| continue_lendo |                                        | Deve retornar o nome do botão clicado. |

<br />



**No clique dos livros** <br />

- **Onde:** Na sessão de continue lendo
 
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:home',
    'eventAction': 'clique:continue_lendo',
    'eventLabel': '[[nome-livro]]',
    'dimension3': '[[ Titulo do Livro ]]',      
    'dimension4': '[[ Quantidade de paginas ]]',
    'dimension5': '[[ Nome da Editora ]]',
    'dimension6': '[[ Numero da Edicao ]]',
    'dimension7': '[[ Capitulo da leitura  ]]',
    'dimension8': '[[ Avaliar Leitura  ]]',
    'dimension9': '[[ Selecionar Categoria  ]]',
    'dimension10': '[[ Idioma da Leitura  ]]' 
    
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-livro]]  |  espirito_empreendedor, quimica_geral | Deve retornar o nome do livro clicado. |


<br />


**No clique em fechar dos livros ** <br />

- **Onde:** Na sessão de continue lendo
 
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:home',
    'eventAction': 'clique:continue_lendo',
    'eventLabel': 'fechar'
    
    
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| fechar  |  espirito_empreendedor, quimica_geral | Deve retornar a ação efetuada. |


<br />



**Nas setas para rolar o carrossel** <br />

- **Onde:** Na sessão de continue lendo
 
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:home',
    'eventAction': 'clique:continue_lendo',
    'eventLabel': setas:[direita|esquerda]
    
    
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| setas:[direita|esquerda]  |  [direita|esquerda] = Inserir direita para quando a seta for clicada para na direita e esquerda quando for clicada na esquerda | Deve retornar o nome do livro clicado. |


<br />


**No clique em minha lista** <br />

- **Onde:** Sessão de minha Lista

     
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:home',
    'eventAction': 'clique:minha_lista',
    'eventLabel': [[nome-livro]],
    'dimension3': '[[ Titulo do Livro ]]',      
    'dimension4': '[[ Quantidade de paginas ]]',
    'dimension5': '[[ Nome da Editora ]]',
    'dimension6': '[[ Numero da Edicao ]]',
    'dimension7': '[[ Capitulo da leitura  ]]',
    'dimension8': '[[ Avaliar Leitura  ]]',
    'dimension9': '[[ Selecionar Categoria  ]]',
    'dimension10': '[[ Idioma da Leitura  ]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-livro]]  |   onze_tons_de_felicidade_no_trabalho     | Deve retornar o nome do livro clicado. |


<br />



**No clique dos botoes dentro dos livros** <br />

- **Onde:** Sessão de minha Lista

 
  ```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:home',
    'eventAction': 'clique:minha_lista',
    'eventLabel': [[nome-botao]],
    'dimension3': '[[ Titulo do Livro ]]',      
    'dimension4': '[[ Quantidade de paginas ]]',
    'dimension5': '[[ Nome da Editora ]]',
    'dimension6': '[[ Numero da Edicao ]]',
    'dimension7': '[[ Capitulo da leitura  ]]',
    'dimension8': '[[ Avaliar Leitura  ]]',
    'dimension9': '[[ Selecionar Categoria  ]]',
    'dimension10': '[[ Idioma da Leitura  ]]'   
    
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | ler_agora, adicionar_a_uma_lista, ver_mais_detalhes, fechar     | Deve retornar o nome do botão cliclado. |


<br />



**Nos cliques dentro da sua citação do dia** <br />

- **Onde:** Sessão de Sua Citação do dia
 
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:home',
    'eventAction': 'clique:sua_citacao_do_dia',
    'eventLabel': [[nome-botao]],
    'dimension3': '[[ Titulo do Livro ]]',      
    'dimension4': '[[ Quantidade de paginas ]]',
    'dimension5': '[[ Nome da Editora ]]',
    'dimension6': '[[ Numero da Edicao ]]',
    'dimension7': '[[ Capitulo da leitura  ]]',
    'dimension8': '[[ Avaliar Leitura  ]]',
    'dimension9': '[[ Selecionar Categoria  ]]',
    'dimension10': '[[ Idioma da Leitura  ]]'     
    
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] |  ler_mais, compartilhar, curtir, ir_para_a_pagina_do_livro     | Deve retornar o nome do botão cliclado. |


<br />



**Nos cliques dentro da sua citação do dia** <br />

- **Onde:** Sessão de Sua Citação do dia
 
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:home',
    'eventAction': 'clique:adicionados_recentemente',
    'eventLabel': [[nome-livro]],
    'dimension3': '[[ Titulo do Livro ]]',      
    'dimension4': '[[ Quantidade de paginas ]]',
    'dimension5': '[[ Nome da Editora ]]',
    'dimension6': '[[ Numero da Edicao ]]',
    'dimension7': '[[ Capitulo da leitura  ]]',
    'dimension8': '[[ Avaliar Leitura  ]]',
    'dimension9': '[[ Selecionar Categoria  ]]',
    'dimension10': '[[ Idioma da Leitura  ]]'    
    
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-livro]] |  espirito_empreendedor, quimica_geral     | Deve retornar o nome do livro cliclado. |


<br />


**Nas setas para rolar o carrossel** <br />

- **Onde:**  Na sessão de adicionados recentemente
 
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:home',
    'eventAction': 'clique:adicionados_recentemente',
    'eventLabel': setas:[direita|esquerda]
    
    
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| setas:[direita|esquerda]  |  [direita|esquerda] = Inserir direita para quando a seta for clicada para na direita e esquerda quando for clicada na esquerda | Deve retornar o nome da seta clicado. |


<br />




### EXPERT READER

**No clique no menu** <br />

- **Onde:** Expert Reader
    - **Titulo ou nome do botão/link:** todas_as_materias; reviews; dicas_de_leitura; 
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:expert_reader',
    'eventAction': 'clique:botao',
    'eventLabel': '[[nome-botao]]'
    
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] |  todas_as_materias, reviews, dicas_de_leitura; | Deve retornar o nome do botao clicado. |


<br />


**No clique dos livros** <br />

- **Onde:** Sessão todas as materias
 
    
  ```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:expert_reader',
    'eventAction': 'clique:todas_as_materias',
    'eventLabel': '[[nome-livro]]',
    'dimension3': '[[ Titulo do Livro ]]',      
    'dimension4': '[[ Quantidade de paginas ]]',
    'dimension5': '[[ Nome da Editora ]]',
    'dimension6': '[[ Numero da Edicao ]]',
    'dimension7': '[[ Capitulo da leitura  ]]',
    'dimension8': '[[ Avaliar Leitura  ]]',
    'dimension9': '[[ Selecionar Categoria  ]]',
    'dimension10': '[[ Idioma da Leitura  ]]'      
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-livro]] |  o_mundo_como_eu_vejo, a_amiga_de_leonardo_da_vinci | Deve retornar o nome do livro clicado. |


<br />



**No clique de Reviews** <br />

- **Onde:** Expert Reader
 
    
  ```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:expert_reader',
    'eventAction': 'clique:reviews',
    'eventLabel': '[[nome-livro]]',
    'dimension3': '[[ Titulo do Livro ]]',      
    'dimension4': '[[ Quantidade de paginas ]]',
    'dimension5': '[[ Nome da Editora ]]',
    'dimension6': '[[ Numero da Edicao ]]',
    'dimension7': '[[ Capitulo da leitura  ]]',
    'dimension8': '[[ Avaliar Leitura  ]]',
    'dimension9': '[[ Selecionar Categoria  ]]',
    'dimension10': '[[ Idioma da Leitura  ]]'       
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-livro]] |  o_mundo_como_eu_vejo, a_amiga_de_leonardo_da_vinci | Deve retornar o nome do livro clicado. |


<br />


**No clique de Reviews** <br />

- **Onde:** Expert Reader
 
    
  ```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:expert_reader',
    'eventAction': 'clique:dicas_de_leitura',
    'eventLabel': '[[nome-livro]]',
    'dimension3': '[[ Titulo do Livro ]]',      
    'dimension4': '[[ Quantidade de paginas ]]',
    'dimension5': '[[ Nome da Editora ]]',
    'dimension6': '[[ Numero da Edicao ]]',
    'dimension7': '[[ Capitulo da leitura  ]]',
    'dimension8': '[[ Avaliar Leitura  ]]',
    'dimension9': '[[ Selecionar Categoria  ]]',
    'dimension10': '[[ Idioma da Leitura  ]]'       
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-livro]] |  o_mundo_como_eu_vejo, a_amiga_de_leonardo_da_vinci | Deve retornar o nome do livro clicado. |


<br />



**No clique de Para ler depois** <br />

- **Onde:** Expert Reader
 
    
  ```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:expert_reader',
    'eventAction': 'clique:para_ler_depois',
    'eventLabel': '[[nome-livro]]',
    'dimension3': '[[ Titulo do Livro ]]',      
    'dimension4': '[[ Quantidade de paginas ]]',
    'dimension5': '[[ Nome da Editora ]]',
    'dimension6': '[[ Numero da Edicao ]]',
    'dimension7': '[[ Capitulo da leitura  ]]',
    'dimension8': '[[ Avaliar Leitura  ]]',
    'dimension9': '[[ Selecionar Categoria  ]]',
    'dimension10': '[[ Idioma da Leitura  ]]'    
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-livro]] |  o_mundo_como_eu_vejo, a_amiga_de_leonardo_da_vinci | Deve retornar o nome do livro clicado. |


<br />



**No clique dos botoes** <br />

- **Onde:** Após clicar em um livro dentro de Expert Reader
 
    
  ```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:expert_reader:detalhes',
    'eventAction': 'clique:botao',
    'eventLabel': '[[nome-botao]]',
    'dimension3': '[[ Titulo do Livro ]]',      
    'dimension4': '[[ Quantidade de paginas ]]',
    'dimension5': '[[ Nome da Editora ]]',
    'dimension6': '[[ Numero da Edicao ]]',
    'dimension7': '[[ Capitulo da leitura  ]]',
    'dimension8': '[[ Avaliar Leitura  ]]',
    'dimension9': '[[ Selecionar Categoria  ]]',
    'dimension10': '[[ Idioma da Leitura  ]]'     
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] |  o_mundo_como_eu_vejo, a_amiga_de_leonardo_da_vinci | Deve retornar o nome do botao clicado. |


<br />



**No Preenchimento do form de comentário** <br />

- **Onde:** Após clicar em um livro dentro de Expert Reader
   
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:expert_reader:detalhes',
    'eventAction': 'preencheu:campo',
    'eventLabel': 'comentario',
    'dimension3': '[[ Titulo do Livro ]]',      
    'dimension4': '[[ Quantidade de paginas ]]',
    'dimension5': '[[ Nome da Editora ]]',
    'dimension6': '[[ Numero da Edicao ]]',
    'dimension7': '[[ Capitulo da leitura  ]]',
    'dimension8': '[[ Avaliar Leitura  ]]',
    'dimension9': '[[ Selecionar Categoria  ]]',
    'dimension10': '[[ Idioma da Leitura  ]]'   

  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| 'comentario' |                                          | Deve retornar o nome do link clicado. |


<br />


**No Preenchimento do form de comentário** <br />

- **Onde:** Após clicar em um livro dentro de Expert Reader
   
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:expert_reader:detalhes',
    'eventAction': 'clique:enviar',
    'eventLabel':  
    'dimension3': '[[ Titulo do Livro ]]',      
    'dimension4': '[[ Quantidade de paginas ]]',
    'dimension5': '[[ Nome da Editora ]]',
    'dimension6': '[[ Numero da Edicao ]]',
    'dimension7': '[[ Capitulo da leitura  ]]',
    'dimension8': '[[ Avaliar Leitura  ]]',
    'dimension9': '[[ Selecionar Categoria  ]]',
    'dimension10': '[[ Idioma da Leitura  ]]'    
  
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
|                 |                                       | Deve retornar o nome do link clicado. |


<br />


**No Preenchimento do form de comentário** <br />

- **Onde:** Após clicar em um livro dentro de Expert Reader
   
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:expert_reader:detalhes',
    'eventAction': 'clique:conteudo_relacionado',
    'eventLabel':  '[[nome-livro]]'
    'dimension3': '[[ Titulo do Livrosro ]]',      
    'dimension4': '[[ Quantidade de paginas ]]',
    'dimension5': '[[ Nome da Editora ]]',
    'dimension6': '[[ Numero da Edicao ]]',
    'dimension7': '[[ Capitulo da leitura  ]]',
    'dimension8': '[[ Avaliar Leitura  ]]',
    'dimension9': '[[ Selecionar Categoria  ]]',
    'dimension10': '[[ Idioma da Leitura  ]]'     

  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
|  [[nome-livro]] |  o_mundo_como_eu_vejo, a_amiga_de_leonardo_da_vinci  | Deve retornar o nome do link clicado. |


<br />


### Acervo


**No clique em aplicar dentro de categoria** <br />

- **Onde:** Acervo
     
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:acervo',
    'eventAction': 'clique:categoria',
    'eventLabel': 'aplicar:[[nome-categoria]]'
  
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| aplicar:[[nome-categoria]] |  concursos|matematica, teologia" | Deve retornar o nome do link clicados. |


<br />



**No clique em aplicar dentro de subcategoria** <br />

- **Onde:** Acervo
     
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:acervo',
    'eventAction': 'clique:subcategoria',
    'eventLabel': 'aplicar:[[nome-subcategoria]]'
  
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| aplicar:[[nome-subcategoria]] |  mba|contabilidade|secretariado" | Deve retornar o nome do link clicados. |


<br />


**No clique em aplicar dentro de editora** <br />

- **Onde:** Acervo
     
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:acervo',
    'eventAction': 'clique:editora',
    'eventLabel': 'aplicar:[[nome-editora]]'
  
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| aplicar:[[nome-editora]] |  galenus|icone_editora"| Deve retornar o nome do link clicado. |


<br />



**No clique em aplicar dentro da nota de avaliação** <br />

- **Onde:** Acervo
     
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:acervo',
    'eventAction': 'clique:avaliacao',
    'eventLabel': 'aplicar:[[nota_avaliacao]]'
  
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| aplicar:[[nota_avaliacao]] |  1|2|3"| Deve retornar o nome do link clicados. |


<br />


**No clique em Filtrar por palavra chave** <br />

- **Onde:** Acervo
     
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:acervo',
    'eventAction': 'clique:filtrar_por_palavra_chave',
    'eventLabel': '[[nome-palavra_chave]]'
  
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-palavra_chave]] | filtro utilizado   | Deve retornar filtro ulitizado |


<br />


**No clique em Ordenar** <br />

- **Onde:** Acervo
     
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:acervo',
    'eventAction': 'clique:ordenar_por',
    'eventLabel': '[[valor-clique]]'
  
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[valor-clique]]| melhor_avaliados, mais_novos, etc"  |  Deve retornar o nome do link clicado.  |


<br />


**No clique dos livros** <br />

- **Onde:** Acervo
     
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:acervo',
    'eventAction': 'clique:acervo',
    'eventLabel': '[[nome-clique]]',
    'dimension3': '[[ Titulo do Livro ]]',      
    'dimension4': '[[ Quantidade de paginas ]]',
    'dimension5': '[[ Nome da Editora ]]',
    'dimension6': '[[ Numero da Edicao ]]',
    'dimension7': '[[ Capitulo da leitura  ]]',
    'dimension8': '[[ Avaliar Leitura  ]]',
    'dimension9': '[[ Selecionar Categoria  ]]',
    'dimension10': '[[ Idioma da Leitura  ]]'   
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-clique]] |  Retorna especificações do livros como: Título e Autor |  Deve retornar o nome do link clicado.  |


<br />


**No clique dos livros** <br />

- **Onde:** Acervo
     
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:acervo',
    'eventAction': 'clique:options_book',
    'eventLabel': '[[nome-menu]]'
  
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-menu]] |  "Visão_Geral, Capítulos, Comentários, Expert_Reader, Livros_Similares, Citações_Compartilhadas" |  Deve retornar o nome do link clicado.  |


<br />


**No clique de livro dentro do menu 'Livros Similares'** <br />

- **Onde:** Acervo
     
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:acervo',
    'eventAction': 'clique:similar_book',
    'eventLabel': '[[nome-clique]]',
    'dimension3': '[[ Titulo do Livro ]]',      
    'dimension4': '[[ Quantidade de paginas ]]',
    'dimension5': '[[ Nome da Editora ]]',
    'dimension6': '[[ Numero da Edicao ]]',
    'dimension7': '[[ Capitulo da leitura  ]]',
    'dimension8': '[[ Avaliar Leitura  ]]',
    'dimension9': '[[ Selecionar Categoria  ]]',
    'dimension10': '[[ Idioma da Leitura  ]]'     
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-clique]]|  Retorna especificações do livros como: Título e Autor |  Deve retornar o nome do link clicado.  |


<br />


**No clique de livro dentro do menu 'Livros Similares'** <br />

- **Onde:** Acervo
     
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:acervo',
    'eventAction': 'clique:ler_agora',
    'eventLabel': '[[nome-livro]]'
  
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-livro]]  |   ir_para_formato / "ler_em_pdf , ler_em_e-pub, etc" |  Deve retornar o nome do link clicado.  |


<br />


**No clique Adicionar a uma lista** <br />

- **Onde:** Acervo
     
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:acervo',
    'eventAction': 'clique:adicionar_a_uma_lista ',
    'eventLabel': '[[nome-lista]]'
  
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-lista]]]  |  "Minha_ lista, Escola, Iphone, etc" |  Deve retornar o nome do link clicado.  |


<br />


**No clique  nos botões dentro do livro** <br />

- **Onde:** Acervo
     
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:acervo',
    'eventAction': 'clique:buttons',
    'eventLabel': '[[nome-clique]]'
  
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-clique]] | Voltar, Ler_agora, Adicionar_a_uma_lista, Comprar_esse_livro |  Deve retornar o nome do link clicado.  |


<br />


### Minhas Listas

**No clique em Menu** <br /> 

- **Onde:** Minhas Listas

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:minhas_listas',
  'eventAction': 'clique:botao',
  'eventLabel': '[[nome-botao]]'
  
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] |  "Minhas_Listas, Continuar_Lendo, Livros_Lidos, Sugestões_para_você" |  Deve retornar o nome do botao clicado.  |


<br />



**No clique no Livro dentro de  Continuar Lendo** <br /> 

- **Onde:** Minhas Listas

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:minhas_listas',
  'eventAction': 'clique:continuar_lendo',
  'eventLabel': '[[nome-clique]]',
    'dimension3': '[[ Titulo do Livro ]]',      
    'dimension4': '[[ Quantidade de paginas ]]',
    'dimension5': '[[ Nome da Editora ]]',
    'dimension6': '[[ Numero da Edicao ]]',
    'dimension7': '[[ Capitulo da leitura  ]]',
    'dimension8': '[[ Avaliar Leitura  ]]',
    'dimension9': '[[ Selecionar Categoria  ]]',
    'dimension10': '[[ Idioma da Leitura  ]]'    
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-clique]] |   Retorna especificações do livros como: Título e Autor |  Deve retornar o nome do livro clicado.  |


<br />


**No clique no Livro dentro de  Livros Lidos** <br /> 

- **Onde:** Minhas Listas

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:minhas_listas',
  'eventAction': 'clique:livros_lidos',
  'eventLabel': '[[nome-clique]]',
    'dimension3': '[[ Titulo do Livro ]]',      
    'dimension4': '[[ Quantidade de paginas ]]',
    'dimension5': '[[ Nome da Editora ]]',
    'dimension6': '[[ Numero da Edicao ]]',
    'dimension7': '[[ Capitulo da leitura  ]]',
    'dimension8': '[[ Avaliar Leitura  ]]',
    'dimension9': '[[ Selecionar Categoria  ]]',
    'dimension10': '[[ Idioma da Leitura  ]]'    
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-clique]] |  Retorna especificações do livros como: Título e Autor |  Deve retornar o nome do livro clicado.  |


<br />



**No clique no Livro dentro de  Sugestões** <br /> 

- **Onde:** Minhas Listas

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:minhas_listas',
  'eventAction': 'clique:sugestoes',
  'eventLabel': '[[nome-clique]]',
    'dimension3': '[[ Titulo do Livro ]]',      
    'dimension4': '[[ Quantidade de paginas ]]',
    'dimension5': '[[ Nome da Editora ]]',
    'dimension6': '[[ Numero da Edicao ]]',
    'dimension7': '[[ Capitulo da leitura  ]]',
    'dimension8': '[[ Avaliar Leitura  ]]',
    'dimension9': '[[ Selecionar Categoria  ]]',
    'dimension10': '[[ Idioma da Leitura  ]]'    
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-clique]] |  Retorna especificações do livros como: Título e Autor |  Deve retornar o nome do livro clicado.  |


<br />


**No clique no Livro dentro de  Sugestões** <br /> 

- **Onde:** Minhas Listas

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:minhas_listas',
  'eventAction': 'clique:adicionar_lista',
  'eventLabel': '[[nome-lista]]'
  
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-lista]] |  "Criar_nova_lista, nome_da_lista, criar, etc" |  Deve retornar o nome do link clicado.  |


<br />


**No clique em Minha Lista de Leituras** <br /> 

- **Onde:** Minhas Listas

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:minhas_listas',
  'eventAction': 'clique:listas',
  'eventLabel': '[[nome-lista]]'
  
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-lista]]  |  "Onze_tons_de_felicidade_no_trabalho, Politica:Para_nao_ser_idiota |  Deve retornar o nome do link clicado.  |


<br />


### CONTINUAR LENDO


**No clique em Menu** <br /> 

- **Onde:** Continuar Lendo

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:continuar_lendo',
  'eventAction': 'clique:botao',
  'eventLabel': '[[nome-botao]]'
  
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]]  | "Minhas_Listas, Continuar_Lendo, Livros_Lidos, Sugestões_para_você" |  Deve retornar o nome do link clicado.  |


<br />


**No clique no Livro Adicionado** <br /> 

- **Onde:** Continuar Lendo

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:continuar_lendo',
  'eventAction': 'clique:livro',
  'eventLabel': '[[nome-clique]]',
    'dimension3': '[[ Titulo do Livro ]]',      
    'dimension4': '[[ Quantidade de paginas ]]',
    'dimension5': '[[ Nome da Editora ]]',
    'dimension6': '[[ Numero da Edicao ]]',
    'dimension7': '[[ Capitulo da leitura  ]]',
    'dimension8': '[[ Avaliar Leitura  ]]',
    'dimension9': '[[ Selecionar Categoria  ]]',
    'dimension10': '[[ Idioma da Leitura  ]]'    
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-clique]]  | Retorna especificações do livros como: Título e Autor |  Deve retornar o nome do livro clicado.  |


<br />



**No clique no Livro Adicionado** <br /> 

- **Onde:** Continuar Lendo

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:continuar_lendo',
  'eventAction': 'clique:opcao_leitura',
  'eventLabel': '[[nome-clique]]'
  
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-clique]]  | "Ler_em_e-pub", "Ler_em_PDF" |  Deve retornar o nome do botao clicado.  |


<br />


### CARTÃO DE ESTUDO 

**No clique em Filtrar** <br /> 

- **Onde:** Cartões de Estudo

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:cartoes',
  'eventAction': 'clique:filtrar',
  'eventLabel': '[[valor-digitado]]'
  
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[valor-digitado]]  |  filtro_palavra_chave |  Deve retornar o nome do botao clicado.  |


<br />

**No clique em Cartões de Estudos Criados** <br /> 

- **Onde:** Cartões de Estudo

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:cartoes',
  'eventAction': 'clique:cartao',
  'eventLabel': '[[nome-cartao]]'
  
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-cartao]] |  nome_cartao_selecionado |  Deve retornar o nome do cartao clicado.  |


<br />

### DESTAQUES E NOTAS

**No clique no Livro Adicionado** <br /> 

- **Onde:** Destaques e Notas 

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:destaques',
  'eventAction': 'clique:livro',
  'eventLabel': '[[nome-clique]]',
    'dimension3': '[[ Titulo do Livro ]]',      
    'dimension4': '[[ Quantidade de paginas ]]',
    'dimension5': '[[ Nome da Editora ]]',
    'dimension6': '[[ Numero da Edicao ]]',
    'dimension7': '[[ Capitulo da leitura  ]]',
    'dimension8': '[[ Avaliar Leitura  ]]',
    'dimension9': '[[ Selecionar Categoria  ]]',
    'dimension10': '[[ Idioma da Leitura  ]]'  
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-clique]] |   Retorna especificações do livros como: Título e Autor|  Deve retornar o nome do livro clicado.  |


<br />


**No clique dentro do Livro Adicionado** <br /> 

- **Onde:** Destaques e Notas 

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:destaques',
  'eventAction': 'clique:botao',
  'eventLabel': '[[nome-botao]]'
 
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]]  |   "Páginas_Marcadas, Destaques_e_Notas, Citações_Compartilhadas"  |  Deve retornar o nome do botao clicado.  |


<br />


### SUGESTÕES DE LEITURA


**No clique em Menu** <br /> 

- **Onde:** Sugestões de Leitura

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:sugestoes',
  'eventAction': 'clique:botao',
  'eventLabel': '[[nome-botao]]'
 
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]]  |  "Minhas_Listas, Continuar_Lendo, Livros_Lidos, Sugestões_para_você"  |  Deve retornar o nome do botao clicado.  |


<br />


**No clique em Menu** <br /> 

- **Onde:** Sugestões de Leitura

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:sugestoes',
  'eventAction': 'clique:livro',
  'eventLabel': '[[nome-clique]]',
    'dimension3': '[[ Titulo do Livro ]]',      
    'dimension4': '[[ Quantidade de paginas ]]',
    'dimension5': '[[ Nome da Editora ]]',
    'dimension6': '[[ Numero da Edicao ]]',
    'dimension7': '[[ Capitulo da leitura  ]]',
    'dimension8': '[[ Avaliar Leitura  ]]',
    'dimension9': '[[ Selecionar Categoria  ]]',
    'dimension10': '[[ Idioma da Leitura  ]]'    
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-clique]]  |  Retorna especificações do livros como: Título e Autor |  Deve retornar o nome do livro clicado.  |


<br />

### LIVROS LIDOS


**No clique em Menu** <br /> 

- **Onde:** Livros Lidos

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:livros_lidos',
  'eventAction': 'clique:botao',
  'eventLabel': '[[nome-botao]]'
 
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]]  |  "Minhas_Listas, Continuar_Lendo, Livros_Lidos, Sugestões_para_você" |  Deve retornar o nome do botao clicado.  |


<br />


**No clique em Menu** <br /> 

- **Onde:** Livros Lidos

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:livros_lidos',
  'eventAction': 'clique:livro',
  'eventLabel': '[[nome-clique]]',
    'dimension3': '[[ Titulo do Livro ]]',      
    'dimension4': '[[ Quantidade de paginas ]]',
    'dimension5': '[[ Nome da Editora ]]',
    'dimension6': '[[ Numero da Edicao ]]',
    'dimension7': '[[ Capitulo da leitura  ]]',
    'dimension8': '[[ Avaliar Leitura  ]]',
    'dimension9': '[[ Selecionar Categoria  ]]',
    'dimension10': '[[ Idioma da Leitura  ]]'    
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-clique]]  |  Retorna especificações do livros como: Título e Autor |  Deve retornar o nome do livro clicado.  |


<br />


### METAS DE LEITURA


**No clique em Metas de Leitura** <br /> 

- **Onde:** Metas de Leitura

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:metas',
  'eventAction': 'clique:ativar',
  'eventLabel': '[[botao-ativar]]'
 
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao-ativar]] |   ativa_ou_desativa_botao  |  Deve retornar o nome do botao ativo.  |


<br />


**No clique em Quantidade de Páginas** <br /> 

- **Onde:** Metas de Leitura

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:metas',
  'eventAction': 'clique:quantidade_paginas',
  'eventLabel': '[[valor-digitado]]'
 
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[valor-digitado]] |  "numero_de_páginas" |  Deve retornar o valor digitado.  |


<br />


**No clique em Frequência** <br /> 

- **Onde:** Metas de Leitura

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:metas',
  'eventAction': 'clique:frequencia',
  'eventLabel': '[[valor-botao]]'
 
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[valor-botao]] |   "Por_dia, Por_Semana, Por_Mês" |  Deve retornar o valor clicado.  |


<br />



**No clique em Dias de Folga** <br /> 

- **Onde:** Metas de Leitura

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:metas',
  'eventAction': 'clique:dias_de_folga',
  'eventLabel': '[[valor-botao]]'
 
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[valor-botao]] |    "Seg, Ter, Qua, etc" |  Deve retornar o valor clicado.  |


<br />



**No clique em  Salvar Preferências** <br /> 

- **Onde:** Metas de Leitura

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:metas',
  'eventAction': 'clique:salvar_preferencias',
  'eventLabel': '[[nome-salvar]]'
 
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-salvar]] |   "Sucesso, Metas_salvas, etc"  |  Deve retornar o valor clicado.  |


<br />

