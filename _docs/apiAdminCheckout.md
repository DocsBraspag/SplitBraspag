---
title: API Cielo X Admin
category: CHECKOUT CIELO
order: 1
---


### O que é a API Cielo X Admin

Está API é utilizada para criar e editar lojas Checkout. Hoje ela é utilizada pelo Admin Braspag e a Cielo (Via Admin Braspag),


### Utilização pela Cielo.

A Cielo realiza a criação e edição de lojas via essa API quando uma Afialiação é criada no SEC. é uma chamada automatica, que visa manter uma coesão e sincronia de dados cadastrais. 



### Consultando Lojas

Basta realizar um GET via a URL abaixo.


> https://cieloecommerce.cielo.com.br/api/private/v1/Merchant/GetByAffiliationCode/`(Afiliação da loja)`


### Criação e atualização das Lojas

Para realizar a criação de uma loja, basta realizar um `POST` ou um `PUT` para a URL abaixo. 

> *Criação* (`POST`) e *Atualização* (`PUT`): https://cieloecommerce.cielo.com.br/api/private/v1/Merchant


 O Contrato de criação/Atualização está exemplificado abaixo:


```



```
**MerchantViewModel**:

| Campo                     | Descrição                                                           | Tipo                                              | Tamanho | Obrigatório          | Campos equivalente  no contrato Admin |
|----------------------------|---------------------------------------------------------------------|---------------------------------------------------|---------|----------------------|---------------------------------------|
| Id                         | MerchantID do Checkout                                              | Guid                                              | -       | Apenas para o update | N/A                                   |
| Nickname                   | Nome uusado  no "Como gostaria de ser chamado"                      | string                                            | 64      | sim                  | N/A                                   |
| Email                      | E-mail principal - Usado como Login                                 | string                                            | 64      | sim                  | E-MAIL                                |
| ContactName                | Nome de contato do lojista                                          | string                                            | 64      | sim                  | CONTACTNAME / FULLNAME                |
| ContactPhone               | Telefone de contato do lojista                                      | string                                            | 16      | sim                  | N/A                                   |
| Identity                   | CPF ou CNPJ - Deve ser enviado sem formatação                       | string                                            | 32      | sim                  | CNPJ / CPF                            |
| DoingBusinessAs            | Nome fantasia da loja                                               | string                                            | 128     | sim                  | FANCYNAME                             |
| CorporateName              | Razão social                                                        | string                                            | 128     | sim                  | CORPORATENAME                         |
| Website                    | Url do lojista                                                      | string                                            | 256     | não                  | N/A                                   |
| Origin                     | Origem do cadastro da loja                                          | string                                            | 100     | não                  | N/A                                   |
| Gateway                    | Gateway utilizado pelo lojista                                      | string                                            | 128     | não                  | N/A                                   |
| Comments                   | Comentários a respeito do cadastro                                  | string                                            | 4000    | não                  | N/A                                   |
| SecondEmail                | E-mail secundario opcional                                          | string                                            | 64      | sim                  | N/A                                   |
| Category                   | Perfil de Anti-fraude a ser utilizado na loja                       | Enum-Category                                     | -       | sim                  | N/A                                   |
| PersonType                 | Tipo de registro (Pessoal fisica ou empresa)                        | Enum-PersonEnum                                   | -       | não                  | DOCUMENTTYPE                          |
| NotAccredited              | Motivo do não credenciamento do lojista                             | Enum-NotAccredited                                | -       | sim                  | N/A                                   |
| Address                    | Dados de endereço do lojista/loja                                   | Ver tabela 01 - Objeto-Adress                     | -       | não                  | N/A                                   |
| TechnicalContact           | Dados de responsavel tecnico da loja                                | Ver tabela 02 - Objeto-TechnicalContact           | -       | sim                  | N/A                                   |
| TransactionalConfiguration | Configurações transacionais.Ver tabela 03 para descrição dos campos | Ver tabela 03 - Objeto-TransactionalConfiguration | -       | não                  | N/A                                   |
| ShipmentConfiguration      | Dados de contratos dos correios da loja                             | Ver Tabela 04 - Objeto-ShipmentConfiguration      | -       | não                  | N/A                                   |
| CorreiosServices           | Tipo de contrato dos correios                                       | Ver tabela 05 - Lista-Objetos-CorreiosServices    | -       | não                  | N/A                                   |
| PaymentMethods             | Meios de pagamento disponiveis no cadastro                          | Ver tabela 06 - Lista-Objetos-PaymentMethods      | -       | não                  | PAYMENTMETHOD                         |


**Objeto-Adress**

| Campo      | Descrição                   | Tipo       | Tamanho | Obrigatório | Campos equivalente no contrato Admin  |
|------------|-----------------------------|------------|---------|-------------|---------------------------------------|
| City       | Cidade da loja              | string     | 64      | sim         | CITY                                  |
| Complement | Complemento                 | string     | 256     | não         | COMPLEMENT                            |
| Street     | Nome da rua                 | string     | 256     | sim         | ADDRESS                               |
| District   | Bairro                      | string     | 64      | não         | DISTRICT                              |
| Number     | Numero da loja              | string     | 8       | sim         | NUMBER                                |
| State      | Estado (UNIDADE FEDERATIVA) | Enum-State | -       | sim         | STATE                                 |
| ZipCode    | CEP                         | string     | 9       | sim         | ZIPCODE                               |





[GitHub](http://github.com)







-
