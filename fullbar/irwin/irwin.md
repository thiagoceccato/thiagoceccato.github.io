![enter image description here](https://thiagoceccato.github.io/fullbar/irwin/img/fullbar_logo.jpg)

#**Fullbar**
-----------------------


##**Projeto Irwin - Hotsite**

---------------

###**Índice**

- [Objetivo](#objetivo)
- [KPIs](#kpis)
- [Interações](#interações)
  - [Login](#login)
  - [Cadastro](#cadastro)
  - [Cupom](#cupom)
  - [Fale-conosco](#fale-conosco)
  - [Demais Eventos](#demais-eventos)
 
---------------



###**Objetivo**

---------------

Esse documento tem como objetivo exemplificar os principais KPIs e insights que o GA pode oferecer em relação ao hotsite de Irwin.

###**KPIs**

---------------



 - Cadastro no Hotsite

> - Mensurar quantas pessoas estão cadastradas no site, mas não ativaram a promoção
> - Mensurar erros no cadastro, visando sempre ter uma melhor performance para o usuário.

 - Login no Hotsite

> - Intenção de verificar quantas pessoas estão se logando para verificar a promoção. 
> - Mensurar erros de logins, visando verificar se há como o usuário se logar de uma forma mais fácil.

 - Envio de cupom pelo site
 
> - Verificar quantidade de cupons enviados para o hotsite podendo cruzar esses dados com o número de cadastros
> - Verificar quantos cupons estão sendo enviados por empresas, visando saber qual a empresa que mais retorna cupons enviados no site.
>  

 - Fluxo do site
 
> - Verificar páginas mais acessadas  

 - Download do Regulamento

> - Verificar o quanto os usuário estão se "importando" com a promoção.


###**Interações**

----------------------

A maioria das interações que há no site serão tagueadas, segue alguns exemplos:

####**Login**

 - **Clicks em Consumidor/Vendedor:**

![enter image description here]
(https://thiagoceccato.github.io/fullbar/irwin/img/home_deslogada.jpg)


	Teremos os dados de intenção de clicks em cada botão e poderemos cruzar com os cadastros de consumidor/vendedor.


- **Sucesso **

		Teremos dado de sucesso de login, visando verificar a proximidade do usuário com a promoção e o hotsite em si.
		
Quando usuário se cadastrar com sucesso, deve ser enviado o comando a seguir:

``` js

dataLayer.push({'event' : 'sucesso', 'tipo': 'sucesso-login'});

```

 - **Erro**

		Teremos dados de usuários que erraram algum campo do login.

Quando o usuário receber algum erro na tela, devem ser disparados os comandos a seguir:

![enter image description here]
(https://thiagoceccato.github.io/fullbar/irwin/img/login2.jpg)

``` js

dataLayer.push({'event' : 'erro', 'tipo': 'erro-login', 'propriedade':'senha-incorreta'});

```

![enter image description here]
(https://thiagoceccato.github.io/fullbar/irwin/img/login3.jpg)

``` js

dataLayer.push({'event' : 'erro', 'tipo': 'erro-login', 'propriedade':'cpf-não-cadastrado'});

```

 - **Esqueci a senha**

		Mensuraremos o botão de esqueci a senha, dando margem a informações de como é enviada a senha para o usuário.

Quando o usuário preencher o CPF e for enviado a nova senha para o usuário, deve ser enviado o comando a seguir:

![enter image description here]
(https://thiagoceccato.github.io/fullbar/irwin/img/login4.jpg)

``` js

dataLayer.push({'event' : 'sucesso', 'tipo': 'receber-senha-via-email'});



```

####**Cadastro**

Teremos dados de cadastros tais como:

 - Fluxo dentro do cadastro

		Dados de preenchimento de cada campo, onde poderemos  verificar onde os usuários estão parando no fluxo desse formulário e campos que não são preenchidos.

![enter image description here]
(https://thiagoceccato.github.io/fullbar/irwin/img/cadastro_consumidor.jpg)

 - Erros de campos no cadastro

		Dados de erros de cada campo, onde poderemos verificar quais campos os usuários mais erram dentro de um cadastro.

![enter image description here]
(https://thiagoceccato.github.io/fullbar/irwin/img/cadastro_consumidor_retorno.jpg)

 - Sucesso de cadastro

		Dados de sucessos de cadastro mostram quantos cadastros foram efetuados. Teremos dados de sucesso de vendedor e consumidor que poderemos cruzar com quantos usuarios ou vendedores efetuaram o cadastro mas não enviaram cupom.


	Ao usuário completar o cadastro sem errar nenhum campo, deve ser disparado o comando a seguir:

``` js
dataLayer.push({'event' : 'sucesso', 'tipo': 'sucesso-cadastro'});

```


####**Cupom**

- **Erro**

		Teremos dados dos erros referentes a cadastro de cuspons.

Quando usuário receber um erro na tela, devem ser disparados os comandos a seguir:

![enter image description here]
(https://thiagoceccato.github.io/fullbar/irwin/img/cadastro_cupom3.jpg)

``` js

dataLayer.push({'event' : 'erro', 'tipo': 'cupom','propriedade':'cupom-ja-cadastrado'});

```
![enter image description here]
(https://thiagoceccato.github.io/fullbar/irwin/img/cadastro_cupom5.jpg)

``` js

dataLayer.push({'event' : 'erro', 'tipo': 'cupom','propriedade':'código-inválido'});

```

 - **Sucesso**

		Teremos dados de Sucesso que poderemos cruzar com cadastros efetuados, vendedores que ativaram mais cupons, consumidores que ativaram mais cupons, etc.
	

Quando o usuário recebe a mensagem de sucesso de cupom ativado, deve ser disparado o comando a seguir:

![enter image description here]
(https://thiagoceccato.github.io/fullbar/irwin/img/cadastro_cupom4.jpg)

``` js

dataLayer.push({'event' : 'sucesso', 'tipo': 'cupom','propriedade':'cupom-cadastrado-com-sucesso'});
```


####**Fale Conosco**

		Teremos dados de pessoas que enviaram mensagens via fale-conosco, que mostra a interatividade do usuário coma promoção.


 - **Sucesso**

![enter image description here]
(https://thiagoceccato.github.io/fullbar/irwin/img/fale_conosco3.jpg)

``` js

dataLayer.push({'event' : 'sucesso', 'tipo': 'fale-conosco','propriedade':'mensagem-enviada-com-sucesso'});
```


####**Demais Eventos**

Os demais eventos mostrarão interações diversas do usuário com o site como:

 - Cliques nos menus
 - Download do regulamento
 - Pesquisa de lojas participantes




