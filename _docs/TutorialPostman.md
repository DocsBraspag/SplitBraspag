---
title: POSTMAN
category: TUTORIAIS
order: 1
---

# O que é o POSTMAN?

O Postman é um API Client que facilita aos desenvolvedores criar, compartilhar, testar e documentar APIs. Isso é feito, permitindo aos usuários criar e salvar solicitações HTTP / s simples e complexas, bem como ler suas respostas.


## Por que utilizar o POSTMAN?

Além da praticidade de ter todos os exemplos e códigos de integração prontos, o POSTMAN é a ferramenta oficial de teste pelas equipes de desenvolvimento e suporte Cielo. Dessa maneira ao realizar integrações, caso você possua duvidas, será mais rápido e simples de confirmar o que pode estar ocorrendo com o seu código.

**Outras vantagens do POSTMAN**:

* Ferramenta gratuita
* Não é necessário instalar EXE - é uma extensão do Google Chrome
* Funciona em qualquer plataforma: Windows, MacOS e Linux
* Converte JSON em várias linguagens (EX: Python, PHP, RUBY)
* Sincronização entre diversos aplicativos
* Sincroniza código entre equipes (Versão paga)
	
## Download e cadastro

Para utilizar o Postman, basta instalar o APP em seu computador. Isso pode ser realizado de duas maneiras:

* **Instalando a versão desktop**: Basta acessar <https://www.getpostman.com/> , baixar a versão para sua plataforma e instalar o executável
* **Instalando a extensão do Chrome**: Basta acessar a Chrome Store e instalar a extensão do Postman

### Instalando a extensão do Chrome

1.Acesse a Chrome Store, pesquise por POSTMAN  em APPs
![](/uploads/versions/p1.png)
		
2.Na aba de Apps no seu Google Chrome acesse o ícone do Postman
		P2
	
3.Ao acessar o Postman pela primeira vez, o App ira requisitar um login. Essa etapa é opcional, mas sugerismos que uma conta seja criada, pois isso sincroniza suas configurações com a sua conta POSTMAN, ou seja, se for necessario realiza um login em outro computador, suas configurações já estarão prontas para serem usadas
		P3
		
4.Pronto, basta configurar suas Coleções  e ambientes para iniciar os testes
		P4

# Explicando Componentes do POSTMAN

Nesta área vamos explicar os diferentes componentes do Postman e suas funções. Depois dessa introdução, as próximas partes deste tutorial vão focar na configurações e usos para teste das APIs



> Imagem com os nomes das áreas



A - Environment (Ambiente):
Ambiente para onde serão direcionadas as requisições.
Aqui existem dados de:

	• MerchantId - Identificador de sua loja nas APIs Cielo
	• MerchantKey - Chave de segurança da sua loja nas APIs Cielo
	• URL do POST/PUT - Endpoint Para criar ou editar transações
	• URL do GET - Endpoint para consulta de transações

Sugerimos que sejam criados dois ambientes, um com dados de produção e outro para Sandbox, cada um com suas respectivas credenciais e URLs.
Desta maneira se torna muito mais simples realizar testes com o mesmo contrato para ambos os ambientes.


B - Header
Aqui existem o MerchantId/MerchantKey, que por padrão usam os mesmos dados registrados em Environment.


C - Body:
É o conteúdo das Requisições. Aqui é onde você pode alterar ou criar exemplos para a API e validar o conteúdo do seu 

D - Collection (Coleções):
Local que contém todas os exemplos e códigos que podem ser utilizados na API. Aqui existem as criações de transações, consultas e outras funcionalidades que existem nas APIs Cielo.
O número de coleções é ilimitado, ou seja, você pode criar várias coleções para se adequar ao seu estilo de uso do Postman.


Criando Environment Cielo (ProdutoX)

O primeiro passo na utilização do postman é a criação do ambiente (environment) da API. Essa configuração vai definir quais credenciais e endpoints serão utilizados como padrão, assim evitando a necessidade de realizar configurações a cada teste.

Realizando a criação do ambiente:

	1. No canto superior direito, clique na engrenagem e selecione "Manage environment".
		P7
		
	2. Na tela de gerenciamento, basta preencher as configurações de acordo com a tela abaixo:
		P9
	3. Pronto,  agora os endereços e credenciais para teste já estão cadastrada. Sugerimos que você crie um ambiente para Produção e um para Sandbox assim,
	


Importando uma Collection

A Cielo dispõe de coleções padrões para suas APIs. Você pode importa-las diretamente para o seu POSTMAN e ter todos os exemplos prontos para utilização instantaneamente, sem a necessidade de copia-los diretamente do manual.

Para realizar a importação basta:

	1. Acessar a área do manual onde o link da Coleção está disponível e copia-lo
		a. IMPORTANTE: para que a sua coleção sempre esteja atualizada, sugerimos que sempre busque a ultima versão da coleção no manual. O link NÃO ATUALIZA A COLEÇÃO IMPORTADA AUTOMATICAMENTE
	
	2. Com o Postman aberto, use o botão IMPORT, e selecione a opção "IMPORT FROM LINK".
		P5
		
	3. Pronto, sua coleção Cielo já está disponível. Basta selecionar Environment e a requisição. Ao clicar em SEND, o Postman vai executar a comunicação com a Cielo.
		a. P6
	


Realizando uma requisição

Com a sua Collection e Environmet configurados, realizar uma transação junto a Cielo é extremamente facil:

P10

	1. Seleciona qual requisição você deseja usar.
	2. Verifique que o Environment correto está selecionado
	3. Clique em SEND.
	4. Na parte inferior você verá 2 informações:
		a. O Body do Response - Aqui é onde ficam listadas os dados retornados de suas transações
		b. O HTTP Status - Aqui você verifica se a sua requisição funcionou. Se você não receber um response verifique o status HTTP.



Collections e Environments  Cielo

Abaixo, listamos as collections e os Environments Cielo. Use-as em seu Postman para realizar testes e integrações.