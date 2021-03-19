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
        <iframe src=https://www.googletagmanager.com/ns.html?id=GTM-KTG9WHQ
                height=0 width=0 style=display:none;visibility:hidden></iframe>
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


- **Quando:** No clique do Header <br />

- **Onde:** Em todas as páginas
    - **Titulo ou nome do botão:** Ir para conteudo, Ir para o menu e etc
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:geral',
    'eventAction': 'clique:header_topo',
    'eventLabel': '[[nome_menu]]'
  });
  
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_menu]] | ir_para_conteudo, ir_para_o_menu | Deve retornar o nome do menu clicado. |

<br />



- **Quando:** No clique do menu dentro do perfil <br />

- **Onde:** Em todas as páginas
    - **Titulo ou nome do botão:** caixa_de_entrada; meu_perfil; dados_de_pagamento; a_biblioteca_virtual; 
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:geral',
    'eventAction': 'clique:menu_perfil',
    'eventLabel': '[[nome_menu]]'
  });
    
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_menu]] | caixa_de_entrada; meu_perfil; dados_de_pagamento; a_biblioteca_virtual;, e etc | Deve retornar o nome do menu clicado. |

<br />


- **Quando:** No clique do abrir menu do perfil  <br />

- **Onde:** Em todas as páginas
    - **Titulo ou nome do botão:** abrir_perfil;  
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:geral',
    'eventAction': 'clique:abrir_perfil'
  });
        
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | abrir_menu                            | Deve retornar o nome do botao clicado. |

<br />



- **Quando:** No clique do Fechar Menu do perfil <br />

- **Onde:** Em todas as páginas
    - **Titulo ou nome do botão:** fechar_perfil;  
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:geral',
    'eventAction': 'clique:fechar_perfil'
  });
    
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | fechar_menu                           | Deve retornar o nome do botao clicado. |

<br />



- **Quando:** No clique do Menu Lateral <br />
 
- **Onde:** Em todas as páginas
    - **Titulo ou nome do botão:** Inicio, Acervo, Minhas listas, Continuar lendo, e etc
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:geral',
    'eventAction': 'clique:menu_lateral',
    'eventLabel': '[[nome_menu]]'
    
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_menu]] | inicio,acervo,minhas_listas,continuar_lendo,etc | Deve retornar o nome do menu clicado |

<br />


### HOME


- **Quando:** No clique em Sugestões de Leitura <br />

- **Onde:** No Inicio
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:home',
    'eventAction': 'clique:botao',
    'eventLabel': 'sugestoes_de_leitura'
  
  });
</script> 
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_livro]] | gestão_de_meios_de_hospedagem,cálculo_numérico,transtornos_de_ansiedade | Deve retornar o nome do livro clicado. |

<br />



- **Quando:** No clique das setas em Sugestões de Leitura <br />

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
| [direita'|'esquerda] | [direita'|'esquerda] | Deve retornar o nome da seta clicada. |

<br />



- **Quando:** No clique dos livros em Sugestões de Leitura <br />

- **Onde:** No Inicio
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:home',
    'eventAction': 'clique:sugestoes_de_leitura',
    'eventLabel': '[[nome_livro]]',
    'dimension3': '[[titulo_do_livro]]',      
    'dimension4': '[[quantidade_de_paginas]]',
    'dimension5': '[[nome_da_editora]]',
    'dimension6': '[[numero_da_edicao ]]',
    'dimension7': '[[capitulo_da_leitura]]',
    'dimension8': '[[avaliar_leitura]]',
    'dimension9': '[[selecionar_categoria]]',
    'dimension10': '[[idioma_da_leitura]]'

});
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_livro]] |  gestão_de_meios_de_hospedagem,cálculo_numérico,transtornos_de_ansiedade | Deve retornar o nome do livro clicado. |

<br />



- **Quando:** No clique dos botões dentro de um livro <br />

