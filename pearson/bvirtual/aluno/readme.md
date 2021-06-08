> Documento de Especificação Técnica

<br />

## Implementação da Camada de dados - Bibot Aluno
Última atualização: 31/05/2021 <br />
Em caso de dúvidas, entrar em contato com: [thiago.ceccato@pearson.com](mailto:thiago.ceccato@pearson.com)

<br />

## Objetivo
Este documento tem como objetivo instruir a implementação da camada de dados e de data attributes para utilização de recursos de monitoramento do Google Analytics referentes ao ambiente de [URL Produção: https://aluno.bibotdigital.com.br//](https://aluno.bibotdigital.com.br//).

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
})(window,document,'script','dataLayer','GTM-57K6HF5');</script>
<!-- End Google Tag Manager -->
    //...
  </head>
```

#### 2. Copie o seguinte trecho e cole-o imediatamente após a marcação `<body>` de abertura em cada página do seu site.

```html
<body>
  <!-- Google Tag Manager (noscript) -->
<noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-57K6HF5"
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


### Geral


- **Quando:** No clique do menu do Header <br />

- **Onde:** Em todas as páginas
    - **Titulo ou nome do botão:** Caixa de entrada, Sobre a Bibot, Fale Conosco e etc
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:geral',
    'eventAction': 'clique:menu_header',
    'eventLabel': '[[nome_menu]]'
  
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_menu]] | caixa_de_entrada, sobre_a_bibot, fale_conosco, e etc | Deve retornar o nome do menu clicado. |

<br />



- **Quando:** No clique do abrir menu do perfil <br />

- **Onde:** Em todas as páginas
    - **Titulo ou nome do botão:** abrir_perfil
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:geral',
    'eventAction': 'clique:abrir_perfil',
        
</script>
```

<br />


- **Quando:** No clique do fechar menu do perfil <br />

- **Onde:** Em todas as páginas
    - **Titulo ou nome do botão:** fechar_perfil
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:geral',
    'eventAction': 'clique:fechar_perfil',
        
</script>
```

<br />



- **Quando:** No clique do Menu Lateral <br />
 
- **Onde:** Em todas as páginas
    - **Titulo ou nome do botão:** Minha Trilha, Continuar Lendo, Favoritos, Atividades, e etc
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:geral',
    'eventAction': 'clique:menu_lateral',
    'eventLabel': '[[nome_menu]]'
    
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_menu]] | minha_trilha, continuar_lendo, favoritos, etc | Deve retornar o nome do menu clicado |

<br />



- **Quando:** No envio do valor digitado da caixa de pesquisa <br />
 
- **Onde:** Em todas as páginas
    - **Titulo ou nome do botão:** Aladin, Mitos Gregos, Os Sandubas, e etc
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:geral',
    'eventAction': 'preencheu:campo',
    'eventLabel': '[[valor_digitado]]'
    
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[valor_digitado]] | aladin, mitos_gregos, os_sandubas, etc | Deve retornar o valor digitado |

<br />



### HOME


- **Quando:** No clique nos livros em Trilha do Conhecimento <br />

