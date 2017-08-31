---
title: Split de pagamentos
category: API CIELO ECOMMERCE
order: 1
---


### O que &eacute; o Split de pagamentos ?

O Split de pagamentos &eacute; uma funcionalidade Braspag que permite a divis&atilde;o de uma transa&ccedil;&otilde;es em diferentes lojas de maneira organizadada e hierarquizada.

Ele funcionando recebendo uma transa&ccedil;&atilde;o que pode ser subdividida para diferentes atores. Os valores dessa transa&ccedil;&atilde;o &eacute; dividido e pago em contas bancarias separadas.

Essa &eacute; uma estrutura transacional muito utilizada em MarketPlaces (MKP), onde **o carrinho &eacute; formado por produtos de diferentes fornecedores** que receberar&atilde;o partes do pagamento total da transa&ccedil;&atilde;o. O Split &eacute; uma funcionalidade que possui as seguintes vantagens:

* Evita que comprador realize v&aacute;rias transa&ccedil;&otilde;es, elevando a chance de convers&atilde;o
* Permite que um Marketplace possa cobrar uma taxa sobre o valor transacional utilizando apenas uma integra&ccedil;&atilde;o t&eacute;cnica
* Permite que o Marketplace possa montar um carrinho com produtos de diferentes fornecedores de maneira total transparente para o comprador.

### Como funciona o Split de Pagamentos.

O Split de pagamento funciona quando um Marketplace realiza uma transa&ccedil;&atilde;o e-commerce enviado a Braspag sobre como esse pagamento ser&aacute; dividido e quais participantes ser&atilde;o cobrados ou receber&aacute; o valor vendido.

Nesse modelo de split de pagamentos existem 3 entidades b&aacute;sicas:

| **Entidades** | **Descri&ccedil;&atilde;o** |
| --- | --- |
| **Marketplace** | Dono do carrinho e da Transa&ccedil;&atilde;o.
<br>Possui Sellers que fornecem o contudo do Carrinho.
<br>Realiza a cobran&ccedil;a de uma Taxa sobre a venda do Seller |
| **Seller** | Lojas do Marketplace que fornecem os produtos que formam o carrinho.
<br>Um Marketplace possui inumeros Seller.
<br>Recebem parte da venda, descontado o valor da taxa do MarketPlace |
| **Braspag** | Responsavel pela autoriza&ccedil;&atilde;o da transa&ccedil;&atilde;o.
<br>Realiza a cobran&ccedil;a da Taxa definida pelo Marketplace, retirando esse valor da transa&ccedil;&atilde;o.
<br>Deposita o valor da Transa&ccedil;&atilde;o na conta do Seller.
<br>Deposita o valor da taxa cobrada pelo Marketplace so Seller |

O Split &eacute; um processo de divis&atilde;o transacional, onde o **Marketplace** separa o valor pertecente da transa&ccedil;&atilde;o do **Seller** em duas partes:

* **Taxa do MKP:** Taxa cobrada pelo marketplace sobre o Seller para que a venda seja realizada. Pode ser um valor **%** ou/e uma **taxa fixa por venda.**
* **Valor da venda:** Parte do valor da transa&ccedil;&atilde;o/carrinho que ser&aacute; destinada ao Seller, menos o valor cobrado como **Taxa do MKP**

A **Braspag** receber&aacute; a transa&ccedil;&atilde;o, separando a transa&ccedil;&atilde;o entre valores a serem direcionados ao MKP, originado da **Taxa do MKP** e aos Sellers, originado do valor devido dentro da venda.

> **Sobre Contas**: Ao utilizar o SPLIT, o Markteplace dever&aacute; junto aos seus Sellers se cadastrar na **Braspag**, assim recebendo um identificador de loja e registrando uma conta bancaria para cada ator como ponto de deposito dos valores Splitados

Abaixo um exemplo do Fluxo transacional de autoriza&ccedil;&atilde;o e divis&atilde;o de valores no Split de pagamentos:

> Teste

> > Teste02

> > > teste 03

EX: Uma venda de R$100, feita pelo **Seller** no Marketplace **MKP**

![](/uploads/versions/Split1---x----1398-720x---.jpg)

&nbsp;

1. O **MKP** tem Taxa de **5%** Sobre a venda do **Seller**
2. Essa Taxa &eacute; formada por uma margem de `3 Pontos Percentuais`{: .highlighter-rouge} sobre o total da transa&ccedil;&atilde;o + o custo braspag (`2 pontos percentuais + 0,30 centavos`{: .highlighter-rouge}) sobre o total da transa&ccedil;&atilde;o
3. A Transa&ccedil;&atilde;o &eacute; processada. O **Seller** recebe o montante da venda - a `Taxa MKP`{: .highlighter-rouge}
4. O MKP Recebe a parte da transa&ccedil;&atilde;o menos o custo da taxa Braspag.

&nbsp;

![](/uploads/versions/Split2---x----1459-1033x---.jpg)

### Tarifas e Custos

Nesta &aacute;rea do manual vamos detalhar como o processo de cobran&ccedil;a e custos afeta cada uma das entidades envolvidas no Split

#### Custo Marketplace

O Split de pagamentos Braspag funciona com base em uma taxa basica tabelada contratada pelo Marketplace e uma tarifa fixa sobre a transa&ccedil;&atilde;o executada. A Taxa braspag &eacute; cobrada sobre o valor da transa&ccedil;&atilde;o

> **Custo MKP:** TAXA BRASPAG (%) + TARIFA FIXA (R$)

1 - A `taxa Braspag`{: .highlighter-rouge} &eacute; vinculada ao valor total da transa&ccedil;&atilde;o do Seller e n&atilde;o sobre o valor que MKP ir&aacute; receber.
<br>
<br>**EX:** 2% sobre a Venda de R$100,00 do SELLER
<br>
<br>2 - O valor da tarifa fixa Braspag &eacute; debitada do montante destinado ao MKP.
<br>
<br>**EX:** 2% sobre a Venda de R$100,00 do SELLER = R$2,00, ser&aacute; retirado da Fatia cobrada pelo MKP do Seller
<br>&nbsp;

#### Custo Seller

O Custo total operacional para o Seller &eacute; baseado na Taxa Braspag a ser retirada da participa&ccedil;&atilde;o do marketplace. Ela &eacute; gerada com base no valor da transa&ccedil;&atilde;o e a margem do MKP

