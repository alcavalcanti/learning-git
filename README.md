
<div align="center">
<h1>Learning git</h1>
<h4>Um tutorial com o objetivo de ensinar os conceitos e o funcionamento do git de uma maneira didática e os comandos de maneira mais organica, levando em conta as situações mais importantes que vivênciei até hoje trabalhando com o git.</h4>
</div>
<br>

**Sumário**
- [Visão geral](#visão-geral)
- [O famoso _Remote Repository_](#o-famoso-remote-repository)
- [Adicionando arquivos](#adicionando-arquivos)
- [Fazendo alterações](#fazendo-alterações)
- [Branches](#branches)
- [Merging](#merging)
    - [Merge Fast-Forward](#merge-fast-forward)
    - [Mesclando branches divergentes](#mesclando-branches-divergentes)
    - [Resolvendo conflitos de merge](#resolvendo-conflitos-de-merge)
- [Rebasing](#rebasing)
    - [Resolvendo conflitos de rebase](#resolvendo-conflitos-de-rebase)
- [Atualizando o _Working Environment_](#atualizando-o-working-environment)
    - [Fetch](#fetch)
    - [Pull](#pull)
    - [Escondendo as alterações (Stash)](#stash)
    - [Pull com conflitos](#pull-com-conflitos)
- [Cherry-picking](#cherry-picking)
- [Reescrevendo a linha do tempo](#reescrevendo-a-linha-do-tempo)
    - [Alterando o último commit (Amend)](#alterando-o-último-commit-amend)
    - [Rebase interativo](#rebase-interativo)    
    - [Linha do tempo pública, porque não devemos reescrevê-la e como fazer isso com segurança](#linha-do-tempo-pública-porque-não-devemos-reescrevê-la-e-como-fazer-isso-com-segurança)
- [Lendo a linha do tempo](#lendo-a-linha-do-tempo)
- [O Contexto JSM](#o-contexto-jsm)
- [Evolução continua](#evolução-continua)
---


## Visão geral

Na imagem abaixo existem 4 caixas. Uma delas está isolada, as outras três formam o que nós chamaremos de _Ambiente de Trabalho (Working Enviroment)_.
<!-- working-enviroment.png -->
![working-enviroment](https://user-images.githubusercontent.com/40603968/157252301-a9874788-f9e6-4c01-a00b-6f7495a2665a.png)

Vamos começar com o componente que está sozinho. O _Remote Repository_ é para onde você envia as alterações quando deseja compartilhá-las com outras pessoas e de onde obtém as alterações que outras pessoas fazem quando estão trabalhando.

O _Working Environment_ é o que você possui na sua máquina local.
As três partes são seu _Working Directory_, a _Staging Area_ e o _Local Repository_. Aprenderemos mais sobre eles quando começarmos a usar o git

Escolha um local em que você deseja colocar seu _Working Environment_.
Basta ir para a sua pasta pessoal ou para onde você quiser colocar seus projetos. Você não precisa criar uma nova pasta para o seu _Working Environment_.

## O famoso _Remote Repository_

Agora queremos pegar um _Remote Repository_ e colocar o que está nele na sua máquina.

Eu sugiro que usemos o repositório preparado para o tutorial [github.com/alcavalcanti/learning-git](https://github.com/alcavalcanti/learning-git/) no nosso treinamento.

> Para fazer isso use o comando `git clone https://github.com/alcavalcanti/learning-git.git`
> 
>## IMPORTANTE!
> Ao seguir o tutorial você precisará enviar as alterações feitas no seu _Working Environmnet_ ao _Remote Repository_ e o github não permite que você faça isso no repositório de outra pessoa, portanto a melhor solução é fazer um [fork](https://guides.github.com/activities/forking). Há um botão para fazer isso no canto superior direito desta página.

Agora que você possui uma cópia do _Remote Repository_ na sua conta do github através do _fork_, é hora de transferir isso para sua máquina.

Para isso usamos `git clone https://github.com/{SEU USUÁRIO}/learning-git.git`

Como você pode ver no diagrama abaixo, isso copia o _Remote Repository_ para dois lugares distintos, seu _Working Directory_ e o _Local Repository_.
Assim vemos na prática como o git é um controle de versão _robusto e distribuído_. O _Local Repository_ é uma cópia do _Remote_ e age exatamente como ele. A única diferença é que você não o compartilha com ninguém.

O que o `git clone` também faz é criar uma nova pasta no local aonde você executou o comando. Deve haver uma pasta `treinamento-git` agora. Abra-a.

<!-- clone.png -->
![Clonando o repositório remoto](https://user-images.githubusercontent.com/40603968/157280592-097e523e-3178-4eef-90f0-4c401389ff15.png)

## Adicionando arquivos

Alguém já colocou um arquivo chamado `JSM.txt` no _Remote Repository_.Então vamos realizar o mesmo processo e criar um novo arquivo e chamá-lo de `Lord.txt`.

O que você acabou de fazer é adicionar um arquivo no seu _Working Directory_.
 arquivos no seu _Working Directory_: Arquivos _tracked_, que o git conhece, e _untracked_, arquivos que o git (ainda) não conhece.

<!-- tracking_files.png -->
![Arquivos rastreados e não rastreados](https://user-images.githubusercontent.com/40603968/157286589-6dba8afe-33c1-40a8-8ec2-00f6c1ee3871.png)

Para ver o que está acontecendo no seu _Working Directory_ execute `git status`, que informará em que branch você está, se o seu _Local Repository_ é diferente do _Remote Repository_ e os arquivos _tracked_ e _untracked_.

Você verá que `Lord.txt` não é rastreado (_untracked_) e o `git status` até lhe diz como mudar isso.
Na figura abaixo, você pode ver o que acontece quando você segue a dica e executa `git add Lord.txt`: Você adicionou o arquivo à _Staging Area_, onde você coleta todas as alterações que deseja incluir no _Repository_.

<!-- add.png -->
![Adicionando arquivos ao stag](https://user-images.githubusercontent.com/40603968/157286675-e26b811e-b4b9-4e0a-8366-41b6eff761d8.png)

Quando você adicionar todas as suas alterações (que agora é apenas adicionar `Lord.txt`), você estará pronto para fazer o _commit_ do que acabou de fazer no _Local Repository_.

As alterações que você fez são uma parte significativa do trabalho, portanto, quando você executa o `git commit`, um editor de texto será aberto e permitirá que você escreva uma mensagem dizendo tudo o que você acabou de fazer. Quando você salva e fecha o arquivo de mensagens, seu _commit_ é adicionado ao _Local Repository_.

<!-- commit.png -->
![Commitando no repositório local](https://user-images.githubusercontent.com/40603968/157286741-99a89379-8eae-4894-97ba-5ebcb0fc5183.png)

Você também pode adicionar sua _mensagem de commit_ na linha de comando se você chamar `git commit` assim: `git commit -m "Adicionar Lord"`. Mas como você deseja escrever [boas mensagens de commit](https://chris.beams.io/posts/git-commit/), você deve gastar um tempo estudando e usar o editor.

Agora suas alterações estão no seu repositório local, o que é um bom local para elas, desde que ninguém mais precise delas ou você ainda não esteja pronto para compartilhá-las.

Para compartilhar seus commits com o _Remote Repository_ você precisa envia-los (`push`).

<!-- push.png -->
![Enviando para o repositório remoto](https://user-images.githubusercontent.com/40603968/157287361-00d06483-4bc3-408e-9a3d-f07d5ae98775.png)

Depois de executar o comando `git push` as alterações serão enviadas para o _Remote Repository_. No diagrama abaixo, você vê o estado após o seu `push`.

<!-- after_push.png -->
![Estado dos diretórios após o push](https://user-images.githubusercontent.com/40603968/157287725-432298f5-cf65-45e3-8eee-009932bde7bb.png)

## Fazendo alterações
Até agora apenas adicionamos um novo arquivo. Obviamente a parte mais interessante do controle de versão é a alteração de arquivos.

Dê uma olhada no arquivo `JSM.txt`.

Na verdade ele contém algum texto, mas `Lord.txt` não, então vamos mudar isso e colocar `Oi!! Eu sou o Lord e sou novo aqui.`.

Se você executar o `git status` agora, verá que o `Lord.txt` está modificado (`modified`).
Nesse estado as alterações estão apenas no seu _Working Directory_.

Se você deseja ver o que mudou no seu _Working Directory_, você pode executar o `git diff` e ver a seguinte saída:

```Diff
diff --git a/Lord.txt b/Lord.txt
index e69de29..3ed0e1b 100644
--- a/Lord.txt
+++ b/Lord.txt
@@ -0,0 +1 @@
+Oi!! Eu sou o Lord e sou novo aqui..
```

Execute `git add Lord.txt` como você fez anteriormente. Como sabemos, isso move suas alterações para a _Staging Area_.

Eu quero ver as mudanças que acabamos de realizar, então vamos executar `git diff` novamente! Você notará que desta vez a saída está em branco. Isso acontece porque o `git diff` opera apenas nas alterações no seu _Working Directory_.

Para mostrar quais mudanças já estão na _Staging Area_, podemos executar `git diff --staged` e veremos a mesma saída diff de antes.

Você pode notar que colocamos dois pontos de exclamação após o 'Oi'. Vamos mudar o `Lord.txt` novamente, para que seja apenas 'Oi!'

Se agora rodarmos `git status`, veremos que existem duas mudanças: A que já enviamos para a _Staging Area_, onde adicionamos texto, e a que acabamos de fazer, que ainda está apenas no diretório de trabalho.

Podemos dar uma olhada no `git diff` entre o _Working Directory_ e o que já enviamos para a _Staging Area_, para mostrar o que mudou desde que nos sentimos prontos para realizar um commit das mudanças.

```Diff
diff --git a/Lord.txt b/Lord.txt
index 8eb57c4..3ed0e1b 100644
--- a/Lord.txt
+++ b/Lord.txt
@@ -1 +1 @@
-Oi!! Eu sou o Lord e sou novo aqui..
+Oi! Eu sou o Lord e sou novo aqui..
```

Como a mudança é o que queríamos, vamos executar `git add Lord.txt` para enviar o estado atual do arquivo para _stage_.

Agora estamos prontos para realizar o `commit` com o que acabamos de fazer. Eu criei o commit com `git commit -m "Alterar texto de Lord"` porque senti que, para uma mudança tão pequena, escrever uma linha seria suficiente.

Como sabemos, as alterações estão agora no _Local Repository_.
Ainda podemos querer saber que mudança acabamos de commitar e o que havia antes.

Podemos fazer isso comparando commits.
Todo commit no git tem um hash exclusivo pelo qual é referenciado.

Se dermos uma olhada no `git log`, não apenas veremos uma lista de todos os commits com _hash_, como _Autor_ e _Data_, também veremos o estado do nosso _Local Repository_ e as informações locais mais recentes sobre _branches remotas_ .

No momento, o `git log` se parece com isso:

```ShellSession
commit 87a4ad48d55e5280aa608cd79e8bce5e13f318dc (HEAD -> master)
Author: {VOCÊ} <{SEU EMAIL}>
Date:   Tue Mar 08 14:02:48 2022 -0300

    Alterar texto de Lord

commit 8af2ff2a8f7c51e2e52402ecb7332aec39ed540e (origin/master, origin/HEAD)
Author: {VOCÊ} <{SEU EMAIL}>
Date:   Tue Mar 08 13:32:48 2022 -0300

    Adicionar Lord

commit 71a6a9b299b21e68f9b0c61247379432a0b6007c \1
Author: André Albuquerque <andre.albuquerque@juntossomosmais.com.br>
Date:  Tue Mar 08 09:02:48 2022 -0300

    Adicionar JSM

commit ddb869a0c154f6798f0caae567074aecdfa58c46
Author: André Albuquerque <andre.albuquerque@juntossomosmais.com.br>
Date:  Tue Mar 08 08:02:48 2022 -0300

    Adicionar texto do tutorial

```

Aqui vemos algumas coisas interessantes:
* Os dois primeiros commits são feitos por mim.
* Seu commit inicial para adicionar Lord é o _HEAD_ atual da branch _master_ no _Remote Repository_. Veremos isso novamente quando falarmos sobre ramificações (branches) e obter alterações remotas.
* O último commit no _Local Repository_ é o que acabamos de fazer e agora sabemos o seu hash.

> Observe que os hashes dos commits serão diferentes para você. Se você quiser saber exatamente como o git chega a esses IDs de revisão, dê uma olhada [neste artigo sobre a anatomia de um commit](https://blog.thoughtram.io/git/2014/11/18/the-anatomy-of-a-git-commit.html).

Para comparar esse commit e o anterior, podemos utilizar `git diff <commit>^!` (Onde `^!` diz ao git para comparar o commit com o que veio antes dele). Portanto, neste caso, eu executo `git diff 87a4ad48d55e5280aa608cd79e8bce5e13f318dc^!`.

Também podemos fazer o `git diff 8af2ff2a8f7c51e2e52402ecb7332aec39ed540e 87a4ad48d55e5280aa608cd79e8bce5e13f318dc` para o mesmo resultado e, em geral, comparar quaisquer dois commits. Note que o formato aqui é `git diff <de commit> <para commit>`, então nosso novo commit fica em segundo.

No diagrama abaixo você vê novamente os diferentes estágios de uma alteração e os comandos diff correspondentes.

<!-- diffs.png -->
![Estados de uma mudança e comandos diff relacionados](https://user-images.githubusercontent.com/40603968/157291052-0878fef3-696e-4623-9940-9da9735a6417.png)

Agora que temos certeza de que fizemos a alteração que queríamos, vá em frente e execute `git push`.

## Branches

Outra coisa que torna o git excelente é o fato de que trabalhar com branches. Na pratica, trabalhamos em uma branch desde que começamos.

Quando você clona o _Remote Repository_, seu _Working Environment_ inicia automaticamente na branch principal do repositório, ou seja, _master_.

A maioria dos fluxos de trabalho com o git incluem fazer suas alterações em uma _branch_ antes de você mesclá-las (`merge`) novamente na _master_.
Normalmente você estará trabalhando por conta própria até que esteja pronto e confiante das suas alterações, que poderão ser mescladas (mergeadas) na _master_.

> Muitos gerenciadores de repositório git, como o _GitLab_ e o _GitHub_, permitem que as branches sejam _protegidas_, o que significa que nem todo mundo pode simplesmente fazer um `push` de mudanças para lá. O _master_ geralmente é protegido por padrão.

Retornaremos a todas essas coisas com mais detalhes quando precisarmos delas.

No momento queremos criar uma branch para fazermos algumas alterações. Fazemos isso porque no momento não queremos alterar o estado de trabalho da branch _master_ uma vez que é uma alteração nova e de certa forma ainda insegura para que afete a _master_.

As branches ficam no _Local_ e no _Remote Repository_. Quando você cria uma nova branch, o conteúdo dessa branch será uma cópia de qualquer branch em que você esteja trabalhando no momento.

Vamos fazer algumas alterações no `JSM.txt`. Vamos adicionar um texto na segunda linha do arquivo.

Queremos compartilhar essa mudança, mas não colocá-la na _master_ imediatamente, então vamos criar uma branch para ela usando `git branch <branch name>`.

Para criar uma nova branch chamada `change_JSM`, você pode executar o comando `git branch change_JSM`.

Isso adiciona a nova branch ao _Local Repository_.

Enquanto seu _Working Directory_ e _Staging Area_ realmente não são afetados pelas branches, você sempre cria um `commit` com a branch em que está atualmente.

Você pode pensar em _branches_ no git como pacotes, agrupando uma série de mudanças. Quando você faz um `commit`, você adiciona o que está empacotado no momento.

Apenas adicionar uma branch não aplica ou compartilha todas essas mudanças de imediato, apenas cria um pacote.
De fato, o estado em que seu _Local Repository_ está atualmente, pode ser visto como outro pacote, chamado _HEAD_, que aponta para qual branch e commit você está atualmente.

Se isso parecer complicado inicialmente, os diagramas abaixo ajudarão a esclarecer um pouco as coisas:

<!-- add_branch.png -->
![Estado após adicionar branch](https://user-images.githubusercontent.com/40603968/157496619-fc5b276f-f781-4c58-93fe-0cb9ae2a441a.png)

Para mudar para a nossa nova branch, você terá que usar o comando `git checkout change_JSM`. O que isso faz é simplesmente mover o _HEAD_ para a branch que você especificar.

> Como você normalmente deseja mudar para uma branch logo após criá-la, existe a conveniente opção `-b` disponível para o comando `checkout`, que permite realizar `checkout` diretamente em uma branch nova, para que você não precisa criá-la de antemão.
>
> Então, para criar e mudar automáticamente mover o head para nossa branch `change_JSM`, também poderíamos ter executado `git checkout -b change_JSM`.

<!-- checkout_branch.png -->
![Estado após trocar de branch](https://user-images.githubusercontent.com/40603968/157497587-d4c2c67c-8e5a-4e55-9aa9-871e17beb2cb.png)

Você notará que seu _Working Directory_ não mudou e o fato de termos modificado `JSM.txt` ainda não está relacionado à branch em que estamos inseridos. Agora você pode adicionar (`add`) e fazer `commit` da alteração em `JSM.txt`, como fizemos no _master_ antes, que irá mover o arquivo para a _Staging Area_ (nesse ponto ainda não está relacionado à branch) e, finalmente, _'committar'_ sua alteração na branch `change_JSM`.

Há apenas uma coisa que você não pode fazer ainda. Tente enviar (`git push`) suas alterações para o _Remote Repository_.

Você verá o seguinte erro e uma sugestão de como resolver o problema:

```ShellSession
fatal: The current branch change_JSM has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin change_JSM 
```

Mas fazer isso sem entender o que realmente está acontecendo nunca é uma boa opção. Então, o que são _upstream branches_ e _remotes_?

Lembra quando clonamos o _Remote Repository_ há um tempo atrás? Nesse ponto, ele não continha apenas este tutorial e o arquivo `JSM.txt`, mas na verdade duas ramificações.

Quando copiamos as coisas do _Remote Repository_ para o seu _Working Environment_, algumas etapas extras ocorreram implicitamente.

O Git configurou o _remote_ do seu _Local Repository_ para ser o _Remote Repository_ que você clonou e também deu a ele o nome padrão `origin`.

> Seu _Local Repository_ pode rastrear vários _remotes_ inclusive com nomes diferentes, mas seguiremos apenas com a `origin` nesse tutorial para simplificar as coisas.

Em seguida, ele copiou as duas branches remotas no seu _Local Repository_ e, finalmente, alterou para a _master_ para você (`git checkout`).

Ao fazer isso, outra etapa implícita acontece. Quando você faz `checkout` de um nome de uma branch que tenha uma correspondência exata nas branches remotas, você obterá uma nova branch _local_ que está vinculada à branch _remote_. A branch _remote_ é a branch _upstream_ do seu _local_.

Nos diagramas anteriores, você pode ver apenas as branches locais que possui. Você pode ver essa lista de branches locais executando o comando `git branch`.

Se você quiser ver também as branches remotas que seu _Local Repository_ conhece, você pode executar `git branch -a` para listar todas elas.

<!-- branches.png -->
![Branches remotas e locais](https://user-images.githubusercontent.com/40603968/157501089-94a6dfa4-64d3-4004-889c-209efaaed630.png)

Agora que já investigamos o que houve quanto tentamos enviar as alterações podemos efetivamente executar o `git push --set-upstream origin change_JSM`, e efetuar o `push` das alterações em nossa branch para um novo _remote_. Isso criará a branch `change_JSM` no _Remote Repository_ e definirá o nosso _local_ `change_JSM` para rastrear essa nova branch.

> Existe outra opção para quando realmente queremos que nosso branch rastreie algo que já existe no _Remote Repository_. Um exemplo de cenário comum é quando um colega já realizou algumas mudanças, enquanto estávamos simultaneamente trabalhando em algo relacionado em nossa branch local, e gostaríamos de integrar as duas. Então poderíamos simplesmente definir o _upstream_ para a nossa branch `change_JSM` como um novo _remote_ usando `git branch --set-upstream-to=origin/change_JSM` e rastrear a branch _remote_.

Depois disso, dê uma olhada no seu _Remote Repository_ no github, sua branch estará lá, devidamente compartilhada e pronta para outras pessoas verem e trabalharem.

Vamos ver como você pode obter as alterações de outras pessoas em seu _Working Environment_ em breve, mas primeiro trabalharemos um pouco mais com as branches, para introduzir todos os conceitos que também entram em jogo quando obtemos novidades do _Remote Repository_.

## Merging

Como você e todos os outros engenheiros e desenvolvedores em geral trabalharão em branches, precisamos entender sobre como obter alterações de uma branch para outra, realizando o _merge_ delas.

<!-- NO CONFLICT -->
Acabamos de alterar o arquivo `JSM.txt` na branch `change_JSM`.

Se você executar `git checkout master`, vai perceber que o `commit` que fizemos na outra branch ainda não esta lá. Para colocar as alterações na master, precisamos mesclar (`merge`) a branch `change_JSM` na master.

**Note que você sempre mescla uma branch específica com a que você está atualmente.**

### Merge Fast-Forward

Como já fizemos o checkout na _master_, agora podemos executar `git merge change_JSM`.

Como não existem outras alterações conflitando no arquivo `JSM.txt` e não mudamos nada na _master_, isso ocorrerá sem problemas na chamada 'mesclagem' _fast forward_ (rápida).

Nos diagramas abaixo, você pode ver que isso significa apenas que o pacote _master_ pode simplesmente ser entregue para onde o _change_JSM_ está.

O primeiro diagrama mostra o estado antes de realizar nossa mesclagem (`merge`). A _master_ ainda está no commit anterior e, na outra branch, fizemos mais um commit adicionando mais um passo na linha do tempo.

<!-- before_ff_merge.png -->
![Antes do merge fast forward](https://user-images.githubusercontent.com/40603968/157505361-b8ab8532-5612-49f6-8dbc-951338f2ea8a.png)

O segundo diagrama mostra o que mudou com o nosso `merge`.

<!-- ff_merge.png -->
![Após o merge fast forward](https://user-images.githubusercontent.com/40603968/157505779-d37a2e49-7238-4769-b7d3-5631425047c8.png)

### Mesclando branches divergentes

Vamos aumentar a complexidade. Adicione um trecho texto em uma nova linha em `Lord.txt` na _master_ e faça um commit.

Então execute `git checkout change_JSM`, altere `JSM.txt` e faça outro commit.

No diagrama abaixo, você vê como nosso histórico de commits agora se parece. _Master_ e `change_JSM` se originaram do mesmo commit, mas desde então eles _divergiram_, cada um tendo seu próprio commit adicional.

<!-- branches_diverge.png -->
![Commits divergentes](https://user-images.githubusercontent.com/40603968/157507868-79a3c0b5-1af0-444c-9ea5-85fd69ad8b65.png)

Se você voltar para a master (`git checkout master`) e executar `git merge change_JSM`, uma mesclagem de avanço rápido (_fast forward_) não será possível. Em vez disso, o editor de texto será aberto e permitirá que você altere a mensagem do commit de merge que o git está prestes a criar para reunir as duas branches. O diagrama abaixo mostra o estado do git após a mesclagem (`merge`).

<!-- merge.png -->
![Mesclando branches](https://user-images.githubusercontent.com/40603968/157508889-4b62136d-80df-4f43-83bc-0190da0dd53e.png)

O novo commit envia as alterações que fizemos na branch `change_JSM` para a _master_.

As revisões no git não são apenas uma captura instantânea de seus arquivos, mas também contêm informações de onde elas vieram. Cada `commit` tem um ou mais commits pais. Nosso novo commit de `merge` possui como seus pais o último commit da _master_ e o commit que fizemos na branch `change_JSM`.

### Resolvendo conflitos de merge

Até agora nossas mudanças não causaram nenhum conflito. Vamos criar um cenário conflitante e depois solucioná-lo.

Crie e faça o checkout de uma nova branch. Use o `git checkout -b` para simplificar o processo. Eu chamei a minha de `branch_do_lord`.

Nessa branch faremos uma alteração no `Lord.txt`.
A primeira linha ainda deve ser `Oi! Eu sou o Lord. E sou novo aqui.` Mude isso para `Oi! Eu sou o Lord. E sou novo aqui.`

Faça o `commit` da sua alteração e volte (`checkout`) para a branch _master_. Aqui vamos mudar a mesma linha para `Oi!! Eu sou o Lord e estou aqui há algum tempo.` e realizar um `commit` da alteração.

Agora é hora de fazer o `merge` da branch `branch_do_lord` com a _master_.

Ao tentar isso, você receberá a seguinte mensagem de retorno:

```ShellSession
Auto-merging Lord.txt
CONFLICT (content): Merge conflict in Lord.txt
Automatic merge failed; fix conflicts and then commit the result.
```
A mesma linha mudou nas duas branches, e o git não consegue resolver esse problema sem a nossa intervenção.

Se você executar o comando `git status`, receberá dicas úteis de como continuar.

Primeiro, temos que resolver o conflito manualmente.

> Para um conflito fácil como este o editor de texto servirá bem. Para mesclar arquivos grandes com muitas alterações, uma ferramenta mais robusta tornará sua vida muito mais fácil, geralmente as IDEs mais conhecidas do mercado contam com ferramentas de controle de versão e uma visualização comparativa para mesclagem e resolução de conflitos.

Se você abrir `Lord.txt`, verá algo semelhante a isso:

```Diff
<<<<<<< HEAD
Oi!! Eu sou o Lord e estou aqui há algum tempo.
=======
Oi! Eu sou o Lord e sou novo aqui.
>>>>>>> branch_do_lord
[... a linha 2 não é importante para nosso cenário de conflito]
```

No topo, você vê o que mudou em `Lord.txt` no HEAD atual. Abaixo, o que mudou na branch que estamos mesclando.

Para resolver o conflito manualmente, você só precisa ter um conteúdo razoável e sem as linhas especiais que o git introduziu no arquivo.

Então vá em frente e mude o arquivo para algo assim:

```
Oi! Eu sou o Lord e estou aqui há algum tempo.
[...]
```

A partir daqui, o que estamos fazendo é exatamente o que faríamos para qualquer alteração.
Nós enviamos para _stage_ quando executamos `add Lord.txt`, e então fazemos o `commit`.

Já conhecemos o commit das alterações que fizemos para resolver o conflito. É o _merge commit_ que está sempre presente ao mesclar.

Se por ventura você perceber no meio da resolução de conflitos que realmente não deseja seguir com o `merge`, você pode simplesmente cancelar o procedimento (`abort`) executando o comando `git merge --abort`.

## Rebasing

O Git também oferece outra maneira de integrar mudanças entre duas branches, que é chamada de `rebase`.

É importante lembrar que uma branch é sempre baseada em outra. Quando você a cria, ela é baseada e ramifica de algum lugar.

No nosso exemplo de mesclagem simples (_não conflitante_), ramificamos a _master_ em um commit específico e, em seguida, fizemos commit de algumas alterações na _master_ e na branch `change_JSM`.

Quando uma branch está divergindo daquela em que se baseia e você deseja integrar as alterações mais recentes na atual, o `rebase` oferece uma maneira mais limpa de fazer isso do que uma `mesclagem` faria.

Como vimos anteriormente um `merge` introduz um _merge commit_ no qual os dois anteriores são integrados novamente.

Visto de forma simples, o rebasing muda apenas o ponto da linha do tempo (o commit) no qual sua branch se baseia.

Para simular esse cenário vamos primeiro fazer o checkout da branch _master_ novamente e depois criar uma nova branch baseada nela.
Chamei a minha branch de `add_nicolas`, adicionei um novo arquivo chamado `Nicolas.txt` e fiz um commit com a mensagem 'Adicionar Nicolas'.

Após o commit, volte para a _master_, faça outra alteração e faça o commit. Eu modifiquei o arquivo `JSM.txt` adicionando mais texto.

Como em nosso segundo cenário de mesclagem, a linha do tempo dessas duas branches divergem a partir de um ponto comum, como você pode ver no diagrama abaixo.

<!-- before_rebase.png -->
![Estado antes do rebase](https://user-images.githubusercontent.com/40603968/157514252-398a4670-c810-4b8d-abfe-115fff855a95.png)

Agora vamos executar `checkout add_nicolas` novamente, pegar a mudança que foi feita na _master_ e enviar para a branch em que estamos trabalhando!

Quando executamos `git rebase master`, baseamos nossa branch `add_nicolas` no estado atual da branch _master_.

A mensagem de retorno do comando nos da uma boa idéia da situação:

```ShellSession
First, rewinding head to replay your work on top of it...
Applying: Adicionar Nicolas
```

_HEAD_ aponta para o commit atual em que estamos trabalhando no _Working Environment_.

Atualmente ele está apontando para o mesmo lugar que a branch `add_nicolas` antes do rebase acontecer. O comando rebase volta ao primeiro ponto comum da linha do tempo, antes de passar para o head atual da branch que queremos nos basear.

Portanto, o _HEAD_ passa do commit _cfc112..._ para o commit  _7639f4..._ que está no head da _master_ e então o rebase aplica cada commit que fizemos na nossa branch `add_nicolas`.

Para ser mais exato o que o _git_ faz depois de retornar o _HEAD_ de volta ao ponto comum dos branches, é armazenar partes de cada commit que você fez na branch (o `diff` das mudanças, o texto do commit, autor, etc. .).

Depois disso, ele faz um `checkout` do último commit da branch na qual você está reestruturando e aplica cada uma das alterações armazenadas como __um novo commit__ no topo dela.

Em nossa visão simplificada original, assumiríamos que após o `rebase` o commit _cfc112..._ não aponta mais para o ponto comum em sua linha do tempo, mas aponta para o head da _master_.
De fato, o commit _cfc112..._ não existe mais, e a branch `add_nicolas` começa com um novo commit _0ccaba..._, que tem o commit mais recente de _master_ como seu ponto anterior.
Nós fizemos parecer que a branch `add_nicolas` foi baseada na _master_ atual, e não em uma versão mais antiga, mas ao fazê-lo reescrevemos o histórico da branch.
No fim do treinamento, vamos estudar um pouco mais sobre como reescrever o histórico e quando é correto e incorreto fazer isso.

<!-- rebase.png -->
![Histórico após rebase](https://user-images.githubusercontent.com/40603968/157525920-3516026f-c9d6-4f91-a95e-67e9fa501525.png)

`Rebase` é um recurso útil quando você está trabalhando em sua própria branch de desenvolvimento, que é baseada em uma branch compartilhada, por exemplo, a _master_.

Usando o rebase, você pode garantir que integra frequentemente as alterações que outras pessoas fazem e enviam para _master_, mantendo uma linha do tempo linear limpa e que permite fazer uma mesclagem de avanço rápido (`fast-forward merge`) quando chegar a hora de colocar seu trabalho na branch compartilhada.

Manter uma linha do tempo linear também torna a leitura ou a visualização (use o comando `git log --graph` ou dê uma olhada na visualização de branch do _GitHub_) do log de commits muito mais agradável do que ter um histórico repleto de _merge commits_, geralmente usando o texto padrão.

### Resolvendo conflitos de rebase

Assim como em um `merge`, você pode ter conflitos se você tiver dois commits alterando as mesmas partes de um arquivo.

No entanto, quando você encontra um conflito durante um `rebase` você não o corrige em um _merge commit_ extra, mas pode simplesmente resolvê-lo no commit que está sendo aplicado no momento.
Baseando como vimos anteriormente, suas alterações diretamente no estado atual da branch original.

Resolver os conflitos de `rebase` é muito parecido com o que você faria para um `merge`. A única distinção é que, como você não está introduzindo um _merge commit_, não há necessidade de fazer um `commit` da sua resolução. Simplesmente execute o `add` das alterações, enviando para _Staging Environment_, e depois execute `git rebase --continue`. O conflito será resolvido no commit que estava sendo aplicado.

Assim como no processo de merge, você sempre pode parar e cancelar tudo o que fez até o momento executando `git rebase --abort`.

## Atualizando o _Working Environment_

Até aqui vimos apenas como fazer e compartilhar alterações.

Isso se encaixa no que você fará se estiver trabalhando por conta própria, mas geralmente haverá muitas pessoas que fazem o mesmo no seu ambiente e queremos que as alterações sejam transferidas do _Remote Repository_ para o nosso _Working Environment_ para mantê-lo atualizado.

Como já faz algum tempo , vamos dar uma olhada novamente nos componentes do git:

<!-- components.png -->
![componentes do git](https://user-images.githubusercontent.com/40603968/157252301-a9874788-f9e6-4c01-a00b-6f7495a2665a.png)

Assim como você possui seu _Working Environment_, todos os outros que trabalham no mesmo código-fonte possuem o seu próprio.

<!-- many_dev_environments.png -->
![muitos dev environments](https://user-images.githubusercontent.com/40603968/157536646-692f6cd9-9519-4210-8ff4-5b8533ed40ad.png)

Todos esses _Working Environments_ possuem suas próprias alterações no _Working Directory_ e _Staging Area_, que em algum momento geram um novo `commit` no _Local Repository_ e são finalmente enviadas através (`push`) para o _Remote Repository_.

No nosso exemplo, usaremos as ferramentas on-line oferecidas pelo _GitHub_ para simular alguém fazendo alterações no _remote_ enquanto trabalhamos.

Vá para o seu `fork` deste repositório no [github.com](https://www.github.com) e abra o arquivo `JSM.txt`.

Encontre o botão de editar o arquivo, faça uma alteração e crie o commit através do site.

<!-- github.png -->
![editar o arquivo pelo github](https://user-images.githubusercontent.com/40603968/157538234-5e7e5926-28b2-48ff-9378-619a55b2d564.png)

Assim adicionamos uma alteração remota ao `JSM.txt` em uma branch chamada `fetching_changes`, mas na sua versão do repositório você pode, alterar o arquivo na `master`.

### Fetch

Lembrando que quando você executa `git push`, sincroniza as alterações feitas no _Local Repository_ no _Remote Repository_.

Para receber as alterações feitas no _Remote_ e atualizar o seu _Local Repository_, utilizamos o comando `git fetch`.

Ao executar o `git fetch` sincronizamos o nosso _Local Repository_ com o Remote recebendo assim todas as alterações (commits e branches) que ocorreram por lá.

Observe que, no estado atual, as alterações ainda não estão integradas com as branches locais e consequentemente também não estão no _Working Directory_ e na _Staging Area_.

<!-- fetch.png -->
![Fazendo fetch das alterações](https://user-images.githubusercontent.com/40603968/157540661-18dad8cd-c2e0-4a39-b4d2-6d0b12ca78a5.png)

Se você executar `git status` agora, verá outro ótimo exemplo de comandos git dizendo exatamente o que está acontecendo:

```ShellSession
> git status
On branch fetching_changes
Your branch is behind 'origin/fetching_changes' by 1 commit, and can be fast-forwarded.
  (use "git pull" to update your local branch)
```

### Pull

Como não temos nenhuma alteração em _working_ ou _staged_, podemos executar o `git pull` agora para obter as alterações do _Remote Repository_ até a nossa área de trabalho.

> Puxar implicitamente também faz `fetch` do _Remote Repository_, mas as vezes é uma boa idéia fazer um `fetch` por si só.
> Por exemplo, quando você deseja sincronizar qualquer nova branch _remote_, ou quando deseja garantir que seu _Local Repository_ esteja atualizado antes de fazer uma `git rebase` em algo como `origin/master`.

<!-- pull.png -->
![Realizando pull de alterações](https://user-images.githubusercontent.com/40603968/157546820-8e358c60-8a00-4dfd-8588-b3b2ac9f77df.png)

Antes de fazer o `pull`, vamos alterar um arquivo localmente para ver o que acontece.

Vamos alterar o arquivo `JSM.txt` no nosso _Working Directory_.

Agora, se você tentar fazer um `git pull`, verá o seguinte erro:

```ShellSession
> git pull
Updating df3ad1d..418e6f0
error: Your local changes to the following files would be overwritten by merge:
        JSM.txt
Please commit your changes or stash them before you merge.
Aborting
```

Você não pode executar `pull` enquanto existirem modificações nos arquivos no _Working Directory_ que também são alteradas pelos commits que você está puxando (`pull`).

Embora uma maneira de contornar isso seja adicioná-las (`add`) ao _Staging Environment_ e criar o `commit`, este é um bom momento para aprender sobre outro recurso importante do git, o `git stash`.

### Stash

Um `git stash` é basicamente um local seguro onde você armazena as alterações ainda no _Working Directory_.

Os comandos que você mais irá utilizar relacionados a essa armazenagem são o `git stash`, que coloca qualquer modificação feita no _Working Directory_ em stash (oculta), e o `git stash pop`, que recebe a última alteração que foi salva em stash e a aplica ao _Working Directory_ novamente.

Vale ressaltar que o `git stash pop` remove a última alteração escondida antes de aplicá-la novamente.
Se você não deseja remover as alterações do stash no momento de aplicá-las, pode usar o `git stash apply`.

Para inspecionar o seu `stash` atual, você pode usar o `git stash list` para listar as entradas individuais, e o `git stash show` para mostrar as alterações da última entrada no `stash`.

> Outro comando interessante é o `git stash branch {BRANCH NAME}`, que cria um branch a partir do HEAD no momento em que você armazenou as alterações e aplica as alterações armazenadas nessa branch.

Agora que sabemos sobre `git stash`, vamos executá-lo para remover nossas alterações locais em `JSM.txt` do _Working Directory_ para que possamos prosseguir e realizar o `git pull` das alterações que fizemos no Github.

Depois disso, vamos executar `git stash pop` para recuperar as alterações.
Como tanto o commit que puxamos quanto a alteração que ocultamos (`stash`) modificam `JSM.txt`, você terá que resolver o conflito da mesma forma que faria em um `merge` ou `rebase`. Quando terminar, aexecute o `add` e em seguida o _commit_ da alteração.

### Pull com conflitos

Agora que entendemos como buscar (`fetch`) e puxar (`pull`) as mudanças remotas para o nosso _Working Environment_, é hora de criar alguns conflitos.

Não efetue o `push` do commit que alterou o arquivo `JSM.txt` e volte ao seu _Remote Repository_ em [github.com](https://www.github.com).

Vamos alterar o `JSM.txt` novamente e _commitar_ a alteração.

Agora existem dois conflitos entre nossos _Local_ e _Remote Repositories_.

Não se esqueça de executar o `git fetch` para ver a mudança remota sem puxá-la (`pull`) imediatamente.

Se você executar o `git status`, verá que as duas branches têm um commit nelas diferente da outra.

```ShellSession
> git status
On branch fetching_changes
Your branch and 'origin/fetching_changes' have diverged,
and have 1 and 1 different commits each, respectively.
  (use "git pull" to merge the remote branch into yours)
```

Alteramos o mesmo arquivo em ambos os commits para introduzir um conflito de `merge` que precisaremos resolver.

Quando você executa `git pull` enquanto existe uma diferença entre o _Local_ e o _Remote Repository_, exatamente a mesma situação que acontece quando você executa `merge` de 2 branches.

Podemos pensar no relacionamento entre branches no _Remote_ e aquele no _Local Repository_ como um caso especial de criação de uma branch com base em outra.
Uma branch local é baseada no estado de uma branch no _Remote_ desde a última vez que você a buscou utilizando o (`fetch`).

Pensando assim, as duas opções que possuimos para receber mudanças remotas fazem muito sentido:

Quando você executa `git pull`, as versões _Local_ e _Remote_ de uma branch serão mescladas através de um(`merge`). Assim como mesclagem de branches isso apresentará um commit de _merge_.

Como qualquer branch _local_ é baseada em sua respectiva versão _remote_, também podemos executar `rebase`, para que qualquer alteração que possamos ter feito localmente apareçam como se fossem baseadas na versão mais recente disponível no _Remote Repository_.
Para fazer isso, podemos usar `git pull --rebase` (ou a abreviação `git pull -r`).

Se tiver dúvidas pode voltar ao tópico de [Rebasing](#rebasing).

> Você também pode dizer ao git para usar `rebase` em vez de `merge` como estratégia padrão quando executar `git pull`, configurando a configuração `pull.rebase` com um comando como este: `git config --global pull.rebase true`.

Vamos executar o `git pull -r` para obter as mudanças do _remote_, fazendo parecer que nosso novo commit aconteceu depois delas.

Obviamente, como em um `rebase` normal (ou `merge`) você terá que resolver o conflito que introduzimos para que o `git pull` finalize o processo.

## Cherry-picking

> Agora você entende como usar todos os comandos git mais comuns e como eles funcionam.
>
> Espero que isso torne os conceitos a seguir muito mais simples de entender do que apenas listado quais comandos executar.
>
> Então, vamos aprender a fazer um `cherry-pick` de commits.

> _Curiosidade:_ A tradução de `cherry-pick` é colher cereja 🍒.

Sempre que quisermos apenas fazer algumas alterações em uma branch e aplicá-las a outra, precisaremos do recurso de `cherry-pick`.

É exatamente isso que o `git cherry-pick` permite que você faça com commits isolados ou com um grupo específico de commits.

Assim como durante um `rebase`, isso realmente colocará as alterações desses commits em um novo commit em sua branch atual.

Vamos dar uma olhada nos exemplos de cada `cherry-pick` com um ou mais commits.

A figura abaixo mostra três branches antes de fazermos qualquer coisa. Vamos supor que realmente queremos receber algumas mudanças da branch `add_nicolas` na branch `change_JSM`. Infelizmente, eles ainda não entraram na master, portanto, não podemos apenas executar `rebase` na master para obter essas alterações (juntamente com outras alterações da outra branch, que talvez nem temos interesse).

<!-- cherry_branches.png -->
![Branches antes do cherry-pick](https://user-images.githubusercontent.com/40603968/157669548-8f0f6802-0f14-46d9-a48e-f350f62e8554.png)

Então vamos fazer `git cherry-pick` do commit _63fc42_.
A figura abaixo mostra o que acontece quando executamos `git cherry-pick 63fc42`

<!-- cherry_pick.png -->
![Cherry-pick de um único commit](https://user-images.githubusercontent.com/40603968/157671452-f8e73d28-df0e-4a4e-a19c-f5f38302656f.png)

Um novo commit com as alterações específicas que queríamos aparece na branch.

> É importante salientar que como qualquer outro tipo de alteração de uma branch, qualquer conflito que ocorra durante um `cherry-pick` precisará ser resolvido antes que o comando possa ser executado.
>
> Como todos os outros comandos, você pode continuar (`--continue`) um `cherry-pick` quando resolver os conflitos, ou abortar (`--abort`) o comando por completo.

O diagrama abaixo mostra um `cherry-pick` de um conjunto de commits em vez de um único. Você pode simplesmente fazer isso chamando o comando no formato `git cherry-pick <from>..<to>` ou, no nosso exemplo abaixo, como `git cherry-pick 0cfc1d2..41fbfa7`.

<!-- cherry_pick_range.png -->
![Cherry-pick de um conjunto de commits](https://user-images.githubusercontent.com/40603968/157673299-7dc2982a-0dd7-4a6e-92b6-bdb161e02514.png)

## Reescrevendo a linha do tempo

> Você ainda se lembra de [`rebase`](#rebasing) bem o suficiente, certo? Caso contrário, volte rapidamente para essa tópico antes de continuar aqui, pois usaremos o que já sabemos enquanto aprendemos a mudar a linha do tempo.

Como você sabe, um `commit` contém basicamente suas alterações, uma mensagem e algumas outras coisas.

A linha do tempo de uma branch é composta por todos os seus commits.

Mas digamos que você acabou de fazer um `commit` e, em seguida, observou que esqueceu de adicionar um arquivo ou que você cometeu um erro de digitação e a alteração deixou você com um código com bug.

Examinaremos brevemente duas coisas que poderíamos fazer para corrigir isso e fazer parecer que nunca aconteceu.

Vamos mudar para uma nova branch com `git checkout -b rewrite_timeline`.

Agora vamos fazer algumas alterações no `JSM.txt` e no `Lord.txt` e executar o comando `git add JSM.txt`.

Então vamos fazer o `commit` definindo a mensagem como "Essa é uma linha do tempo".

Você verá que cometemos alguns erros aqui:

* Nós esquecemos de adicionar as mudanças de `Lord.txt`
* Nós não escrevemos uma [boa mensagem de commit](https://chris.beams.io/posts/git-commit/) já que ela não descreve com clareza e objetividade o que foi feito.

<!-- amending -->
### Alterando o último commit (Amend)
Uma maneira de corrigir ambos os itens de uma só vez seria alterar (`amend`) o commit que acabamos de fazer.

Alterar o último commit basicamente funciona como criar um novo.

Dê uma olhada no seu último commit executando o`git show {COMMIT}`. Coloque o hash de confirmação (que você provavelmente ainda verá em sua linha de comando ao ter executado `git commit` ou no `git log`).

Assim como no `git log`, você verá a mensagem, o autor, a data e as alterações.

Agora vamos alterar (`amend`) o que fizemos nesse commit.

Execute `git add Lord.txt` para enviar as alterações para a _Staging Area_ e, em seguida, `git commit --amend`.

O que acontece a seguir é o desenrolar do commit, as novas alterações da _Staging Area_ adicionadas no commit existente e a abertura do editor da mensagem de commit.

No editor, você verá a mensagem de commit anterior.
Vamos alterá-la para algo mais condizente com o trabalho.

Depois que você terminar, dê uma olhada no último commit com `git show HEAD`.

O hash de confirmação é diferente. O commit original se foi e agoraexiste um novo, com as alterações combinadas e a nova mensagem de commit.

> Observe como os outros dados de commit, como autor e data, não são alterados em relação ao commit original. Você pode altera-los também, se você realmente quiser, usando os sinalizadores extras `--author={AUTHOR}` e `--date={DATE}`.

### Rebase interativo
<!-- squashing -->
Geralmente, quando executamos `git rebase`, nós fazemos `rebase` em uma branch. Quando fazemos algo como `git rebase origin/master`, o que realmente acontece é um rebase no _HEAD_ dessa branch.

Se quiséssemos, poderíamos fazer `rebase` em qualquer commit.

> Lembre-se que um commit contém informações sobre o histórico que veio antes dele

Como muitos outros comandos, o `git rebase` possui um modo _interactive_.

O `rebase` _interativo_ permite alterar o histórico o quanto quiser e pode ser muito útil na sua rotina de trabalho, especialmente se você seguir um fluxo realizando muitos pequenos commits de suas alterações, o `rebase` interativo será o seu aliado mais próximo porque te permitirá voltar facilmente se cometer um erro.

Vamos voltar para branch _master_ e fazer `git checkout` de uma nova branch para trabalharmos nela.

Como ja fizemos antes vamos realizar algumas alterações nos arquivos `JSM.txt` e `Lord.txt` e, em seguida, executar o comando `git add JSM.txt`.

O próximo passo é fazer `git commit` definindo uma mensagem como "Adicionar texto em JSM ".

Agora, em vez de alterar esse commit, execute `git add Lord.txt` e `git commit`. Como mensagem podemos escrever "Adicionar Lord.txt".

E para tornar as coisas mais interessantes, faremos outra alteração em `JSM.txt`, na qual faremos `git add` e `git commit`. Como mensagem, defini "Adicionar mais texto a JSM".

Se agora analisarmos o histórico da branch com `git log` (ou apenas uma rápida olhada, de preferência com `git log --oneline`), veremos nossos três commits em cima do que estiver na sua _master_.

Para mim, aparece assim:
```ShellSession
> git log --oneline
0b22064 (HEAD -> interactiveRebase) Adicionar mais texto a JSM
062ef13 Adicionar Lord.txt
9e06fca Adicionar texto em JSM
df3ad1d (origin/master, origin/HEAD, master) Adicionar JSM
800a947 Adicionar texto do tutorial
```

Há duas coisas que podemos melhorar sobre esse log e para aprendermos mais sobre o git, usaremos uma abordagem diferente do que a que vimos no topico anterior sobre `amend` com os seguintes objetivos:

* Colocar as duas alterações de `JSM.txt` em um único commit
* Nomeie as coisas de forma consistente e remova o _.txt_ da mensagem sobre `Lord.txt`

Para alterar os três novos commits, queremos fazer um rebase no commit antes deles. Esse commit para mim é `df3ad1d`, mas também podemos referenciá-lo como o terceiro commit do _HEAD_ atual como `HEAD~3`

Para iniciar um `rebase` _interativo_, usamos `git rebase -i {COMMIT}`, então vamos executar `git rebase -i HEAD~3`

O que você verá é o editor de sua escolha mostrando algo como isto:

```bash
pick 9e06fca Adicionar texto em JSM
pick 062ef13 Adicionar Lord.txt
pick 0b22064 Adicionar mais texto a JSM

# Rebase df3ad1d..0b22064 onto df3ad1d (3 commands)
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
# d, drop = remove commit
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

Observe que o `git` sempre explica tudo o que você pode fazer quando você chama o comando.

Observe que os commits são listados do mais antigo para o mais novo e, portanto, na direção oposta à saída do `git log`.

Vou começar com a alteração fácil e fazer com que possamos alterar a mensagem do commit do meio.

```bash
pick 9e06fca Adicionar texto em JSM
reword 062ef13 Adicionar Lord.txt
pick 0b22064 Adicionar mais texto a JSM

# Rebase df3ad1d..0b22064 onto df3ad1d (3 commands)
[...]
```

Agora para colocar as duas alterações do `JSM.txt` em um único commit.

Obviamente, o que queremos fazer é `squash` (compactar) o último dos dois commits no primeiro, então vamos colocar esse comando no lugar do `pick` no segundo commit, alterando `JSM.txt`. Para mim, como no exemplo, o commit de hash _0b22064_.

```bash
pick 9e06fca Adicionar texto em JSM
reword 062ef13 Adicionar Lord.txt
squash 0b22064 Adicionar mais texto a JSM

# Rebase df3ad1d..0b22064 onto df3ad1d (3 commands)
[...]
```

Isso fará o que queremos?

Não vai, né? Como os comentários no terminal nos dizem:

```bash
# s, squash = use commit, but meld into previous commit
```

Portanto, o que fizemos até agora mesclará (merge) as alterações do segundo commit de alterações no JSM, com o commit de alterações no Lord. Não é isso que queremos.

Outra coisa muito útil que podemos fazer em um `rebase` _interativo_ é mudar a ordem dos commits.

Se lermos com atenção o que os comentários no terminal disseram, podemos concluir que basta mover as linhas para atingir o nosso objetivo.Portanto vamos mover o segundo commit JSM para ficar logo após o primeiro como no exemplo abaixo:

```bash
pick 9e06fca Adicionar texto em JSM
squash 0b22064 Adicionar mais texto a JSM
reword 062ef13 Adicionar Lord.txt

# Rebase df3ad1d..0b22064 onto df3ad1d (3 commands)
[...]
```

Então basta fechar o editor e o `git` começará executar os comandos.

O que acontece a seguir é como uma `rebase` normal: começando com o commit que você referenciou no início, cada um dos commits que você listou será aplicado um após o outro.

> Lembrando que, apesar de não fazer parte do nosso cenário atual, quando você reordenar as alterações do código, poderá comumente ocorrer conflitos durante o `rebase`. Afinal, você possivelmente misturou as mudanças que estavam desenvolvendo.
>
> Apenas [resolva](#resolvendo-conflitos) como faria normalmente.

Após aplicar o primeiro commit, o editor abrirá e permitirá que você coloque uma nova mensagem para o commit combinando as alterações em `JSM.txt`. Vamos descartar a mensagem inicial dos commits e colocar "Adicionar vários textos importantes em JSM".

Depois de fechar o editor para concluir o commit, ele será aberto novamente para permitir que você altere a mensagem do commit `Adicionar Lord.txt`. Vamos remover o ".txt" e fechar novamente o editor.

Reescrevemos a linha do tempo novamente. Desta vez, com muito mais dominio da situação do que quando utilizamos apenas o `amend`.

Se você olhar o `git log` novamente verá que há dois novos commits no lugar dos três que tínhamos anteriormente. 

```
> git log --oneline
105177b (HEAD -> interactiveRebase) Adicionar Lord
ed78fa1 Adicionar vários textos importantes em JSM
df3ad1d (origin/master, origin/HEAD, master) Adicionar JSM
800a947 Adicionar texto do tutorial
```

<!-- changing meta data?>

<!-- force pushing -->
### Linha do tempo pública, porque não devemos reescrevê-la e como fazer isso com segurança

Como observado anteriormente, a alteração da linha do tempo é uma parte incrivelmente útil de qualquer fluxo de trabalho que envolve fazer muitos pequenos commit enquanto você desenvolve.

Embora todas as pequenas alterações se tornem muito fácil para você, por exemplo, verificar se as validações do sonar estão satisfatórias, e se não estiverem, remover ou mesclar apenas alterações específicas, os 100 commits que você fez para escrever o seu `HelloWorld` provavelmente não são algo que você deseja compartilhar com as pessoas.

A melhor maneira de compartilhar com eles são algumas alterações bem-formadas, com boas mensagens de commit, informando aos colegas o que você fez por qual motivo.

Enquanto todos esses pequenos commits existirem apenas no seu _Working Environment_, você estará perfeitamente seguro para fazer um `git rebase -i` e alterar a linha do tempo como desejar (_lembrando de mantê-la limpa, linear e objetiva_).

As coisas ficam problemáticas quando se trata de mudar a _linha do tempo pública_. Isso significa qualquer coisa que já tenha chegado ao _Remote Repository_.

Nesse ponto, tornou-se público e as branches de outras pessoas podem se basear nessa linha do tempo. Isso realmente faz com que você geralmente não queira se envolver.

O conselho usual é "Nunca reescrever a linha do tempo pública" apsesar disso devemos admitir que existem casos em que você ainda pode reescrever a _linha do tempo pública_.

Em todos esses casos, a linha do tempo não é "realmente" pública. Você certamente não deseja reescrever o histórico na branch _master_ de um projeto de código aberto, ou algo como a branch _release_ da sua empresa.

Onde você pode querer reescrever a linha do tempo são branches que você empurrou (`push`) apenas para compartilhar com alguns colegas.

Se você fizer um `git push`, você será notificado de que não está autorizado a fazer isso, pois sua branch _local_ divergiu da _remote_.

Você precisará forçar (`force`) o envio das alterações e substituir o remoto pela sua versão local.

Por mais que sejamos tentados a usar o `git push --force` no momento. O ideal é que façamos as coisas com mais cuidado ao reescrever a _linha do tempo pública_, para que isso seja feito com segurança.

Para isso o git oferece um recurso semelhante ao `--force` porém mais cuidadoso, o `--force-with-lease`.

O `--force-with-lease` irá verificar se a sua versão _local_ da branch _remote_ e o atual _remote_ correspondem, antes de fazer o `push`.

Com isso, podemos garantir que não apagaremos acidentalmente nenhuma alteração que alguém possa ter dado `push` enquanto estavamos trabalhando na _linha do tempo pública_.

<!-- force_push.png -->
![O que acontece com push --force-with-lease](https://user-images.githubusercontent.com/40603968/157717845-7c82d956-7c0d-4bb0-afa1-8ceda5dd9933.png)

Portanto vamos alterar o mantra para:

_Não reescreva o histórico público, a menos que tenha certeza do que está fazendo. E se você o fizer, que seja da maneira segura utilizando o force-with-lease._

## Lendo a linha do tempo

Conhecendo as diferenças entre as áreas em nosso _Working Environment_ - especialmente o _Local Repository_ - e como os commits e a linha do tempo funcionam, fazer um `rebase` não deve ser assustador.

Mesmo assim, as vezes as coisas dão errado. Você pode ter feito um `rebase` e acidentalmente aceitado a versão errada do arquivo ao resolver um conflito.

Felizmente o `git`, brihante como sempre, possui um recurso de segurança interno chamado logs de referência (_Reference Logs_), também conhecido como `reflog`.

Sempre que qualquer _referência_ como a ponta de uma branch é atualizada no seu _Local Repository_, uma entrada no _Log de Referência_ é adicionada.

Portanto, há um registro de qualquer momento em que você faz um `commit`, mas também de quando você redefiniu (`reset`) ou moveu o `HEAD` e etc.

Você vê como isso pode ser útil quando, por exemplo, estragamos um `rebase`?

Sabemos que um `rebase` move o `HEAD` da nossa branch até o ponto em que o baseamos e aplica nossas alterações. Um `rebase` interativo funciona da mesma forma, mas podemos manipular melhores esses commits utilizando sinais como _squashing_ ou _rewording_.

Vamos mudar novamente para a branch que criamos para entender o [rebase interativo](#rebase-interativo).E, em seguida dar uma olhada no `reflog` das coisas que fizemos nessa branch executando o `git reflog`.

Veremos muitas informações no retorno, mas as primeiras linhas na parte superior devem ser semelhantes a esta:

```bash
> git reflog
105177b (HEAD -> interactiveRebase) HEAD@{0}: rebase -i (finish): returning to refs/heads/interactiveRebase
105177b (HEAD -> interactiveRebase) HEAD@{1}: rebase -i (reword): Adicionar Lord
ed78fa1 HEAD@{2}: rebase -i (squash): Adicionar vários textos importantes em JSM
9e06fca HEAD@{3}: rebase -i (start): checkout HEAD~3
0b22064 HEAD@{4}: commit: Adicionar mais texto a JSM
062ef13 HEAD@{5}: commit: Adicionar Lord.txt
9e06fca HEAD@{6}: commit: Adicionar texto em JSM
df3ad1d (origin/master, origin/HEAD, master) HEAD@{7}: checkout: moving from master to interactiveRebase
```

Tudo o que fizemos estará listado, desde a mudança para a branch até o `rebase`.

É muito legal ver as coisas que fizemos, mas tudo isso se torna inútil se erramos em algum ponto.

Se compararmos a saída do `reflog` com a última vez que examinamos o `log`, verá esses pontos relacionados às referências de commit, e dessa maneira podemos usá-las.

Digamos que nos arrependemos de fazer o rebase. Como poderiamos nos livrar das alterações feitas anteriormente?

Nós movemos o `HEAD` para o ponto anterior ao `rebase` iniciado com um `git reset 0b22064`.

> `0b22064` é o commit antes de `rebase` no meu caso. De um modo mais geral, você também pode fazer referência a ele como _HEAD_ de quatro mudanças atrás_ via `HEAD@{4}`.

Se dermos uma olhada no `log` agora, veremos o estado original com os três commits individuais restaurados.

Mas digamos que agora percebemos que não era isso que queríamos. O `rebase` está bom, nós simplesmente não gostamos de como mudamos a mensagem do commit de Lord.

Nós poderíamos simplesmente fazer outro `rebase -i` no estado atual, exatamente como fizemos originalmente.

Ou usamos o reflog e voltamos para depois do rebase e alteramos o commit a partir daí com `amend`.

## O Contexto JSM
Dentro do contexto da JSM existem conteúdos muito importantes para sua jornada de trabalho. O intuito do treinamento learning-git também é deixar a absorção e entendimento desses conteúdos mais fácil e divertida para todos. Você pode e deve consultar esses materiais listados a seguir sempre que necessário:

- [Gitflow: Backend](https://github.com/juntossomosmais/gitflow/tree/main/gitflow/backend)
- [Playbook: Jedi Academy](https://github.com/juntossomosmais/jedi-academy)

## Evolução continua
É extremamente importante ressaltar que a JSM tem como uma de suas maiores caracteristicas a evolução continua, esse treinamento não é diferente e é totalmente aberto para que todos possam participar, sugerir alterações, enfim, enriquecer o conteúdo de uma maneira geral, suas contribuições sempre serão bem vindas e para te auxiliar nesse caminho existe um artigo do github ensinando sobre [Sintaxe básica de escrita e formatação no GitHub](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).

Obrigado a todos!

Juntos somos mais.

---

> Esse treinamento é baseado no conteúdo [Learn git concepts, not commands](https://dev.to/unseenwizzard/learn-git-concepts-not-commands-4gjc), de [Nicola Riedmann](https://www.linkedin.com/in/nicola-michel-henry-riedmann/).

> Menção honrosa ao material mais rico relacionado a git atualmente na minha opinião. [Pro Git Book (traduzido para PT-BR)](http://git-scm.com/book/pt-br) e a [página de referência](https://git-scm.com/docs).

> É extremamente recomendado que também entendamos bem a respeito das _pull requests_ que são utilizadas rotineiramente no nosso workflow, você pode encontrar uma explicação sobre esse tema no artigo [Sobre pull requests](https://docs.github.com/pt/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests) do github.

>Agora depois de entender mais sobre o funcionamento do git de uma maneira geral e seus comandos mais úteis você deve conseguir absorver com clareza o nosso conteudo que explica sobre o [Gitflow para desenvolvedores backend](https://github.com/juntossomosmais/gitflow/blob/main/gitflow/backend/README.md)

> Há um movimento atual para a branch principal deixar de ser chamada como _master_ e passar a ser _trunk_ ou _main_. Linux, Github e outras companhias estão adotando a nova nomenclatura. É uma ótima proposta e totalmente alinhada ao movimento `#BlackLivesMatter`. Você pode entender mais lendo o artigo [The bigger picture behind the GitHub master branch name change](https://dev.to/sylviapap/the-bigger-picture-behind-the-github-master-branch-name-change-35h8).

---