- **Onde:** No Inicio
    - **Titulo ou nome do botão:** Mitos Gregos, Sardenta, Diaria de uma Sonhadora, e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:home',
    'eventAction': 'clique:trilha_do_conhecimento',
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
| [[nome_livro]] | mitos_gregos, sardenta, diario_de_uma_sonhadora, etc | Deve retornar o nome do livro clicado. |
| [[titulo_do_livro]] | mitos_gregos | Nome do Livro Clicado |	
| [[quantidade_de_paginas]] | 35, 88 | Quantidade de Páginas do Livro |	
| [[nome_da_editora]] | Nova Fronteira Bibot | Nome da Editora do Livro |	
| [[numero_da_edicao ]] | 1º(2020) | Número da Edição |	
| [[capitulo_da_leitura] | 3,6,2 | Número do Capitulo de Leitura |	
| [[avaliar_leitura]] |  1,2,3 | Quantidade de estrelas de avaliação que o livro possui |	
| [[selecionar_categoria]] | Literatura Infanto Juvenil, Ficção | Categoria do livro |	
| [[idioma_da_leitura]] | ingles, portugues | 	Idioma do livro |

<br />



- **Quando:** No clique das setas em Trilha do Conhecimento <br />

- **Onde:** No Inicio
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:home',
    'eventAction': 'clique:trilha_do_conhecimento',
    'eventLabel': 'setas:[direita|esquerda]'
    
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [direita\|esquerda] | [direita\|esquerda] | Deve retornar o nome da seta clicada. |

<br />



- **Quando:** No clique dos botões dentro de um livro <br />

- **Onde:** Após o clique inicial em um livro
    - **Titulo ou nome do botão:** Ler Agora, Adicionar aos Favoritos, e etc
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:home',
    'eventAction': 'clique:trilha_do_conhecimento',
    'eventLabel': '[[nome_botao]]'
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
| [[nome_botao]] | ler_agora, adicionar_aos_favoritos | Deve retornar o nome do botao clicado. |

<br />



- **Quando:** No clique do menu dentro de um livro <br />

- **Onde:** Após o clique inicial em um livro
    - **Titulo ou nome do botão:** Conheca o Livro, Capitulos, Atividades, Livros Similares
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:home',
    'eventAction': 'clique:menu_livro',
    'eventLabel': '[[nome_menu]]'
  
  });
</script> 
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_menu]] | conheca_o_livro, capitulos, atividades, livros_similares | Deve retornar o nome do menu clicado. |

<br />



- **Quando:** No clique do menu Capitulos dentro de um livro <br />

- **Onde:** Após o clique inicial em um livro
    - **Titulo ou nome do botão:** Capa, Rosto, Creditos, Sumario, Apresentacao, e etc
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:home',
    'eventAction': 'clique:capitulos_livro',
    'eventLabel': '[[nome_botao]]'
  
  });
</script> 
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | capa, rosto, creditos, sumario, apresentacao, etc | Deve retornar o nome do botao clicado. |

<br />


- **Quando:** No clique do menu Livros Similares dentro de um livro <br />

- **Onde:** Após o clique inicial em um livro
    - **Titulo ou nome do botão:** Mitos Gregos, Sardenta, Diario de uma sonhadora, e etc
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:home',
    'eventAction': 'clique:livros_similares',
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
| [[nome_livro]] | mitos_gregos, sardenta, diario_de_uma_sonhadora, etc | Deve retornar o nome do livro clicado. |
| [[titulo_do_livro]] | mitos_gregos | Nome do Livro Clicado |	
| [[quantidade_de_paginas]] | 35, 88 | Quantidade de Páginas do Livro |	
| [[nome_da_editora]] | Nova Fronteira Bibot | Nome da Editora do Livro |	
| [[numero_da_edicao ]] | 1º(2020) | Número da Edição |	
| [[capitulo_da_leitura] | 3,6,2 | Número do Capitulo de Leitura |	
| [[avaliar_leitura]] |  1,2,3 | Quantidade de estrelas de avaliação que o livro possui |	
| [[selecionar_categoria]] | Literatura Infanto Juvenil, Ficção | Categoria do livro |	
| [[idioma_da_leitura]] | ingles, portugues | 	Idioma do livro |

<br />




- **Quando:** No clique em Continue Lendo <br />

- **Onde:** Inicio
    - **Titulo ou nome do botão:** Mitos Gregos, Sardenta, Diario de uma sonhadora, e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:home',
    'eventAction': 'clique:continuar_lendo',
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
| [[nome_livro]]   | mitos_gregos, sardenta, diario_de_uma_sonhadora, etc | Deve retornar o nome do livro clicado |
| [[titulo_do_livro]] | mitos_gregos | Nome do Livro Clicado |	
| [[quantidade_de_paginas]] | 35, 88 | Quantidade de Páginas do Livro |	
| [[nome_da_editora]] | Nova Fronteira Bibot | Nome da Editora do Livro |	
| [[numero_da_edicao ]] | 1º(2020) | Número da Edição |	
| [[capitulo_da_leitura] | 3,6,2 | Número do Capitulo de Leitura |	
| [[avaliar_leitura]] |  1,2,3 | Quantidade de estrelas de avaliação que o livro possui |	
| [[selecionar_categoria]] | Literatura Infanto Juvenil, Ficção | Categoria do livro |	
| [[idioma_da_leitura]] | ingles, portugues | 	Idioma do livro |

<br />



- **Quando:** No clique no livro em Meus Favoritos <br />

- **Onde:** Inicio
    - **Titulo ou nome do botão:** Ler agora, Ver Detalhes

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:home',
    'eventAction': 'clique:meus_favoritos',
    'eventLabel': '[[nome_botao]]
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]]   | ler_agora, ver_detalhes | Deve retornar o nome do botao clicado |