- **Onde:** Após o clique inicial em um livro
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:home',
    'eventAction': 'clique:sugestoes_de_leitura',
    'eventLabel': '[[nome_botao]]',
    'dimension3': '[[titulo_do_livro]]',      
    'dimension4': '[[quantidade_de_paginas]]',
    'dimension5': '[[nome_da_editora]]',
    'dimension6': '[[numero_da_edicao ]]',
    'dimension7': '[[capitulo_da_leitura]]',
    'dimension8': '[[avaliar_leitura]]',
    'dimension9': '[[selecionar_categoria]]',
    'dimension10': '[[idioma_da_leitura]]' 
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]]  |  ler_agora,adicionar_a_uma_lista,ver_mais_detalhes,fechar | Deve retornar o nome do botão clicado. |

<br />



- **Quando:** No clique informar dados <br />

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
|                 |  jose_da_silva,10_01_1990,teste@teste.com  | Deve retornar o nome do botão clicado. |

<br />



- **Quando:** No clique em Continue Lendo <br />

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
| [[nome_menu]]   | continue_lendo  | Deve retornar o nome do menu clicado |

<br />



- **Quando:** No clique dos livros <br />

- **Onde:** Na sessão de continue lendo
 
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:home',
    'eventAction': 'clique:continue_lendo',
    'eventLabel': '[[nome_livro]]',
    'dimension3': '[[titulo_do_livro]]',      
    'dimension4': '[[quantidade_de_paginas]]',
    'dimension5': '[[nome_da_editora]]',
    'dimension6': '[[numero_da_edicao ]]',
    'dimension7': '[[capitulo_da_leitura]]',
    'dimension8': '[[avaliar_leitura]]',
    'dimension9': '[[selecionar_categoria]]',
    'dimension10': '[[idioma_da_leitura]]' 
    
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_livro]]  |  gestão_de_meios_de_hospedagem,cálculo_numérico,transtornos_de_ansiedade | Deve retornar o nome do livro clicado. |


<br />


- **Quando:** Nas setas para rolar o carrossel <br />

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
| [direita'|'esquerda]  |  [direita'|'esquerda]  | Deve retornar o nome do botao clicado. |


<br />


- **Quando:** No clique em minha lista <br />

- **Onde:** Sessão de minha Lista

     
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:home',
    'eventAction': 'clique:botao',
    'eventLabel': 'minha_lista',
    'dimension3': '[[titulo_do_livro]]',      
    'dimension4': '[[quantidade_de_paginas]]',
    'dimension5': '[[nome_da_editora]]',
    'dimension6': '[[numero_da_edicao ]]',
    'dimension7': '[[capitulo_da_leitura]]',
    'dimension8': '[[avaliar_leitura]]',
    'dimension9': '[[selecionar_categoria]]',
    'dimension10': '[[idioma_da_leitura]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_menu]]  |   minhas_listas    | Deve retornar o nome do menu clicado. |


<br />



- **Quando:** No clique dos botoes dentro dos livros <br />

