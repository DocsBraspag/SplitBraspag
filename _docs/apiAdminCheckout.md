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

| Campo                      | Descrição                                                           | Tipo                                              | Tamanho | Obrigatório          | Equivalente ao Admin                  |
|----------------------------|---------------------------------------------------------------------|:-------------------------------------------------:|:-------:|----------------------|---------------------------------------|
| Id                         | MerchantID do Checkout                                              | Guid                                              | -       | Para o update        | N/A                                   |
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

| Campo      | Descrição                   | Tipo       | Tamanho | Obrigatório | Equivalente ao Admin                  |
|------------|-----------------------------|------------|---------|-------------|---------------------------------------|
| City       | Cidade da loja              | string     | 64      | sim         | CITY                                  |
| Complement | Complemento                 | string     | 256     | não         | COMPLEMENT                            |
| Street     | Nome da rua                 | string     | 256     | sim         | ADDRESS                               |
| District   | Bairro                      | string     | 64      | não         | DISTRICT                              |
| Number     | Numero da loja              | string     | 8       | sim         | NUMBER                                |
| State      | Estado (UNIDADE FEDERATIVA) | Enum-State | -       | sim         | STATE                                 |
| ZipCode    | CEP                         | string     | 9       | sim         | ZIPCODE                               |







**TechnicalContact**

| Campo   | Descrição                   | Tipo   | Tamanho | Obrigatório | Equivalente ao Admin                  |
|---------|-----------------------------|--------|---------|-------------|---------------------------------------|
| Name    | Nome do Responsavel tecnico | string | 64      | não         | N/A                                   |
| Phone   | Telefone fixo ou Movel      | string | 16      | não         | PHONE                                 |
| Email   | E-mail de contato           | string | 64      | não         | N/A                                   |
| Website | URL do desenvolvedor        | string | 256     | não         | N/A                                   |
| Company | Empresa de desenvolvimento  | string | 128     | não         | N/A                                   |





**TransactionalConfiguration**

| Campo   | Descrição                   | Tipo   | Tamanho | Obrigatório | Equivalente ao Admin                  |
|---------|-----------------------------|--------|---------|-------------|---------------------------------------|
| Profile | Perfil da loja dentro do código. | Enum-Profile | - | sim | INTEGRATIONTYPE |
| Status | Status do lojista dentro do Chekcout | Enum-Status | - | sim | N/A |
| IsAutomaticCaptureForLowRisk | Captura automatica para baixo risco no AF | bool | - | não | N/A |
| IsAutomaticCancellationForHighRisk | Cancelamento automatico para Alto risco no AF | bool | - | sim | N/A |
| TransactionalMerchantId | MerchantID do Pagador Associada ao Checkout Cielo | guid | - | sim | N/A |
| ReturnUrl | URL de Retorno da Loja. Maiores informações no manual do desenvolvedor Checkout Cielo | string | 256 | não | N/A |
| NotificationUrl | URL de Notificação da Loja. Maiores informações no manual do desenvolvedor Checkout Cielo | string | 256 | não | N/A |
| StatusNotificationUrl | URL de Mudança de status da Loja. Maiores informações no manual do desenvolvedor Checkout Cielo | string | 256 | não | N/A |
| TestModeEnabled | Modo de teste ativo no momento da criação ? Padrão: False | bool | - | não | N/A |
| MinimumInstallmentAmount | Valor minimo para o parcelamento | int | - | não | N/A |
| MinimumBoletoAmount | Valor minimo para  emissão de boleto | int | - | não | N/A |
| BoletoDiscountPercentage | Porcentagem de desconto para pagamento com boletos | byte | - | não | N/A |
| OnlineDebitDiscountPercentage | Porcentagem de desconto para pagamento com Débito online | byte | - | não | N/A |
| AffiliationCode | Afiliação Cielo | string | 16 | não | EC |
| AffiliationKey | Chave de produção Cielo | string | 64 | não | PRODUCTIONKEY |
| AntifraudEnabled | Anti Fraude (AF) ativo no momento da criação ? Padrão: False | bool | - | não | N/A |
| CartaoProtegidoEnabled | Defini se a loja poderá utilizar o cartão protegido, junto com suporte a clientes registrados. Padrão: False | bool | - | não | N/A |
| CvvRequired | Obrigatoriedade de envio do CVV. Padrão: False | bool | - | sim | CVVREQUIRED |
| Mcc | Identificados do segmento CIELO que defini a taxa a ser cobrada | string | 16 | não | Mcc |
| ContractedSolution | Tipo de Loja com base no serviço prestado | Enum-ContractedSolution | - | sim | N/A |
| CreditCardAuthenticationRequired | Autenticação do Cartão de crédito junto ao banco? Padrão: False | bool | - | não | N/A |
| CaptchaIsRequired | Obrigatoriedade do Captcha no transacional. Padrão: False | bool | - | não | N/A |
| CaptchaRequiredUpDate | Data em que o Captcha  deixará de ser exibido. | DateTime | - | não | N/A |
| AntifraudMinimumAmount | Valor minimo para ocorrer a analise de risco | int | - | não | N/A |
| IsAutomaticCaptureForAllTransactions | Captura automatica para todas as transações | bool | - | não | N/A |
| TransactionalMerchantKey | MerchantKey do Pagador Associada ao Checkout Cielo | string | 40 | sim | N/A |