<br />


- **Quando:** No clique no livro em Adicionados Recentemente <br />

- **Onde:** Inicio
    - **Titulo ou nome do botão:** Ler agora, Ver Detalhes, Adicionar aos Favoritos

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:home',
    'eventAction': 'clique:adicionados_recentemente',
    'eventLabel': '[[nome_botao]]'
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
| [[nome_botao]]   | ler_agora, ver_detalhes, adicionar aos favoritos | Deve retornar o nome do botao clicado |
| [[titulo_do_livro]] | mitos_gregos | Nome do Livro Clicado |	
| [[quantidade_de_paginas]] | 35, 88 | Quantidade de Páginas do Livro |	
| [[nome_da_editora]] | Nova Fronteira Bibot | Nome da Editora do Livro |	
| [[numero_da_edicao ]] | 1º(2020) | Número da Edição |	
| [[capitulo_da_leitura] | 3,6,2 | Número do Capitulo de Leitura |	
| [[avaliar_leitura]] |  1,2,3 | Quantidade de estrelas de avaliação que o livro possui |	
| [[selecionar_categoria]] | Literatura Infanto Juvenil, Ficção | Categoria do livro |	
| [[idioma_da_leitura]] | ingles, portugues | 	Idioma do livro |

<br />



- **Quando:**  No clique das setas em Adicionados Recentemente <br />

- **Onde:** Inicio

    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bvirtual:home',
    'eventAction': 'clique:setas_add_recentemente',
    'eventLabel': 'setas:[direita|esquerda]'
    
    
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [direita\|esquerda]  |  [direita\|esquerda]  | Deve retornar o nome da seta clicada. |


<br />



### Minha Trilha


- **Quando:** No clique no livro  <br />

- **Onde:** Menu Minha Trilha
	- **Titulo ou nome do botão:** Ler agora, Ver Detalhes, Adicionar aos Favoritos

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:minha_trilha',
    'eventAction': 'clique:minha_trilha',
    'eventLabel': '[[nome_botao]]'
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
| [[nome_botao]]   | ler_agora, ver_detalhes, adicionar aos favoritos | Deve retornar o nome do botao clicado |
| [[titulo_do_livro]] | mitos_gregos | Nome do Livro Clicado |	
| [[quantidade_de_paginas]] | 35, 88 | Quantidade de Páginas do Livro |	
| [[nome_da_editora]] | Nova Fronteira Bibot | Nome da Editora do Livro |	
| [[numero_da_edicao ]] | 1º(2020) | Número da Edição |	
| [[capitulo_da_leitura] | 3,6,2 | Número do Capitulo de Leitura |	
| [[avaliar_leitura]] |  1,2,3 | Quantidade de estrelas de avaliação que o livro possui |	
| [[selecionar_categoria]] | Literatura Infanto Juvenil, Ficção | Categoria do livro |	
| [[idioma_da_leitura]] | ingles, portugues | 	Idioma do livro |

<br />



- **Quando:** No clique no livro  <br />

- **Onde:** Menu Minha Trilha
	- **Titulo ou nome do botão:** Voltar, Ler agora, Ver Detalhes, Adicionar aos Favoritos

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:minha_trilha',
    'eventAction': 'clique:menu_minha_trilha',
    'eventLabel': '[[nome_menu]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_menu]]   | voltar, trilha, favoritos, continuar_lendo, lidos | Deve retornar o nome do menu clicado |

<br />


### Favoritos


- **Quando:** No clique no livro  <br />

- **Onde:** Menu Favoritos
	- **Titulo ou nome do botão:** Trilha, Favoritos, Continuar Lendo, Lidos

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:favoritos',
    'eventAction': 'clique:favoritos',
    'eventLabel': '[[nome_botao]]'
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
| [[nome_botao]]   | trilha, favoritos, continuar_lendo, lidos | Deve retornar o nome do botao clicado |
| [[titulo_do_livro]] | mitos_gregos | Nome do Livro Clicado |	
| [[quantidade_de_paginas]] | 35, 88 | Quantidade de Páginas do Livro |	
| [[nome_da_editora]] | Nova Fronteira Bibot | Nome da Editora do Livro |	
| [[numero_da_edicao ]] | 1º(2020) | Número da Edição |	
| [[capitulo_da_leitura] | 3,6,2 | Número do Capitulo de Leitura |	
| [[avaliar_leitura]] |  1,2,3 | Quantidade de estrelas de avaliação que o livro possui |	
| [[selecionar_categoria]] | Literatura Infanto Juvenil, Ficção | Categoria do livro |	
| [[idioma_da_leitura]] | ingles, portugues | 	Idioma do livro |

<br />


### Continuar Lendo


- **Quando:** No clique no livro  <br />

- **Onde:** Menu Continuar Lendo
	- **Titulo ou nome do botão:** Ler agora, Ver Detalhes, Adicionar aos Favoritos

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:continuar_lendo',
    'eventAction': 'clique:continuar_lendo',
    'eventLabel': '[[nome_botao]]'
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
| [[nome_botao]]   | ler_agora, ver_detalhes, adicionar aos favoritos | Deve retornar o nome do botao clicado |
| [[titulo_do_livro]] | mitos_gregos | Nome do Livro Clicado |	
| [[quantidade_de_paginas]] | 35, 88 | Quantidade de Páginas do Livro |	
| [[nome_da_editora]] | Nova Fronteira Bibot | Nome da Editora do Livro |	
| [[numero_da_edicao ]] | 1º(2020) | Número da Edição |	
| [[capitulo_da_leitura] | 3,6,2 | Número do Capitulo de Leitura |	
| [[avaliar_leitura]] |  1,2,3 | Quantidade de estrelas de avaliação que o livro possui |	
| [[selecionar_categoria]] | Literatura Infanto Juvenil, Ficção | Categoria do livro |	
| [[idioma_da_leitura]] | ingles, portugues | 	Idioma do livro |

<br />


### Acervo


- **Quando:** No clique em aplicar dentro de categoria  <br />

- **Onde:** acervo
	- **Titulo ou nome do botão:** Ler agora, Ver Detalhes, Adicionar aos Favoritos

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:continuar_lendo',
    'eventAction': 'clique:continuar_lendo',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]]   | ler_agora, ver_detalhes, adicionar_aos_favoritos | Deve retornar o nome do botao clicado |

<br />


- **Quando:** No clique em aplicar dentro de categoria <br />

- **Onde:** Acervo
     
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:acervo',
    'eventAction': 'clique:categoria',
    'eventLabel': 'aplicar:[[nome_categoria]]'
  
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_categoria]] | apologo\|ensaio\|classicos_da_literatura | Deve retornar o nome das categorias clicadas |


