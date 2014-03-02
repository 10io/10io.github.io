---
layout: post
title: Hello world!
tags: 10io
category: 10io
---

Après quelques mois d'absence, voici le retour du blog 10io. Cette
fois-ci, on va essayer le garder up un peu plus longtemps :) Les
sujets sont toujours les mêmes: développement web, quelques geekeries
et du fun.

Qui dit nouveau blog, dit nouveaux essais comme moteur de blog. Après
[Lighttpd][1], [Ruby][2], [Rails][3] et [Simplelog][4], je voulais
essayer autre chose. Il me fallait quelque chose de simple, quelque
chose avec laquelle je puisse poster, personnaliser et déployer
facilement. Je savais que je voulais rester avec [Ruby][2], mais les
systèmes de blog existant me faisait hésiter: soit ils m'ont pas
convaincu([Typo][6]), soit ils étaient mort ([Simplelog][4] ou [Mephisto][5]
anyone?). J'ai même hésiter à faire mon propre système de blog
(*Make your own bread and eat it!*). Bref, c'était un peu la jungle.

## Hello [Jekyll][7]

Puis je suis retombé sur le moteur de [Jekyll][7], qui se définit
comme suit:
> Jekyll is a simple, blog aware, static site generator.

En gros, Jekyll prend le contenu d'un site avec quelques fichiers de
*layout*, les fait passer à travers [Markdown][8], [Textile][9] et
[Liquid][10] et crache un site *statique* tout beau, tout frais. La
simplicité du processus est limite choquante. Tellement choquante que
je me suis dit en premier *On est oú là... un site statique...on
retourne au web 0.5alpha*.

Et puis, en réflechissant et en fouillant le web pour trouver
d'[autres][11] [utilisateurs][12] [de][13] [Jekyll][14], c'était évident
 que l'approche avait des avantages non négligeables:
* Site statique: n'importe quel serveur web fera l'affaire pour le
déploiement.
* Pas de base de données, pas besoin de faire des backups: les billets
sont à la fois sur le serveur et sur ma machine perso qui me fait la
génération du site.
* Tous les billets sont des bêtes fichiers markdown(voici un
[exemple][15]): on peut même les imprimer tels quels.
* Tous les liens dynamiques(ex. les 5 derniers billets) sont générés
*offline*.
* Les fichiers layout basés sur [Liquid][10] sont très simples à faire.
* Il permet la coloration syntaxique grâce à [Pygments][19]

Mais il y avait aussi des inconvénients:
* Site statique: au revoir les parties dynamiques d'un blog dont les
outils de création de billet(pas de formulaire pour créer un billet)
sur le site et....
* Au revoir les commentaires.
* A chaque nouveau billet, il faut regénérer tout le site de manière
*offline*, l'envoyer au serveur et éventuellement dire au serveur web
que les fichiers ont changé.

A ces inconvenients, les utilisateurs ont trouvés des parades
intéressantes. Etant dans le développement web, j'ai toujours à portée
ma machine personnelle qui s'occupe de la génération du site. De plus,
tout le code du site, le *layout* et le contenu sont sauvés sur un
serveur [git][16]. Ce qui permet aussi de contrer le troisième
inconvénient. [Git][16] propose des *hooks* qui permette de faire des
actions sur le serveur dès qu'il y a un *commit*.

Le problème des commentaires me semblait le plus gros. Là encore, il
y avait une solution intéressante: il suffit de les déléguer à des
services tiers comme [Disqus][17]. L'avantage de ce genre de service
c'est de s'intégrer très facilement (du javascript et un peu de css), il
propose aux visiteurs de s'identifier avec différents systèmes(Twitter,
Facebook, OpenID et d'autres) et enfin, plus besoin de s'occuper du
spam. J'étais pas très motivé par héberger les commentaires sur un
site tiers. Mais pour finir, je me suis dit *Hell, why not?*.

Alors bien sûr, ce genre de système de blog n'est pas fait pour
monsieur tout le monde: il est destiné à ceux qui doivent mettre les
mains dans le moteur pour être content. Ce qui est bien pour ma part.
Enfin, j'allais pouvoir mettre à jour un blog comme je code: je
*checkout* les sources, je crée mon billet et je *commit* les
modifications. Finit. Plus rien à faire. Et comme je m'intéresse à
[git][16], ça tombait bien j'allais pouvoir faire d'une pierre deux
coups.

Il restait plus qu'un dernier détail à régler: je suis un *newb* en
design web. Il fallait donc que je me base sur un thème existant pour
un blog. Après quelques recherches je suis tombé sur le design de
[Hemingway][18]. Super simpliste(très peu de couleurs utilisées), il
propose une organisation des sections sur la page originale qui était
sympa. De plus, il y avait une version obscure (blanc sur noir). Il
en fallait pas plus, et hop je l'ai embarqué.

Après l'intégration de [Hemingway][18] et des modifications sur
[Jekyll][20], vous avez sous les yeux le résultat qui tourne sous
[nginx][21]. Pas mal, non? :)

[1]:http://www.lighttpd.net
[2]:http://www.ruby-lang.org
[3]:http://www.rubyonrails.org
[4]:http://www.simplelog.net
[5]:http://github.com/technoweenie/mephisto
[6]:http://github.com/fdv/typo/wiki
[7]:http://jekyllrb.com
[8]:http://daringfireball.net/projects/markdown
[9]:http://www.textism.com/tools/textile
[10]:http://github.com/tobi/liquid
[11]:http://metajack.im
[12]:http://henrik.nyh.se
[13]:http://www.cyborginstitute.com
[14]:http://paradigmatic.streum.org
[15]:http://github.com/mojombo/mojombo.github.com/raw/master/_posts/2010-08-23-readme-driven-development.md
[16]:http://git-scm.com
[17]:http://www.disqus.com
[18]:http://warpspire.com/hemingway
[19]:http://pygments.org
[20]:http://github.com/10io/jekyll
[21]:http://nginx.org
