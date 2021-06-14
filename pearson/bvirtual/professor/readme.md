> Documento de Especificação Técnica

<br />

## Implementação da Camada de dados - Bibot Professor
Última atualização: 14/06/2021 <br />
Em caso de dúvidas, entrar em contato com: [thiago.ceccato@pearson.com](mailto:thiago.ceccato@pearson.com)

<br />

## Objetivo
Este documento tem como objetivo instruir a implementação da camada de dados e de data attributes para utilização de recursos de monitoramento do Google Analytics referentes ao ambiente de [URL Produção: https://professor.bibotdigital.com.br//](https://professor.bibotdigital.com.br//).

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
    - **Titulo ou nome do botão:** Ver Como Aluno, Meus Dados, Sair, Pearson Brasil
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:geral',
    'eventAction': 'clique:menu_header',
    'eventLabel': '[[nome_menu]]'
  
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_menu]] | ver_como_aluno, meus_dados, sair, pearson_brasil e etc | Deve retornar o nome do menu clicado. |

<br />




- **Quando:** No clique do Menu Lateral <br />
 
- **Onde:** Em todas as páginas
    - **Titulo ou nome do botão:** Bibot, Gestao do Acervo, Minhas Turmas, Resultados, e etc
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:geral',
    'eventAction': 'clique:menu_lateral',
    'eventLabel': '[[nome_menu]]'
    
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_menu]] | bibot, gestao_do_acervo, minhas_turmas, resultados, etc | Deve retornar o nome do menu clicado |

<br />



### Gestão do Acervo - Publicações


- **Quando:** No clique em Gestão de Acervo <br />
 
- **Onde:** Menu Lateral - Em todas as páginas
    - **Titulo ou nome do botão:** Publicacoes, Trilha do Conhecimento
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:publicacoes',
    'eventAction': 'clique:menu_lateral',
    'eventLabel': '[[nome_menu]]'
    
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_menu]] | publicacoes, trilha_do_conhecimento | Deve retornar o nome do menu clicado |

<br />




- **Quando:** No envio do valor digitado da caixa de pesquisa <br />
 
- **Onde:** Publicações
    - **Titulo ou nome do botão:** A Grande Corrida, Nicolas e a Lanterna, e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:publicacoes',
    'eventAction': 'preencheu:campo',
    'eventLabel': '[[valor_digitado]]'
    
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[valor_digitado]] | a_grande_corrida, nicolas_e_a_lanterna, frankenstein, etc | Deve retornar o nome do valor digitado |

<br />



- **Quando:** No clique na categoria desejada <br />
 
- **Onde:** Publicações
    - **Titulo ou nome do botão:** Todas, Conto, Crônica, Novela, e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:publicacoes',
    'eventAction': 'clique:categorias',
    'eventLabel': '[[nome_categoria]]'
    
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_categoria]] | todas, conto, crônica, novela, e etc | Deve retornar o nome da categoria selecionada |

<br />




- **Quando:** No clique na editora desejada <br />
 
- **Onde:** Publicações
    - **Titulo ou nome do botão:** Todas, Bicho Esperto, Pearson Readers e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:publicacoes',
    'eventAction': 'clique:editoras',
    'eventLabel': '[[nome_editora]]'
    
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_editora]] |  todas, bicho_esperto, pearson_readers e etc | Deve retornar o nome da editora selecionada |

<br />


- **Quando:**  No preenchimento da idade minima e máxima <br />
 
- **Onde:** Publicações
    - **Titulo ou nome do botão:** 1, 2, 13, 16 e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:publicacoes',
    'eventAction': 'preencheu:campo',
    'eventLabel': '[[valor_digitado]]'
    
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[valor_digitado]] |  1, 2, 13, 16 e etc | Deve retornar o nome do valor digitado |

<br />




- **Quando:** No clique das setas da idade minima e máxima <br />
 
- **Onde:** Publicações
    - **Titulo ou nome do botão:**  cima ou baixo

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:publicacoes',
    'eventAction': 'clique:setas',
    'eventLabel': 'setas:[cima|baixo]'
    
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [cima\|baixo] | [cima\|baixo] | Deve retornar o nome da seta clicada |

<br />



- **Quando:** No clique na data de inicio desejada <br />
 
- **Onde:** Publicações
    - **Titulo ou nome do botão:**   03, 05, 09, 18 e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:publicacoes',
    'eventAction': 'clique:data_de_inicio',
    'eventLabel': '[[nome_botao]]'
    
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  03, 05, 09, 18 e etc | Deve retornar o nome do botao clicado |

<br />



- **Quando:** No clique na data de fim desejada <br />
 
- **Onde:** Publicações
    - **Titulo ou nome do botão:**   Direita ou Esquerda

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:publicacoes',
    'eventAction': 'clique:data_de_fim',
    'eventLabel': 'setas:[direita|esquerda]'
    
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [direita\|esquerda] |  [direita\|esquerda] | Deve retornar o nome da seta clicada |

<br />



- **Quando:**  No clique no botão buscar <br />
 
- **Onde:** Publicações
    - **Titulo ou nome do botão:**   Buscar

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:publicacoes',
    'eventAction': 'clique:buscar',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  buscar | Deve retornar o nome do botao clicado |

<br />



- **Quando:**  No clique no botão Ler <br />
 
- **Onde:** Publicações
    - **Titulo ou nome do botão:**   Ler

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:publicacoes',
    'eventAction': 'clique:ler',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  ler | Deve retornar o nome do botao clicado |

<br />



- **Quando:**  No clique no botão Visualizar <br />
 
- **Onde:** Publicações
    - **Titulo ou nome do botão:**   Visualizar

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:publicacoes',
    'eventAction': 'clique:visualizar',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | visualizar | Deve retornar o nome do botao clicado |

<br />



- **Quando:**  No clique nos números para escolha de página <br />
 
- **Onde:** Publicações
    - **Titulo ou nome do botão:**    1, 2, 3, 4, >, >>, e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:publicacoes',
    'eventAction': 'clique:selecao_paginas',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  1, 2, 3, 4, >, >>, e etc | Deve retornar o nome do botao clicado |