- **Onde:** Sessão de minha Lista

 
  ```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:home',
    'eventAction': 'clique:minha_lista',
    'eventLabel': [[nome_botao]],
    'dimension3': '[[titulo_do_livro]]',      
    'dimension4': '[[quantidade_de_paginas]]',
    'dimension5': '[[nome_da_editora]]',
    'dimension6': '[[numero_da_edicao ]]',
    'dimension7': '[[capitulo_da_leitura]]',
    'dimension8': '[[avaliar_leitura]]',
    'dimension9': '[[selecionar_categoria]]',
    'dimension10': '[[idioma_da_leitura]]'  
    
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | ler_agora,adicionar_a_uma_lista,ver_mais_detalhes,fechar     | Deve retornar o nome do botão cliclado. |


<br />



- **Quando:** Nos cliques dentro da sua citação do dia <br />

- **Onde:** Sessão de Sua Citação do dia
 
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:home',
    'eventAction': 'clique:sua_citacao_do_dia',
    'eventLabel': [[nome_botao]],
    'dimension3': '[[titulo_do_livro]]',      
    'dimension4': '[[quantidade_de_paginas]]',
    'dimension5': '[[nome_da_editora]]',
    'dimension6': '[[numero_da_edicao ]]',
    'dimension7': '[[capitulo_da_leitura]]',
    'dimension8': '[[avaliar_leitura]]',
    'dimension9': '[[selecionar_categoria]]',
    'dimension10': '[[idioma_da_leitura]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  ler_mais,compartilhar,curtir,ir_para_a_pagina_do_livro     | Deve retornar o nome do botão cliclado. |


<br />


- **Quando:** No clique dos livros  <br />

- **Onde:** Na sessão de adicionados recentemente
 
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:home',
    'eventAction': 'clique:adicionados_recentemente',
    'eventLabel': [[nome_livro]],
    'dimension3': '[[titulo_do_livro]]',      
    'dimension4': '[[quantidade_de_paginas]]',
    'dimension5': '[[nome_da_editora]]',
    'dimension6': '[[numero_da_edicao ]]',
    'dimension7': '[[capitulo_da_leitura]]',
    'dimension8': '[[avaliar_leitura]]',
    'dimension9': '[[selecionar_categoria]]',
    'dimension10': '[[idioma_da_leitura]]'  
    
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_livro]] |  gestão_de_meios_de_hospedagem,cálculo_numérico,transtornos_de_ansiedade   | Deve retornar o nome do livro cliclado. |


<br />


- **Quando:** Nas setas para rolar o carrossel <br />

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
| [direita'|'esquerda]  |  [direita'|'esquerda]  | Deve retornar o nome da seta clicada. |


<br />



- **Quando:** No clique nos livros  <br />

- **Onde:**  Na sessão de Trending
 
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:home',
    'eventAction': 'clique:trending',
    'eventLabel': [[nome_livro]], 
    
    
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_livro]] | gestão_de_meios_de_hospedagem,cálculo_numérico,transtornos_de_ansiedade | Deve retornar o nome do livro clicado. |


<br />


### EXPERT READER

- **Quando:** No clique no menu <br />

- **Onde:** Expert Reader
    - **Titulo ou nome do botão:** todas_as_materias; reviews; dicas_de_leitura; 
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:expert_reader',
    'eventAction': 'clique:botao',
    'eventLabel': '[[nome_botao]]'
    
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  todas_as_materias,reviews,dicas_de_leitura | Deve retornar o nome do botao clicado. |


<br />


- **Quando:** No clique dos livros <br />

- **Onde:** Sessão todas as materias
 
    
  ```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:expert_reader',
    'eventAction': 'clique:todas_as_materias',
    'eventLabel': '[[nome_livro]]',
    'dimension3': '[[titulo_do_livro]]',      
    'dimension4': '[[quantidade_de_paginas]]',
    'dimension5': '[[nome_da_editora]]',
    'dimension6': '[[numero_da_edicao ]]',
    'dimension7': '[[capitulo_da_leitura]]',
    'dimension8': '[[avaliar_leitura]]',
    'dimension9': '[[selecionar_categoria]]',
    'dimension10': '[[idioma_da_leitura]]'       
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_livro]] | gestão_de_meios_de_hospedagem,cálculo_numérico,transtornos_de_ansiedade | Deve retornar o nome do livro clicado. |


<br />



- **Quando:** No clique de Reviews <br />

- **Onde:** Expert Reader
 
    
  ```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:expert_reader',
    'eventAction': 'clique:reviews',
    'eventLabel': '[[nome_livro]]',
    'dimension3': '[[titulo_do_livro]]',      
    'dimension4': '[[quantidade_de_paginas]]',
    'dimension5': '[[nome_da_editora]]',
    'dimension6': '[[numero_da_edicao ]]',
    'dimension7': '[[capitulo_da_leitura]]',
    'dimension8': '[[avaliar_leitura]]',
    'dimension9': '[[selecionar_categoria]]',
    'dimension10': '[[idioma_da_leitura]]'       
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_livro]] | gestão_de_meios_de_hospedagem,cálculo_numérico,transtornos_de_ansiedade | Deve retornar o nome do livro clicado. |


<br />


- **Quando:** No clique Dicas de leitura <br />

- **Onde:** Expert Reader
 
    
  ```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:expert_reader',
    'eventAction': 'clique:dicas_de_leitura',
    'eventLabel': '[[nome_livro]]',
    'dimension3': '[[titulo_do_livro]]',      
    'dimension4': '[[quantidade_de_paginas]]',
    'dimension5': '[[nome_da_editora]]',
    'dimension6': '[[numero_da_edicao ]]',
    'dimension7': '[[capitulo_da_leitura]]',
    'dimension8': '[[avaliar_leitura]]',
    'dimension9': '[[selecionar_categoria]]',
    'dimension10': '[[idioma_da_leitura]]'    
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_livro]] | gestão_de_meios_de_hospedagem,cálculo_numérico,transtornos_de_ansiedade | Deve retornar o nome do livro clicado. |


