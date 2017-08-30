---
title: Split de pagamentos
category: API CIELO ECOMMERCE
order: 1
---



### O que é o Split de pagamentos ?

O Split de pagamentos &eacute; uma funcionalidade Braspag que permite a divis&atilde;o de uma transa&ccedil;&otilde;es em diferentes lojas de maneira organizadada e hierarquizada.

O Split &eacute; uma estrutura transacional muito utilizada em MarketPlaces onde o carrinho &eacute; formado por produtos de diferentes fornecedores que receberar&atilde;o partes do pagamento em contas separadas.









### Como funciona o Split de Pagamentos.

O Split funciona como parte da integra&ccedil;&atilde;o transacional da Braspag.

Nesse modelo de integra&ccedil;&atilde;o existem 3 entidades:

| Entidades   | Descrição                                                                                                                                                                                |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Marketplace | Dono do carrinho e da Transação. <br> Possui Sellers que fornecem o contudo do Carrinho.<br> Realiza a cobrança de uma Taxa sobre a venda do Seller <br>                                 |
| Seller      | Lojas do Marketplace que fornecem os produtos que formam o carrinho.<br> Um Marketplace possui inumeros Seller. <br> Recebem parte da venda, descontado o valor da taxa do MarketPlace   |
| Braspag     | Responsavel pela auytorização da transação <br> Realiza a cobrança da Taxa definida pelo Marketplace, retirando esse valor da transação <br> Deposita o valor da Transação na conta do Seller <br> Deposita o valor da taxa cobrada pelo Marketplace so Seller |

O Fluxo transacional de autoriza&ccedil;&atilde;o e retirada de ocorre como na imagem abaixo:

![](/uploads/versions/Split---x----1398-720x---.png)


Descrevendo os Passos: 

1. O Marketplace cria o carrinho contendo dados sobre Sellers e a Taxa que deseja Cobrar
2. A Taxa do Marketplace é na verdade constituidade do Custo da operação e a margem que o Marketplace deseja obter.
3. A Braspag identifica e realiza a partilha dos valores.
4. O Seller recebe o valor da transação, menos o valor da Taxa de MKP.
5. A Braspag retira sua taxa de operação do Valor contindo no MKP. Esse valor é calculado sobre o valor total da transação e não sobre a participação do MKP na venda
6. O Marketplace recebe o valor restante da participação gerada com a taxa de Marketplace


> Na integração braspag, é possivel que dentro de um carrinho, o MarketPlace possa cobrar taxas diferentes dependendo o Seller.








### Tarifas e Custos

Nesta área do manual vamos detalhar como o processo de cobrança e custos afeta cada uma das entidades envolvidas no Split

#### Custo Marketplace

O Split de pagamentos Braspag funciona com base em uma taxa basica tabelada contratada pelo Marketplace e uma tarifa fixa sobre a transação executada. 
A Taxa braspag é cobrada sobre o valor da transação

> **Custo MKP:** TAXA BRASPAG + R$0,30


O valor da tarifa fixa Braspag é retirada do montante destinado ao marketplace.

#### Custo Seller

O Custo total operacional para o Seller é baseado na Taxa Braspag a ser retirada da participação do marketplace. Ela é gerada com base no valor da transação e a margem do MKP

> **Custo Seller:**   Taxa MKP = Margem MKP + (TAXA BRASPAG + R$0,30)





### transacional (Contrato)















```
 "SplitPayments":[{
        "SellerMerchantId" :"E41356F9-461C-43F3-BEE6-409A4A49DD29",
        "Amount":1000,
        "Fares":{
            "Mdr":3,
            "Fee":0
        }
```



| Propriedade                    | Tipo   | Tamanho | Obrigatório | Descrição |                  
|--------------------------------|--------|---------|-------------|-----------|
|`SplitPayments.SellerMerchantId`| Tipo   | Tamanho | Obrigatório | Descrição |
|`SplitPayments.Amount`          | Tipo   | Tamanho | Obrigatório | Descrição |
|`SplitPayments.Fares`           | Tipo   | Tamanho | Obrigatório | Descrição |
|`SplitPayments.Fares.Mdr`       | Tipo   | Tamanho | Obrigatório | Descrição |
|`SplitPayments.Fares.Fee`       | Tipo   | Tamanho | Obrigatório | Descrição |




REQUEST

```

--request POST "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary


{
   "MerchantOrderId":"2014111701",
   "Payment":{
     "Type":"SplittedCreditCard",
     "Amount":1000,
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
        "SellerMerchantId" :"E41356F9-461C-43F3-BEE6-409A4A49DD29",
        "Amount":1000,
        "Fares":{
            "Mdr":3,
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
                "sellerMerchantId": "E41356F9-461C-43F3-BEE6-409A4A49DD29",
                "amount": 10000,
                "fares": {
                    "mdr": 3,
                    "fee": 0
                },
                "splits": [                
                    {
                        "sellerMerchantId": "94d0ba02-9c0e-4eac-be4e-3a9cc24574c1",
                        "amount": 100,
                        "mdr": 1,
                        "fee": 0
                    },
                    {
                        "sellerMerchantId": "e41356f9-461c-43f3-bee6-409a4a49dd29",
                        "amount": 9900,
                        "mdr": 99,
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



### Quais features não podem ser usadas (Tokens / Recorrencia)

### Definições de regras de Split (Durante transacional/pós Transacional)






