**ShipmentConfiguration**

| Campo    | Descrição                   | Tipo   | Tamanho | Obrigatório | Equivalente ao Admin                  |
|----------|-----------------------------|--------|---------|-------------|---------------------------------------|
| Login    | Login do lojista no Correio             | string | 32      | sim         | N/A                       |
| Password | Senha do contrato do lojista no Correio | string | 32      | sim         | N/A                       |



**CorreiosServiceConfiguration**

| Campo       | Descrição                           | Tipo             | Tamanho | Obrigatório | Equivalente ao Admin |
|-------------|-------------------------------------|------------------|---------|-------------|----------------------|
| ServiceCode | Tipo de contrato de frete utilizado | Enum-ServiceCode | -       | sim         | N/A                  |


**PaymentMethod**

| Campo               | Descrição                                                             | Tipo                   | Tamanho | Obrigatório | Campos equivalente  no contrato Admin |
|---------------------|-----------------------------------------------------------------------|------------------------|---------|-------------|---------------------------------------|
| Type                | Meio de pagamento Disponibilizado ao lojista                          | Enum-PaymentMethodName | -       | sim         | NAME                                  |
| Enabled             | Liberar meio de pagamento no Backoffice? Padrão: True                 | bool                   | -       | sim         | N/A                                   |
| MaxNumberOfPayments | Numero de parcelas liberado no meio de pagamento. Padrão: 1 (A vista) | byte                   | -       | sim         | MAXINSTALLMENTS                       |





Enums


**01**

| Category     | Código |
|--------------|--------|
| Undefined    | 0      |
| Retail       | 1      |
| Cosmeticos   | 2      |
| DigitalGoods | 3      |
| Servicos     | 4      |
| Turismo      | 5      |
| Joalheria    | 6      |



| Person   | Código |
|----------|--------|
| Personal | 1      |
| Company  | 2      |



| NotAccredited            | Código |
|--------------------------|--------|
| Undefined                | 0      |
| InvalidDataBank          | 1      |
| CpfWithError             | 2      |
| CnpjWithError            | 3      |
| InconsistentBirthDate    | 4      |
| RegisterWithRestrictions | 5      |
| Other                    | 6      |


| State |
|-------|
| ND    |
| AC    |
| AL    |
| AP    |
| AM    |
| BA    |
| CE    |
| DF    |
| ES    |
| GO    |
| MA    |
| MT    |
| MS    |
| MG    |
| PA    |
| PB    |
| PR    |
| PE    |
| PI    |
| RJ    |
| RN    |
| RS    |
| RO    |
| RR    |
| SC    |
| SP    |
| SE    |
| TO    |


| Profile       | Código |
|---------------|--------|
| Undefined     | 0      |
| Standard      | 1      |
| Guaranteed    | 2      |
| ClicaSorocaba | 3      |
| BuyPage       | 4      |
| CheckoutCielo | 5      |


| Status   | Código |
|----------|--------|
| Inactive | 0      |
| Active   | 1      |
| Blocked  | 2      |



| ContractedSolution | Código |
|--------------------|--------|
| Undefined          | 0      |
| SolucaoIntegrada   | 1      |
| LojaVirtual        | 2      |
| WebService         | 3      |


| ServiceCode                  | Código |
|------------------------------|--------|
| Undefined                    | 0      |
| SEDEXSemContrato             | 40010  |
| SEDEX10SemContrato           | 40215  |
| SEDEXHojeSemContrato         | 40290  |
| SEDEXComContrato1            | 40096  |
| SEDEXComContrato2            | 40436  |
| SEDEXComContrato3            | 40444  |
| SEDEXComContrato4            | 40568  |
| SEDEXComContrato5            | 40606  |
| PACSemContrato               | 41106  |
| PACComContrato1              | 41211  |
| PACComContrato2              | 41068  |
| eSEDEXComContrato            | 81019  |
| eSEDEXPrioritárioComContrato | 81027  |
| eSEDEXExpressComContrato     | 81035  |
| Grupo1eSEDEXComContrato      | 81868  |
| Grupo2eSEDEXComContrato      | 81833  |
| Grupo3eSEDEXComContrato      | 81850  |


| PaymentMethodName         | Código |
|---------------------------|--------|
| NotDefined                | 0      |
| BoletoBradesco            | 1      |
| BoletoBancoDoBrasil       | 2      |
| OnlineDebitBancoDoBrasil  | 3      |
| OnlineDebitBradesco       | 4      |
| DebitCardVisa             | 5      |
| DebitCardMaster           | 6      |
| CreditCardVisa            | 7      |
| CreditCardMaster          | 8      |
| CreditCardJcb             | 9      |
| CreditCardDiscover        | 10     |
| CreditCardAmericanExpress | 11     |
| CreditCardElo             | 12     |
| CreditCardDiners          | 13     |
| CreditCardAura            | 14     |



























































[GitHub](http://github.com)







-
