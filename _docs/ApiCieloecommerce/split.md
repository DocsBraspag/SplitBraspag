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
| **Marketplace** | Responsável pelo carrinho. <BR> Possui subordinados (**Subordinados**) que fornecem os produtos que compõem o carrinho.<BR> Define MDR\* e Tarifa (opcional) a serem descontados sobre a venda do **Subordinado**.<BR>  | 
| **Subordinados** | Fornecedor dos produtos que compõem o carrinho.<BR>Recebe parte do valor da venda, descontado o MDR\* do **Marketplace**.<BR>  |
| **Braspag (Facilitador)** | Responsável pelo fluxo transacional.<BR> Define um MDR\* e Tarifa a serem descontados sobre o valor total da venda realizada pelo **Marketplace**.<br> Responsável pela liquidação dos pagamentos para os **Subordinados** e **Marketplace**.|

> \***MDR**  (*Merchant Discount Rate*): Percentual a ser descontado sobre o valor de uma transação.

### Como funciona o Split de Pagamentos?
<BR>
No Split de Pagamentos o responsável pelo fluxo transacional é o facilitador.

O Marketplace se integra à Braspag para transacionar e informa como será divida a transação entre cada paticipante, podendo ser no momento de captura ou em um momento posterior, conhecido como split pós-transacional, desde que seja dentro de um limite de tempo pré-estabelecido. 

Com a transação capturada, a Braspag calcula o valor destinado a cada participante e repassa esses valores, no prazo estabelecido de acordo com cada produto (regime de pagamento\*), para cada envolvido na transação. 

> \***Regime de Pagamento**: Prazo estabelecido para liquidação de acordo com o produto (crédito ou débito) e bandeira.<BR>
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
A Braspag acordará um **MDR** e uma **Tarifa Fixa** com o **Marketplace** a serem descontados em cada transação.

O **Marketplace** com, o conhecimento destas taxas, negociará o seu **MDR** e uma **Tarifa Fixa (opcional)** juntamente com seus **Subornidandos**, já embutindo o seu **MDR** junto à **Braspag**.

O desconto da **Tarifa Fixa** não é aplicado no valor total da transação, não entrando no cálculo da divisão e sim sendo debitado do montande que o **Marketplace** tem para receber junto à **Braspag**. O **MDR** entra no cálculo de divisão da transação, já que o mesmo deve estar embutido no **MDR** acordado entre o **Marketplace** e seus **Subordinados.**

> **Custo Marketplace:** MDR Braspag(%) + Tarifa Fixa(R$)

> O MDR acordado entre um **Marketplace** e um **Subordinado** deve ser sempre maior que o MDR acordado entre a **Braspag** e o **Marketplace**. 

<BR>
#### Marketplace
<BR>
O **Marketplace** é responsável por acordar as taxas a serem cobradas de seus **Subordinados**, onde dever ser defindo um **MDR** maior ou igual ao **MDR** definido entre a **Braspag** e o **Marketplace** e uma Tarifa Fixa, que é opcional. 

> **Custo Subordinado:** MDR Marketplace(%) + Tarifa Fixa(R$), onde no MDR Marketplace(%) está embutido o MDR Braspag(%).

<BR>
#### Exemplo
<BR>
Uma transação de **R$100**, realizada por um **Marketplace** com participação do **Subordinado 01**.

![Split](http://able-caribou.cloudvent.net/images/Split/Split.PNG)

Neste exemplo, foram assumidos os seguintes acordos:

**Taxa Braspag**: 2% MDR + R$0,30 Tarifa Fixa.  
**Taxa Marketplace**: 5% MDR, já embutindo os 2% do MDR Braspag + 0,30 Tarifa Fixa.

Após o split, cada participante terá sua agenda sensibilizada com os seguintes eventos:

**Subordinado**:  
Crédito: R$94,70 = R$100 - 5%.

**Marketplace**:  
Crédito: R$3,30 = R$100 * (5% - 2%) + R$0,30.  
Débito: R$0,30 (Tarifa Fixa Braspag)

**Braspag (Facilitador)**:  
Crédito: R$2,00 = R$100 * 2%.  
Crédito: R$0,30 (Tarifa Fixa Braspag)

### Utilizando o Split de Pagamentos
<BR>
O Split de Pagamentos é parte da API Cielo E-Commerce, desenvolvida utilizando a arquitetura REST e JSON como mensageria. Para mais informações sobre a API Cielo E-Commerce, consulte o [Manual de Integração](https://developercielo.github.io/Webservice-3.0/#criando-uma-transação-simples) da Plataforma.

OBS: Neste manual serão apresentados os contratos de integração da API Cielo E-Commerce, porém o foco da análise será nas operações referentes ao Split de Pagamentos.

> Atualmente o Split de Pagamentos está disponivel para os seguintes tipos de pagamento:
> * Cartão de Crédito
  
#### Autorização
  
A autorização de uma transação no Split de Pagamentos deve ser realizada através da API Cielo E-Commerce seguindo os mesmos contratos descritos na documentação da plataforma.

Porém, para indentificar que a transação enviada se trata de uma transação de Split de Pagamentos, deve-se modificar o tipo de pagamento utilizado, conforme abaixo:

|                     | **Cartão de Crédito**  | **Cartão de Débito**  | 
| **Transação Comum** | CreditCard             | DebitCard             |
| **Transação Split** | SpplitedCreditCard     | SplittedDebitCard     |



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