<br />



- **Quando:** No clique de Para ler depois <br />

- **Onde:** Expert Reader
 
    
  ```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:expert_reader',
    'eventAction': 'clique:para_ler_depois',
    'eventLabel': '[[nome_livro]]',
    'dimension3': '[[titulo_do_livro]]',      
    'dimension4': '[[quantidade_de_paginas]]',
    'dimension5': '[[nome_da_editora]]',
    'dimension6': '[[numero_da_edicao ]]',
    'dimension7': '[[capitulo_da_leitura]]',
    'dimension8': '[[avaliar_leitura]]',
    'dimension9': '[[selecionar_categoria]]',
    'dimension10': '[[idioma_da_leitura]]'    
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_livro]] | gestão_de_meios_de_hospedagem,cálculo_numérico,transtornos_de_ansiedade | Deve retornar o nome do livro clicado. |


<br />



- **Quando:** No clique dos botoes <br />

- **Onde:** Após clicar em um livro dentro de Expert Reader
 
    
  ```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:expert_reader:detalhes',
    'eventAction': 'clique:botao',
    'eventLabel': '[[nome_botao]]',
    'dimension3': '[[titulo_do_livro]]',      
    'dimension4': '[[quantidade_de_paginas]]',
    'dimension5': '[[nome_da_editora]]',
    'dimension6': '[[numero_da_edicao ]]',
    'dimension7': '[[capitulo_da_leitura]]',
    'dimension8': '[[avaliar_leitura]]',
    'dimension9': '[[selecionar_categoria]]',
    'dimension10': '[[idioma_da_leitura]]'    
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
|[[nome_botao]] | ler_agora,adicionar_a_uma_lista,ver_mais_detalhes,fechar| Deve retornar o nome do botao clicado. |


<br />



- **Quando:** No Preenchimento do form de comentário <br />

- **Onde:** Após clicar em um livro dentro de Expert Reader
   
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:expert_reader:detalhes',
    'eventAction': 'preencheu:campo',
    'eventLabel': '[[valor-digitado]]',
    'dimension3': '[[titulo_do_livro]]',      
    'dimension4': '[[quantidade_de_paginas]]',
    'dimension5': '[[nome_da_editora]]',
    'dimension6': '[[numero_da_edicao ]]',
    'dimension7': '[[capitulo_da_leitura]]',
    'dimension8': '[[avaliar_leitura]]',
    'dimension9': '[[selecionar_categoria]]',
    'dimension10': '[[idioma_da_leitura]]'

  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
|  [[valor-digitado]]  | 'otimo_livro,recomendo,otima_leitura'   | Deve retornar o nome do link clicado. |


<br />


- **Quando:** No clique de enviar do form de comentário <br />

- **Onde:** Após clicar em um livro dentro de Expert Reader
   
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:expert_reader:detalhes',
    'eventAction': 'clique:enviar',
    'eventLabel': '[[nome_botao]]', 
    'dimension3': '[[titulo_do_livro]]',      
    'dimension4': '[[quantidade_de_paginas]]',
    'dimension5': '[[nome_da_editora]]',
    'dimension6': '[[numero_da_edicao ]]',
    'dimension7': '[[capitulo_da_leitura]]',
    'dimension8': '[[avaliar_leitura]]',
    'dimension9': '[[selecionar_categoria]]',
    'dimension10': '[[idioma_da_leitura]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]]  |                                       | Deve retornar o nome do botao clicado. |


<br />


- **Quando:** No clique dos conteudos relacionados <br />