> **Custo Seller:** Taxa MKP = {Margem MKP + (TAXA BRASPAG (%) + TARIFA FIXA (R$))}

### Integrando o Split

O Split funciona como parte da API transacional da Braspag via API Cielo Ecommerce.

<div class="highlighter-rouge"><pre class="highlight"><code>"SplitPayments":[{
        "SellerMerchantId" :"MID SELLER 01",
        "Amount":10000,
        "Fares":{
            "Mdr":5,
            "Fee":0
        }
</code></pre></div>

| Propriedade | Tipo | Tamanho | Obrigat&oacute;rio | Descri&ccedil;&atilde;o | &nbsp; | &nbsp; |
| --- | --- | --- | --- | --- | --- | --- |
| `SplitPayments.SellerMerchantId`{: .highlighter-rouge} | Identificador do Seller | Tamanho | Obrigat&oacute;rio | Descri&ccedil;&atilde;o | &nbsp; | &nbsp; |
| `SplitPayments.Amount`{: .highlighter-rouge} | Tipo | Tamanho | Obrigat&oacute;rio | Descri&ccedil;&atilde;o | &nbsp; | &nbsp; |
| `SplitPayments.Fares`{: .highlighter-rouge} | Tipo | Tamanho | Obrigat&oacute;rio | Descri&ccedil;&atilde;o | &nbsp; | &nbsp; |
| `SplitPayments.Fares.Mdr`{: .highlighter-rouge} | Tipo | Tamanho | Obrigat&oacute;rio | Descri&ccedil;&atilde;o | &nbsp; | &nbsp; |
| `SplitPayments.Fares.Fee`{: .highlighter-rouge} | Tipo | Tamanho | Tipo | Tamanho | Obrigat&oacute;rio | Descri&ccedil;&atilde;o |

EXEMPLO 01 - Apenas 1 Seller

REQUEST

Header

<div class="highlighter-rouge"><pre class="highlight"><code>--request POST "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/"
--header "Content-Type: application/json"
--header "MerchantId: MID DO MKP"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
</code></pre></div>

Body

<div class="highlighter-rouge"><pre class="highlight"><code><span class="p">{</span><span class="w">
   </span><span class="nt">"MerchantOrderId"</span><span class="p">:</span><span class="s2">"2014111701"</span><span class="p">,</span><span class="w">
   </span><span class="nt">"Payment"</span><span class="p">:{</span><span class="w">
     </span><span class="nt">"Type"</span><span class="p">:</span><span class="s2">"SplittedCreditCard"</span><span class="p">,</span><span class="w">
     </span><span class="nt">"Amount"</span><span class="p">:</span><span class="mi">10000</span><span class="p">,</span><span class="w">
     </span><span class="nt">"Installments"</span><span class="p">:</span><span class="mi">1</span><span class="p">,</span><span class="w">
     </span><span class="nt">"SoftDescriptor"</span><span class="p">:</span><span class="s2">"Split*LosCone"</span><span class="p">,</span><span class="w">
     </span><span class="nt">"Capture"</span><span class="p">:</span><span class="kc">false</span><span class="p">,</span><span class="w">
     </span><span class="nt">"CreditCard"</span><span class="p">:{</span><span class="w">
         </span><span class="nt">"CardNumber"</span><span class="p">:</span><span class="s2">"4551870000000181"</span><span class="p">,</span><span class="w">
         </span><span class="nt">"Holder"</span><span class="p">:</span><span class="s2">"Teste Holder"</span><span class="p">,</span><span class="w">
         </span><span class="nt">"ExpirationDate"</span><span class="p">:</span><span class="s2">"12/2021"</span><span class="p">,</span><span class="w">
         </span><span class="nt">"SecurityCode"</span><span class="p">:</span><span class="s2">"123"</span><span class="p">,</span><span class="w">
         </span><span class="nt">"Brand"</span><span class="p">:</span><span class="s2">"Visa"</span><span class="w">
     </span><span class="p">},</span><span class="w">
     </span><span class="nt">"SplitPayments"</span><span class="p">:[{</span><span class="w">
        </span><span class="nt">"SellerMerchantId"</span><span class="w"> </span><span class="p">:</span><span class="s2">"MID SELLER 01"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Amount"</span><span class="p">:</span><span class="mi">10000</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Fares"</span><span class="p">:{</span><span class="w">
            </span><span class="nt">"Mdr"</span><span class="p">:</span><span class="mi">5</span><span class="p">,</span><span class="w">
            </span><span class="nt">"Fee"</span><span class="p">:</span><span class="mi">0</span><span class="w">
        </span><span class="p">}</span><span class="w">
     </span><span class="p">}]</span><span class="w">
   </span><span class="p">}</span><span class="w">
</span><span class="p">}</span><span class="w">

</span></code></pre></div>

RESPONSE

<div class="highlighter-rouge"><pre class="highlight"><code><span class="p">{</span><span class="w">
    </span><span class="nt">"MerchantOrderId"</span><span class="p">:</span><span class="w"> </span><span class="s2">"2014111701"</span><span class="p">,</span><span class="w">
    </span><span class="nt">"Customer"</span><span class="p">:</span><span class="w"> </span><span class="p">{</span><span class="w">
        </span><span class="nt">"Name"</span><span class="p">:</span><span class="w"> </span><span class="s2">"[Guest]"</span><span class="w">
    </span><span class="p">},</span><span class="w">
    </span><span class="nt">"Payment"</span><span class="p">:</span><span class="w"> </span><span class="p">{</span><span class="w">
    </span><span class="nt">"SplitPayments"</span><span class="p">:</span><span class="w"> </span><span class="p">[</span><span class="w">
            </span><span class="p">{</span><span class="w">
                </span><span class="nt">"sellerMerchantId"</span><span class="p">:</span><span class="w"> </span><span class="s2">"MID SELLER 01"</span><span class="p">,</span><span class="w">
                </span><span class="nt">"amount"</span><span class="p">:</span><span class="w"> </span><span class="mi">10000</span><span class="p">,</span><span class="w">
                </span><span class="nt">"fares"</span><span class="p">:</span><span class="w"> </span><span class="p">{</span><span class="w">
                    </span><span class="nt">"mdr"</span><span class="p">:</span><span class="w"> </span><span class="mi">5</span><span class="p">,</span><span class="w">
                    </span><span class="nt">"fee"</span><span class="p">:</span><span class="w"> </span><span class="mi">0</span><span class="w">
                </span><span class="p">},</span><span class="w">
                </span><span class="nt">"splits"</span><span class="p">:</span><span class="w"> </span><span class="p">[</span><span class="w">
                    </span><span class="p">{</span><span class="w">
                        </span><span class="nt">"sellerMerchantId"</span><span class="p">:</span><span class="w"> </span><span class="s2">"MID DO MKP"</span><span class="p">,</span><span class="w">
                        </span><span class="nt">"amount"</span><span class="p">:</span><span class="w"> </span><span class="mi">500</span><span class="p">,</span><span class="w">
                    </span><span class="err">}</span><span class="p">,</span><span class="w">
                    </span><span class="p">{</span><span class="w">
                        </span><span class="nt">"sellerMerchantId"</span><span class="p">:</span><span class="w"> </span><span class="s2">"MID SELLER 01"</span><span class="p">,</span><span class="w">
                        </span><span class="nt">"amount"</span><span class="p">:</span><span class="w"> </span><span class="mi">9500</span><span class="p">,</span><span class="w">
                    </span><span class="err">}</span><span class="w">
                </span><span class="p">]</span><span class="w">
            </span><span class="p">}</span><span class="w">
        </span><span class="p">],</span><span class="w">
        </span><span class="nt">"ServiceTaxAmount"</span><span class="p">:</span><span class="w"> </span><span class="mi">0</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Installments"</span><span class="p">:</span><span class="w"> </span><span class="mi">1</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Interest"</span><span class="p">:</span><span class="w"> </span><span class="mi">0</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Capture"</span><span class="p">:</span><span class="w"> </span><span class="kc">false</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Authenticate"</span><span class="p">:</span><span class="w"> </span><span class="kc">false</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Recurrent"</span><span class="p">:</span><span class="w"> </span><span class="kc">false</span><span class="p">,</span><span class="w">
        </span><span class="nt">"CreditCard"</span><span class="p">:</span><span class="w"> </span><span class="p">{</span><span class="w">
            </span><span class="nt">"CardNumber"</span><span class="p">:</span><span class="w"> </span><span class="s2">"455187******0181"</span><span class="p">,</span><span class="w">
            </span><span class="nt">"Holder"</span><span class="p">:</span><span class="w"> </span><span class="s2">"Teste Holder"</span><span class="p">,</span><span class="w">
            </span><span class="nt">"ExpirationDate"</span><span class="p">:</span><span class="w"> </span><span class="s2">"12/2021"</span><span class="p">,</span><span class="w">
            </span><span class="nt">"SaveCard"</span><span class="p">:</span><span class="w"> </span><span class="kc">false</span><span class="p">,</span><span class="w">
            </span><span class="nt">"Brand"</span><span class="p">:</span><span class="w"> </span><span class="s2">"Visa"</span><span class="w">
        </span><span class="p">},</span><span class="w">
        </span><span class="nt">"Tid"</span><span class="p">:</span><span class="w"> </span><span class="s2">"1006993069000AF39CAA"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"ProofOfSale"</span><span class="p">:</span><span class="w"> </span><span class="s2">"483594"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"AuthorizationCode"</span><span class="p">:</span><span class="w"> </span><span class="s2">"123456"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"SoftDescriptor"</span><span class="p">:</span><span class="w"> </span><span class="s2">"Split*LosCone"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Provider"</span><span class="p">:</span><span class="w"> </span><span class="s2">"Cielo"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Eci"</span><span class="p">:</span><span class="w"> </span><span class="s2">"7"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"PaymentId"</span><span class="p">:</span><span class="w"> </span><span class="s2">"c5b7ea6e-e604-463a-a1b1-123c3c598e05"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Amount"</span><span class="p">:</span><span class="w"> </span><span class="mi">1000</span><span class="p">,</span><span class="w">
        </span><span class="nt">"ReceivedDate"</span><span class="p">:</span><span class="w"> </span><span class="s2">"2017-08-30 14:37:14"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Status"</span><span class="p">:</span><span class="w"> </span><span class="mi">1</span><span class="p">,</span><span class="w">
        </span><span class="nt">"ReturnMessage"</span><span class="p">:</span><span class="w"> </span><span class="s2">"Transa&ccedil;&atilde;o autorizada"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"ReturnCode"</span><span class="p">:</span><span class="w"> </span><span class="s2">"00"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Type"</span><span class="p">:</span><span class="w"> </span><span class="s2">"SplittedCreditCard"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Currency"</span><span class="p">:</span><span class="w"> </span><span class="s2">"BRL"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Country"</span><span class="p">:</span><span class="w"> </span><span class="s2">"BRA"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Links"</span><span class="p">:</span><span class="w"> </span><span class="p">[</span><span class="w">
            </span><span class="p">{</span><span class="w">
                </span><span class="nt">"Method"</span><span class="p">:</span><span class="w"> </span><span class="s2">"GET"</span><span class="p">,</span><span class="w">
                </span><span class="nt">"Rel"</span><span class="p">:</span><span class="w"> </span><span class="s2">"self"</span><span class="p">,</span><span class="w">
                </span><span class="nt">"Href"</span><span class="p">:</span><span class="w"> </span><span class="s2">"http://splitsandbox.braspag.com.br/schedules/c5b7ea6e-e604-463a-a1b1-123c3c598e05"</span><span class="w">
            </span><span class="p">},</span><span class="w">
            </span><span class="p">{</span><span class="w">
                </span><span class="nt">"Method"</span><span class="p">:</span><span class="w"> </span><span class="s2">"GET"</span><span class="p">,</span><span class="w">
                </span><span class="nt">"Rel"</span><span class="p">:</span><span class="w"> </span><span class="s2">"self"</span><span class="p">,</span><span class="w">
                </span><span class="nt">"Href"</span><span class="p">:</span><span class="w"> </span><span class="s2">"https://apiquerydev.cieloecommerce.cielo.com.br/1/sales/c5b7ea6e-e604-463a-a1b1-123c3c598e05"</span><span class="w">
            </span><span class="p">},</span><span class="w">
            </span><span class="p">{</span><span class="w">
                </span><span class="nt">"Method"</span><span class="p">:</span><span class="w"> </span><span class="s2">"PUT"</span><span class="p">,</span><span class="w">
                </span><span class="nt">"Rel"</span><span class="p">:</span><span class="w"> </span><span class="s2">"capture"</span><span class="p">,</span><span class="w">
                </span><span class="nt">"Href"</span><span class="p">:</span><span class="w"> </span><span class="s2">"https://apidev.cieloecommerce.cielo.com.br/1/sales/c5b7ea6e-e604-463a-a1b1-123c3c598e05/capture"</span><span class="w">
            </span><span class="p">},</span><span class="w">
            </span><span class="p">{</span><span class="w">
                </span><span class="nt">"Method"</span><span class="p">:</span><span class="w"> </span><span class="s2">"PUT"</span><span class="p">,</span><span class="w">
                </span><span class="nt">"Rel"</span><span class="p">:</span><span class="w"> </span><span class="s2">"void"</span><span class="p">,</span><span class="w">
                </span><span class="nt">"Href"</span><span class="p">:</span><span class="w"> </span><span class="s2">"https://apidev.cieloecommerce.cielo.com.br/1/sales/c5b7ea6e-e604-463a-a1b1-123c3c598e05/void"</span><span class="w">
            </span><span class="p">}</span><span class="w">
        </span><span class="p">]</span><span class="w">
    </span><span class="p">}</span><span class="w">
</span><span class="p">}</span><span class="w">

</span></code></pre></div>

EXEMPLO 02 - 2 Sellers dividindo a mesma transa&ccedil;&atilde;o

REQUEST

Header

<div class="highlighter-rouge"><pre class="highlight"><code>--request POST "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/"
--header "Content-Type: application/json"
--header "MerchantId: MID DO MKP"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
</code></pre></div>

Body

<div class="highlighter-rouge"><pre class="highlight"><code><span class="p">{</span><span class="w">
   </span><span class="nt">"MerchantOrderId"</span><span class="p">:</span><span class="s2">"2014111701"</span><span class="p">,</span><span class="w">
   </span><span class="nt">"Payment"</span><span class="p">:{</span><span class="w">
     </span><span class="nt">"Type"</span><span class="p">:</span><span class="s2">"SplittedCreditCard"</span><span class="p">,</span><span class="w">
     </span><span class="nt">"Amount"</span><span class="p">:</span><span class="mi">10000</span><span class="p">,</span><span class="w">
     </span><span class="nt">"Installments"</span><span class="p">:</span><span class="mi">1</span><span class="p">,</span><span class="w">
     </span><span class="nt">"SoftDescriptor"</span><span class="p">:</span><span class="s2">"Split*LosCone"</span><span class="p">,</span><span class="w">
     </span><span class="nt">"Capture"</span><span class="p">:</span><span class="kc">false</span><span class="p">,</span><span class="w">
     </span><span class="nt">"CreditCard"</span><span class="p">:{</span><span class="w">
         </span><span class="nt">"CardNumber"</span><span class="p">:</span><span class="s2">"4551870000000181"</span><span class="p">,</span><span class="w">
         </span><span class="nt">"Holder"</span><span class="p">:</span><span class="s2">"Teste Holder"</span><span class="p">,</span><span class="w">
         </span><span class="nt">"ExpirationDate"</span><span class="p">:</span><span class="s2">"12/2021"</span><span class="p">,</span><span class="w">
         </span><span class="nt">"SecurityCode"</span><span class="p">:</span><span class="s2">"123"</span><span class="p">,</span><span class="w">
         </span><span class="nt">"Brand"</span><span class="p">:</span><span class="s2">"Visa"</span><span class="w">
     </span><span class="p">},</span><span class="w">
     </span><span class="nt">"SplitPayments"</span><span class="p">:[</span><span class="w">
        </span><span class="p">{</span><span class="w">
        </span><span class="nt">"SellerMerchantId"</span><span class="w"> </span><span class="p">:</span><span class="s2">"MID SELLER 01"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Amount"</span><span class="p">:</span><span class="mi">5000</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Fares"</span><span class="p">:{</span><span class="w">
            </span><span class="nt">"Mdr"</span><span class="p">:</span><span class="mi">5</span><span class="p">,</span><span class="w">
            </span><span class="nt">"Fee"</span><span class="p">:</span><span class="mi">0</span><span class="w">
        </span><span class="p">},</span><span class="w">
        </span><span class="err">{</span><span class="w">
        </span><span class="nt">"SellerMerchantId"</span><span class="w"> </span><span class="p">:</span><span class="s2">"MID SELLER 02"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Amount"</span><span class="p">:</span><span class="mi">5000</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Fares"</span><span class="p">:{</span><span class="w">
            </span><span class="nt">"Mdr"</span><span class="p">:</span><span class="mi">10</span><span class="p">,</span><span class="w">
            </span><span class="nt">"Fee"</span><span class="p">:</span><span class="mi">0</span><span class="w">
        </span><span class="p">}</span><span class="w">
      </span><span class="p">}]</span><span class="w">
   </span><span class="p">}</span><span class="w">
</span><span class="p">}</span><span class="w">

</span></code></pre></div>

RESPONSE

| `SplitPayments.SellerMerchantId`{: .highlighter-rouge} | Tipo | Tamanho | Obrigat&oacute;rio | Descri&ccedil;&atilde;o |

<div class="highlighter-rouge"><pre class="highlight"><code><span class="p">{</span><span class="w">
    </span><span class="nt">"MerchantOrderId"</span><span class="p">:</span><span class="w"> </span><span class="s2">"2014111701"</span><span class="p">,</span><span class="w">
    </span><span class="nt">"Customer"</span><span class="p">:</span><span class="w"> </span><span class="p">{</span><span class="w">
        </span><span class="nt">"Name"</span><span class="p">:</span><span class="w"> </span><span class="s2">"[Guest]"</span><span class="w">
    </span><span class="p">},</span><span class="w">
    </span><span class="nt">"Payment"</span><span class="p">:</span><span class="w"> </span><span class="p">{</span><span class="w">
    </span><span class="nt">"SplitPayments"</span><span class="p">:</span><span class="w"> </span><span class="p">[</span><span class="w">
            </span><span class="p">{</span><span class="w">
                </span><span class="nt">"sellerMerchantId"</span><span class="p">:</span><span class="w"> </span><span class="s2">"MID SELLER 01"</span><span class="p">,</span><span class="w">
                </span><span class="nt">"amount"</span><span class="p">:</span><span class="w"> </span><span class="mi">5000</span><span class="p">,</span><span class="w">
                </span><span class="nt">"fares"</span><span class="p">:</span><span class="w"> </span><span class="p">{</span><span class="w">
                    </span><span class="nt">"mdr"</span><span class="p">:</span><span class="w"> </span><span class="mi">5</span><span class="p">,</span><span class="w">
                    </span><span class="nt">"fee"</span><span class="p">:</span><span class="w"> </span><span class="mi">0</span><span class="w">
                </span><span class="p">},</span><span class="w">
                </span><span class="nt">"splits"</span><span class="p">:</span><span class="w"> </span><span class="p">[</span><span class="w">
                    </span><span class="p">{</span><span class="w">
                        </span><span class="nt">"sellerMerchantId"</span><span class="p">:</span><span class="w"> </span><span class="s2">"MID DO MKP"</span><span class="p">,</span><span class="w">
                        </span><span class="nt">"amount"</span><span class="p">:</span><span class="w"> </span><span class="mi">250</span><span class="p">,</span><span class="w">
                    </span><span class="err">}</span><span class="p">,</span><span class="w">
                    </span><span class="p">{</span><span class="w">
                        </span><span class="nt">"sellerMerchantId"</span><span class="p">:</span><span class="w"> </span><span class="s2">"MID SELLER 01"</span><span class="p">,</span><span class="w">
                        </span><span class="nt">"amount"</span><span class="p">:</span><span class="w"> </span><span class="mi">4750</span><span class="p">,</span><span class="w">
                   </span><span class="err">}</span><span class="w">
                </span><span class="p">]</span><span class="w">
            </span><span class="p">},</span><span class="w">
        	</span><span class="p">{</span><span class="w">
                </span><span class="nt">"sellerMerchantId"</span><span class="p">:</span><span class="w"> </span><span class="s2">"MID SELLER 02"</span><span class="p">,</span><span class="w">
                </span><span class="nt">"amount"</span><span class="p">:</span><span class="w"> </span><span class="mi">5000</span><span class="p">,</span><span class="w">
                </span><span class="nt">"fares"</span><span class="p">:</span><span class="w"> </span><span class="p">{</span><span class="w">
                    </span><span class="nt">"mdr"</span><span class="p">:</span><span class="w"> </span><span class="mi">10</span><span class="p">,</span><span class="w">
                    </span><span class="nt">"fee"</span><span class="p">:</span><span class="w"> </span><span class="mi">0</span><span class="w">
                </span><span class="p">},</span><span class="w">
                </span><span class="nt">"splits"</span><span class="p">:</span><span class="w"> </span><span class="p">[</span><span class="w">
                    </span><span class="p">{</span><span class="w">
                        </span><span class="nt">"sellerMerchantId"</span><span class="p">:</span><span class="w"> </span><span class="s2">"MID DO MKP"</span><span class="p">,</span><span class="w">
                        </span><span class="nt">"amount"</span><span class="p">:</span><span class="w"> </span><span class="mi">500</span><span class="p">,</span><span class="w">
                    </span><span class="err">}</span><span class="p">,</span><span class="w">
                    </span><span class="p">{</span><span class="w">
                        </span><span class="nt">"sellerMerchantId"</span><span class="p">:</span><span class="w"> </span><span class="s2">"MID SELLER 02"</span><span class="p">,</span><span class="w">
                        </span><span class="nt">"amount"</span><span class="p">:</span><span class="w"> </span><span class="mi">4500</span><span class="p">,</span><span class="w">
                    </span><span class="err">}</span><span class="w">
                </span><span class="p">]</span><span class="w">
            </span><span class="p">}</span><span class="w">
        </span><span class="p">],</span><span class="w">
        </span><span class="nt">"ServiceTaxAmount"</span><span class="p">:</span><span class="w"> </span><span class="mi">0</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Installments"</span><span class="p">:</span><span class="w"> </span><span class="mi">1</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Interest"</span><span class="p">:</span><span class="w"> </span><span class="mi">0</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Capture"</span><span class="p">:</span><span class="w"> </span><span class="kc">false</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Authenticate"</span><span class="p">:</span><span class="w"> </span><span class="kc">false</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Recurrent"</span><span class="p">:</span><span class="w"> </span><span class="kc">false</span><span class="p">,</span><span class="w">
        </span><span class="nt">"CreditCard"</span><span class="p">:</span><span class="w"> </span><span class="p">{</span><span class="w">
            </span><span class="nt">"CardNumber"</span><span class="p">:</span><span class="w"> </span><span class="s2">"455187******0181"</span><span class="p">,</span><span class="w">
            </span><span class="nt">"Holder"</span><span class="p">:</span><span class="w"> </span><span class="s2">"Teste Holder"</span><span class="p">,</span><span class="w">
            </span><span class="nt">"ExpirationDate"</span><span class="p">:</span><span class="w"> </span><span class="s2">"12/2021"</span><span class="p">,</span><span class="w">
            </span><span class="nt">"SaveCard"</span><span class="p">:</span><span class="w"> </span><span class="kc">false</span><span class="p">,</span><span class="w">
            </span><span class="nt">"Brand"</span><span class="p">:</span><span class="w"> </span><span class="s2">"Visa"</span><span class="w">
        </span><span class="p">},</span><span class="w">
        </span><span class="nt">"Tid"</span><span class="p">:</span><span class="w"> </span><span class="s2">"1006993069000AF39CAA"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"ProofOfSale"</span><span class="p">:</span><span class="w"> </span><span class="s2">"483594"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"AuthorizationCode"</span><span class="p">:</span><span class="w"> </span><span class="s2">"123456"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"SoftDescriptor"</span><span class="p">:</span><span class="w"> </span><span class="s2">"Split*LosCone"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Provider"</span><span class="p">:</span><span class="w"> </span><span class="s2">"Cielo"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Eci"</span><span class="p">:</span><span class="w"> </span><span class="s2">"7"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"PaymentId"</span><span class="p">:</span><span class="w"> </span><span class="s2">"c5b7ea6e-e604-463a-a1b1-123c3c598e05"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Amount"</span><span class="p">:</span><span class="w"> </span><span class="mi">1000</span><span class="p">,</span><span class="w">
        </span><span class="nt">"ReceivedDate"</span><span class="p">:</span><span class="w"> </span><span class="s2">"2017-08-30 14:37:14"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Status"</span><span class="p">:</span><span class="w"> </span><span class="mi">1</span><span class="p">,</span><span class="w">
        </span><span class="nt">"ReturnMessage"</span><span class="p">:</span><span class="w"> </span><span class="s2">"Transa&ccedil;&atilde;o autorizada"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"ReturnCode"</span><span class="p">:</span><span class="w"> </span><span class="s2">"00"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Type"</span><span class="p">:</span><span class="w"> </span><span class="s2">"SplittedCreditCard"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Currency"</span><span class="p">:</span><span class="w"> </span><span class="s2">"BRL"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Country"</span><span class="p">:</span><span class="w"> </span><span class="s2">"BRA"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Links"</span><span class="p">:</span><span class="w"> </span><span class="p">[</span><span class="w">
            </span><span class="p">{</span><span class="w">
                </span><span class="nt">"Method"</span><span class="p">:</span><span class="w"> </span><span class="s2">"GET"</span><span class="p">,</span><span class="w">
                </span><span class="nt">"Rel"</span><span class="p">:</span><span class="w"> </span><span class="s2">"self"</span><span class="p">,</span><span class="w">
                </span><span class="nt">"Href"</span><span class="p">:</span><span class="w"> </span><span class="s2">"http://splitsandbox.braspag.com.br/schedules/c5b7ea6e-e604-463a-a1b1-123c3c598e05"</span><span class="w">
            </span><span class="p">},</span><span class="w">
            </span><span class="p">{</span><span class="w">
                </span><span class="nt">"Method"</span><span class="p">:</span><span class="w"> </span><span class="s2">"GET"</span><span class="p">,</span><span class="w">
                </span><span class="nt">"Rel"</span><span class="p">:</span><span class="w"> </span><span class="s2">"self"</span><span class="p">,</span><span class="w">
                </span><span class="nt">"Href"</span><span class="p">:</span><span class="w"> </span><span class="s2">"https://apiquerydev.cieloecommerce.cielo.com.br/1/sales/c5b7ea6e-e604-463a-a1b1-123c3c598e05"</span><span class="w">
            </span><span class="p">},</span><span class="w">
            </span><span class="p">{</span><span class="w">
                </span><span class="nt">"Method"</span><span class="p">:</span><span class="w"> </span><span class="s2">"PUT"</span><span class="p">,</span><span class="w">
                </span><span class="nt">"Rel"</span><span class="p">:</span><span class="w"> </span><span class="s2">"capture"</span><span class="p">,</span><span class="w">
                </span><span class="nt">"Href"</span><span class="p">:</span><span class="w"> </span><span class="s2">"https://apidev.cieloecommerce.cielo.com.br/1/sales/c5b7ea6e-e604-463a-a1b1-123c3c598e05/capture"</span><span class="w">
            </span><span class="p">},</span><span class="w">
            </span><span class="p">{</span><span class="w">
                </span><span class="nt">"Method"</span><span class="p">:</span><span class="w"> </span><span class="s2">"PUT"</span><span class="p">,</span><span class="w">
                </span><span class="nt">"Rel"</span><span class="p">:</span><span class="w"> </span><span class="s2">"void"</span><span class="p">,</span><span class="w">
                </span><span class="nt">"Href"</span><span class="p">:</span><span class="w"> </span><span class="s2">"https://apidev.cieloecommerce.cielo.com.br/1/sales/c5b7ea6e-e604-463a-a1b1-123c3c598e05/void"</span><span class="w">
            </span><span class="p">}</span><span class="w">
        </span><span class="p">]</span><span class="w">
    </span><span class="p">}</span><span class="w">
</span><span class="p">}</span><span class="w">
</span></code></pre></div>

EXEMPLO 03 - 2 Sellers, sendo um deles o MKP

REQUEST

Header

<div class="highlighter-rouge"><pre class="highlight"><code>--request POST "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/"
--header "Content-Type: application/json"
--header "MerchantId: MID DO MKP"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
</code></pre></div>

Body

<div class="highlighter-rouge"><pre class="highlight"><code><span class="p">{</span><span class="w">
   </span><span class="nt">"MerchantOrderId"</span><span class="p">:</span><span class="s2">"2014111701"</span><span class="p">,</span><span class="w">
   </span><span class="nt">"Payment"</span><span class="p">:{</span><span class="w">
     </span><span class="nt">"Type"</span><span class="p">:</span><span class="s2">"SplittedCreditCard"</span><span class="p">,</span><span class="w">
     </span><span class="nt">"Amount"</span><span class="p">:</span><span class="mi">10000</span><span class="p">,</span><span class="w">
     </span><span class="nt">"Installments"</span><span class="p">:</span><span class="mi">1</span><span class="p">,</span><span class="w">
     </span><span class="nt">"SoftDescriptor"</span><span class="p">:</span><span class="s2">"Split*LosCone"</span><span class="p">,</span><span class="w">
     </span><span class="nt">"Capture"</span><span class="p">:</span><span class="kc">false</span><span class="p">,</span><span class="w">
     </span><span class="nt">"CreditCard"</span><span class="p">:{</span><span class="w">
         </span><span class="nt">"CardNumber"</span><span class="p">:</span><span class="s2">"4551870000000181"</span><span class="p">,</span><span class="w">
         </span><span class="nt">"Holder"</span><span class="p">:</span><span class="s2">"Teste Holder"</span><span class="p">,</span><span class="w">
         </span><span class="nt">"ExpirationDate"</span><span class="p">:</span><span class="s2">"12/2021"</span><span class="p">,</span><span class="w">
         </span><span class="nt">"SecurityCode"</span><span class="p">:</span><span class="s2">"123"</span><span class="p">,</span><span class="w">
         </span><span class="nt">"Brand"</span><span class="p">:</span><span class="s2">"Visa"</span><span class="w">
     </span><span class="p">},</span><span class="w">
     </span><span class="nt">"SplitPayments"</span><span class="p">:[</span><span class="w">
        </span><span class="p">{</span><span class="w">
        </span><span class="nt">"SellerMerchantId"</span><span class="w"> </span><span class="p">:</span><span class="s2">"MID SELLER 01"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Amount"</span><span class="p">:</span><span class="mi">5000</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Fares"</span><span class="p">:{</span><span class="w">
            </span><span class="nt">"Mdr"</span><span class="p">:</span><span class="mi">5</span><span class="p">,</span><span class="w">
            </span><span class="nt">"Fee"</span><span class="p">:</span><span class="mi">0</span><span class="w">
        </span><span class="p">},</span><span class="w">
        </span><span class="err">{</span><span class="w">
        </span><span class="nt">"SellerMerchantId"</span><span class="w"> </span><span class="p">:</span><span class="s2">"MID DO MKP"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Amount"</span><span class="p">:</span><span class="mi">5000</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Fares"</span><span class="p">:{</span><span class="w">
            </span><span class="nt">"Mdr"</span><span class="p">:</span><span class="mi">0</span><span class="p">,</span><span class="w">
            </span><span class="nt">"Fee"</span><span class="p">:</span><span class="mi">0</span><span class="w">
        </span><span class="p">}</span><span class="w">
      </span><span class="p">}]</span><span class="w">
   </span><span class="p">}</span><span class="w">
</span><span class="p">}</span><span class="w">

</span></code></pre></div>

RESPONSE

<div class="highlighter-rouge"><pre class="highlight"><code><span class="p">{</span><span class="w">
    </span><span class="nt">"MerchantOrderId"</span><span class="p">:</span><span class="w"> </span><span class="s2">"2014111701"</span><span class="p">,</span><span class="w">
    </span><span class="nt">"Customer"</span><span class="p">:</span><span class="w"> </span><span class="p">{</span><span class="w">
        </span><span class="nt">"Name"</span><span class="p">:</span><span class="w"> </span><span class="s2">"[Guest]"</span><span class="w">
    </span><span class="p">},</span><span class="w">
    </span><span class="nt">"Payment"</span><span class="p">:</span><span class="w"> </span><span class="p">{</span><span class="w">
    </span><span class="nt">"SplitPayments"</span><span class="p">:</span><span class="w"> </span><span class="p">[</span><span class="w">
            </span><span class="p">{</span><span class="w">
                </span><span class="nt">"sellerMerchantId"</span><span class="p">:</span><span class="w"> </span><span class="s2">"MID SELLER 01"</span><span class="p">,</span><span class="w">
                </span><span class="nt">"amount"</span><span class="p">:</span><span class="w"> </span><span class="mi">5000</span><span class="p">,</span><span class="w">
                </span><span class="nt">"fares"</span><span class="p">:</span><span class="w"> </span><span class="p">{</span><span class="w">
                    </span><span class="nt">""</span><span class="err">mdr</span><span class="p">:</span><span class="w"> </span><span class="mi">5</span><span class="p">,</span><span class="w">
                    </span><span class="nt">"fee"</span><span class="p">:</span><span class="w"> </span><span class="mi">0</span><span class="w">
                </span><span class="p">},</span><span class="w">
                </span><span class="nt">"splits"</span><span class="p">:</span><span class="w"> </span><span class="p">[</span><span class="w">
                    </span><span class="p">{</span><span class="w">
                        </span><span class="nt">"sellerMerchantId"</span><span class="p">:</span><span class="w"> </span><span class="s2">"MID DO MKP"</span><span class="p">,</span><span class="w">
                        </span><span class="nt">"amount"</span><span class="p">:</span><span class="w"> </span><span class="mi">250</span><span class="p">,</span><span class="w">

                    </span><span class="err">}</span><span class="p">,</span><span class="w">
                    </span><span class="p">{</span><span class="w">
                        </span><span class="nt">"sellerMerchantId"</span><span class="p">:</span><span class="w"> </span><span class="s2">"MID SELLER 01"</span><span class="p">,</span><span class="w">
                        </span><span class="nt">"amount"</span><span class="p">:</span><span class="w"> </span><span class="mi">4750</span><span class="p">,</span><span class="w">

                    </span><span class="err">}</span><span class="w">
                </span><span class="p">]</span><span class="w">
            </span><span class="p">},</span><span class="w">
        	</span><span class="p">{</span><span class="w">
                </span><span class="nt">"sellerMerchantId"</span><span class="p">:</span><span class="w"> </span><span class="s2">"MID DO MKP"</span><span class="p">,</span><span class="w">
                </span><span class="nt">"amount"</span><span class="p">:</span><span class="w"> </span><span class="mi">5000</span><span class="p">,</span><span class="w">
                </span><span class="nt">"fares"</span><span class="p">:</span><span class="w"> </span><span class="p">{</span><span class="w">
                    </span><span class="nt">"mdr"</span><span class="p">:</span><span class="w"> </span><span class="mi">0</span><span class="p">,</span><span class="w">
                    </span><span class="nt">"fee"</span><span class="p">:</span><span class="w"> </span><span class="mi">0</span><span class="w">
                </span><span class="p">},</span><span class="w">
                </span><span class="nt">"splits"</span><span class="p">:</span><span class="w"> </span><span class="p">[</span><span class="w">
                    </span><span class="p">{</span><span class="w">
                        </span><span class="nt">"sellerMerchantId"</span><span class="p">:</span><span class="w"> </span><span class="s2">"MID DO MKP"</span><span class="p">,</span><span class="w">
                        </span><span class="nt">"amount"</span><span class="p">:</span><span class="w"> </span><span class="mi">0</span><span class="p">,</span><span class="w">
                        </span><span class="nt">"mdr"</span><span class="p">:</span><span class="w"> </span><span class="mi">0</span><span class="p">,</span><span class="w">
                        </span><span class="nt">"fee"</span><span class="p">:</span><span class="w"> </span><span class="mi">0</span><span class="w">
                    </span><span class="p">},</span><span class="w">
                    </span><span class="p">{</span><span class="w">
                        </span><span class="nt">"sellerMerchantId"</span><span class="p">:</span><span class="w"> </span><span class="s2">"MID DO MKP"</span><span class="p">,</span><span class="w">
                        </span><span class="nt">"amount"</span><span class="p">:</span><span class="w"> </span><span class="mi">5000</span><span class="w">
                        </span><span class="s2">"mdr"</span><span class="err">:</span><span class="w"> </span><span class="mi">100</span><span class="p">,</span><span class="w">
                        </span><span class="nt">"fee"</span><span class="p">:</span><span class="w"> </span><span class="mi">0</span><span class="w">
                    </span><span class="p">}</span><span class="w">
                </span><span class="p">]</span><span class="w">
            </span><span class="p">}</span><span class="w">
        </span><span class="p">],</span><span class="w">
        </span><span class="nt">"ServiceTaxAmount"</span><span class="p">:</span><span class="w"> </span><span class="mi">0</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Installments"</span><span class="p">:</span><span class="w"> </span><span class="mi">1</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Interest"</span><span class="p">:</span><span class="w"> </span><span class="mi">0</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Capture"</span><span class="p">:</span><span class="w"> </span><span class="kc">false</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Authenticate"</span><span class="p">:</span><span class="w"> </span><span class="kc">false</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Recurrent"</span><span class="p">:</span><span class="w"> </span><span class="kc">false</span><span class="p">,</span><span class="w">
        </span><span class="nt">"CreditCard"</span><span class="p">:</span><span class="w"> </span><span class="p">{</span><span class="w">
            </span><span class="nt">"CardNumber"</span><span class="p">:</span><span class="w"> </span><span class="s2">"455187******0181"</span><span class="p">,</span><span class="w">
            </span><span class="nt">"Holder"</span><span class="p">:</span><span class="w"> </span><span class="s2">"Teste Holder"</span><span class="p">,</span><span class="w">
            </span><span class="nt">"ExpirationDate"</span><span class="p">:</span><span class="w"> </span><span class="s2">"12/2021"</span><span class="p">,</span><span class="w">
            </span><span class="nt">"SaveCard"</span><span class="p">:</span><span class="w"> </span><span class="kc">false</span><span class="p">,</span><span class="w">
            </span><span class="nt">"Brand"</span><span class="p">:</span><span class="w"> </span><span class="s2">"Visa"</span><span class="w">
        </span><span class="p">},</span><span class="w">
        </span><span class="nt">"Tid"</span><span class="p">:</span><span class="w"> </span><span class="s2">"1006993069000AF39CAA"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"ProofOfSale"</span><span class="p">:</span><span class="w"> </span><span class="s2">"483594"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"AuthorizationCode"</span><span class="p">:</span><span class="w"> </span><span class="s2">"123456"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"SoftDescriptor"</span><span class="p">:</span><span class="w"> </span><span class="s2">"Split*LosCone"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Provider"</span><span class="p">:</span><span class="w"> </span><span class="s2">"Cielo"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Eci"</span><span class="p">:</span><span class="w"> </span><span class="s2">"7"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"PaymentId"</span><span class="p">:</span><span class="w"> </span><span class="s2">"c5b7ea6e-e604-463a-a1b1-123c3c598e05"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Amount"</span><span class="p">:</span><span class="w"> </span><span class="mi">1000</span><span class="p">,</span><span class="w">
        </span><span class="nt">"ReceivedDate"</span><span class="p">:</span><span class="w"> </span><span class="s2">"2017-08-30 14:37:14"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Status"</span><span class="p">:</span><span class="w"> </span><span class="mi">1</span><span class="p">,</span><span class="w">
        </span><span class="nt">"ReturnMessage"</span><span class="p">:</span><span class="w"> </span><span class="s2">"Transa&ccedil;&atilde;o autorizada"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"ReturnCode"</span><span class="p">:</span><span class="w"> </span><span class="s2">"00"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Type"</span><span class="p">:</span><span class="w"> </span><span class="s2">"SplittedCreditCard"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Currency"</span><span class="p">:</span><span class="w"> </span><span class="s2">"BRL"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Country"</span><span class="p">:</span><span class="w"> </span><span class="s2">"BRA"</span><span class="p">,</span><span class="w">
        </span><span class="nt">"Links"</span><span class="p">:</span><span class="w"> </span><span class="p">[</span><span class="w">
            </span><span class="p">{</span><span class="w">
                </span><span class="nt">"Method"</span><span class="p">:</span><span class="w"> </span><span class="s2">"GET"</span><span class="p">,</span><span class="w">
                </span><span class="nt">"Rel"</span><span class="p">:</span><span class="w"> </span><span class="s2">"self"</span><span class="p">,</span><span class="w">
                </span><span class="nt">"Href"</span><span class="p">:</span><span class="w"> </span><span class="s2">"http://splitsandbox.braspag.com.br/schedules/c5b7ea6e-e604-463a-a1b1-123c3c598e05"</span><span class="w">
            </span><span class="p">},</span><span class="w">
            </span><span class="p">{</span><span class="w">
                </span><span class="nt">"Method"</span><span class="p">:</span><span class="w"> </span><span class="s2">"GET"</span><span class="p">,</span><span class="w">
                </span><span class="nt">"Rel"</span><span class="p">:</span><span class="w"> </span><span class="s2">"self"</span><span class="p">,</span><span class="w">
                </span><span class="nt">"Href"</span><span class="p">:</span><span class="w"> </span><span class="s2">"https://apiquerydev.cieloecommerce.cielo.com.br/1/sales/c5b7ea6e-e604-463a-a1b1-123c3c598e05"</span><span class="w">
            </span><span class="p">},</span><span class="w">
            </span><span class="p">{</span><span class="w">
                </span><span class="nt">"Method"</span><span class="p">:</span><span class="w"> </span><span class="s2">"PUT"</span><span class="p">,</span><span class="w">
                </span><span class="nt">"Rel"</span><span class="p">:</span><span class="w"> </span><span class="s2">"capture"</span><span class="p">,</span><span class="w">
                </span><span class="nt">"Href"</span><span class="p">:</span><span class="w"> </span><span class="s2">"https://apidev.cieloecommerce.cielo.com.br/1/sales/c5b7ea6e-e604-463a-a1b1-123c3c598e05/capture"</span><span class="w">
            </span><span class="p">},</span><span class="w">
            </span><span class="p">{</span><span class="w">
                </span><span class="nt">"Method"</span><span class="p">:</span><span class="w"> </span><span class="s2">"PUT"</span><span class="p">,</span><span class="w">
                </span><span class="nt">"Rel"</span><span class="p">:</span><span class="w"> </span><span class="s2">"void"</span><span class="p">,</span><span class="w">
                </span><span class="nt">"Href"</span><span class="p">:</span><span class="w"> </span><span class="s2">"https://apidev.cieloecommerce.cielo.com.br/1/sales/c5b7ea6e-e604-463a-a1b1-123c3c598e05/void"</span><span class="w">
            </span><span class="p">}</span><span class="w">
        </span><span class="p">]</span><span class="w">
    </span><span class="p">}</span><span class="w">
</span><span class="p">}</span><span class="w">
</span></code></pre></div>