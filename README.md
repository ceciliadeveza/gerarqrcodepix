# Início

Qualquer pessoa que possui uma chave Pix cadastrada pode criar um QrCode válido e receber valores por ele.
Este repositório tem o objetivo de facilitar a criação, bem como o entendimento desse qrcode, que deve ser construído seguindo a especificação do BRCODE.

# Demo

[gerarqrcodepix.com.br](http://gerarqrcodepix.com.br)

# Tipos de qrcodes 

## Estático

Em resumo, um qrcode estático é um qrcode que pode ser pago diversas vezes. Ele pode ter um valor associado ou não, e neste último caso, é o pagador quem define o valor.

Recomendações de uso:

- Pessoas físicas
- Recebimento de doações
- Pequenos varejistase prestadores de serviço

## Dinâmico

O qrcode dinâmico é um qrcode que pode ser pago uma única vez, isso significa que após o primeiro pagamento ele se torna inválido e o recebedor precisa criar um novo. A vantagem deste tipo de qrcode é que facilita conciliação financeira e pode embutir nele mais informações como a identificação do recebedor.

Recomendações de uso:

- Serviços online (e-commerce)
- Serviços automatizados de pagamento (maquininhas, self-checkout, etc)
- Serviços que necessitam de maior controle e conciliação comercial

# API

Para criar um **qrcode dinâmico** o recebedor precisa, necessariamente, ter um vínculo com um PSP direto/indireto do Pix. Todos os PSPs autorizados pelo BACEN podem ofertar a API Pix, desde que sigam os padrões especificados pelo autorizador. Para geração de qrcodes estáticos, não é necessário nenhuma integração com nenhuma API Pix.

Independentemente do qrcode que o recebedor deseja gerar, a responsabilidade de construir o BRCODE é do recebedor e não do PSP. Por isso, neste repositório ofereço uma API para geração do qrcode e não uma API Pix, ou seja, você pode utilizá-la para gerar qrcodes estáticos a qualquer momento se tiver qualquer chave Pix cadastrada, e você pode utilizá-la para geração de qrcodes dinâmicos desde que tenha implementado a API Pix com algum PSP que a ofertou.

A API possui um único endpoint GET que retorna a imagem do qrcode ou a string brcode de acordo com as informações enviadas nos parâmetros.

```
GET https://gerarqrcodepix.com.br/api/v1?[parametros]
```

## Gerar qrcodes estáticos

**Parâmetros de URL**

`nome*` Nome do recebedor.

`cidade*` Cidade do recebedor.

`valor*` Valor do qrcode com um ponto separando os centavos. Não se deve utilizar a separação de milhar nem vírgula. Se não estiver setado, o pagador pode definir ele mesmo o valor.

`saida*` Utilize **br** para geração da string Copia e Cola e **qr** para geração da imagem do qrcode.

`tamanho` Determina a altura em pixels do qrcode a ser gerado. Parâmetro indiferente quando se utiliza saida = br.

`chave*` Chave Pix do recebedor, que pode ser:
  - Telefone (deve incluir +55, DDD, e telefone sem hífen ou espaços. Ex: +5531912345678)
  - Email
  - Documento (CPF ou CNPJ sem traços, pontos ou barras. Apenas números.)
  - Chave aleatória (chave no formato UUID)
  
\**Dados obrigatórios.*
  
**Exemplo**

`https://gerarqrcodepix.com.br/api/v1?nome=Cecília Devêza&cidade=Ouro Preto&valor=10.00&saida=qr&chave=2aa96c40-d85f-4b98-b29f-d158a1c45f7f`

É preciso lembrar que os parâmetros podem conter espaços ou caracteres que precisam estar encodados, logo, o exemplo acima ficaria:

`https://gerarqrcodepix.com.br/api/v1?nome=Cec%C3%ADlia%20Dev%C3%AAza&cidade=Ouro%20Preto&valor=10.00&saida=qr&chave=2aa96c40-d85f-4b98-b29f-d158a1c45f7f`

## Gerar qrcodes dinâmicos

`nome*` Nome do recebedor.

`cidade*`: Cidade do recebedor.

`saida*` Utilize **br** para geração da string Copia e Cola e **qr** para geração da imagem do qrcode.

`tamanho*` Determina a altura em pixels do qrcode a ser gerado. Parâmetro indiferente quando se utiliza saida = br.

`payload*` URL do payload obtido via API Pix integrada à algum PSP.

\**Dados obrigatórios.*

**Exemplo**

`https://gerarqrcodepix.com.br/api/v1?nome=Cecília Devêza&cidade=Ouro Preto&saida=qr&payload=qrcodes-pix.gerencianet.com.br/v2/232023aab07f40ec9a383e47792f7345`

## Gerar qrcodes a partir de um BrCode já criado

`brcode*` Utilize esta opção quando já se tem o brcode criado e deseja apenas gerar a imagem do qrcode.

\**Dados obrigatórios.*

# Collection Postman

Se preferir, você pode utilizar esta collection no Postman para realizar seus testes:

[![Run in Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/98e73bcb5782024ca150)

# Doações

Esta API facilitou sua geração de qrcodes Pix ou contribuiu para que você entendesse um pouco mais sobre eles?
Faça uma doação ;)

![Qrcode estático](https://gerarqrcodepix.com.br/api/v1?nome=Cec%C3%ADlia%20Dev%C3%AAza&cidade=Ouro%20Preto&tamanho=256&saida=qr&chave=2aa96c40-d85f-4b98-b29f-d158a1c45f7f)