<br />


- **Quando:**  No clique na capa do livro  <br />
 
- **Onde:** Publicações
    - **Titulo ou nome do botão:**   A grande corrida, Nicolas e a Lanterna, Frankenstein, etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:publicacoes',
    'eventAction': 'clique:livro',
    'eventLabel': '[[nome_livro]]'
    'dimension3': '[[titulo_do_livro]]',      
    'dimension4': '[[numero_do_livro]]',
    'dimension5': '[[nome_da_editora]]',
    'dimension6': '[[ano_da_edicao ]]',
    'dimension7': '[[quantidade_de_paginas]]',
    'dimension8': '[[faixa_etaria]]',
    'dimension9': '[[selecionar_categoria]]',
    'dimension10': '[[autor_do_livro]]',
    'dimension11': '[[URL]]',
    'dimension12': '[[numero_edicao]]',
    'dimension13': '[[idioma_da_leitura]]',
    'dimension14': '[[impressao_do_livro]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_livro]]   | a_grande_corrida, nicolas_e_a_lanterna, frankenstein, etc | Deve retornar o nome do livro clicado |
| [[titulo_do_livro]] | nicolas_e_a_lanterna | Nome do Livro Clicado |  
| [[numero_do_livro]] | 9786500190465 | ISBN | 
| [[nome_da_editora]] | Editora ABarros BIBOT | Nome da Editora| 
| [[ano_da_edicao ]] | 2021,2020 | Ano | 
| [[quantidade_de_paginas]] | 40,50,45 | Quantidade de Páginas | 
| [[faixa_etaria]] | a_partir_de_13 anos | Faixa Etária Indicativa | 
| [[selecionar_categoria]] | Literatura Infanto Juvenil, Ficção | Categoria do livro |
| [[autor_do_livro]] |  gabriel_verçoza | Nome do Autor do Livro | 
| [[URL]] |  https://www.abarroseditora.com | URL Compra | 
| [[numero_edicao]] |  1ª,2ª,3ª | Número da Edição |   
| [[idioma_da_leitura]] | ingles, portugues |   Idioma do livro |
| [[impressao_do_livro]] | nao, sim| Livro impresso? | 


<br />



- **Quando:**  Após o clique na capa do livro  <br />
 
- **Onde:** Publicações
    - **Titulo ou nome do botão:**  Publicações, Gestao de acervo e etc 

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:publicacoes',
    'eventAction': 'clique:atalhos_topo',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | publicações, gestao_de_acervo e etc  | Deve retornar o nome do botao clicado |

<br />




- **Quando:**  Após o clique na capa do livro  <br />
 
- **Onde:** Publicações
    - **Titulo ou nome do botão:**    https://www.abarroseditora.com , https://www.parabole.com.br/home/conversas-com-versos e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:publicacoes',
    'eventAction': 'clique:url_compra',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | https://www.abarroseditora.com , https://www.parabole.com.br/home/conversas-com-versos e etc | Deve retornar o nome do botao clicado |

<br />



- **Quando:**  No clique para visualizar PDF do livro  <br />
 
- **Onde:** Publicações
    - **Titulo ou nome do botão:**   visualizar

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:publicacoes',
    'eventAction': 'clique:visualizar_pdf',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  visualizar | Deve retornar o nome do botao clicado  |


<br />

- **Quando:**  No clique em Habilidades Avaliadas  <br />
 
- **Onde:** Publicações
    - **Titulo ou nome do botão:**   Voltar, Simular Atividades

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:publicacoes',
    'eventAction': 'clique:habilidades_avaliadas',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  voltar, simular_atividades | Deve retornar o nome do botao clicado  |

<br />


### Gestão do Acervo - Trilha do Conhecimento


- **Quando:**  No envio do valor digitado da caixa de pesquisa  <br />
 
- **Onde:** Trilha do Conhecimento
    - **Titulo ou nome do botão:**   A grande corrida, Nicolas e a Lanterna, Frankenstein, etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:trilha_do_conhecimento',
    'eventAction': 'preencheu:campo',
    'eventLabel': '[[valor_digitado]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[valor_digitado]] |   a_grande_corrida, nicolas_e_a_lanterna, frankenstein, etc | Deve retornar o nome do valor digitado |

<br />



- **Quando:**  No clique na série desejada  <br />
 
- **Onde:** Trilha do Conhecimento
    - **Titulo ou nome do botão:**   1º ano, 2º ano, Infantil 1 e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:trilha_do_conhecimento',
    'eventAction': 'clique:series',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  1º_ano, 2º_ano, infantil_1 e etc | Deve retornar o nome do botao clicado  |

<br />


- **Quando:**  No envio do valor digitado da caixa de ano letivo  <br />
 
- **Onde:** Trilha do Conhecimento
    - **Titulo ou nome do botão:**   2020, 2018, 2019 e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:trilha_do_conhecimento',
    'eventAction': 'preencheu:campo',
    'eventLabel': '[[valor_digitado]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[valor_digitado]] |   2020, 2018, 2019 e etc | Deve retornar o nome do valor digitado  |

<br />


- **Quando:**  No clique da sugestão de ano letivo  <br />
 
- **Onde:** Trilha do Conhecimento
    - **Titulo ou nome do botão:**   2020, 2018, 2019 e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:trilha_do_conhecimento',
    'eventAction': 'clique:ano_letivo',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |   2020, 2018, 2019 e etc | Deve retornar o nome do botao clicado  |

<br />


- **Quando:**  No clique em buscar <br />
 
- **Onde:** Trilha do Conhecimento
    - **Titulo ou nome do botão:**   buscar

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:trilha_do_conhecimento',
    'eventAction': 'clique:buscar',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  buscar | Deve retornar o nome do botao clicado  |

<br />



- **Quando:**  No clique dos atalhos no topo <br />
 
