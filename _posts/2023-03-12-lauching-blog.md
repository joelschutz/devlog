---
title: Criando um blog de desenvolvimento
layout: post
date: 2023-03-12T23:10:41.024Z
preview: ""
tags:
  - github
  - tips
  - web
  - markdown
categories:
  - tutorials
  - pt-br
lastmod: 2023-03-13T11:24:42.300Z
---
Eu sou da época que quando para criar um blog bastava entrar no [Blogger](https://www.blogger.com/), [Wordpress](https://wordpress.com/) ou [Tumblr](https://www.tumblr.com/) e criar uma conta. Em minutos você escolhia um tema, configurava tudo e jã estava online e pronto para publicar para o mundo inteiro.

Claro que todos esses sites ainda existem e funcionam muito bem. Mas além de serem bastante limitados, qualquer um que entrar no seu blog vai ficar com a impressão que seu post foi escrito em 1997 em um i386.

## Github Pages

![they don't know my development blog is hosted for free on github pages]({{site.baseurl}}/assets/7e8xd3.jpg)

Hoje em dia a galera da moda tem um novo jeito de hospedar um blog sem pagar nada por isso. Envolve um pouco mais de trabalho, mais o resultado e um site estático, completamente customizável e que talvez te renda até um emprego numa startup por ai.

Eu não vou explicar aqui como criar tudo do zero porque [esse tutorial](https://github.com/skills/github-pages) do Github vai guiar você a cada passo. O que eu vou mostrar aqui são algumas dicas para deixar seu blog ainda melhor de usar e de olhar.

## Jekyll

![Jekyll X GH Pages: They're the same picture]({{site.baseurl}}/assets/7e90ar.jpg)

Por baixo dos panos o Github usa o [Jekyll](https://jekyllrb.com/) para gerar esse site estático, é possível hospedar qualquer repositório com páginas HTML, porém usando essa ferramenta conseguimos criar as páginas em Markdown e deixar o Jekyll fazer o trabalho sujo.

Claro que não é nenhuma mágica e ai que entra a primeira dica:

### Themes e Remote Themes

No arquivo `_config.yml` que está na raiz do repositório, você pode inserir um tema usando a chave `theme`. O que não te contaram á que você pode usar qualquer tema hospedado no Github com a chave `remote_theme`. Fica algo assim:

```yaml
title: My Blog
author: My Name
remote_theme: daattali/beautiful-jekyll
```

Fazendo isso o tema vai ser automaticamente baixado na hora do build e não precisa incluir nada novo no seu repositório. Basta apenas encontrar um tema que te agrade.

Agora é só escrever o conteúdo, e para ajudar nessa parte vem a segunda dica:

### Frontmatter

![Frontmatter + Jekyll = CMS Local]({{site.baseurl}}/assets/7e96di.jpg)

Na hora de administrar os seus posts, editar tags, adicionar categorias, visualizar histórico, etc, o nosso esquema de editar o Markdown e subir para o repositório não é o mais prática. Para isso existe o [Frontmatter](https://frontmatter.codes/), se você usa um editor compatível.

A ideia desse plugin é trazer as funcionalidades de um CMS como o Wordpress para o editor. Assim você consegue facilmente administrar os seus posts, imagens e um monte de outras coisas que você vai descobrir lendo na [doc deles](https://frontmatter.codes/docs).

Só uma coisa antes de começar. A configuração padrão deles não trabalha muito bem com o Jekyll, pelo menos na minha experiência, então eu recomendo copiar o [frontmatter.json](https://github.com/joelschutz/devlog/blob/main/frontmatter.json) que criei para esse blog. Assim a maioria das funcionalidades vão funcionar corretamente.

## Concluindo

Seu blog esta no ar, bonitão, com todas as mordomias que pode querer, tudo direto no seu editor favorito. Agora não tem  mais desculpa.

Já sabe o que vai publicar, certo?

![Oh](https://media.giphy.com/media/wlObAWD80Ukfe/giphy.gif)

---
