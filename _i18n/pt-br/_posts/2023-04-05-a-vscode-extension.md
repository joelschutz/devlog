---
title: Lecture Timer - Minha primeira extensão do VSCode
description: ""
date: 2023-04-05T18:14:02.881Z
preview: ""
tags:
  - markdown
  - vscode
  - web
categories:
  - devlog
  - study
lastmod: 2023-04-07T11:06:32.333Z
---
## Introdução

Depois de um tempo trabalhando com desenvolvimento web, e agora com a esfriada no mercado, eu decidi que é hora de voltar para a faculdade. Quem lê o blog percebe que o que eu mais quero é encontrar algum desafio que me deixe o mais longe possível de dessas tecnologias. Por isso criei uma extensão usando JS+HTML+CSS para o VSCode.

Essa ironia vai se explicar em breve, em resumo, é o jeito que a Microsoft escolheu para tornar o desenvolvimento de extensões mais "simples" no VSCode. Mas o que isso tem a ver com voltar para a faculdade? Bem deixa eu explicar o problema:

## Minhas notas horrendas

![I don't understand anything](https://media.giphy.com/media/3o6MbbwX2g2GA4MUus/giphy.gif)

Já teve a experiencia de estudar para uma prova, voltar para as suas anotações e não fazer ideia do que se tratam? Eu tenho um pouco de dislexia, sou prolixo na hora de me expressar, e para piorar, sou a única pessoa que conheço a ter reprovado nas aulas de caligrafia. Quando me concentrava para anotar, me perdia na aula, quando fazia na pressa, não entendia nada do que escrevi. O resultado era um caderno cheio de notas confusas e feias de uma aula que eu já não lembrava sobre o que era.

Escutar as gravações das aulas acabava sendo a solução, ainda mais depois da pandemia, esse material virou essencial nos meus estudos já que eu poderia pausar, voltar e dar a atenção necessária para cada parte. Minhas notas ficaram muito mais organizadas e úteis, fui tão bem que consegui aprender a programar, fui empregado na área e acabei deixando a faculdade. Mas tem um aspecto desse processo que eu preciso resolver para ter alguma vida além dos estudos.

Gravações são ótimas, mas não são fáceis de encontrar o que está procurando. Em um texto você consegue usar o famoso `Ctrl+F` e procurar o que precisa, mesmo em um livro é muito fácil sublinhar e marcar páginas para voltar depois. Em um arquivo de audio ou video não é possível fazer algo assim, o que eu acabava fazendo era anotar a minutagem e uma frase ao lado para tentar me achar depois, quase sempre sem sucesso.

O problema não era tanto o método, mas o fato de ele ser trabalhoso e ineficiente. Para anotar uma minutagem era necessário pausar e escrever o exata valor, depois para retornar era preciso encontrar o segundo exato na linha. Sem falar que era impossível fazer algo assim durante a gravação de uma aula, coisa que vai ser essencial agora com aulas presenciais. Eu precisava de alguma ferramenta que me ajudasse a usar essa técnica e é isso que a minha extensão faz.

## Anotando em tempo real

![Typing while shit burn in front of you](https://media.giphy.com/media/1Aj491qX7K45qZs6EP/giphy.gif)

A ideia que eu tive é de uma ferramenta que me permitisse:

0. Anotar minutagens com apenas um click
1. Navegar rapidamente entre as anotações e o arquivo de video ou audio
2. Sincronizar anotações feitas em aula com feitas posteriormente
3. Permitisse marcar e encontrar facilmente os trechos da aula que mais interessam

Em outras palavras, eu queria alguma forma de rabiscar meus arquivos de audio da mesma forma que um texto ou livro, para isso eu precisava dar uma interface visual para esse audio. Que lugar melhor para começar que pelo `Visual Studio Code`, trocadilhos a parte o que pensei de fazer foi criar um arquivo de texto que pudesse conter toda as minhas anotações. O `VSCode` entra como uma boa opção de editor, não só pela capacidade de extensão, mas também porque é o editor de texto que uso diariamente e já estou acostumado.

Escrever a extensão não foi fácil, mas admito que depois de um pouco de esforço a coisa passa a fazer mais sentido. Eu queria poder dizer que a minha falta de experiencia e, em geral, ódio pelo `JavaScript` foram as causas dos meus problemas, mas nem foi tão esse o caso. O que mais complica é o design adotado pela Microsoft, a API parece uma grande sopa de decisões impulsivas e a documentação exige que você leia tudo com muita calma para não perder nenhum detalhe. De todo o material que usei, os que eu recomendo para quem quiser tentar essa empreitada são a [documentação oficial](https://code.visualstudio.com/api/get-started/your-first-extension), principalmente os exemplos, e [esse video](https://youtu.be/a5DX5pQ9p5M) de quase 4 horas. Não se incomode com o comprimento, a melhor parte desse tutorial é que conseguimos acompanhar ele debugando certas coisas que são importantíssimas, mas que raramente aparecem nos outros tutoriais.

## Ok, mas o que essa extensão faz?

Antes de tudo, vamos precisar de um arquivo `.md` com um [frontmatter](https://daily-dev-tips.com/posts/what-exactly-is-frontmatter/) preenchido com alguns dados, a extensão já vem com um snippet chamado `lecture-timer.initTimer` que vai criar algo assim:

```markdown
---
lecture:
  title: "My Title"
  file: "example.mp3" # Opcional
  duration: 120       # Opcional
---
```

Isso é tudo que precisamos para começar, o campo `file` é opcional e caso fique em branco a extensão funcionará como um timer simples. Alternativamente você pode utilizar um campo `duration` para especificar o limite de tempo do seu timer. Clique no ícone da extensão na barra lateral para abrir o painel.

![Lecture Timer Panel]({{site.baseurl}}/assets/lt-0.png)

Os controles na parte superior funcionam como você espera: `tocar`, `pausar`, `parar`, `avançar`, `retroceder`, etc. Eles servem tanto para operar os timers quanto navegar pelo audio. Na parte inferior temos os timestamps(ou minutagens) registradas no nosso arquivo, clicando em cada um ou usando as botões conseguimos navegar por eles no texto. Adicionamos novos usando os seguintes marcadores no texto:

```markdown
<!-- Marcador vazio -->
:lec-timer{time="3.774" preview="00:00:03" type="alone"}

<!-- Marcador com conteúdo inline -->
:lec-timer[content]{time="3.774" preview="00:00:03" type="inline"}

<!-- Marcador de bloco -->
:::lec-timer{time="3.774" preview="00:00:03" type="block"}
multi
line
content
:::
```

Não é necessário saber digitar esses marcadores inteiros, a extensão adiciona um novo comando para isso, basta utilizar `Ctrl+Shift+P` para abrir a palheta comandos e procurar por `lecture-timer.stampMarker`. Com ele você consegue adicionar a minutagem e caso tenha algum texto selecionado ele já formata corretamente o seu conteúdo dentro do marcador. Uma dica que eu dou é definir um atalho do teclado para esse comando que até mais fácil de usar.

A syntax desses marcadores também é proposital, eles respeitam a proposta de [Generic Directives](https://talk.commonmark.org/t/generic-directives-plugins-syntax/444) e podem facilmente ser parceados para html utilizando um plugin como [markdown-it-directive](https://github.com/hilookas/markdown-it-directive). Inclusive é esse o plugin que utilizei para renderizar os marcadores no preview do markdown:

![Lecture Timer Preview]({{site.baseurl}}/assets/lt-1.png)

Essas são as funcionalidades até agora, não são muitas mas já é o suficiente para começar a utilizar.

## Conclusão

Eu devo continuar a adicionar funcionalidades e remover bugs no futuro, ainda quero dar suporte a arquivos de video e conteúdo online. Além disso ainda tem algumas coisas de qualidade de vida que eu acredito serem necessárias, mas ainda quero testar mais na prática para ter certeza.

Caso esteja interessado, já pode instalar a versão em [pre-release](https://marketplace.visualstudio.com/items?itemName=joelschutz.lecture-timer) no seu VSCode. Sugestões e bugs podem ser enviados para o [repositório](https://github.com/joelschutz/lecture-timer), assim como contribuições caso queira dar uma força.

Esse era um dos projetos que me desafiei a terminar antes do começo das aulas da UFRGS em Maio, já vou riscar esse da lista. O outro era uma ferramenta para extrair comentários do código Go, essa já existe e funciona, mas preciso fazer um post como esse aqui no blog para dar como encerrado também e o tempo está acabando.

Depois disso devo voltar ao regime normal de posts sobre gamedev, já tenho uma demo nova e estou animado para começar a dar forma ao nosso jogo. Porém eu tenho certeza que vamos ter um hiato em breve para eu poder dar conta da minha mudança de cidade e me aclimatar na nova rotina. Não tenho também muita certeza de quanto tempo livre vou ter para me dedicar a projetos como estes, sobre isso só o futuro dirá.

---

Espero que tenha gostado desse material e que tenha sido de ajuda, estou ainda trabalhando nesse site e nas demos para ajudar a quem tem interesse em programar coisas práticas e fora do mundinho web. Caso tenha alguma dúvida ou sugestão, por favor entre em contato pelo [GitHub](https://github.com/joelschutz) ou [e-mail](mailto:joelsschutz@yahoo.com.br).
![thank you!](https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExNDU3M2ZhZmUxZDQ4NGM3NGY1YjJlMzFkZmNkYTA2NmFhZGExNGFiNCZjdD1n/htebeL9yH0ZI9K47Jo/giphy.gif)
