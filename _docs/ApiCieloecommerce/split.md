---
title: Split de pagamentos
category: API CIELO ECOMMERCE
order: 1
---


### O que é o Split de pagamentos ?

O Split de pagamentos é uma funcionalidade Braspag que permite a divisão de uma transações em diferentes lojas de maneira organizadada e hierarquizada.

Ele funciona recebendo uma transação que pode ser subdividida para diferentes atores. Os valores dessa transação é dividido e pago em contas bancarias separadas. 


Essa é uma  estrutura transacional muito utilizada em MarketPlaces (MKP), onde **o carrinho é formado por produtos de diferentes fornecedores** que receberarão partes do pagamento total da transação.
O Split é uma funcionalidade que possui as seguintes vantagens:

* Evita que comprador realize várias transações, elevando a chance de conversão
* Permite que um Marketplace possa cobrar uma taxa sobre o valor transacional utilizando apenas uma integração técnica
* Permite que o Marketplace possa montar um carrinho com produtos de diferentes fornecedores de maneira transparente para o comprador.



### Como funciona o Split de Pagamentos.

O Split de pagamento funciona quando um Marketplace realiza uma transação e-commerce enviado a Braspag sobre como esse pagamento será dividido e quais participantes serão cobrados ou receberá o valor vendido.

Nesse modelo de split de pagamentos existem 3 entidades básicas:

| **Entidades** | **Descrição** | 
|-----------|---------- |
| **Marketplace** | Dono do carrinho e da Transação. <BR> Possui Sellers que fornecem o contudo do Carrinho.<BR> Realiza a cobrança de uma Taxa sobre a venda do Seller<BR>  | 
| **Seller** | Lojas do Marketplace que fornecem os produtos que formam o carrinho.<BR> Um Marketplace possui inumeros Seller. <BR>Recebem parte da venda, descontado o valor da taxa do MarketPlace<BR>  |
| **Braspag** | Responsavel pela autorização da transação.<BR>Realiza a cobrança da Taxa definida pelo Marketplace, retirando esse valor da transação.<BR> Deposita o valor da Transação na conta do Seller.<BR> Deposita o valor da taxa cobrada pelo Marketplace so Seller <BR> |


O Split é um processo de divisão transacional, onde o **Marketplace** separa o valor pertecente da transação do **Seller** em duas partes: 

* **Taxa do MKP:** Taxa cobrada pelo marketplace sobre o Seller para que a venda seja realizada. Pode ser um valor **%** ou/e uma **taxa fixa por venda.**
* **Valor da venda:** Parte do valor da transação/carrinho que será destinada ao Seller, menos o valor cobrado como **Taxa do MKP**


A **Braspag** receberá a transação, separando a transação entre valores a serem direcionados ao MKP, originado da **Taxa do MKP** e aos Sellers, originado do valor devido dentro da venda.

> **Sobre Contas**: Ao utilizar o SPLIT, o Markteplace deverá junto aos seus Sellers se cadastrar na **Braspag**, assim recebendo um identificador de loja e registrando uma conta bancaria para cada ator como ponto de deposito dos valores Splitados



Abaixo um exemplo do Fluxo transacional de autorização e divisão de valores no Split de pagamentos:


