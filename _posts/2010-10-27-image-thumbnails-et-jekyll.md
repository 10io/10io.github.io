---
layout: post
title: Jekyll et les images
tags: ruby
---

Après quelques semaines avec [Jekyll][1], le bilan est positif: c'est
un générateur statique awesome! De plus, couplé avec [git][2], les
billets de ce blog sont postés en faisant un bête _git push_ et boum,
le billet est sauvé, le site est régénéré et instantanément
disponible. Je me répète mais c'est awesome!

## Mais Jekyll aime pas les images

Oui, il n'y a pas que des avantages à utiliser Jekyll. Exemple
d'aujourd hui, les images ne sont pas gérées. Enfin plutôt, si vous
voulez des images, il faut coder soit même les balises &lt;a&gt; et
&lt;img&gt;.

Beurk! Que cela ne tienne, Jekyll peut être étendu par des [_tags_ custom][3].
 C'est ce que j'ai fait avec mes petits doigts de fée. Avec ce tag, on
 pourra écrire dans un billet ce genre de commande:

<pre>
{&#37; image foobar.png &#37;}
</pre>

Ce qui générera tout seul ce bout d'html:

```html
<a class='image' href='/images/foobar.png'>
    <img src='/images/foobar_t.png' />
</a>
```

Plutôt cool, non? Cerise sur le gâteau, le tag s'appuie sur [RMagick][4]
pour générer un _thumbnail_ de l'image. En résumé, il suffit
d'utiliser le tag pour les images, générer le site et admirer le
résultat :)

## Installation

C'est plutôt simple. Il suffit de récupérer le code du tag: [image.rb][5]
et de le mettre dans un dossier _\_plugins_ dans son installation de
Jekyll. C'est tout.

Avant de son utilisation, ne pas oublier d'installer [RMagick][4]:
{% highlight bash %}
# sudo gem update
# sudo gem install rmagick
{% endhighlight %}

Puis vient un peu de config. Le tag définit deux variables de
configuration dont voici les valeurs par défaut:
<pre>
images_directory: images
images_css_class: image
</pre>

La première variable décrit le dossier où sont stockées les images.
On peut très bien décrire un chemin plus complexe, par exemple
images/post/my/awesome/directory. Et pour les attentifs: oui,le tag exige
que toutes les images utilisées soit dans le même répertoire.

La deuxième variable est simplement la classe CSS appliquée aux liens
&lt;a&gt;. La classe CSS permet de pouvoir récupérer plus facilement
les liens sur les images. utilisables par les effets Javascript
[lightbox][6] et compagnie.

A noter que le nom de l'image thumbnail est fixe: il s'agit
du nom de l'image originale avec \_t comme suffixe.

## Démo

Et voila ce que ça donne. En insérant ce code dans ce billet:

<pre>
{&#37; image pocket_ocr_lorem.png &#37;}
</pre>

j'obtiens ceci (couplé avec [fancybox][7] pour l'effet de zoom):

![ocr](/assets/images/posts/pocket_ocr_lorem.png)



[1]:http://jekyllrb.com
[2]:http://git-scm.com
[3]:http://github.com/mojombo/jekyll/wiki/Plugins
[4]:http://rmagick.rubyforge.org
[5]:http://github.com/10io/jekyll/blob/master/lib/jekyll/tags/image.rb
[6]:http://www.lokeshdhakar.com/projects/lightbox2
[7]:http://fancybox.net
