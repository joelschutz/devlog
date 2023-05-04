---
title: "Goingo: Meu primeiro jogo"
description: ""
date: 2023-05-02T21:08:50.443Z
preview: ""
tags: []
categories: []
lastmod: 2023-05-04T23:15:47.197Z
---
Eu já perdi a conta de quantos protótipos eu já criei com Ebiten(ou Ebitengine como me pediram para chamar), muitos foram direto para a lixeira e outros estão perdidos em branches abandonadas em algum repositório. Porém tem ideias que são ~~bobas~~ boas demais para deixar passar e precisam de um tratamento à altura. Hoje vou falar sobre o primeiro jogo que lancei: **Goingo**, um uma versão do ancestral jogo de Go feito na totalmente na linguagem Go.

## Porque Go?

A confusão é totalmente intencional, tudo começou porque queria testar uma nova biblioteca e precisava de algo mais complexo que uma demo para ver ele em ação. Mas no fim eu acabei aprendendo bastante sobre ambos e decidi levar o projeto até o lançamento, muito porque já estava tudo ali, só precisava dar aquele pequeno esforço para finalizar.

### O jogo de tabuleiro

![Ko Loop](https://www.learn-go.net/images/ko.gif)

Go é o jogo de tabuleiro mais antigo do mundo ainda jogado segundo a [Wikipedia](https://pt.wikipedia.org/wiki/Go), teria sido criado a 2500 anos atrás na China onde ainda é muito popular. Aqui no ocidente ele ficou mais conhecido depois que o [AlphaGO](https://pt.wikipedia.org/wiki/AlphaGo), uma inteligencia artificial, venceu o campeão mundial em 2016.

Apesar de ser um jogo complexo e do desafio que é criar uma IA para jogar ele, as regras são extremamente simples. Esse foi o principal motivo de ter criado esse jogo, é um ótimo exemplo de jogo de tabuleiro que pode ser criado com algumas coisas linhas de código. Basicamente em Go:

- Cada jogador se alterna colocando uma peça de sua cor no tabuleiro
- Peças adjacentes a outras da mesma cor formam grupos
- Quando um grupo é certado completamente por peças do adversário esse grupo é captura e retirado do tabuleiro.
- Vence quem cercar mais espaços no tabuleiro ao fim da partida

Claro, a partir dessas regras nascem outras como Ko, Atari, etc, mas para criar a engine essas três são as básicas e definem o game loop. Para criar uma experiencia divertida e interessante é necessário mais funcionalidades, testes para impedir que o jogador faça movimento ilegais e uma método de contar os pontos de cada jogador.

Essa é uma das coisas que não tenho ainda no jogo, se quiserem saber quem venceu a partida os jogadores precisam contar seus próprios territórios. Parece preguiça da minha parte não adicionar isso ao jogo, mas depois de passar uma noite inteira tentando entender esse [artigo](https://www.oipaz.net/Carta.pdf), eu preferi deixar para uma depois. Eu já comentei que desenvolvi o jogo inteiro em uma semana?

### A Linguagem de Programação

![Gophers playing Go](https://raw.githubusercontent.com/MariaLetta/free-gophers-pack/master/illustrations/svg/23.svg){: style="object-fit: cover;height: 300px;" width="100%"}

É obvio que escolhi Go como linguagem porque é a minha favorita, mas quis dedicar essa parte do artigo para contar sobre uma situação que eu provavelmente não teria conseguido resolver se fosse em outra linguagem.

A biblioteca que eu queria utilizar e que deu inicio a
esse projeto se chama [Donburi](https://github.com/yohamta/donburi), é um ECS para a [Ebitengine](https://ebitengine.org/) que sempre uso por aqui. Eu ainda vou voltar a falar de ECS no futuro, é um tópico muito importante, mas o que me deu problemas foi um gerenciador de eventos que acompanha a biblioteca. Da forma que eu planejei o meu programa o gerenciador entrava em panico e derrubava o programa. Haviam formas de contornar isso, mas se tratava de um bug no código que precisava ser corrigido, não era para ela se comportar daquela forma.

Pedir uma correção em um projeto open-source é sempre uma aposta, principalmente quando você não tem familiaridade com o projeto e seus desenvolvedores. Alguns podem ser super abertos, outros podem extremamente relutantes quanto a qualquer tipo de mudança. Sem falar que nunca pode contar que alguém vai tirar parte do tempo dela para resolver um problema seu, te forçando a ter que entrar no código, achar o problema, resolver e propor uma pull-request. Ai que eu acredito que o Go entrou como um grande aliado nesse projeto, pela forma que o Go funciona e a sua sintaxe, é muito fácil entrar em um projeto e identificar como ele funciona. Não existem macros, escopos invisíveis, tipagem dinâmica, magicas de qualquer tipo, tudo em Go é explícito ao ponto que em minutos eu já sabia onde estava o problema e em poucas horas já tinha uma solução.

Contactar os desenvolvedores para incorporar essas mudanças é outra história, mas isso independe da linguagem. Minha pull-request para o [pkg/browser](https://github.com/pkg/browser/pull/52) ainda não foi vista até o momento que estou escrevendo esse artigo, por sorte [Yohamta](https://twitter.com/spaceshooting99) foi super rápido em aprovar e eu agradeço muito a ele.

## O que fiz de diferente?

Algo que é um defeito meu: começar algo e abandonar no meio do caminho. Desde a escola, eu ficava muito animado com um assunto e pesquisava tudo que conseguia sobre ele mas na hora de escrever o trabalho e amarrar tudo eu ficava com preguiça e abandonava. A experiência de fazer sempre era melhor que a de terminar, hoje ainda tenho mais um agravante que é a ideia de que o resultado não vai valer a pena então porque se dedicar até o fim? Eu venho sempre me sabotando dessa forma e vivo buscando formas de superar, mas o que ainda me prende é ter dificuldade de definir onde é o ponto que posso considerar algo como pronto.

Finalizar algo é difícil para mim porque não consigo definir claramente o resultado esperado e como chegar nele. A forma que encontrei para lidar com isso é diminuindo o escopo do trabalho e aceitando a incompletude dele, desde que eu fizesse o mínimo para me sentir confortável em mostrar para os outros. Assim o projeto nunca está "pronto", sempre é possível encontrar algo a melhorar, mas também está organizado a um ponto que não é um experimento pessoal que só você pode entender e aproveitar.

Para deixar o jogo nesse estado eu decidi que precisava de pelo menos:

- Sons: Geralmente negligenciados no desenvolvimento, sons dão uma sensação táctil ao jogo que ajuda muito a polir o jogo. Demorou um pouco até encontrar o som certo, mas também no fim foi apenas 3 samples que eu usei para as movimentações das peças.
- Menu: UI é um porre de fazer, mas é totalmente necessária, eu não preciso te convencer disso. É o tipo de coisa que é preciso criar coragem e fazer mesmo que fique feio, sem ela o jogo não é acessível jogador.
- Logo: O logo tipo é o rosto do seu produto, é o que identifica ele tanto quanto o seu nome. Eu não sou designer e imagino que também não seja, mesmo assim eu me desafiei a fazer esse desenho porque com ele eu tinha tudo que precisava para mostrar o jogo para o mundo. É cruel pensar nisso, o publico vai ser muito mais impactado por esse desenho que pelas horas de pesquisa e desenvolvimento que eu passei no código.

No fim desses três eu ainda acabei incluindo mais algumas coisinhas como alguns overlays, diferentes tamanhos de tabuleiro e a opção de jogar com uma IA usando GoGNU.

## Como jogar?

O jogo está disponível no [Itch.io](https://kam1sama.itch.io/goingo) e no [Github](https://github.com/joelschutz/goingo), o código é aberto e caso queira me ajudar pode pagar quanto quiser pelo jogo. Eu não vou falar muito porque acho que vale a pena entrar nas páginas e ver como ficou o resultado. É impressionante o que as IA de hoje em dia conseguem fazer, não se preocupe que aqui nesse post foi tudo escrito por mãos humanas.

## Conclusão

Eu deveria ter escrito esse artigo antes, mas depois de voltar da minha viagem eu fiquei me coçando para escrever código e precisava codar algumas coisas, esse jogo inclusive. Além dele ainda tem uma biblioteca que eu criei e publiquei essa semana que vou escrever um artigo a respeito. Uma coisa que me ajudou bastante para produzir tanto foram algumas ferramentas de IA, eu ainda tenho alguma resistência, mas deu para perceber essa semana o potencial que elas tem. Aqui por exemplo é um espaço que eu não acho justo pedir para o ChatGPT me ajudar, demoro um tanto para escrever esses artigos, acho importante manter essa pessoalidade nesse blog. No README e descrições desses programas eu achei o maximo poder usar a IA, você explica as coisas direto ao ponto e ela enche linguiça para você.

Um outro ponto que quero começar a usar mais IA é na tradução dos textos desse blog. No momento, a maioria do material está apenas em português e eu gostaria muito de ter ele disponível em inglês para ajudar na divulgação. Meu plano é usar os modelos da [Libretranslate](https://libretranslate.com/) para ajudar no processo, se estiver lendo isso em inglês provavelmente o meu plano deu certo.

Em 2 semanas começam as minhas aulas da faculdade e na semana que vem eu já me mudo em definitivo. Não tenho ideia de como vai ficar meu tempo livre, mas quero continuar publicando por aqui e criando jogos e outros programas legais. Mas se tiver que codar apenas ferramentas de estudo, vou vir aqui contar tudo para vocês. Me desejem sorte nessa nova jornada.

---

Espero que tenha gostado desse material e que tenha sido de ajuda, estou ainda trabalhando nesse site e nas demos para ajudar a quem tem interesse em programar coisas práticas e fora do mundinho web. Caso tenha alguma dúvida ou sugestão, por favor entre em contato pelo [GitHub](https://github.com/joelschutz) ou [e-mail](mailto:joelsschutz@yahoo.com.br).
![thank you!](https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExNDU3M2ZhZmUxZDQ4NGM3NGY1YjJlMzFkZmNkYTA2NmFhZGExNGFiNCZjdD1n/htebeL9yH0ZI9K47Jo/giphy.gif)
