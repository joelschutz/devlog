---
title: DIY datascience
description: ""
date: 2024-04-11T11:08:25.731Z
preview: ""
tags: []
categories:
    - datascience
type: default
---

Esses últimos meses eu realizei um sonho, eu consegui um carro! Fiz várias viagens e descobri que a minha vontade e da minha namorada é pegar o nosso Fox vermelho e desbravar esse mundo uma estradinha de cada vez. Porém, logo na primeira parada para abastecer nós já sentimos o golpe e entramos em desespero. 5,80 O LITRO DA GASOLINA! E na nossa primeira semana na cidade em que estudamos já tínhamos gastado quase 2 tanques inteiros, nunca que íamos conseguir realizar nosso sonho assim.

![VW Fox 2004]({{site.baseurl}}/assets/fox5p2005.jpg)

No caso dessa primeira semana, o motivo desse gasto todo foi muito besta: os pneus estavam descalibrados. Parece brincadeira, mas depois de calibrar os pneus o consumo do nosso carro na cidade caiu pela metade. Isso me colocou para pensar, o que mais influencia no consumo do nosso carro? Claro bastava ler um blog sobre o assunto que eu ia descobrir que coisas como a quantidade de passageiros, a qualidade do combustível, estilo de direção, se o trecho é urbano, o desgaste do motor, tudo isso influencia no consumo do carro. Mas o que nenhum desses blogs vai ser capaz de me dizer é o quanto o MEU carro consome realmente e é isso que importa no final.

Com o meu interesse em Datascience reestabelecido graças ao BAH(Business Analytics Hub), decidi que era hora de responder a esse meu pavor com dados. Afinal, para participar do curso eu estou tendo de dirigir quase 240km, ida e volta, todas as sextas-feiras. A ideia é coletar uma boa quantidade de dados para descobrir qual o consumo real do nosso carro e mais algumas informações úteis como: Qual posto é o melhor custo-benefício na minha região? Com que frequência devo calibrar os pneus? Quais trajetos valem mais a pena de carro ou de ônibus? Quanto devo gastar em combustível em uma viagem antes mesmo de pegar a estrada? Compensa usar uma rota alternativa para não pagar o pedágio? Vamos ao plano:

## Coleta de dados

![I've collected about 3800 Lists](https://media.giphy.com/media/MYehRmhUGxmDG2JiGS/giphy.gif)

O estudo dos `Sistemas Complexos` me ensinou que o mundo real é totalmente diferente do mundo ideal. Se o objetivo fosse estimar o consumo do meu carro, bastava marcar quanto o odômetro acusa entre visitas ao posto de gasolina. Uma álgebra simples ia me dizer teoricamente quanto meu carro faz de km/L. Mas o meu objetivo é ter um valor real, e a realidade ela é muito mais complexa que isso. Além disso eu estou muito empolgado de poder usar esse projeto para poder trabalhar com dados geoespaciais. Então a metodologia será a seguinte:

- A cada trajeto eu vou:
  - Fotografar o painel deixando visível o odômetro e o marcador de gasolina antes e depois de dirigir
  - Gravar o trajeto usando um aplicativo de GPS no meu celular
  - Anotar quantos passageiros tinham no carro

- A cada visita ao posto eu vou:
  - Fotografar o painel antes e depois de abastecer
  - Fotografar o painel da bomba deixando visível qual o combustível abastecido(álcool, gasolina ou aditivada), o valor total pago, a quantidade de litros e valor por litro
  - Registrar no aplicativo de GPS a localização do posto
  - Anotar:
    1. Se eu calibrei os pneus
    2. Se eu usei um aplicativo para ter desconto e quanto que foi dado de desconto
    3. Qual a bandeira do posto

Com essas informações já conseguimos alguns dados, mas mais alguns vão poder ser inferidos a partir deles.

## Inferências
![Turn data into more data](https://media.giphy.com/media/KYh1vSXtcdl0RYgLCR/giphy.gif)

Perceba que existem algumas informações redundantes que estão sendo coletadas e outras que parecem não ter muita relevância com o que estamos tentando medir, mas o objetivo disso é fazer algumas inferências que podem ser úteis depois na análise dos dados.

Comparando os dados redundantes conseguimos descobrir:

- Qual o desvio da distância acusada pelo odômetro e o GPS
- Qual o desvio da litragem acusada pela bomba e o medidor do carro, e consequentemente o erro do medidor
- Se o trajeto foi feito em trânsito intenso
- Qual a proporção da mistura de cada combustível dentro do tanque

Cruzando os dados com outras bases de dados

- Quais trajetos foram feitos em trecho urbano e estrada
- Qual a temperatura, umidade e condições de chuva no dia e local do trajeto
- Quais trechos foram realizados antes e depois de troca de óleo e outras manutenções
- Quais trechos passaram por pedágio e o valor

## Conclusões

Uma parte crucial da análise que não está nesse post é exatamente quais as questões que eu vou tentar responder com esses dados. Além das óbvias que fiz no início, muitas outras vão surgir ao longo do processo e a partir dos dados e problemas que vou acumular ao longo dos próximos meses. Por agora a maior preocupação vai ser coletar religiosamente esses dados e buscar uma formas de organizar e tratar esses dados.

Entendo que quem chegar a esse post deve ter muita curiosidade de ver os resultados e talvez até analisá-los por si mesmo. Porém, eu preciso ainda encontrar uma forma segura de fazer isso. Não quero que ninguém use os meus dados de GPS para descobrir onde eu moro por exemplo. Meus próximos posts sobre o assunto devem tratar disso, mas ainda não sei qual será o próximo feedback, fiquem atentos para mais novidades.

> Dirijam com responsabilidade

![Drive Safe!](https://media.giphy.com/media/Ts7b89eJpcyAM/giphy.gif)