- **Onde:** Trilha do Conhecimento
    - **Titulo ou nome do botão:**  Trilha do Conhecimento, Gestao de Acervo e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:trilha_do_conhecimento',
    'eventAction': 'clique:atalhos_topo',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  trilha_do_conhecimento, gestao_de_acervo e etc | Deve retornar o nome do botao clicado  |

<br />


- **Quando:**  No clique dos atalhos das pesquisas realizadas <br />
 
- **Onde:** Trilha do Conhecimento
    - **Titulo ou nome do botão:**  Visualizar, Excluir, Editar e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:trilha_do_conhecimento',
    'eventAction': 'clique:atalhos_trilha',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  visualizar, excluir, editar e etc | Deve retornar o nome do botao clicado  |

<br />



- **Quando:**  No clique dentro de editar trilha  <br />
 
- **Onde:** Trilha do Conhecimento
    - **Titulo ou nome do botão:**  Voltar, Enviar

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:trilha_do_conhecimento',
    'eventAction': 'clique:editar',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  voltar, enviar | Deve retornar o nome do botao clicado  |

<br />


- **Quando:**  No clique dentro de visualizar trilha  <br />
 
- **Onde:** Trilha do Conhecimento
    - **Titulo ou nome do botão:**  Voltar, Editar, Adicionar um livro

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:trilha_do_conhecimento',
    'eventAction': 'clique:visualizar',
    'eventLabel': '[[nome_botao]]',
    'dimension3': '[[titulo_do_livro]]',      
    'dimension4': '[[numero_do_livro]]',
    'dimension5': '[[nome_da_editora]]',
    'dimension6': '[[ano_da_edicao ]]',
    'dimension7': '[[quantidade_de_paginas]]',
    'dimension8': '[[faixa_etaria]]',
    'dimension9': '[[selecionar_categoria]]',
    'dimension10': '[[autor_do_livro]]',
    'dimension11': '[[URL]]',
    'dimension12': '[[numero_edicao]]',
    'dimension13': '[[idioma_da_leitura]]',
    'dimension14': '[[impressao_do_livro]]',
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]]   | voltar, editar, adicionar_um_livro | Deve retornar o nome do botao clicado |
| [[titulo_do_livro]] | nicolas_e_a_lanterna | Nome do Livro Clicado |  
| [[numero_do_livro]] | 9786500190465 | ISBN | 
| [[nome_da_editora]] | Editora ABarros BIBOT | Nome da Editora| 
| [[ano_da_edicao ]] | 2021,2020 | Ano | 
| [[quantidade_de_paginas]] | 40,50,45 | Quantidade de Páginas | 
| [[faixa_etaria]] | a_partir_de_13 anos | Faixa Etária Indicativa | 
| [[selecionar_categoria]] | Literatura Infanto Juvenil, Ficção | Categoria do livro |
| [[autor_do_livro]] |  gabriel_verçoza | Nome do Autor do Livro | 
| [[URL]] |  https://www.abarroseditora.com | URL Compra | 
| [[numero_edicao]] |  1ª,2ª,3ª | Número da Edição |   
| [[idioma_da_leitura]] | ingles, portugues |   Idioma do livro |
| [[impressao_do_livro]] | nao, sim| Livro impresso? | 


<br />



### Minhas Turmas - Alunos



- **Quando:**  No envio do valor digitado da caixa de pesquisa <br />
 
- **Onde:** Minhas Turmas - Alunos
    - **Titulo ou nome do botão:**   Ariane, Airton, clayton@tocalivros.com, e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:minhas_turmas_alunos',
    'eventAction': 'preencheu:campo',
    'eventLabel': '[[valor_digitado]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[valor_digitado]] |  ariane, airton, clayton@tocalivros.com, e etc | Deve retornar o nome do valor digitado  |

<br />



- **Quando:** Na seleção dentro de Segmentos <br />
 
- **Onde:** Minhas Turmas - Alunos
    - **Titulo ou nome do botão:**   Anos finais, Ensino infantil, Anos iniciais, e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:minhas_turmas_alunos',
    'eventAction': 'clique:segmento',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  anos_finais, ensino_infantil, anos_iniciais, e etc | Deve retornar o nome do botao clicado  |

<br />


- **Quando:** Na seleção dentro de Série <br />
 
- **Onde:** Minhas Turmas - Alunos
    - **Titulo ou nome do botão:**   6º ano, 7º ano e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:minhas_turmas_alunos',
    'eventAction': 'clique:serie',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  6º_ano, 7º_ano e etc | Deve retornar o nome do botao clicado  |

<br />


- **Quando:** Na seleção dentro de Série <br />
 
- **Onde:** Minhas Turmas - Alunos
    - **Titulo ou nome do botão:**   Todas 

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:minhas_turmas_alunos',
    'eventAction': 'clique:turma',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |   todas  | Deve retornar o nome do botao clicado  |

<br />


- **Quando:** No clique em Status dos perfis  <br />
 
- **Onde:** Minhas Turmas - Alunos
    - **Titulo ou nome do botão:**   Fechar, x, Sim, eu quero

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:minhas_turmas_alunos',
    'eventAction': 'clique:status',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |   fechar, x, sim_eu_quero | Deve retornar o nome do botao clicado  |

<br />



- **Quando:** No clique em visualizar dos perfis  <br />
 
- **Onde:** Minhas Turmas - Alunos
    - **Titulo ou nome do botão:**   Fechar, x, Sim, eu quero

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:minhas_turmas_alunos',
    'eventAction': 'clique:visualizar',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  visualizar | Deve retornar o nome do botao clicado  |

<br />



- **Quando:**  No clique no menu  <br />
 
- **Onde:** Dentro de Visualizar Perfis em Minhas Turmas - Alunos
    - **Titulo ou nome do botão:**   Geral, Historico de leitura, Dados do usuario e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:minhas_turmas_alunos',
    'eventAction': 'clique:menu',
    'eventLabel': '[[nome_menu]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_menu]] |  geral, historico_de_leitura, dados_do_usuario e etc | Deve retornar o nome do menu clicado  |

<br />