<br />


- **Quando:** No clique em aplicar dentro de autor <br />

- **Onde:** Acervo
     
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:acervo',
    'eventAction': 'clique:autor',
    'eventLabel': 'aplicar:[[nome_autor]]'
  
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_autor]] | caio_maia\|edna_alencar_rivera\|etc | Deve retornar o nome dos autores clicados. |


<br />


- **Quando:** No clique em Filtrar por palavra chave <br />

- **Onde:** Acervo
     - **Titulo ou nome do botão:** Aladin, Mitos Gregos, Os Sandubas, e etc
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:acervo',
    'eventAction': 'clique:filtrar_por_palavra_chave',
    'eventLabel': '[[valor_digitado]]'
  
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[valor_digitado]] |  aladin, mitos_gregos, os_sandubas, etc | Deve retornar o valor digitado |


<br />


- **Quando:** No clique em Ordenar  <br />

- **Onde:** Acervo
     - **Titulo ou nome do botão:** Relevancia, Mais Novos e etc
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:acervo',
    'eventAction': 'clique:ordenar_por',
    'eventLabel': '[[valor_botao]]'
  
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[valor_botao]] |  relevancia, mais_novos, etc | Deve retornar o nome do botao clicado. |


<br />


- **Quando:** No clique no livro  <br />

