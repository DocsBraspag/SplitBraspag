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
| Campo                      | Descrição                                                           | Tipo                                              | Tamanho | Obrigatório          |
|----------------------------|---------------------------------------------------------------------|---------------------------------------------------|---------|----------------------|
| Id                         | MerchantID do Checkout                                              | Guid                                              | -       | Apenas para o update |
| Nickname                   | Nome uusado  no "Como gostaria de ser chamado"                      | string                                            | 64      | sim                  |
| Email                      | E-mail principal - Usado como Login                                 | string                                            | 64      | sim                  |
| ContactName                | Nome de contato do lojista                                          | string                                            | 64      | sim                  |
| ContactPhone               | Telefone de contato do lojista                                      | string                                            | 16      | sim                  |
| Identity                   | CPF ou CNPJ - Deve ser enviado sem formatação                       | string                                            | 32      | sim                  |
| DoingBusinessAs            | Nome fantasia da loja                                               | string                                            | 128     | sim                  |
| CorporateName              | Razão social                                                        | string                                            | 128     | sim                  |
| Website                    | Url do lojista                                                      | string                                            | 256     | não                  |
| Origin                     | Origem do cadastro da loja                                          | string                                            | 100     | não                  |
| Gateway                    | Gateway utilizado pelo lojista                                      | string                                            | 128     | não                  |
| Comments                   | Comentários a respeito do cadastro                                  | string                                            | 4000    | não                  |
| SecondEmail                | E-mail secundario opcional                                          | string                                            | 64      | sim                  |
| Category                   | Perfil de Anti-fraude a ser utilizado na loja                       | Enum-Category                                     | -       | sim                  |
| PersonType                 | Tipo de registro (Pessoal fisica ou empresa)                        | Enum-PersonEnum                                   | -       | não                  |
| NotAccredited              | Motivo do não credenciamento do lojista                             | Enum-NotAccredited                                | -       | sim                  |
| Address                    | Dados de endereço do lojista/loja                                   | Ver tabela 01 - Objeto-Adress                     | -       | não                  |
| TechnicalContact           | Dados de responsavel tecnico da loja                                | Ver tabela 02 - Objeto-TechnicalContact           | -       | sim                  |
| TransactionalConfiguration | Configurações transacionais.Ver tabela 03 para descrição dos campos | Ver tabela 03 - Objeto-TransactionalConfiguration | -       | não                  |
| ShipmentConfiguration      | Dados de contratos dos correios da loja                             | Ver Tabela 04 - Objeto-ShipmentConfiguration      | -       | não                  |
| CorreiosServices           | Tipo de contrato dos correios                                       | Ver tabela 05 - Lista-Objetos-CorreiosServices    | -       | não                  |
| PaymentMethods             | Meios de pagamento disponiveis no cadastro                          | Ver tabela 06 - Lista-Objetos-PaymentMethods      | -       | não                  |