- **Quando:**  No clique em geral no menu   <br />
 
- **Onde:** Dentro de Visualizar Perfis em Minhas Turmas - Alunos
    - **Titulo ou nome do botão:**   Voltar

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:minhas_turmas_alunos',
    'eventAction': 'clique:geral',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  voltar | Deve retornar o nome do botao clicado  |

<br />


- **Quando:**  No clique em histórico de leituras no menu   <br />
 
- **Onde:** Dentro de Visualizar Perfis em Minhas Turmas - Alunos
    - **Titulo ou nome do botão:**   Voltar, Pesquisar Historico de Leitura

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:minhas_turmas_alunos',
    'eventAction': 'clique:historico_de_leituras',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  voltar, pesquisar_historico_de_leitura | Deve retornar o nome do botao clicado  |

<br />


- **Quando:**  No clique em habilidades leitoras no menu   <br />
 
- **Onde:** Dentro de Visualizar Perfis em Minhas Turmas - Alunos
    - **Titulo ou nome do botão:**   Voltar 

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:minhas_turmas_alunos',
    'eventAction': 'clique:habilidades_leitoras',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  voltar  | Deve retornar o nome do botao clicado  |

<br />


- **Quando:**  No clique em atividades no menu  <br />
 
- **Onde:** Dentro de Visualizar Perfis em Minhas Turmas - Alunos
    - **Titulo ou nome do botão:**   Voltar 

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:minhas_turmas_alunos',
    'eventAction': 'clique:atividades',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  voltar  | Deve retornar o nome do botao clicado  |

<br />


- **Quando:**  No clique em dados do usuário no menu   <br />
 
- **Onde:** Dentro de Visualizar Perfis em Minhas Turmas - Alunos
    - **Titulo ou nome do botão:**   Voltar, Resetar senha e etc 

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:minhas_turmas_alunos',
    'eventAction': 'clique:dados_do_usuario',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  voltar, resetar_senha e etc  | Deve retornar o nome do botao clicado  |

<br />

- **Quando:**  No clique dos atalhos no topo  <br />
 
- **Onde:**  Minhas Turmas - Alunos
    - **Titulo ou nome do botão:**  Minhas turmas, Alunos e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:minhas_turmas_alunos',
    'eventAction': 'clique:atalhos_topo',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  minhas_turmas, alunos e etc | Deve retornar o nome do botao clicado  |

<br />



### Minhas Turmas - Atividades


- **Quando:**  No clique dos atalhos no topo   <br />
 
- **Onde:** Minhas Turmas - Atividades
    - **Titulo ou nome do botão:**  Minhas turmas, Alunos e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:minhas_turmas_atividades',
    'eventAction': 'clique:atalhos_topo',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  minhas_turmas, alunos e etc | Deve retornar o nome do botao clicado  |

<br />


- **Quando:**  No envio do valor digitado da caixa de pesquisa   <br />
 
- **Onde:**  Minhas Turmas - Atividades
    - **Titulo ou nome do botão:**   A volta ao mundo em 80 dias, 365 historias, e etc 

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:minhas_turmas_atividades',
    'eventAction': 'preencheu:campo',
    'eventLabel': '[[valor_digitado]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[valor_digitado]] |  a_volta_ao_mundo_em_80_dias, 365_historias, e etc | Deve retornar o nome do valor digitado  |

<br />


- **Quando:**  Na seleção dentro de Habilidades  <br />
 
- **Onde:**  Minhas Turmas - Atividades
    - **Titulo ou nome do botão:**   Todos, Identificar a ideia central do texto, Analisar a modalizacao epistemica

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:minhas_turmas_atividades',
    'eventAction': 'clique:habilidades',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  todos, identificar_a_ideia_central_do_texto, analisar_a_modalizacao_epistemica | Deve retornar o nome do botao clicado  |

<br />


- **Quando:**  No clique no botão buscar  <br />
 
- **Onde:**  Minhas Turmas - Atividades
    - **Titulo ou nome do botão:**   Buscar

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:minhas_turmas_atividades',
    'eventAction': 'clique:buscar',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  buscar | Deve retornar o nome do botao clicado  |

<br />


- **Quando:**  No clique em visualizar livro  <br />
 
- **Onde:**  Minhas Turmas - Atividades
    - **Titulo ou nome do botão:**   Visualizar

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:minhas_turmas_atividades',
    'eventAction': 'clique:visualizar',
    'eventLabel': '[[nome_botao]]',
    'dimension3': '[[titulo_do_livro]]',      
    'dimension4': '[[numero_do_livro]]',
    'dimension5': '[[nome_da_editora]]',
    'dimension6': '[[ano_da_edicao ]]',
    'dimension7': '[[quantidade_de_paginas]]',
    'dimension8': '[[faixa_etaria]]',
    'dimension9': '[[selecionar_categoria]]',
    'dimension10': '[[autor_do_livro]]',
    'dimension11': '[[URL]]',
    'dimension12': '[[numero_edicao]]',
    'dimension13': '[[idioma_da_leitura]]',
    'dimension14': '[[impressao_do_livro]]',
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]]   | visualizar | Deve retornar o nome do botao clicado |
| [[titulo_do_livro]] | nicolas_e_a_lanterna | Nome do Livro Clicado |  
| [[numero_do_livro]] | 9786500190465 | ISBN | 
| [[nome_da_editora]] | Editora ABarros BIBOT | Nome da Editora| 
| [[ano_da_edicao ]] | 2021,2020 | Ano | 
| [[quantidade_de_paginas]] | 40,50,45 | Quantidade de Páginas | 
| [[faixa_etaria]] | a_partir_de_13 anos | Faixa Etária Indicativa | 
| [[selecionar_categoria]] | Literatura Infanto Juvenil, Ficção | Categoria do livro |
| [[autor_do_livro]] |  gabriel_verçoza | Nome do Autor do Livro | 
| [[URL]] |  https://www.abarroseditora.com | URL Compra | 
| [[numero_edicao]] |  1ª,2ª,3ª | Número da Edição |   
| [[idioma_da_leitura]] | ingles, portugues |   Idioma do livro |
| [[impressao_do_livro]] | nao, sim| Livro impresso? | 


