---
title: Split de Pagamentos
category: API CIELO E-COMMERCE
order: 1
---


### O que é o Split de Pagamentos ?
<BR>
O Split de Pagamentos permite a divisão de uma transação entre diferentes participantes de uma venda.

Muito utilizado em Marketplaces, onde **o carrinho é composto por produtos de diferentes fornecedores** e o valor total da venda deve ser dividido entre os participantes.

| **Participantes** | **Descrição** | 
|-----------|---------- |
| **Marketplace** | Responsável pelo carrinho. <BR> Possui subordinados (**Subordinados**) que fornecem os produtos que compõem o carrinho.<BR> Define um MDR\* + Taxa (opcional) a ser descontado sobre a venda do **Subordinado**.<BR>  | 
| **Subordinados** | Fornecedor dos produtos que compõem o carrinho.<BR>Recebe parte do valor da venda, descontadoo o MDR\* do **Marketplace**.<BR>  |
| **Braspag (Facilitador)** | Responsável pelo fluxo transacional.<BR> Define um MDR\* + Taxa a serem descontadas sobre o valor total da venda realizada pelo **Marketplace**.<br> Responsável pela liquidação dos pagamentos para os **Subordinados** e **Marketplace**.|

> \***MDR**  (*Merchant Discount Rate*): Percentual a ser descontado sobre o valor de uma transação.

### Como funciona o Split de Pagamentos?
<BR>
No Split de Pagamentos o responsável pelo fluxo transacional é o facilitador.

O Marketplace se integra à Braspag para transacionar e informa como será divida a transação entre cada paticipante, podendo ser no momento de captura ou em um momento posterior, conhecido como split pós-transacional, desde que seja dentro de um limite de tempo pré-estabelecido. 

Com a transação capturada, a Braspag calcula o valor destinado a cada participante e repassa esses valores, no prazo estabelecido de acordo com cada produto (regime de pagamento\*), para cada envolvido na transação. 

> \***Regime de Pagamento**: Prazo estabelecido para liquidação de acordo com o produto (crédito ou débito).<BR>
> **Crédito**: Em até 31 dias.<BR>
> **Crédito** Parcelado: 1º parcela em até 31 dias, demais a cada 30.<BR>
> **Débito**: Em até 1 dia útil<BR>

Para utilizar o Split de Pagamentos, o **Marketplace** deverá se cadastrar na Braspag informando seus **Subordinados**. Após este processo tanto o **Marketplace** quanto seus supordinados possuirão um identificador único que deverá ser utlizado nas divisões de uma transação. 

A divisão de uma transação deve serguir as seguintes regras:

* Informar os identificadores dos **Subordinados**.
* Os valores de participação de cada **Subordinado**. O somatório deverá ser igual ao valor total da transação.
* MDRs + Taxas (opcional) a ser aplicado sobre o valor de cada **Subordinado** que será destinado ao **Marketplace**. Deverá ser acordado previamente entre o **Marketplace** e o **Subordinado**.

O **Marketplace** também pode ser um participante da divisão. Para isso basta informar seu identificador na divisão, passando o mesmo a ter também o papel de **Subordinado** e ter seus próprios produtos no carrinho. 

### Taxas
<BR>
#### Braspag (Facilitador)
<BR>
Para cada transação a Braspag acordará um **MDR** e uma **Taxa** com o **Marketplace**.

O **Marketplace** com o conhecimento destas taxas negociará o seu MDR + Taxa juntamente com seus **Subornidanos**, 

> O MDR acordado entre um **Marketplace** e um subordinado ser menor que o MDR acordado entreo o **Marketplace** e a **Braspag**. 


> **Custo Marketplace:** TAXA BRASPAG (%) + TARIFA FIXA (R$)

1 - A `taxa Braspag` é vinculada ao valor total da transação do Subordinate e não sobre o valor que Marketplace irá receber.<BR><BR>
**EX:** 2% sobre a Venda de R$100,00 do Subordinate<BR><BR>
2 - O valor da tarifa fixa Braspag é debitada do montante destinado ao Marketplace. <BR><BR>
**EX:** 2% sobre a Venda de R$100,00 do Subordinate = R$2,00, será retirado da fatia cobrada pelo Marketplace do Subordinate<BR><BR>