- **Onde:** Após clicar em um livro dentro de Expert Reader
   
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:expert_reader:detalhes',
    'eventAction': 'clique:conteudo_relacionado',
    'eventLabel': '[[nome_livro]]'
    'dimension3': '[[titulo_do_livro]]',      
    'dimension4': '[[quantidade_de_paginas]]',
    'dimension5': '[[nome_da_editora]]',
    'dimension6': '[[numero_da_edicao ]]',
    'dimension7': '[[capitulo_da_leitura]]',
    'dimension8': '[[avaliar_leitura]]',
    'dimension9': '[[selecionar_categoria]]',
    'dimension10': '[[idioma_da_leitura]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_livro]] | gestão_de_meios_de_hospedagem,cálculo_numérico,transtornos_de_ansiedade | Deve retornar o nome do link clicado. |


<br />


### Acervo


- **Quando:** No clique em aplicar dentro de categoria <br />

- **Onde:** Acervo
     
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:acervo',
    'eventAction': 'clique:categoria',
    'eventLabel': 'aplicar:[[nome_categoria]]'
  
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_categoria]] |  concursos'|'matematica, teologia | Deve retornar o nome do link clicados. |


<br />



- **Quando:** No clique em aplicar dentro de subcategoria <br />

- **Onde:** Acervo
     
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:acervo',
    'eventAction': 'clique:subcategoria',
    'eventLabel': 'aplicar:[[nome_subcategoria]]'
  
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_subcategoria]] |  mba'|'contabilidade'|'secretariado | Deve retornar o nome do link clicados. |


<br />


- **Quando:** No clique em aplicar dentro de editora <br />

- **Onde:** Acervo
     
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:acervo',
    'eventAction': 'clique:editora',
    'eventLabel': 'aplicar:[[nome_editora]]'
  
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_editora]] |  galenus'|'icone_editora | Deve retornar o nome do link clicado. |


<br />



- **Quando:** No clique em aplicar dentro da nota de avaliação <br />

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
| [[nota_botao]] |  1'|'2'|'3 | Deve retornar o nome do botao clicados. |


<br />


- **Quando:** No clique em Filtrar por palavra chave <br />

- **Onde:** Acervo
     
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:acervo',
    'eventAction': 'clique:filtrar_por_palavra_chave',
    'eventLabel': '[[valor_digitado]]'
  
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[valor_digitado]] | medicina,história,transtornos_de_ansiedade   | Deve retornar filtro ulitizado |


<br />


- **Quando:** No clique em Ordenar <br />

- **Onde:** Acervo
     
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:acervo',
    'eventAction': 'clique:ordenar_por',
    'eventLabel': '[[valor_botao]]'
  
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[valor_botao]] | melhor_avaliados,mais_novos,etc  |  Deve retornar o nome do botao clicado.  |


<br />


- **Quando:** No clique dos livros <br />

- **Onde:** Acervo
     
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:acervo',
    'eventAction': 'clique:acervo',
    'eventLabel': '[[nome_livro]]',
    'dimension3': '[[titulo_do_livro]]',      
    'dimension4': '[[quantidade_de_paginas]]',
    'dimension5': '[[nome_da_editora]]',
    'dimension6': '[[numero_da_edicao ]]',
    'dimension7': '[[capitulo_da_leitura]]',
    'dimension8': '[[avaliar_leitura]]',
    'dimension9': '[[selecionar_categoria]]',
    'dimension10': '[[idioma_da_leitura]]' 
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_livro]] | gestão_de_meios_de_hospedagem,cálculo_numérico,transtornos_de_ansiedade |  Deve retornar o nome do link clicado.  |


<br />


- **Quando:**  No clique no menu após escolher livro <br />

- **Onde:** Acervo
     
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:acervo',
    'eventAction': 'clique:funcoes_livro',
    'eventLabel': '[[nome_menu]]'
  
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_menu]] | ler_agora, adicionar_a_uma_lista, comprar_esse_livro |  Deve retornar o nome do link clicado.  |


<br />


- **Quando:** No clique de livro dentro do menu Livros Similares <br />

- **Onde:** Acervo
     
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:acervo',
    'eventAction': 'clique:livros_similares',
    'eventLabel': '[[nome_botao]]',
    'dimension3': '[[titulo_do_livro]]',      
    'dimension4': '[[quantidade_de_paginas]]',
    'dimension5': '[[nome_da_editora]]',
    'dimension6': '[[numero_da_edicao ]]',
    'dimension7': '[[capitulo_da_leitura]]',
    'dimension8': '[[avaliar_leitura]]',
    'dimension9': '[[selecionar_categoria]]',
    'dimension10': '[[idioma_da_leitura]]'     
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_livro]] | gestão_de_meios_de_hospedagem,cálculo_numérico,transtornos_de_ansiedade |  Deve retornar o nome do link clicado.  |