<br />


- **Quando:**  No clique no atalho de Simular  <br />
 
- **Onde:**  Dentro de Visualizar Livro
    - **Titulo ou nome do botão:**   Simular, Voltar

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:minhas_turmas_atividades',
    'eventAction': 'clique:simular',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  simular, voltar | Deve retornar o nome do botao clicado  |

<br />


- **Quando:**  No clique nos exercicios  <br />
 
- **Onde:**  Dentro de Visualizar Livro
    - **Titulo ou nome do botão:**   Sair do simulado, Verificar

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:minhas_turmas_atividades',
    'eventAction': 'clique:exercicios',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  sair_do_simulado, verificar | Deve retornar o nome do botao clicado  |

<br />


### Minhas Turmas - Turmas


- **Quando:**  No clique dos atalhos no topo   <br />
 
- **Onde:** Minhas Turmas - Turmas
    - **Titulo ou nome do botão:**  Minhas turmas, Turmas e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:minhas_turmas_turmas',
    'eventAction': 'clique:atalhos_topo',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  minhas_turmas, turmas e etc | Deve retornar o nome do botao clicado  |

<br />


- **Quando:**  No valor digitado de pesquisa de turma  <br />
 
- **Onde:** Minhas Turmas - Turmas
    - **Titulo ou nome do botão:**  5º ano, 6º ano e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:minhas_turmas_turmas',
    'eventAction': 'preencheu:campo',
    'eventLabel': '[[valor_digitado]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[valor_digitado]] |  5º_ano, 6º_ano e etc | Deve retornar o nome do valor digitado  |

<br />


- **Quando:**  No valor digitado de pesquisa de ano  <br />
 
- **Onde:** Minhas Turmas - Turmas
    - **Titulo ou nome do botão:**  2021, 2020, 2019 e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:minhas_turmas_turmas',
    'eventAction': 'preencheu:ano_turma',
    'eventLabel': '[[valor_digitado]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[valor_digitado]] |  2021, 2020, 2019 e etc | Deve retornar o nome do valor digitado  |

<br />


- **Quando:**  Na seleção dentro de Série  <br />
 
- **Onde:** Minhas Turmas - Turmas
    - **Titulo ou nome do botão:**  6º ano - Anos finais, 7º ano - Anos finais e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:minhas_turmas_turmas',
    'eventAction': 'clique:serie',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  6º_ano_anos_finais, 7º_ano_anos_finais e etc | Deve retornar o nome do botao clicado  |

<br />


- **Quando:**  No clique no botão buscar  <br />
 
- **Onde:** Minhas Turmas - Turmas
    - **Titulo ou nome do botão:**  Buscar

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:minhas_turmas_turmas',
    'eventAction': 'clique:buscar',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  buscar | Deve retornar o nome do botao clicado  |

<br />


- **Quando:**  No clique na seleção de páginas  <br />
 
- **Onde:** Minhas Turmas - Turmas
    - **Titulo ou nome do botão:**  Buscar

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:minhas_turmas_turmas',
    'eventAction': 'clique:selecao_paginas',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  1,2,3 e etc | Deve retornar o nome do botao clicado  |

<br />


- **Quando:**  No clique das setas da seleção de páginas  <br />
 
- **Onde:** Minhas Turmas - Turmas
    - **Titulo ou nome do botão:**  Esquerda, Direita

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:minhas_turmas_turmas',
    'eventAction': 'clique:setas',
    'eventLabel': 'setas:[esquerda|direita]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [esquerda\|direita]  |  [esquerda\|direita]  | Deve retornar o nome da seta clicada  |

<br />  


- **Quando:**  No clique em visualizar turma  <br />
 
- **Onde:** Minhas Turmas - Turmas
    - **Titulo ou nome do botão:**  Visualizar

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:minhas_turmas_turmas',
    'eventAction': 'clique:visualizar',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]]  |  visualizar  | Deve retornar o nome do botao clicado  |

<br />  


- **Quando:**  No clique no menu  <br />
 
- **Onde:** Dentro de visualizar turma
    - **Titulo ou nome do botão:**  Geral, Habilidades leitoras, Alunos da turma

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:minhas_turmas_turmas',
    'eventAction': 'clique:menu',
    'eventLabel': '[[nome_menu]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_menu]]  |  geral, habilidades_leitoras, alunos_da_turma | Deve retornar o nome do menu clicado  |

<br />  



- **Quando:**  No clique no menu geral  <br />
 
- **Onde:** Dentro de visualizar turma
    - **Titulo ou nome do botão:**  Voltar

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:minhas_turmas_turmas',
    'eventAction': 'clique:voltar',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | voltar | Deve retornar o nome do botao clicado  |

<br />  


- **Quando:**  No clique no menu geral  <br />
 
- **Onde:** Dentro de visualizar turma
    - **Titulo ou nome do botão:**  Voltar

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:minhas_turmas_turmas',
    'eventAction': 'clique:habilidades_leitoras',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | voltar | Deve retornar o nome do botao clicado  |

<br />  


- **Quando:**  No clique no menu geral  <br />
 
- **Onde:** Dentro de visualizar turma
    - **Titulo ou nome do botão:**  Voltar, Visualizar

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:minhas_turmas_turmas',
    'eventAction': 'clique:alunos_da_turma',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | voltar, visualizar | Deve retornar o nome do botao clicado  |

<br />  



### Resultados - Ranking da Escola

- **Quando:**  No clique dos atalhos no topo  <br />
 
- **Onde:** Resultados - Ranking da Escola
    - **Titulo ou nome do botão:**  Resultados, ranking da escola e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:resultados_escola',
    'eventAction': 'clique:atalhos_topo',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | resultados, ranking_da_escola e etc | Deve retornar o nome do botao clicado  |

<br />  


- **Quando:**  Na seleção dentro de Série  <br />
 
