---
title: Split de pagamentos
category: API CIELO ECOMMERCE
order: 1
---


### O que &eacute; o Split de pagamentos ?

O Split de pagamentos é uma funcionalidade Braspag que permite a divisão de uma transações em diferentes lojas de maneira organizadada e hierarquizada.

O Split é uma estrutura transacional muito utilizada em MarketPlaces onde o carrinho é formado por produtos de diferentes fornecedores que receberarão partes do pagamento em contas separadas.




### Como funciona o Split de Pagamentos.


O Split funciona como parte da integração transacional da Braspag.
Nesse modelo de integração existem 3 entidades:

| Entidades | Descrição | 
|-----------|---------- |
| Marketplace | Dono do carrinho e da Transação. <BR> Possui Sellers que fornecem o contudo do Carrinho.<BR> Realiza a cobrança de uma Taxa sobre a venda do Seller | 
| Seller | Lojas do Marketplace que fornecem os produtos que formam o carrinho.<BR> Um Marketplace possui inumeros Seller. <BR>Recebem parte da venda, descontado o valor da taxa do MarketPlace |
| Braspag | Responsavel pela autorização da transação.<BR>Realiza a cobrança da Taxa definida pelo Marketplace, retirando esse valor da transação.<BR> Deposita o valor da Transação na conta do Seller.<BR> Deposita o valor da taxa cobrada pelo Marketplace so Seller |



O Fluxo transacional de autorização e retirada de ocorre como na imagem abaixo:



Descrevendo os Passos:

1. O Marketplace cria o carrinho contendo dados sobre Sellers e a Taxa que deseja Cobrar
2. A Taxa do Marketplace &eacute; na verdade constituidade do Custo da opera&ccedil;&atilde;o e a margem que o Marketplace deseja obter.
3. A Braspag identifica e realiza a partilha dos valores.
4. O Seller recebe o valor da transa&ccedil;&atilde;o, menos o valor da Taxa de MKP.
5. A Braspag retira sua taxa de opera&ccedil;&atilde;o do Valor contindo no MKP. Esse valor &eacute; calculado sobre o valor total da transa&ccedil;&atilde;o e n&atilde;o sobre a participa&ccedil;&atilde;o do MKP na venda
6. O Marketplace recebe o valor restante da participa&ccedil;&atilde;o gerada com a taxa de Marketplace

> Na integração braspag, é possivel que dentro de um carrinho, o MarketPlace possa cobrar taxas diferentes dependendo o Seller.







### Tarifas e Custos

Nesta área do manual vamos detalhar como o processo de cobrança e custos afeta cada uma das entidades envolvidas no Split




#### Custo Marketplace

O Split de pagamentos Braspag funciona com base em uma taxa basica tabelada contratada pelo Marketplace e uma tarifa fixa sobre a transação executada. A Taxa braspag é cobrada sobre o valor da transação


> **Custo MKP:** TAXA BRASPAG + R$0,30

O valor da tarifa fixa Braspag é retirada do montante destinado ao marketplace.

#### Custo Seller

O Custo total operacional para o Seller é baseado na Taxa Braspag a ser retirada da participação do marketplace. Ela é gerada com base no valor da transação e a margem do MKP

> **Custo Seller:** Taxa MKP = Margem MKP + (TAXA BRASPAG + R$0,30)

### transacional (Contrato)

```
"SplitPayments":[{
        "SellerMerchantId" :"MID SELLER 01",
        "Amount":100,
        "Fares":{
            "Mdr":3,
            "Fee":0
        }
```

| Propriedade | Tipo | Tamanho | Obrigatório | Descrição |
| --- | --- | --- | --- | --- |
| `SplitPayments.SellerMerchantId`| Tipo | Tamanho | Obrigatório | Descrição |
| `SplitPayments.Amount` | Tipo | Tamanho | Obrigatório | Descrição |
| `SplitPayments.Fares`| Tipo | Tamanho | Obrigatório | Descrição |
| `SplitPayments.Fares.Mdr` | Tipo | Tamanho | Obrigatório | Descrição |
| `SplitPayments.Fares.Fee` | Tipo | Tamanho | Tipo | Tamanho | Obrigatório | Descrição |






EXEMPLO 01 -  Apenas 1 Seller


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


EXEMPLO 02 - 2 Sellers dividindo a mesma transação



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
        "SellerMerchantId" :"MID SELLER 02",
        "Amount":5000,
        "Fares":{
            "Mdr":10,
            "Fee":0
        }
      }]
   }
}

```

RESPONSE


| `SplitPayments.SellerMerchantId`| Tipo | Tamanho | Obrigatório | Descrição |


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
                    "mdr": 5,
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
                "sellerMerchantId": "MID SELLER 02",
                "amount": 5000,
                "fares": {
                    "mdr": 10,
                    "fee": 0
                },
                "splits": [                
                    {
                        "sellerMerchantId": "MID DO MKP",
                        "amount": 500,
                    },
                    {
                        "sellerMerchantId": "MID SELLER 02",
                        "amount": 4500,
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

EXEMPLO 03 - 2 Sellers, sendo um deles o MKP


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