<br />


- **Quando:** No clique Ler agora <br />

- **Onde:** Acervo
     
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:acervo',
    'eventAction': 'clique:ler_agora',
    'eventLabel': '[[nome_botao]]'
  
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]]  | ler_em_pdf,ler_em_e-pub,etc |  Deve retornar o nome do link clicado.  |


<br />


- **Quando:** No clique Adicionar a uma lista <br />

- **Onde:** Acervo
     
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:acervo',
    'eventAction': 'clique:adicionar_a_uma_lista ',
    'eventLabel': '[[nome_lista]]'
  
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_lista]]]  |  minha_lista,escola,iphone, etc |  Deve retornar o nome do link clicado.  |


<br />


- **Quando:** No clique  nos botões dentro do livro <br />

- **Onde:** Acervo
     
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:acervo',
    'eventAction': 'clique:botoes',
    'eventLabel': '[[nome_botao]]'
  
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | voltar,ler_agora,adicionar_a_uma_lista,comprar_esse_livro |  Deve retornar o nome do link clicado.  |


<br />


### Minhas Listas

- **Quando:** No clique em Menu <br /> 

- **Onde:** Minhas Listas

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:minhas_listas',
  'eventAction': 'clique:botao',
  'eventLabel': '[[nome_botao]]'
  
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  minhas_listas,continuar_lendo,livros_lidos,sugestões_para_você |  Deve retornar o nome do botao clicado.  |


<br />



- **Quando:** No clique no Livro dentro de Continuar Lendo <br /> 

- **Onde:** Minhas Listas

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:minhas_listas',
  'eventAction': 'clique:continuar_lendo',
  'eventLabel': '[[nome_botao]]',
    'dimension3': '[[titulo_do_livro]]',      
    'dimension4': '[[quantidade_de_paginas]]',
    'dimension5': '[[nome_da_editora]]',
    'dimension6': '[[numero_da_edicao ]]',
    'dimension7': '[[capitulo_da_leitura]]',
    'dimension8': '[[avaliar_leitura]]',
    'dimension9': '[[selecionar_categoria]]',
    'dimension10': '[[idioma_da_leitura]]'  
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_livro]] | gestão_de_meios_de_hospedagem,cálculo_numérico,transtornos_de_ansiedade |  Deve retornar o nome do livro clicado.  |


<br />


- **Quando:** No clique no Livro dentro de Livros Lidos <br /> 

- **Onde:** Minhas Listas

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:minhas_listas',
  'eventAction': 'clique:livros_lidos',
  'eventLabel': '[[nome_botao]]',
    'dimension3': '[[titulo_do_livro]]',      
    'dimension4': '[[quantidade_de_paginas]]',
    'dimension5': '[[nome_da_editora]]',
    'dimension6': '[[numero_da_edicao ]]',
    'dimension7': '[[capitulo_da_leitura]]',
    'dimension8': '[[avaliar_leitura]]',
    'dimension9': '[[selecionar_categoria]]',
    'dimension10': '[[idioma_da_leitura]]'    
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_livro]] | gestão_de_meios_de_hospedagem,cálculo_numérico,transtornos_de_ansiedade  |  Deve retornar o nome do livro clicado.  |


<br />



- **Quando:** No clique no Livro dentro de Sugestões <br /> 

- **Onde:** Minhas Listas

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:minhas_listas',
  'eventAction': 'clique:sugestoes',
  'eventLabel': '[[nome_botao]]',
    'dimension3': '[[titulo_do_livro]]',      
    'dimension4': '[[quantidade_de_paginas]]',
    'dimension5': '[[nome_da_editora]]',
    'dimension6': '[[numero_da_edicao ]]',
    'dimension7': '[[capitulo_da_leitura]]',
    'dimension8': '[[avaliar_leitura]]',
    'dimension9': '[[selecionar_categoria]]',
    'dimension10': '[[idioma_da_leitura]]'  
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_livro]] | gestão_de_meios_de_hospedagem,cálculo_numérico,transtornos_de_ansiedade  |  Deve retornar o nome do livro clicado.  |