- **Onde:** Resultados - Ranking da Escola
    - **Titulo ou nome do botão:**  2020, 2021, e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:resultados_escola',
    'eventAction': 'clique:selecao_ano',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | 2020, 2021, e etc | Deve retornar o nome do botao clicado  |

<br />  


- **Quando:**  No clique no botão buscar  <br />
 
- **Onde:** Resultados - Ranking da Escola
    - **Titulo ou nome do botão:**  Buscar

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:resultados_escola',
    'eventAction': 'clique:buscar',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | buscar | Deve retornar o nome do botao clicado  |

<br />  


### Resultados - Ranking Estadual


- **Quando:**  No clique dos atalhos no topo  <br />
 
- **Onde:** Resultados - Ranking Estadual
    - **Titulo ou nome do botão:**  Resultados, ranking estadual e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_bibot_professor:ranking_estadual',
    'eventAction': 'clique:atalhos_topo',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | resultados, ranking_estadual e etc | Deve retornar o nome do botao clicado  |

<br />  


- **Quando:**  Na seleção dentro de Série  <br />
 
- **Onde:** Resultados - Ranking Estadual
    - **Titulo ou nome do botão:**  2020, 2021, e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:ranking_estadual',
    'eventAction': 'clique:selecao_ano',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | 2020, 2021, e etc | Deve retornar o nome do botao clicado  |

<br />  


- **Quando:**  No clique no botão buscar  <br />
 
- **Onde:** Resultados - Ranking Estadual
    - **Titulo ou nome do botão:**  Buscar

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:ranking_estadual',
    'eventAction': 'clique:buscar',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | buscar | Deve retornar o nome do botao clicado  |

<br />  


### Resultados - Ranking Nacional


- **Quando:**  No clique dos atalhos no topo  <br />
 
- **Onde:** Resultados - Ranking Nacional
    - **Titulo ou nome do botão:**  Resultados, ranking nacional e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:ranking_nacional',
    'eventAction': 'clique:atalhos_topo',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | resultados, ranking_nacional e etc | Deve retornar o nome do botao clicado  |

<br />  


- **Quando:**  Na seleção dentro de Série  <br />
 
- **Onde:** Resultados - Ranking Nacional
    - **Titulo ou nome do botão:**  2020, 2021, e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:ranking_nacional',
    'eventAction': 'clique:selecao_ano',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | 2020, 2021, e etc | Deve retornar o nome do botao clicado  |

<br />  


- **Quando:**  No clique no botão buscar  <br />
 
- **Onde:** Resultados - Ranking Nacional
    - **Titulo ou nome do botão:**  Buscar

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:ranking_nacional',
    'eventAction': 'clique:buscar',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | buscar | Deve retornar o nome do botao clicado  |

<br />  



### Contato - Enviar Mensagens


- **Quando:**  No clique dos atalhos no topo  <br />
 
- **Onde:** Contato - Enviar Mensagens  
    - **Titulo ou nome do botão:**  contato, enviar mensagens, cadastrar

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:contato_enviar_mensagens',
    'eventAction': 'clique:atalhos_topo',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | contato, enviar_mensagens, cadastrar | Deve retornar o nome do botao clicado  |

<br />  


- **Quando:**  No clique em Cadastro  <br />
 
- **Onde:** Dentro do atalho Cadastrar 
    - **Titulo ou nome do botão:**  enviar, voltar

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:contato_enviar_mensagens',
    'eventAction': 'clique:cadastro',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | enviar, voltar | Deve retornar o nome do botao clicado  |

<br />  


- **Quando:**  No valor digitado de pesquisa por título  <br />
 
- **Onde:** Contato - Enviar Mensagens
    - **Titulo ou nome do botão:**  

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:contato_enviar_mensagens',
    'eventAction': 'preencheu:campo',
    'eventLabel': '[[valor_digitado]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[valor_digitado]] |   | Deve retornar o nome do valor digitado  |

<br />  


- **Quando:**  Na seleção dentro de turmas  <br />
 
- **Onde:** Contato - Enviar Mensagens
    - **Titulo ou nome do botão:**  bko-2021, eficacia_2021, projeto-2021, e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:contato_enviar_mensagens',
    'eventAction': 'clique:turmas',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]]| bko-2021, eficacia_2021, projeto-2021, e etc  | Deve retornar o nome do botao clicado  |

<br />  


- **Quando:**  Na seleção dentro de status  <br />
 
- **Onde:** Contato - Enviar Mensagens
    - **Titulo ou nome do botão:**  todos, ativo, inativo

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:contato_enviar_mensagens',
    'eventAction': 'clique:status',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | todos, ativo, inativo | Deve retornar o nome do botao clicado  |

<br />  


- **Quando:**  No clique no botão buscar <br />
 
- **Onde:** Contato - Enviar Mensagens
    - **Titulo ou nome do botão:**  buscar

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:contato_enviar_mensagens',
    'eventAction': 'clique:buscar',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | buscar | Deve retornar o nome do botao clicado  |

<br />  



### Contato - Entrar em Contato

- **Quando:**  No clique dos atalhos no topo <br />
 
- **Onde:** Contato - Entrar em Contato
    - **Titulo ou nome do botão:**  Contato, entrar em contato, e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:entrar_contato',
    'eventAction': 'clique:atalhos_topo',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  contato, entrar_em_contato, e etc | Deve retornar o nome do botao clicado  |

<br />  


- **Quando:**  Na seleção dentro de status <br />
 
- **Onde:** Contato - Entrar em Contato
    - **Titulo ou nome do botão:**  informacoes, sugestoes, elogios, outros, e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:entrar_contato',
    'eventAction': 'clique:setor',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  informacoes, sugestoes, elogios, outros, e etc | Deve retornar o nome do botao clicado  |

<br />  

- **Quando:**  No clique no botão final <br />
 
- **Onde:** Contato - Entrar em Contato
    - **Titulo ou nome do botão:**  voltar, enviar

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:entrar_contato',
    'eventAction': 'clique:botao',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  voltar, enviar | Deve retornar o nome do botao clicado  |

