---
title: Fontes de Dados Geográficos
description: ""
date: 2024-04-30T00:18:36.480Z
preview: ""
tags: []
categories:
    - datascience
    - cartografia
type: default
---
No nosso ultimo post começamos um projeto de DataScience para descobrir, entre outras coisas, qual o consumo real do meu carro. Hoje, menos de um mês depois, estou com o meu celular abarrotado de fotos de painel de carro e arquivos `.gpx`. Isso porém vai ser assunto de outro dia quando começarmos a tratar e armazenar esses dados.

Nesse post vou falar sobre a minha promessa de cruzar dados de outros bancos de dados para inferir algumas coisas sobre os nossos dados. Isso nada mais vai ser uma desculpa para falar sobre um dos meus interesses particulares que é a Cartografia. Desde que comecei a usar o Qgis eu me apaixonei por esses tipos de dados que meu sonho se tornou viver disso. Então sem mais enrolação, vamos direto ao assunto:

## Dados GeoEspaciais

Um dado georeferenciado, no seu sentido mas simples, é uma informação vinculada a uma parte do globo terrestre. Como todos nós, seres humanos, ainda vivemos sobre esse planeta, essas informações visem respeito diretamente a nós e onde vivemos. Os dados de *Custo de Vida* ou *Precipitação Média* da sua cidade também dizem respeito a você de alguma forma porque é você que em algum momento fazer compras ou se molhar na chuva. Existe uma realidade material nessas informações que é muito pujante.

Nesse nosso projeto, o uso dessas informações será crucial para entender quais as variáveis ambientais do carro que interferem no consumo, sem ter coletado esses dados. Iremos inferir esses dados sobre a realidade que o carro passou usando formas indiretas. 

A confecção de mapas vai ser para mim um dos momentos mais divertidos e o uso de dados geoespaciais foi uma grande desculpa para me permitir isso. Eles serão úteis para nós visualizarmos tanto os nosso dados, quanto as nossas descobertas. 

Para cada seguimento de uma rota é possível associar uma informação, assim como é possível gerar novas informações a partir dela. Assim faremos nossas inferências.

## Inferências

Ainda no exemplo da rota, pense que o nosso aplicativo de GPS registra de tempos em tempos um **ponto** com *coordenadas geográficas* e um *timestamp* do momento do registro. Isso não nos diz muito para além da localização, porém em uma rota temos uma sequencia desses pontos. Ao comparar a distancia percorrida, dada pela distância em linha reta dos dois pontos, e o tempo decorrido, dado pela diferença entre os tempos registrados, podemos descobrir em que velocidade o carro se movia naquele seguimento.

Fazendo isso para toda a rota tempos uma nova informação sobre ela, podemos comparar essas velocidades com a de outras rotas, ou mesmo tomar uma média desses valores para classificar rotas por ele. Podemos usar essa informação e verificar se há mais correlações, mas o mais interessante ainda é cruzar esses dados com outros externos e ter ainda mais insights.

## Dados de terceiros

Existem muitos sites que disponibilizam dados públicos para pesquisa e análise, sobre os mais diferentes assuntos. O mais popular para trabalhos de DataScience é o [Kaggle](https://www.kaggle.com/), onde a comunidade pode compartilhar notebooks e datasets de forma gratuita. Esse normalmente seria o primeiro lugar a se procurar para procurar bancos de dados complementares, porém aqui vamos por outro caminho.

Os dados no Kaggle tem normalmente duas origens: dados originais produzidos pelos usuários; ou dados raspados da internet. A confiabilidade desses dados nem sempre é das melhores, estes dados podem estar desatualizados, incorretos e até adulterado, é preciso muita atenção as fontes desses dados. Como aqui não queremos ter o retrabalho de conferir esses dados, vamos utilizar dados das fontes mais confiáveis que podemos obter, vamos utilizar apenas dados oficiais.

## Bases Oficiais

Para o nosso projeto queremos poder cruzar dados e identificar:

1. Qual a temperatura e umidade no dia do trajeto, assim como as condições de chuva
2. Se o trajeto era em trecho urbano ou rural
3. Se o trajeto foi feito em estrada ou rua e quais as condições da mesma

Para identificar esses dados vamos pesquisar em 3 bases oficiais, uma para cada um desses tópicos.

1. Para obter os dados ambientais vamos utilizar a base do [INMet](https://portal.inmet.gov.br/), que conta com o histórico de medições de estações meteorológicas de todo o país. 
2. Para saber se a localização que o carro estava era em perímetro urbano ou rural vamos utilizar a classificação do [IBGE](https://ibge.gov.br/) de Concentrações Urbanas.
3. Por fim, para ter informações sobre as estradas optamos por usar uma base estadual, isso porque elas costumam estar mais atualizadas que as bases nacionais e como não saí do estado do RS, ela vai ser suficiente. Os dados obtidos são do [DAER](https://www.daer.rs.gov.br/inicial) 

Fico devendo ainda a explicação de como esses dados vão ser processados e como serão utilizados para a análise, mas isso fica para um próximo post. No momento estamos enfrentando fortes chuvas e a recomendação é não sair de casa. Devo ter algum tempo livre para fazer essa parte, mas sem promessas.
