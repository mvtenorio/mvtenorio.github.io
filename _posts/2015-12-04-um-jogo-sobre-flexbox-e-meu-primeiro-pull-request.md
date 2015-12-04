---
layout: post
title:  "Um jogo sobre Flexbox e meu primeiro pull request"
date:   2015-12-04
---

Flexbox Froggy é um jogo feito pelo [@thomashpark](https://twitter.com/thomashpark) para te ajudar a entender alguns conceitos sobre flexbox. Achei a ideia muito bacana e, como o código do jogo estava no GitHub, resolvi contribuir e traduzir para o português. Você pode acessar a versão traduzida [aqui](http://flexboxfroggy.com/lang/pt-br/).

Apesar de usar o Git no trabalho e em casa há algum tempo, eu ainda não tinha contribuido para nenhum projeto Open Source, então achei que essa era uma boa oportunidade. O processo todo é bem simples, caso você já tenha familiaridade com o Git. Vou descrever aqui como fazê-lo no GitHub, mas as etapas não são muito diferentes em outras plataformas, como o Bitbucket.

O primeiro passo é fazer um fork do projeto, isto é, uma ramificação. Nessa etapa, o projeto é copiado para a sua conta no seu estado atual e qualquer alteração que você fizer só afetará a sua cópia. Para fazer o fork, procure essa opção na página do projeto para o qual você quer contribuir.

O segundo passo é o mesmo que você faz para um projeto pessoal: clone o repositório na sua máquina e faça as alterações necessárias. O importante aqui é seguir os padrões definidos pelo dono do projeto, como indentação do código, usar tabs ou espaços, criar testes para garantir que as suas alterações funcionam etc. Muitas vezes, o repositório contém um arquivo chamado Contributing.md com essas regras.

É uma boa prática criar um novo branch antes de fazer um commit, com o comando: `git checkout -b branch-name`. No lugar de "branch-name", dê um nome que descreva sua alteração.

Depois de fazer o commit e o push das alterações para o seu fork, vá ao GitHub e clique no botão "Pull Request". Um pull request nada mais é do que um pedido para que um responsável pelo projeto analise as suas alterações e faça o merge delas no projeto. Na próxima tela, você deve explicar suas modificações e confirmar. Caso o pull request seja aceito, parabéns! Agora você pode deletar o branch criado pela própria interface do GitHub. Caso o responsável tenha negado seu pull request, ele dirá os motivos pelo qual sua contribuição não foi aceita. Veja se você pode corrigir algo e tente novamente.

Existem outras etapas que podem ser necessárias, como sincronizar o seu repositório local com o repositório do projeto (se outros commits forem feitos lá enquanto você estava alterando o código na sua máquina, isso será necessário). Você pode ver essas instruções [aqui](https://help.github.com/articles/syncing-a-fork/).