<br />  


### Contato - Manual da Plataforma

- **Quando:**   No clique dos atalhos no topo <br />
 
- **Onde:** Contato - Manual da Plataforma
    - **Titulo ou nome do botão:**  contato, manual da plataforma, e etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:manual_plataforma',
    'eventAction': 'clique:atalhos_topo',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  contato, manual_da_plataforma, e etc | Deve retornar o nome do botao clicado  |

<br />  


- **Quando:** No clique no botao de download  <br />
 
- **Onde:** Contato - Manual da Plataforma
    - **Titulo ou nome do botão:**  Fazer Download

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:manual_plataforma',
    'eventAction': 'clique:download',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | fazer_download | Deve retornar o nome do botao clicado  |

<br />  


### Dashboard e Relatórios - Pontuação da Turma


- **Quando:** No clique dos atalhos no topo  <br />
 
- **Onde:** Dashboards - Pontuação da Turma
    - **Titulo ou nome do botão:**  dashboards e relatorios,  pontuação da turma, e etc
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:pontuacao_turma',
    'eventAction': 'clique:atalhos_topo',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | dashboards_e_relatorios,  pontuação_da_turma, e etc | Deve retornar o nome do botao clicado  |

<br />  


- **Quando:** Na seleção de séries  <br />
 
- **Onde:** Dashboards - Pontuação da Turma
    - **Titulo ou nome do botão:**   Todas, 5º ano, 6º ano, e etc
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:pontuacao_turma',
    'eventAction': 'clique:series',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  todas, 5º_ano, 6º_ano, e etc | Deve retornar o nome do botao clicado  |

<br />  


- **Quando:** Na seleção de turmas  <br />
 
- **Onde:** Dashboards - Pontuação da Turma
    - **Titulo ou nome do botão:**   Todas, bko-2021, projeto-2021, e etc
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:pontuacao_turma',
    'eventAction': 'clique:turmas',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  todas, bko-2021, projeto-2021, e etc | Deve retornar o nome do botao clicado  |

<br />  


- **Quando:** Na seleção de ano  <br />
 
- **Onde:** Dashboards - Pontuação da Turma
    - **Titulo ou nome do botão:**   2021, 2020, e etc
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:pontuacao_turma',
    'eventAction': 'clique:selecao_ano',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | 2021, 2020, e etc | Deve retornar o nome do botao clicado  |

<br />  


- **Quando:** No clique no botão Solicitar Relatório <br />
 
- **Onde:** Dashboards - Pontuação da Turma
    - **Titulo ou nome do botão:**   Solicitar Relatorio
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:pontuacao_turma',
    'eventAction': 'clique:solicitar',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | solicitar_relatorio | Deve retornar o nome do botao clicado  |

<br />  



### Dashboard e Relatórios - Relatório de Alternativas Respondidas


- **Quando:** No clique dos atalhos no topo  <br />
 
- **Onde:** Dashboard e Relatórios - Relatório de Alternativas Respondidas
    - **Titulo ou nome do botão:**  Dashboards e Relatorios,  Relatorios de Alternativas Respondidas, e etc
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:relatorios_alternativas',
    'eventAction': 'clique:atalhos_topo',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | dashboards_e_relatorios,  relatorios_de_alternativas_respondidas, e etc | Deve retornar o nome do botao clicado  |

<br />  


- **Quando:** No valor digitado de pesquisa por título  <br />
 
- **Onde:** Dashboard e Relatórios - Relatório de Alternativas Respondidas
    - **Titulo ou nome do botão:**  A Volta ao Mundo em 80 dias, 100 textos de Historia Antiga, e etc
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:relatorios_alternativas',
    'eventAction': 'preencheu:campo',
    'eventLabel': '[[valor_digitado]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[valor_digitado]] | a_volta_ao_mundo_em_80_dias, 100_textos_de_historia_antiga, e etc | Deve retornar o nome do botao clicado  |

<br />  


- **Quando:**  Na seleção de turma  <br />
 
- **Onde:** Dashboard e Relatórios - Relatório de Alternativas Respondidas
    - **Titulo ou nome do botão:**  todas, bko-2021, projeto-2021, e etc
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:relatorios_alternativas',
    'eventAction': 'clique:selecao_turma',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | todas, bko-2021, projeto-2021, e etc | Deve retornar o nome do botao clicado  |

<br />  


- **Quando:**  No clique no botão visualizar  <br />
 
- **Onde:** Dashboard e Relatórios - Relatório de Alternativas Respondidas
    - **Titulo ou nome do botão:**   Visualizar
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:relatorios_alternativas',
    'eventAction': 'clique:visualizar',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  visualizar | Deve retornar o nome do botao clicado  |

<br />  



- **Quando:**  No clique no botão buscar  <br />
 
- **Onde:** Dashboard e Relatórios - Relatório de Alternativas Respondidas
    - **Titulo ou nome do botão:**   Buscar
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:relatorios_alternativas',
    'eventAction': 'clique:buscar',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  buscar | Deve retornar o nome do botao clicado  |

<br />  



### Dashboard e Relatórios - Relatório dos Registros de Resposta



- **Quando:** No clique dos atalhos no topo  <br />
 
- **Onde:** Dashboard e Relatórios - Relatório dos Registros de Resposta
    - **Titulo ou nome do botão:**  Dashboards e Relatorios,  Relatorios dos Registros de Pesquisa, e etc
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:relatorios_registros',
    'eventAction': 'clique:atalhos_topo',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | dashboards_e_relatorios,  relatorios_dos_registros_de_pesquisa, e etc | Deve retornar o nome do botao clicado  |

<br />  


- **Quando:** No valor digitado de pesquisa por título  <br />
 
- **Onde:** Dashboard e Relatórios - Relatório dos Registros de Resposta
    - **Titulo ou nome do botão:**  A Volta ao Mundo em 80 dias, 100 textos de Historia Antiga, e etc
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:relatorios_registros',
    'eventAction': 'preencheu:campo',
    'eventLabel': '[[valor_digitado]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[valor_digitado]] | a_volta_ao_mundo_em_80_dias, 100_textos_de_historia_antiga, e etc | Deve retornar o nome do botao clicado  |