![Salve a imagem para uma maior resolução](http://able-caribou.cloudvent.net/images/Split/Split0.jpg)

**EX:** Uma venda de R$100, feita pelo **Seller** no Marketplace **MKP**

0. O **MKP** tem Taxa de **5%** Sobre a venda do **Seller**
0. Essa Taxa é formada por uma margem de `3 Pontos Percentuais` sobre o total da transação + o custo braspag (`2 pontos percentuais + 0,30 centavos`) sobre o total da transação
0. A Transação é processada. O **Seller** recebe o montante da venda - a `Taxa MKP`
1. O MKP Recebe a parte da transação menos o custo da taxa Braspag.

> ** *Importante* **: As porcentagens exibidas no exemplo não são aplicadas uma sobre as outras, mas sim como pontos percentuais que somados formam uma taxa unica sobre o Seller. Isso ocorre pois o valor das taxas sempre é cobrado sobre o total da transação do Seller.





### Tarifas e Custos

Nesta área do manual vamos detalhar como o processo de cobrança e custos afeta cada uma das entidades envolvidas no Split


#### Custo Marketplace

O Split de pagamentos Braspag funciona com base em uma **taxa basica percentual tabelada** e uma **Tarifa fixa por transação** contratada pelo Marketplace. 
A Taxa braspag é cobrada sobre o valor da transação do Seller Como um todo, mas é retirada da parte destinada ao Marketplace


> **Custo MKP:** TAXA BRASPAG (%) + TARIFA FIXA (R$)

1 - A `taxa Braspag` é vinculada ao valor total da transação do Seller e não sobre o valor que MKP irá receber.<BR><BR>
**EX:** 2% sobre a Venda de R$100,00 do SELLER<BR><BR>
2 - O valor da tarifa fixa Braspag é debitada do montante destinado ao MKP. <BR><BR>
**EX:** 2% sobre a Venda de R$100,00 do SELLER = R$2,00, será retirado da Fatia cobrada pelo MKP do Seller<BR><BR>



#### Custo Seller

O Unico custo para o Seller no Split de pagamento Braspag é a Taxa que o Marketplace cobra sobre transações geradas.
Essa taxa normalmente é formada por uma **Margem de lucro Percentual** aplicada sobre o **custo transacional**.

Abaixo demonstramos como essa Taxa é formada pelo MKP com base no custo Braspag.

> **Custo Seller:** Taxa MKP = {Margem MKP + (TAXA BRASPAG (%) + TARIFA FIXA (R$))}


### Integrando o Split

O Split funciona como parte da API transacional da Braspag via API Cielo Ecommerce.

A integração A ser realizara é a mesma descrita para transações de cartão de crédito dentro da API Cielo Ecommerce.
Para mais informações, veja o [Manual de Integração](https://developercielo.github.io/Webservice-3.0/#criando-uma-transação-simples)

Neste manual serão apresentados os contratos de integração da API Cielo ecommerce, mas o foco da analise se dará nos paramêtros do Split de pagamentos.

> O Split está disponivel apenas para transações de CARTÃO DE CRÉDITO


### Tipos de Split

O Split de pagamentos possui dois tipos basicos de integração:


| Tipo                 | Descrição                                                                                                                          |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------|
| **Split Transacional**     | O marketplace envia na transação quais os Sellers, os valores e Taxas a sofrerem SPLIT                                             |
| **Split Pós-Transacional** | O marketplace define quais os Sellers, valores e Taxas a sofrerem SPLIT depois autorização realizando uma atualização da transação |

Cada modelo possui um contrato adicional de integração a API Cielo Ecommerce e Braspag, que será apresentado a seguir.


#### Split Transacional

Esse modelo exige que o lojista envie um  adicional na integração da API Cielo Ecommerce onde serão inclusos os dados do pagamento, Sellers e Taxas a serem cobradas.

Exemplo do Nó de SPLIT no `POST`:
```
"SplitPayments":[{
        "SellerMerchantId" :"MID SELLER 01",
        "Amount":10000,
        "Fares":{
            "Mdr":5,
            "Fee":0
        }
```

| Propriedade                             | Descrição                                                                                   | Tipo   | Tamanho | Obrigatório |
|-----------------------------------------|---------------------------------------------------------------------------------------------|--------|---------|-------------|
| `SplitPayments.SellerMerchantId`        | Identificador do Seller                                                                     | GUID   | 36      | Sim         |
| `SplitPayments.Amount`                  | Valor da transação pertencente ao Seller                                                    | Número | 15      | Sim         |
| `SplitPayments.Fares.Mdr`               | Taxa do MKP (%) a ser retirada do Seller                                                    | Número | 2       | Sim         |
| `SplitPayments.Fares.Fee`               | Tarifa (R$) a ser cobrada do Seller - em Centavos                                           | Número | 15      | Sim         |


Com resposta, A API retornará um nó com as seguintes caracteristicas:

Parte do `RESPONSE`:
```
"SplitPayments": [
            {
                "sellerMerchantId": "MID SELLER 01",
                "amount": 10000,
                "fares": {
                    "mdr": 5,
                    "fee": 0
                },
                "splits": [                
                    {
                        "sellerMerchantId": "MID DO MKP",
                        "amount": 500,
                    },
                    {
                        "sellerMerchantId": "MID SELLER 01",
                        "amount": 9500,
                    }
                ]
            }
        ],
```

| Propriedade                             | Descrição                                                                                   | Tipo   | Tamanho | Obrigatório |
|-----------------------------------------|---------------------------------------------------------------------------------------------|--------|---------|-------------|
| `SplitPayments.SellerMerchantId`        | Identificador do Seller                                                                     | GUID   | 36      | Sim         |
| `SplitPayments.Amount`                  | Valor da transação pertencente ao Seller                                                    | Número | 15      | Sim         |
| `SplitPayments.Fares.Mdr`               | Taxa do MKP (%) a ser retirada do Seller                                                    | Número | 2       | Sim         |
| `SplitPayments.Fares.Fee`               | Tarifa (R$) a ser cobrada do Seller - em Centavos                                           | Número | 15      | Sim         |
| `SplitPayments.split.SellerMerchantId.` | Identificador do Seller ou MKP incluso no Split                                             | GUID   | 36      | Sim         |
| `SplitPayments.split.Amount`            | Valor da transação a ser depositado no Seller ou MKP, descontado a Taxa MKP e/ou Taxa Cielo | Número | 15      | Sim         |





O nó `SPLIT` contido na resposta da transação retorna informações especificas a respeito dos valores a serem depositados para cada entidade do split:


![Salve a imagem para uma maior resolução](http://able-caribou.cloudvent.net/images/Split/Split03.jpg)


O Split aceita varias combinações para transações entre diferentes atores:

* Apenas 1 Seller - Sem taxa ou tarifa Braspag
* 2 Sellers dividindo a mesma transação - Com taxa e tarifa Braspag
* 2 Sellers, sendo um deles o próprio MKP - Com taxa e tarifa Braspag


**EXEMPLO 01 -  Apenas 1 Seller**

Neste primeiro exemplo, para simplificar como a apresentação do Modelo de integração, o MKP não sofrerá cobrança de taxa ou tarifa Braspag


REQUEST 

Header
```
--request POST "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/"
--header "Content-Type: application/json"
--header "MerchantId: MID DO MKP"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
```
Body
```
{
   "MerchantOrderId":"2014111701",
   "Payment":{
     "Type":"SplittedCreditCard",
     "Amount":10000,
     "Installments":1,
     "SoftDescriptor":"Split*LosCone",
     "Capture":false,
     "CreditCard":{
         "CardNumber":"4551870000000181",
         "Holder":"Teste Holder",
         "ExpirationDate":"12/2021",
         "SecurityCode":"123",
         "Brand":"Visa"
     },
     "SplitPayments":[{
        "SellerMerchantId" :"MID SELLER 01",
        "Amount":10000,
        "Fares":{
            "Mdr":5,
            "Fee":0
        }
     }]
   }
}

```

RESPONSE

```
{
    "MerchantOrderId": "2014111701",
    "Customer": {
        "Name": "[Guest]"
    },
    "Payment": {
    "SplitPayments": [
            {
                "sellerMerchantId": "MID SELLER 01",
                "amount": 10000,
                "fares": {
                    "mdr": 5,
                    "fee": 0
                },
                "splits": [                
                    {
                        "sellerMerchantId": "MID DO MKP",
                        "amount": 500,
                    },
                    {
                        "sellerMerchantId": "MID SELLER 01",
                        "amount": 9500,
                    }
                ]
            }
        ],
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": 0,
        "Capture": false,
        "Authenticate": false,
        "Recurrent": false,
        "CreditCard": {
            "CardNumber": "455187******0181",
            "Holder": "Teste Holder",
            "ExpirationDate": "12/2021",
            "SaveCard": false,
            "Brand": "Visa"
        },
        "Tid": "1006993069000AF39CAA",
        "ProofOfSale": "483594",
        "AuthorizationCode": "123456",
        "SoftDescriptor": "Split*LosCone",
        "Provider": "Cielo",
        "Eci": "7",
        "PaymentId": "c5b7ea6e-e604-463a-a1b1-123c3c598e05",
        "Amount": 1000,
        "ReceivedDate": "2017-08-30 14:37:14",
        "Status": 1,
        "ReturnMessage": "Transação autorizada",
        "ReturnCode": "00",
        "Type": "SplittedCreditCard",
        "Currency": "BRL",
        "Country": "BRA",
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "http://splitsandbox.braspag.com.br/schedules/c5b7ea6e-e604-463a-a1b1-123c3c598e05"
            },
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerydev.cieloecommerce.cielo.com.br/1/sales/c5b7ea6e-e604-463a-a1b1-123c3c598e05"
            },
            {
                "Method": "PUT",
                "Rel": "capture",
                "Href": "https://apidev.cieloecommerce.cielo.com.br/1/sales/c5b7ea6e-e604-463a-a1b1-123c3c598e05/capture"
            },
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apidev.cieloecommerce.cielo.com.br/1/sales/c5b7ea6e-e604-463a-a1b1-123c3c598e05/void"
            }
        ]
    }
}

```


**EXEMPLO 02 - 2 Sellers dividindo a mesma transação**

Neste exemplo, o MKP será cobrado pela Braspag. 
Vamos usar como base:

* **Taxa Braspag:** 2% sobre o valor da Venda do Seller
* **Tarifa Braspag:** R$0,30 por transação do Seller



REQUEST 

Header
```
--request POST "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/"
--header "Content-Type: application/json"
--header "MerchantId: MID DO MKP"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
```
Body
```
{
   "MerchantOrderId":"2014111701",
   "Payment":{
     "Type":"SplittedCreditCard",
     "Amount":20000,
     "Installments":1,
     "SoftDescriptor":"Split*LosCone",
     "Capture":false,
     "CreditCard":{
         "CardNumber":"4551870000000181",
         "Holder":"Teste Holder",
         "ExpirationDate":"12/2021",
         "SecurityCode":"123",
         "Brand":"Visa"
     },
     "SplitPayments":[
        {
        "SellerMerchantId" :"MID SELLER 01",
        "Amount":10000,
        "Fares":{
            "Mdr":5,
            "Fee":0
        },
        {
        "SellerMerchantId" :"MID SELLER 02",
        "Amount":10000,
        "Fares":{
            "Mdr":10,
            "Fee":0
        }
      }]
   }
}

```

RESPONSE
```
{
    "MerchantOrderId": "2014111701",
    "Customer": {
        "Name": "[Guest]"
    },
    "Payment": {
    "SplitPayments": [
            {
                "sellerMerchantId": "MID SELLER 01",
                "amount": 10000,
                "fares": {
                    "mdr": 5,
                    "fee": 0
                },
                "splits": [                
                    {
                        "sellerMerchantId": "MID DO MKP",
                        "amount": 500,
                    },
                    {
                        "sellerMerchantId": "MID SELLER 01",
                        "amount": 9500,
                   }
                ]
            },
        	{
                "sellerMerchantId": "MID SELLER 02",
                "amount": 5000,
                "fares": {
                    "mdr": 10,
                    "fee": 0
                },
                "splits": [                
                    {
                        "sellerMerchantId": "MID DO MKP",
                        "amount": 1000,
                    },
                    {
                        "sellerMerchantId": "MID SELLER 02",
                        "amount": 9000,
                    }
                ]
            }
        ],
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": 0,
        "Capture": false,
        "Authenticate": false,
        "Recurrent": false,
        "CreditCard": {
            "CardNumber": "455187******0181",
            "Holder": "Teste Holder",
            "ExpirationDate": "12/2021",
            "SaveCard": false,
            "Brand": "Visa"
        },
        "Tid": "1006993069000AF39CAA",
        "ProofOfSale": "483594",
        "AuthorizationCode": "123456",
        "SoftDescriptor": "Split*LosCone",
        "Provider": "Cielo",
        "Eci": "7",
        "PaymentId": "c5b7ea6e-e604-463a-a1b1-123c3c598e05",
        "Amount": 1000,
        "ReceivedDate": "2017-08-30 14:37:14",
        "Status": 1,
        "ReturnMessage": "Transação autorizada",
        "ReturnCode": "00",
        "Type": "SplittedCreditCard",
        "Currency": "BRL",
        "Country": "BRA",
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "http://splitsandbox.braspag.com.br/schedules/c5b7ea6e-e604-463a-a1b1-123c3c598e05"
            },
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerydev.cieloecommerce.cielo.com.br/1/sales/c5b7ea6e-e604-463a-a1b1-123c3c598e05"
            },
            {
                "Method": "PUT",
                "Rel": "capture",
                "Href": "https://apidev.cieloecommerce.cielo.com.br/1/sales/c5b7ea6e-e604-463a-a1b1-123c3c598e05/capture"
            },
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apidev.cieloecommerce.cielo.com.br/1/sales/c5b7ea6e-e604-463a-a1b1-123c3c598e05/void"
            }
        ]
    }
}
```

**EXEMPLO 03 - 2 Sellers, sendo um deles o próprio MKP**


REQUEST 

Header
```
--request POST "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/"
--header "Content-Type: application/json"
--header "MerchantId: MID DO MKP"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
```
Body
```
{
   "MerchantOrderId":"2014111701",
   "Payment":{
     "Type":"SplittedCreditCard",
     "Amount":10000,
     "Installments":1,
     "SoftDescriptor":"Split*LosCone",
     "Capture":false,
     "CreditCard":{
         "CardNumber":"4551870000000181",
         "Holder":"Teste Holder",
         "ExpirationDate":"12/2021",
         "SecurityCode":"123",
         "Brand":"Visa"
     },
     "SplitPayments":[
        {
        "SellerMerchantId" :"MID SELLER 01",
        "Amount":5000,
        "Fares":{
            "Mdr":5,
            "Fee":0
        },
        {
        "SellerMerchantId" :"MID DO MKP",
        "Amount":5000,
        "Fares":{
            "Mdr":0,
            "Fee":0
        }
      }]
   }
}

```

RESPONSE

```
{
    "MerchantOrderId": "2014111701",
    "Customer": {
        "Name": "[Guest]"
    },
    "Payment": {
    "SplitPayments": [
            {
                "sellerMerchantId": "MID SELLER 01",
                "amount": 5000,
                "fares": {
                    ""mdr: 5,
                    "fee": 0
                },
                "splits": [                
                    {
                        "sellerMerchantId": "MID DO MKP",
                        "amount": 250,
                        
                    },
                    {
                        "sellerMerchantId": "MID SELLER 01",
                        "amount": 4750,
                        
                    }
                ]
            },
        	{
                "sellerMerchantId": "MID DO MKP",
                "amount": 5000,
                "fares": {
                    "mdr": 0,
                    "fee": 0
                },
                "splits": [                
                    {
                        "sellerMerchantId": "MID DO MKP",
                        "amount": 0,
                        "mdr": 0,
                        "fee": 0
                    },
                    {
                        "sellerMerchantId": "MID DO MKP",
                        "amount": 5000
                        "mdr": 100,
                        "fee": 0
                    }
                ]
            }
        ],
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": 0,
        "Capture": false,
        "Authenticate": false,
        "Recurrent": false,
        "CreditCard": {
            "CardNumber": "455187******0181",
            "Holder": "Teste Holder",
            "ExpirationDate": "12/2021",
            "SaveCard": false,
            "Brand": "Visa"
        },
        "Tid": "1006993069000AF39CAA",
        "ProofOfSale": "483594",
        "AuthorizationCode": "123456",
        "SoftDescriptor": "Split*LosCone",
        "Provider": "Cielo",
        "Eci": "7",
        "PaymentId": "c5b7ea6e-e604-463a-a1b1-123c3c598e05",
        "Amount": 1000,
        "ReceivedDate": "2017-08-30 14:37:14",
        "Status": 1,
        "ReturnMessage": "Transação autorizada",
        "ReturnCode": "00",
        "Type": "SplittedCreditCard",
        "Currency": "BRL",
        "Country": "BRA",
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "http://splitsandbox.braspag.com.br/schedules/c5b7ea6e-e604-463a-a1b1-123c3c598e05"
            },
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerydev.cieloecommerce.cielo.com.br/1/sales/c5b7ea6e-e604-463a-a1b1-123c3c598e05"
            },
            {
                "Method": "PUT",
                "Rel": "capture",
                "Href": "https://apidev.cieloecommerce.cielo.com.br/1/sales/c5b7ea6e-e604-463a-a1b1-123c3c598e05/capture"
            },
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apidev.cieloecommerce.cielo.com.br/1/sales/c5b7ea6e-e604-463a-a1b1-123c3c598e05/void"
            }
        ]
    }
}
```

#### Split Pós-Transacional

Esse modelo exige que o lojista envie uma atualização  (via `PUT`) na integração da API Cielo Ecommerce informando qual Sellers e Taxas a serão cobrados.

> EndPoint de atualização: https://apidev.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/split

Exemplo do Nó de SPLIT no `PUT`:
```
"SplitPayments":[{
        "SellerMerchantId" :"MID SELLER 01",
        "Amount":10000,
        "Fares":{
            "Mdr":5,
            "Fee":0
        }
```

| Propriedade                             | Descrição                                                                                   | Tipo   | Tamanho | Obrigatório |
|-----------------------------------------|---------------------------------------------------------------------------------------------|--------|---------|-------------|
| `SplitPayments.SellerMerchantId`        | Identificador do Seller                                                                     | GUID   | 36      | Sim         |
| `SplitPayments.Amount`                  | Valor da transação pertencente ao Seller                                                    | Número | 15      | Sim         |
| `SplitPayments.Fares.Mdr`               | Taxa do MKP (%) a ser retirada do Seller                                                    | Número | 2       | Sim         |
| `SplitPayments.Fares.Fee`               | Tarifa (R$) a ser cobrada do Seller - em Centavos                                           | Número | 15      | Sim         |


Com resposta, A API retornará um nó com as seguintes caracteristicas:

Parte do `RESPONSE`:
```
"SplitPayments": [
            {
                "sellerMerchantId": "MID SELLER 01",
                "amount": 10000,
                "fares": {
                    "mdr": 5,
                    "fee": 0
                },
                "splits": [                
                    {
                        "sellerMerchantId": "MID DO MKP",
                        "amount": 500,
                    },
                    {
                        "sellerMerchantId": "MID SELLER 01",
                        "amount": 9500,
                    }
                ]
            }
        ],
```