<br />


- **Quando:** No clique em Adicionar Lista <br /> 

- **Onde:** Minhas Listas

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:minhas_listas',
  'eventAction': 'clique:adicionar_lista',
  'eventLabel': '[[nome_lista]]'
  
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_lista]] |  criar_nova_lista,nome_da_lista,criar, etc |  Deve retornar o nome do link clicado.  |


<br />


- **Quando:** No clique em Minha Lista de Leituras <br /> 

- **Onde:** Minhas Listas

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:minhas_listas',
  'eventAction': 'clique:listas',
  'eventLabel': '[[nome_livro]]'
  
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_livro]] | gestão_de_meios_de_hospedagem,cálculo_numérico,transtornos_de_ansiedade  |  Deve retornar o nome do livro clicado.  |


<br />


### CONTINUAR LENDO


- **Quando:** No clique em Menu <br /> 

- **Onde:** Continuar Lendo

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:continuar_lendo',
  'eventAction': 'clique:botao',
  'eventLabel': '[[nome_botao]]'
  
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]]  | minhas_listas,continuar_lendo,livros_lidos,sugestões_para_você |  Deve retornar o nome do link clicado.  |


<br />


- **Quando:** No clique no Livro Adicionado <br /> 

- **Onde:** Continuar Lendo

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:continuar_lendo',
  'eventAction': 'clique:livro',
  'eventLabel': '[[nome_livro]]',
    'dimension3': '[[titulo_do_livro]]',      
    'dimension4': '[[quantidade_de_paginas]]',
    'dimension5': '[[nome_da_editora]]',
    'dimension6': '[[numero_da_edicao ]]',
    'dimension7': '[[capitulo_da_leitura]]',
    'dimension8': '[[avaliar_leitura]]',
    'dimension9': '[[selecionar_categoria]]',
    'dimension10': '[[idioma_da_leitura]]'  
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_livro]] | gestão_de_meios_de_hospedagem,cálculo_numérico,transtornos_de_ansiedade  |  Deve retornar o nome do livro clicado.  |


<br />



- **Quando:** No clique no Livro Adicionado <br /> 

- **Onde:** Continuar Lendo

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:continuar_lendo',
  'eventAction': 'clique:opcao_leitura',
  'eventLabel': '[[nome_botao]]'
  
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]]  | ler_em_epub,ler_em_pdf |  Deve retornar o nome do botao clicado.  |


<br />


### CARTÃO DE ESTUDO 

- **Quando:** No clique em Filtrar <br /> 

- **Onde:** Cartões de Estudo

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:cartoes',
  'eventAction': 'clique:filtrar',
  'eventLabel': '[[valor_digitado]]'
  
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[valor_digitado]] | teste,lembrar,anotacao_diaria,importante |  Deve retornar o nome valor digitado.  |


<br />

- **Quando:** No clique em Cartões de Estudos Criados <br /> 

- **Onde:** Cartões de Estudo

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:cartoes',
  'eventAction': 'clique:cartao',
  'eventLabel': '[[nome_botao]]'
  
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  teste,lembrar,anotacao_diaria,importante |  Deve retornar o nome do cartao clicado.  |


<br />

### DESTAQUES E NOTAS

- **Quando:** No clique no Livro Adicionado <br /> 

- **Onde:** Destaques e Notas 

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:destaques',
  'eventAction': 'clique:livro',
  'eventLabel': '[[nome_livro]]',
    'dimension3': '[[titulo_do_livro]]',      
    'dimension4': '[[quantidade_de_paginas]]',
    'dimension5': '[[nome_da_editora]]',
    'dimension6': '[[numero_da_edicao ]]',
    'dimension7': '[[capitulo_da_leitura]]',
    'dimension8': '[[avaliar_leitura]]',
    'dimension9': '[[selecionar_categoria]]',
    'dimension10': '[[idioma_da_leitura]]'
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_livro]] | gestão_de_meios_de_hospedagem,cálculo_numérico,transtornos_de_ansiedade  |  Deve retornar o nome do livro clicado.  |


<br />


- **Quando:** No clique dentro do Livro Adicionado <br /> 