<br />  


- **Quando:**  Na seleção de turma  <br />
 
- **Onde:** Dashboard e Relatórios - Relatório dos Registros de Resposta
    - **Titulo ou nome do botão:**  todas, bko-2021, projeto-2021, e etc
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:relatorios_registros',
    'eventAction': 'clique:selecao_turma',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | todas, bko-2021, projeto-2021, e etc | Deve retornar o nome do botao clicado  |

<br />  


- **Quando:**  No clique no botão visualizar  <br />
 
- **Onde:** Dashboard e Relatórios - Relatório dos Registros de Resposta
    - **Titulo ou nome do botão:**   Visualizar
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:relatorios_registros',
    'eventAction': 'clique:visualizar',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  visualizar | Deve retornar o nome do botao clicado  |

<br />  



- **Quando:**  No clique no botão buscar  <br />
 
- **Onde:** Dashboard e Relatórios - Relatório dos Registros de Resposta
    - **Titulo ou nome do botão:**   Buscar
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:relatorios_registros',
    'eventAction': 'clique:buscar',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  buscar | Deve retornar o nome do botao clicado  |

<br /> 


### Dashboard e Relatórios - Relatório de desempenho consolidado



- **Quando:** No clique dos atalhos no topo  <br />
 
- **Onde:** Dashboard e Relatórios - Relatório de desempenho consolidado
    - **Titulo ou nome do botão:**  Dashboards e Relatorios,  Relatorios de Desempenho Consolidado, e etc
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:relatorios_desempenho',
    'eventAction': 'clique:atalhos_topo',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | dashboards_e_relatorios,  relatorios_de_desempenho_consolidado, e etc| Deve retornar o nome do botao clicado  |

<br />  


- **Quando:** No valor digitado de pesquisa por título  <br />
 
- **Onde:** Dashboard e Relatórios - Relatório de desempenho consolidado
    - **Titulo ou nome do botão:**  A Volta ao Mundo em 80 dias, 100 textos de Historia Antiga, e etc
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:relatorios_desempenho',
    'eventAction': 'preencheu:campo',
    'eventLabel': '[[valor_digitado]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[valor_digitado]] | a_volta_ao_mundo_em_80_dias, 100_textos_de_historia_antiga, e etc | Deve retornar o nome do botao clicado  |

<br />  


- **Quando:**  Na seleção de turma  <br />
 
- **Onde:** Dashboard e Relatórios - Relatório de desempenho consolidado
    - **Titulo ou nome do botão:**  todas, bko-2021, projeto-2021, e etc
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:relatorios_desempenho',
    'eventAction': 'clique:selecao_turma',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] | todas, bko-2021, projeto-2021, e etc | Deve retornar o nome do botao clicado  |

<br />  


- **Quando:**  No clique no botão visualizar  <br />
 
- **Onde:** Dashboard e Relatórios - Relatório de desempenho consolidado
    - **Titulo ou nome do botão:**   Visualizar
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:relatorios_desempenho',
    'eventAction': 'clique:visualizar',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  visualizar | Deve retornar o nome do botao clicado  |

<br />  



- **Quando:**  No clique no botão buscar  <br />
 
- **Onde:** Dashboard e Relatórios - Relatório de desempenho consolidado
    - **Titulo ou nome do botão:**   Buscar
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:relatorios_desempenho',
    'eventAction': 'clique:buscar',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  buscar | Deve retornar o nome do botao clicado  |

<br /> 


### Sobre a Bibot


- **Quando:**  No clique dos atalhos no topo  <br />
 
- **Onde:** Sobre a Bibot - Proposta
    - **Titulo ou nome do botão:**    Sobre a bibot, Proposta e etc
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:sobre_a_bibot_proposta',
    'eventAction': 'clique:atalhos_topo',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  sobre_a_bibot, proposta e etc | Deve retornar o nome do botao clicado  |

<br /> 



### Sobre a Bibot - Habilidades Avaliadas


- **Quando:**  No clique dos atalhos no topo  <br />
 
- **Onde:** Sobre a Bibot - Habilidades Avaliadas
    - **Titulo ou nome do botão:**    Sobre a bibot, Proposta e etc
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:sobre_a_bibot_habilidades',
    'eventAction': 'clique:atalhos_topo',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  sobre_a_bibot, proposta e etc | Deve retornar o nome do botao clicado  |

<br /> 


- **Quando:** No clique em baixar pdf   <br />
 
- **Onde:** Sobre a Bibot - Habilidades Avaliadas
    - **Titulo ou nome do botão:**   Conhecendo as Competencias e Habilidades de Leitura, Exemplo Pratico do Uso dos Descritores e etc
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:sobre_a_bibot_habilidades',
    'eventAction': 'clique:baixar_pdf',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  conhecendo_as_competencias_e_habilidades_de_leitura, exemplo_pratico_do_uso_dos_descritores e etc | Deve retornar o nome do botao clicado  |

<br /> 



### Configurações - Meu Perfil


- **Quando:** No clique dos botões disponíveis  <br />
 
- **Onde:** Sobre a Bibot - Habilidades Avaliadas
    - **Titulo ou nome do botão:**  Voltar, Alterar Senha, Editar
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:configuracoes_meu_perfil',
    'eventAction': 'clique:botoes',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  voltar, alterar_senha, editar | Deve retornar o nome do botao clicado  |

<br /> 


### Configurações - Sair


- **Quando:** No clique em sair <br />
 
- **Onde:** Sobre a Bibot - Habilidades Avaliadas
    - **Titulo ou nome do botão:**  Sair
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'bibot_professor:configuracoes_sair',
    'eventAction': 'clique:sair',
    'eventLabel': '[[nome_botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome_botao]] |  sair | Deve retornar o nome do botao clicado  |

<br /> 
