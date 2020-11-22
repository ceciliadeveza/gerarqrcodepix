
![Gerar Qrode Pix](https://gerarqrcodepix.com.br/site_1.jpg)

# Introdução

Qualquer pessoa que possui uma chave Pix cadastrada pode criar um QrCode válido e receber valores por ele.
Este repositório tem o objetivo de facilitar a criação, bem como o entendimento desse qrcode, que deve ser construído seguindo a especificação do [BrCode](https://www.bcb.gov.br/content/estabilidadefinanceira/spb_docs/ManualBRCode.pdf).

---

# Demo

## Via navegador:

[gerarqrcodepix.com.br](http://gerarqrcodepix.com.br)

## Via API:

[gerarqrcodepix.com.br/api/v1?nome=Cecília Devêza&cidade=Ouro Preto&saida=qr&chave=2aa96c40-d85f-4b98-b29f-d158a1c45f7f](https://gerarqrcodepix.com.br/api/v1?nome=Cec%C3%ADlia%20Dev%C3%AAza&cidade=Ouro%20Preto&saida=qr&chave=2aa96c40-d85f-4b98-b29f-d158a1c45f7f)

---

# Tipos de QrCodes 

## Estático

Um QrCode estático é um QrCode que pode ser pago diversas vezes. Ele pode ter um valor associado ou não, e neste caso, é o pagador quem define.

Recomendações de uso:

- Pessoas físicas
- Recebimento de doações
- Pequenos varejistas e prestadores de serviço

## Dinâmico

O QrCode dinâmico é um QrCode que pode ser pago uma única vez, isso significa que após o primeiro pagamento ele se torna inválido e o recebedor precisa criar um novo. A vantagem deste tipo de QrCode é que facilita conciliação financeira e pode embutir nele mais informações como a identificação do recebedor.

Recomendações de uso:

- Serviços online (e-commerce)
- Serviços automatizados de pagamento (maquininhas, self-checkout, etc)
- Serviços que necessitam de maior controle e conciliação comercial

---

# API

Para criar um **QrCode dinâmico** o recebedor precisa, necessariamente, ter um vínculo com um PSP direto/indireto do Pix. Todos os [PSPs autorizados pelo BACEN](https://www.bcb.gov.br/content/estabilidadefinanceira/pix/ListadeparticipantesdoPix.pdf) podem ofertar a API Pix, desde que sigam os padrões especificados pelo autorizador. Para geração de QrCodes estáticos, não é necessário integração com nenhuma API Pix.

Independentemente do QrCode que o recebedor deseja gerar, a responsabilidade de construir o BrCode é do recebedor e não do PSP. Por isso, neste repositório ofereço uma API para geração do QrCode e não uma API Pix, ou seja, você pode utilizá-la de três formas: 
  
  - Para gerar QrCodes estáticos a qualquer momento se tiver qualquer chave Pix cadastrada;
  - Para gerar QrCodes dinâmicos desde que tenha implementado a API Pix com algum PSP que a ofertou; e
  - Para gerar imagens de QrCode quando já tiver um BrCode construído.

A API possui um único endpoint `GET` que retorna a imagem do QrCode ou a string BrCode de acordo com as informações enviadas nos parâmetros.

```
GET https://gerarqrcodepix.com.br/api/v1?[parametros]
```

## Parâmetros do QrCode estático

| Parâmetro 	| Obrigatório 	| Descrição                                                                                                                                                                                             	|
|-----------	|-------------	|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|
| `nome`    	| Sim         	| Nome do recebedor.                                                                                                                                                                                    	|
| `cidade`  	| Sim         	| Cidade do recebedor.                                                                                                                                                                                  	|
| `valor`   	| Não         	| Valor do QrCode. Exemplo: 1200.99                                                                                                                                                                     	|
| `saida`   	| Sim         	| Use `br` para string e `qr` para imagem.                                                                                                                                                              	|
| `tamanho` 	| Não         	| Define a altura do QrCode em pixels.                                                                                                                                                                  	|
| `chave`   	| Sim         	| Chave Pix cadastrada em qualquer PSP. <br><br>Exemplos:<br>- Telefone: +5531912345678<br>- CPF ou CNPJ: 01234567890<br>- E-mail: teste@pix.com.br<br>- Aleatória: 2aa96c40-d85f-4b98-b29f-d158a1c45f7f 	|

[![Rodar exemplo](https://gerarqrcodepix.com.br/run_button.jpg)](https://gerarqrcodepix.com.br/api/v1?nome=Cec%C3%ADlia%20Dev%C3%AAza&cidade=Ouro%20Preto&valor=10.00&saida=qr&chave=2aa96c40-d85f-4b98-b29f-d158a1c45f7f)

## Parâmetros do QrCode dinâmico

| Parâmetro  	| Obrigatório 	| Descrição                                                                                                                	|
|------------	|-------------	|--------------------------------------------------------------------------------------------------------------------------	|
| `nome`     	| Sim         	| Nome do recebedor.                                                                                                       	|
| `cidade`   	| Sim         	| Cidade do recebedor.                                                                                                     	|
| `saida`    	| Sim         	| Use `br` para string e `qr` para imagem.                                                                                 	|
| `tamanho`  	| Não         	| Define a altura do QrCode em pixels.                                                                                     	|
| `location` 	| Sim         	| URL do payload retornada por uma API Pix.<br>Exemplo: qrcodes-pix.gerencianet.com.br/v2/232023aab07f40ec9a383e47792f7345 	|

[![Rodar exemplo](https://gerarqrcodepix.com.br/run_button.jpg)](https://gerarqrcodepix.com.br/api/v1?nome=Cec%C3%ADlia%20Dev%C3%AAza&cidade=Ouro%20Preto&saida=qr&location=qrcodes-pix.gerencianet.com.br/v2/232023aab07f40ec9a383e47792f7345)


## Parâmetros para geração de imagem com BrCode pronto

| Parâmetro 	| Obrigatório 	| Descrição                                                                         	|
|-----------	|-------------	|-----------------------------------------------------------------------------------	|
| `brcode`  	| Sim         	| Utilize quando já tiver o BrCode criado e deseja apenas gerar a imagem do QrCode. 	|
| `tamanho` 	| Não         	| Define a altura do QrCode em pixels.                                              	|

[![Rodar exemplo](https://gerarqrcodepix.com.br/run_button.jpg)](https://gerarqrcodepix.com.br/api/v1?brcode=00020126580014BR.GOV.BCB.PIX01362AA96C40-D85F-4B98-B29F-D158A1C45F7F5204000053039865802BR5914CECILIA%20DEVEZA6010OURO%20PRETO6304E3E5&tamanho=256)

---

# Collection Postman

![Gerar Qrode Pix](https://gerarqrcodepix.com.br/site_2.jpg)

Se preferir, você pode utilizar esta collection no Postman para realizar seus testes:

[![Rodar no Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/98e73bcb5782024ca150)

---

# Doações

Este site facilitou sua geração de QrCodes Pix ou contribuiu para que você entendesse um pouco mais sobre eles?
Faça uma doação ;)

![Qrcode estático](https://gerarqrcodepix.com.br/api/v1?nome=Cec%C3%ADlia%20Dev%C3%AAza&cidade=Ouro%20Preto&tamanho=256&saida=qr&chave=2aa96c40-d85f-4b98-b29f-d158a1c45f7f)