- **Onde:** Destaques e Notas 

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:destaques',
  'eventAction': 'clique:botao',
  'eventLabel': '[[nome_botao]]'
 
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]]  |   páginas_marcadas,destaques_e_notas,citações_compartilhadas  |  Deve retornar o nome do botao clicado.  |


<br />


### SUGESTÕES DE LEITURA


- **Quando:** No clique em Menu <br /> 

- **Onde:** Sugestões de Leitura

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:sugestoes',
  'eventAction': 'clique:botao',
  'eventLabel': '[[nome_botao]]'
 
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]]  |  minhas_listas,continuar_lendo,livros_lidos,sugestões_para_você  |  Deve retornar o nome do botao clicado.  |


<br />


- **Quando:** No clique no Livro  <br /> 

- **Onde:** Sugestões de Leitura

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:sugestoes',
  'eventAction': 'clique:livro',
  'eventLabel': '[[nome_livro]]',
    'dimension3': '[[titulo_do_livro]]',      
    'dimension4': '[[quantidade_de_paginas]]',
    'dimension5': '[[nome_da_editora]]',
    'dimension6': '[[numero_da_edicao ]]',
    'dimension7': '[[capitulo_da_leitura]]',
    'dimension8': '[[avaliar_leitura]]',
    'dimension9': '[[selecionar_categoria]]',
    'dimension10': '[[idioma_da_leitura]]'  
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_livro]] | gestão_de_meios_de_hospedagem,cálculo_numérico,transtornos_de_ansiedade |  Deve retornar o nome do livro clicado.  |


<br />

### LIVROS LIDOS


- **Quando:** No clique em Menu <br /> 

- **Onde:** Livros Lidos

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:livros_lidos',
  'eventAction': 'clique:botao',
  'eventLabel': '[[nome_botao]]'
 
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]]  |  minhas_listas,continuar_lendo,livros_lidos,sugestões_para_você  |  Deve retornar o nome do botao clicado.  |


<br />


- **Quando:** No clique no Livro Adicionado <br /> 

- **Onde:** Livros Lidos

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',   
  'eventCategory': 'bvirtual:livros_lidos',
  'eventAction': 'clique:livro',
  'eventLabel': '[[nome_livro]]',
    'dimension3': '[[titulo_do_livro]]',      
    'dimension4': '[[quantidade_de_paginas]]',
    'dimension5': '[[nome_da_editora]]',
    'dimension6': '[[numero_da_edicao ]]',
    'dimension7': '[[capitulo_da_leitura]]',
    'dimension8': '[[avaliar_leitura]]',
    'dimension9': '[[selecionar_categoria]]',
    'dimension10': '[[idioma_da_leitura]]'    
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_livro]] | gestão_de_meios_de_hospedagem,cálculo_numérico,transtornos_de_ansiedade |  Deve retornar o nome do livro clicado.  |


<br />


### METAS DE LEITURA


- **Quando:** No clique em Metas de Leitura <br /> 

- **Onde:** Metas de Leitura

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:metas',
  'eventAction': 'clique:ativar',
  'eventLabel': '[[botao_ativar]]'
 
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao_ativar]] |   ativa_ou_desativa_botao  |  Deve retornar o nome do botao ativo no momento.  |


<br />


- **Quando:** No clique em Quantidade de Páginas <br /> 

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
| [[valor-digitado]] |  1,20,24,12,34 |  Deve retornar o valor digitado.  |


<br />


- **Quando:** No clique em Frequência <br /> 

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
| [[valor-botao]] |   por_dia,por_semana,por_mês |  Deve retornar o valor clicado.  |


<br />



- **Quando:** No clique em Dias de Folga <br /> 

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
| [[valor-botao]] |    seg,ter,qua,qui etc |  Deve retornar o valor clicado.  |


<br />



- **Quando:** No clique em  Salvar Preferências <br /> 

- **Onde:** Metas de Leitura

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
  'event': 'event',
  'eventCategory': 'bvirtual:metas',
  'eventAction': 'clique:salvar_preferencias',
  'eventLabel': '[[nome_salvar]]'
 
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_salvar]] |   sucesso,metas_salvas,etc  |  Deve retornar o valor clicado.  |


<br />