Abaixo um exemplo de como a divisão de valores é realizada por transação:

![Salve a imagem para uma maior resolução](http://able-caribou.cloudvent.net/images/Split/splitF.jpg)



#### Custo Subordinate

O Unico custo para o Subordinate no Split de pagamento Braspag é a Taxa que o Marketplace cobra sobre transações geradas.
Essa taxa normalmente é formada por uma **Margem de lucro Percentual** aplicada sobre o **custo transacional**.

Abaixo demonstramos como essa Taxa é formada pelo Marketplace com base no custo Braspag.

> **Custo Subordinate:** Taxa Marketplace = {Margem Marketplace + (TAXA BRASPAG (%) + TARIFA FIXA (R$))}


### Integrando o Split

O Split funciona como parte da API transacional da Braspag via API Cielo Ecommerce.

A integração A ser realizara é a mesma descrita para transações de cartão de crédito dentro da API Cielo Ecommerce.
Para mais informações, veja este [Manual de Integração](https://developercielo.github.io/Webservice-3.0/#criando-uma-transação-simples)

Neste manual serão apresentados os contratos de integração da API Cielo ecommerce, mas o foco da analise se dará nos paramêtros do Split de pagamentos.

> O Split está disponivel apenas para transações de CARTÃO DE CRÉDITO



Abaixo um exemplo do fluxo transacional e regras de divisão:


![Salve a imagem para uma maior resolução](http://able-caribou.cloudvent.net/images/Split/split0.jpg)


**EX:** Uma venda de R$100, realizada pelo **Marketplace** com participação do **Subordinado 01**.


1. O **Marketplace** tem um MDR de **5%** acordado com o **Subordinado 01**
2. Essa Taxa é formada por uma margem de `3 Pontos Percentuais` sobre o total da transação + o custo braspag (`2 pontos percentuais + 0,30 centavos`) sobre o total da transação
3. A Transação é processada. O **Subordinate** recebe o montante da venda menos a `Taxa Marketplace`
4. O Marketplace Recebe a parte da transação menos o custo da taxa Braspag.

> ** *Importante* **: As porcentagens exibidas no exemplo não são aplicadas uma sobre as outras, mas sim como pontos percentuais que somados formam uma taxa unica sobre o Subordinate. Isso ocorre pois o valor das taxas sempre é cobrado sobre o total da transação do Subordinate.


### Tipos de Split

O Split de pagamentos possui dois tipos basicos de integração:


| Tipo                 | Descrição                                                                                                                          |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------|
| **Split Transacional**     | O marketplace envia na transação quais os Subordinates, os valores e Taxas a sofrerem SPLIT                                             |
| **Split Pós-Transacional** | O marketplace define quais os Subordinates, valores e Taxas a sofrerem SPLIT depois autorização realizando uma atualização da transação |

Cada modelo possui um contrato adicional de integração a API Cielo Ecommerce e Braspag, que será apresentado a seguir.


#### Split Transacional

Esse modelo exige que o lojista envie um "nó" adicional na integração da API Cielo Ecommerce onde serão inclusos os dados do pagamento, Subordinates e Taxas a serem cobradas.

Exemplo do Nó de SPLIT no `POST` criando a transação:
```
"SplitPayments":[{
        "SubordinateMerchantId" :"MID Subordinate 01",
        "Amount":10000,
        "Fares":{
            "Mdr":5,
            "Fee":0
        }
```

| Propriedade                             | Descrição                                                                                   | Tipo   | Tamanho | Obrigatório |
|-----------------------------------------|---------------------------------------------------------------------------------------------|--------|---------|-------------|
| `SplitPayments.SubordinateMerchantId`        | Identificador do Subordinate                                                                     | GUID   | 36      | Sim         |
| `SplitPayments.Amount`                  | Valor da transação pertencente ao Subordinate                                                    | Número | 15      | Sim         |
| `SplitPayments.Fares.Mdr`               | Taxa do Marketplace (%) a ser retirada do Subordinate                                                    | Número | 2       | Sim         |
| `SplitPayments.Fares.Fee`               | Tarifa (R$) a ser cobrada do Subordinate - em Centavos                                           | Número | 15      | Sim         |


Com resposta, A API retornará um nó com as seguintes caracteristicas:

Parte do `RESPONSE`:
```
"SplitPayments": [
            {
                "SubordinateMerchantId": "MID Subordinate 01",
                "amount": 10000,
                "fares": {
                    "mdr": 5,
                    "fee": 0
                },
                "splits": [                
                    {
                        "SubordinateMerchantId": "MID DO Marketplace",
                        "amount": 500,
                    },
                    {
                        "SubordinateMerchantId": "MID Subordinate 01",
                        "amount": 9500,
                    }
                ]
            }
        ],
```

| Propriedade                             | Descrição                                                                                   | Tipo   | Tamanho | Obrigatório |
|-----------------------------------------|---------------------------------------------------------------------------------------------|--------|---------|-------------|
| `SplitPayments.SubordinateMerchantId`        | Identificador do Subordinate                                                                     | GUID   | 36      | Sim         |
| `SplitPayments.Amount`                  | Valor da transação pertencente ao Subordinate                                                    | Número | 15      | Sim         |
| `SplitPayments.Fares.Mdr`               | Taxa do Marketplace (%) a ser retirada do Subordinate                                                    | Número | 2       | Sim         |
| `SplitPayments.Fares.Fee`               | Tarifa (R$) a ser cobrada do Subordinate - em Centavos                                           | Número | 15      | Sim         |
| `SplitPayments.split.SubordinateMerchantId.` | Identificador do Subordinate ou Marketplace incluso no Split                                             | GUID   | 36      | Sim         |
| `SplitPayments.split.Amount`            | Valor da transação a ser depositado no Subordinate ou Marketplace, descontado a Taxa Marketplace e/ou Taxa Cielo | Número | 15      | Sim         |





O nó `SPLIT` contido na resposta da transação retorna informações especificas a respeito dos valores a serem depositados para cada entidade do split:


![Salve a imagem para uma maior resolução](http://able-caribou.cloudvent.net/images/Split/Split03.jpg)


O Split aceita varias combinações para transações entre diferentes atores:

* Apenas 1 Subordinate - Sem taxa ou tarifa Braspag
* 2 Subordinates dividindo a mesma transação - Com taxa e tarifa Braspag
* 2 Subordinates, sendo um deles o próprio Marketplace - Com taxa e tarifa Braspag


**EXEMPLO 01 -  Apenas 1 Subordinate**

Neste primeiro exemplo, para simplificar como a apresentação do Modelo de integração, o Marketplace não sofrerá cobrança de taxa ou tarifa Braspag


REQUEST 

Header
```
--request POST "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/"
--header "Content-Type: application/json"
--header "MerchantId: MID DO Marketplace"
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
        "SubordinateMerchantId" :"MID Subordinate 01",
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
                "SubordinateMerchantId": "MID Subordinate 01",
                "amount": 10000,
                "fares": {
                    "mdr": 5,
                    "fee": 0
                },
                "splits": [                
                    {
                        "SubordinateMerchantId": "MID DO Marketplace",
                        "amount": 500,
                    },
                    {
                        "SubordinateMerchantId": "MID Subordinate 01",
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


**EXEMPLO 02 - 2 Subordinates dividindo a mesma transação**

Neste exemplo, o Marketplace será cobrado pela Braspag. 
Vamos usar como base:

* **Taxa Braspag:** 2% sobre o valor da Venda do Subordinate
* **Tarifa Braspag:** R$0,30 por transação do Subordinate



REQUEST 

Header
```
--request POST "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/"
--header "Content-Type: application/json"
--header "MerchantId: MID DO Marketplace"
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
        "SubordinateMerchantId" :"MID Subordinate 01",
        "Amount":10000,
        "Fares":{
            "Mdr":5,
            "Fee":0
        },
        {
        "SubordinateMerchantId" :"MID Subordinate 02",
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
                "SubordinateMerchantId": "MID Subordinate 01",
                "amount": 10000,
                "fares": {
                    "mdr": 5,
                    "fee": 0
                },
                "splits": [                
                    {
                        "SubordinateMerchantId": "MID DO Marketplace",
                        "amount": 500,
                    },
                    {
                        "SubordinateMerchantId": "MID Subordinate 01",
                        "amount": 9500,
                   }
                ]
            },
        	{
                "SubordinateMerchantId": "MID Subordinate 02",
                "amount": 5000,
                "fares": {
                    "mdr": 10,
                    "fee": 0
                },
                "splits": [                
                    {
                        "SubordinateMerchantId": "MID DO Marketplace",
                        "amount": 1000,
                    },
                    {
                        "SubordinateMerchantId": "MID Subordinate 02",
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

**EXEMPLO 03 - 2 Subordinates, sendo um deles o próprio Marketplace**


REQUEST 

Header
```
--request POST "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/"
--header "Content-Type: application/json"
--header "MerchantId: MID DO Marketplace"
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
        "SubordinateMerchantId" :"MID Subordinate 01",
        "Amount":5000,
        "Fares":{
            "Mdr":5,
            "Fee":0
        },
        {
        "SubordinateMerchantId" :"MID DO Marketplace",
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
                "SubordinateMerchantId": "MID Subordinate 01",
                "amount": 5000,
                "fares": {
                    ""mdr: 5,
                    "fee": 0
                },
                "splits": [                
                    {
                        "SubordinateMerchantId": "MID DO Marketplace",
                        "amount": 250,
                        
                    },
                    {
                        "SubordinateMerchantId": "MID Subordinate 01",
                        "amount": 4750,
                        
                    }
                ]
            },
        	{
                "SubordinateMerchantId": "MID DO Marketplace",
                "amount": 5000,
                "fares": {
                    "mdr": 0,
                    "fee": 0
                },
                "splits": [                
                    {
                        "SubordinateMerchantId": "MID DO Marketplace",
                        "amount": 0,
                        "mdr": 0,
                        "fee": 0
                    },
                    {
                        "SubordinateMerchantId": "MID DO Marketplace",
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

Esse modelo exige que o lojista envie uma atualização da transação (via `PUT`) na integração da API Cielo Ecommerce informando qual Subordinates e Taxas a serão cobrados.


> EndPoint de atualização: https://apidev.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/split

Exemplo do Nó de SPLIT no `PUT`:
```
"SplitPayments":[{
        "SubordinateMerchantId" :"MID Subordinate 01",
        "Amount":10000,
        "Fares":{
            "Mdr":5,
            "Fee":0
        }
```

| Propriedade                             | Descrição                                                                                   | Tipo   | Tamanho | Obrigatório |
|-----------------------------------------|---------------------------------------------------------------------------------------------|--------|---------|-------------|
| `SplitPayments.SubordinateMerchantId`        | Identificador do Subordinate                                                                     | GUID   | 36      | Sim         |
| `SplitPayments.Amount`                  | Valor da transação pertencente ao Subordinate                                                    | Número | 15      | Sim         |
| `SplitPayments.Fares.Mdr`               | Taxa do Marketplace (%) a ser retirada do Subordinate                                                    | Número | 2       | Sim         |
| `SplitPayments.Fares.Fee`               | Tarifa (R$) a ser cobrada do Subordinate - em Centavos                                           | Número | 15      | Sim         |


Com resposta, A API retornará um nó com as seguintes caracteristicas:

Parte do `RESPONSE`:
```
"SplitPayments": [
            {
                "SubordinateMerchantId": "MID Subordinate 01",
                "amount": 10000,
                "fares": {
                    "mdr": 5,
                    "fee": 0
                },
                "splits": [                
                    {
                        "SubordinateMerchantId": "MID DO Marketplace",
                        "amount": 500,
                    },
                    {
                        "SubordinateMerchantId": "MID Subordinate 01",
                        "amount": 9500,
                    }
                ]
            }
        ],
```