- **Onde:** Acervo
     - **Titulo ou nome do botão:** Ler Agora, Ver Detalhes e Adicionar aos Favoritos
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:acervo',
    'eventAction': 'clique:livro_acervo',
    'eventLabel': '[[nome_botao]]'
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
| [[nome_botao]] |  ler_agora, ver_detalhes, adicionar_aos_favoritos | Deve retornar o nome do botao clicado. |
| [[titulo_do_livro]] | mitos_gregos | Nome do Livro Clicado |	
| [[quantidade_de_paginas]] | 35, 88 | Quantidade de Páginas do Livro |	
| [[nome_da_editora]] | Nova Fronteira Bibot | Nome da Editora do Livro |	
| [[numero_da_edicao ]] | 1º(2020) | Número da Edição |	
| [[capitulo_da_leitura] | 3,6,2 | Número do Capitulo de Leitura |	
| [[avaliar_leitura]] |  1,2,3 | Quantidade de estrelas de avaliação que o livro possui |	
| [[selecionar_categoria]] | Literatura Infanto Juvenil, Ficção | Categoria do livro |	
| [[idioma_da_leitura]] | ingles, portugues | 	Idioma do livro |


<br />


- **Quando:** No clique no botao voltar do topo  <br />

- **Onde:** Acervo
     - **Titulo ou nome do botão:** Voltar
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:acervo',
    'eventAction': 'clique:voltar',
    'eventLabel': '[[nome_botao]]'
  
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  voltar | Deve retornar o nome do botao clicado. |


<br />


- **Quando:** No clique do menu dentro de um livro <br />

- **Onde:** Após o clique inicial em um livro
     - **Titulo ou nome do botão:** Conheca o Livro, Capitulos, Atividades e Livros Similares
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:acervo',
    'eventAction': 'clique:menu_livro',
    'eventLabel': '[[nome_menu]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_menu]] |  conheca_o_livro, capitulos, atividades, livros_similares | Deve retornar o nome do menu clicado. |


<br />


- **Quando:** No clique do menu Capitulos dentro de um livro <br />

- **Onde:** Após o clique inicial em um livro
     - **Titulo ou nome do botão:** Capa, Rosto, Creditos, Sumario, Apresentacao e etc
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:acervo',
    'eventAction': 'clique:capitulos_livro',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  capa, rosto, creditos, sumario, apresentacao, etc | Deve retornar o nome do botao clicado |


<br />



- **Quando:** No clique do menu Atividades dentro de um livro <br />

- **Onde:** Após o clique inicial em um livro
     - **Titulo ou nome do botão:** Começar
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:acervo',
    'eventAction': 'clique:atividades_livro',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  comecar | Deve retornar o nome do botao clicado. |


<br />


- **Quando:** No clique do menu Livros Similares dentro de um livro <br />

- **Onde:** Após o clique inicial em um livro
     - **Titulo ou nome do botão:** Mitos Gregos, Sardenta, Diario de uma Sonhadora e etc
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:acervo',
    'eventAction': 'clique:livros_similares',
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
| [[nome_botao]] | mitos_gregos, sardenta, diario_de_uma_sonhadora, etc | Deve retornar o nome do botao clicado. |
| [[titulo_do_livro]] | mitos_gregos | Nome do Livro Clicado |	
| [[quantidade_de_paginas]] | 35, 88 | Quantidade de Páginas do Livro |	
| [[nome_da_editora]] | Nova Fronteira Bibot | Nome da Editora do Livro |	
| [[numero_da_edicao ]] | 1º(2020) | Número da Edição |	
| [[capitulo_da_leitura] | 3,6,2 | Número do Capitulo de Leitura |	
| [[avaliar_leitura]] |  1,2,3 | Quantidade de estrelas de avaliação que o livro possui |	
| [[selecionar_categoria]] | Literatura Infanto Juvenil, Ficção | Categoria do livro |	
| [[idioma_da_leitura]] | ingles, portugues | 	Idioma do livro |


<br />


### Leituras


- **Quando:** No clique em aplicar dentro de livro <br />

- **Onde:** Leituras
     - **Titulo ou nome do botão:** Mitos Gregos, Sardenta, Aladin e etc
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:leituras',
    'eventAction': 'clique:livro',
    'eventLabel': 'aplicar:[[nome_livro]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_livro]] | sardenta\|aladin\|mitos_gregos  | Deve retornar o nome do filtro aplicado. |


<br />


- **Quando:** No clique em aplicar dentro de status <br />

- **Onde:** Leituras
     - **Titulo ou nome do botão:** Nao Lido, Lendo, Completa
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:leituras',
    'eventAction': 'clique:status',
    'eventLabel': 'aplicar:[[nome_status]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_status]] | nao_lido\|lendo\|completa | Deve retornar o nome do filtro aplicado. |


<br />


- **Quando:** No clique em Filtrar por palavra chave <br />

- **Onde:** Leituras
     - **Titulo ou nome do botão:** Aladin, Mitos Gregos, Os Sandubas, e etc
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:leituras',
    'eventAction': 'clique:filtrar_por_palavra_chave',
    'eventLabel': '[[valor_digitado]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[valor_digitado]] | aladin, mitos_gregos, os_sandubas, etc | Deve retornar o valor digitado |


<br />


- **Quando:** No clique no livro  <br />

- **Onde:** Leituras
     - **Titulo ou nome do botão:** Ler Agora e Adicionar aos Favoritos
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:leituras',
    'eventAction': 'clique:livro_acervo',
    'eventLabel': '[[nome_botao]]'
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
| [[nome_botao]] | ler_agora, adicionar_aos_favoritos | Deve retornar o nome do botao clicado. |	
| [[titulo_do_livro]] | mitos_gregos | Nome do Livro Clicado |	
| [[quantidade_de_paginas]] | 35, 88 | Quantidade de Páginas do Livro |	
| [[nome_da_editora]] | Nova Fronteira Bibot | Nome da Editora do Livro |	
| [[numero_da_edicao ]] | 1º(2020) | Número da Edição |	
| [[capitulo_da_leitura] | 3,6,2 | Número do Capitulo de Leitura |	
| [[avaliar_leitura]] |  1,2,3 | Quantidade de estrelas de avaliação que o livro possui |	
| [[selecionar_categoria]] | Literatura Infanto Juvenil, Ficção | Categoria do livro |	
| [[idioma_da_leitura]] | ingles, portugues | 	Idioma do livro |

<br />


- **Quando:** No clique no botao voltar do topo  <br />

- **Onde:** Leituras
     - **Titulo ou nome do botão:** Voltar
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:leituras',
    'eventAction': 'clique:voltar',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | voltar | Deve retornar o nome do botao clicados. |


<br />


### Atividades


- **Quando:** No clique em aplicar dentro de livro <br />

- **Onde:** Atividades
     - **Titulo ou nome do botão:** Sardenta, Aladin, Mitos Gregos, e etc
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:atividades',
    'eventAction': 'clique:livro',
    'eventLabel': 'aplicar:[[nome_livro]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_livro]] | sardenta\aladin\|mitos_gregos | Deve retornar o nome do filtro aplicado. |


<br />


- **Quando:** No clique em aplicar dentro de status <br />

- **Onde:** Atividades
     - **Titulo ou nome do botão:** Nao Lido, Lendo, Completa
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:atividades',
    'eventAction': 'clique:status',
    'eventLabel': 'aplicar:[[nome_status]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_status]] | nao_lido\lendo\|completa | Deve retornar o nome do filtro aplicado. |


<br />


- **Quando:** No clique em Filtrar por palavra chave <br />

- **Onde:** Atividades
     - **Titulo ou nome do botão:** Aladin, Mitos Gregos, Os Sandubas, e etc
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:atividades',
    'eventAction': 'clique:filtrar_por_palavra_chave',
    'eventLabel': '[[valor_digitado]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[valor_digitado]] | aladin, mitos_gregos, os_sandubas, etc | Deve retornar o valor digitado |


<br />


- **Quando:** No clique no botao voltar do topo <br />

- **Onde:** Atividades
     - **Titulo ou nome do botão:** Voltar
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:atividades',
    'eventAction': 'clique:voltar',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | voltar | Deve retornar o nome do botao clicados. |


<br />



- **Quando:** No clique das opções de começar a Atividade <br />

- **Onde:** Atividades
     - **Titulo ou nome do botão:** Começar
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:atividades',
    'eventAction': 'clique:atividades_livro',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | comecar | Deve retornar o nome do botao clicados. |


<br />


### Pontuação


- **Quando:** No clique no botao voltar do topo <br />

- **Onde:** Pontuação
     - **Titulo ou nome do botão:** Voltar
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:pontuacao',
    'eventAction': 'clique:voltar',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | voltar | Deve retornar o nome do botao clicados. |


<br />


- **Quando:** No clique na seleção de ano <br />

- **Onde:** Pontuação
     - **Titulo ou nome do botão:** 2021, 2020
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:pontuacao',
    'eventAction': 'clique:selecao_ano',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | 2021, 2020 | Deve retornar o nome do botao clicados. |


<br />


- **Quando:** No clique no botao "como ganhar mais pontos?" <br />

- **Onde:** Pontuação
     - **Titulo ou nome do botão:** Mais Pontos
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:pontuacao',
    'eventAction': 'clique:mais_pontos',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | mais_pontos | Deve retornar o nome do botao clicado. |


<br />



- **Quando:** No clique em minhas pontuações <br />

- **Onde:** Pontuação
     - **Titulo ou nome do botão:** Carregar Mais
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:pontuacao',
    'eventAction': 'clique:carregar_mais',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | carregar_mais | Deve retornar o nome do botao clicado |


<br />



### Ranking


- **Quando:** No clique no botao voltar <br />

- **Onde:** Ranking
     - **Titulo ou nome do botão:** Voltar
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:ranking',
    'eventAction': 'clique:voltar',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | voltar | Deve retornar o nome do botao clicado. |


<br />



### Medalhas


- **Quando:** No clique no botao voltar <br />

- **Onde:** Medalhas
     - **Titulo ou nome do botão:** Voltar
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:medalhas',
    'eventAction': 'clique:voltar',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | voltar | Deve retornar o nome do botao clicados. |


<br />



- **Quando:** No clique no botao voltar <br />

- **Onde:** Medalhas
     - **Titulo ou nome do botão:** Todas, Bloqueadas, Desbloqueadas
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot:medalhas',
    'eventAction': 'clique:selecao_medalha',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | todas, bloqueadas, desbloqueadas | Deve retornar o nome do botao clicados. |


<br />
