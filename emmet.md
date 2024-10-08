# EMMET
## INTRODUCTION
Plugin pour éditeur de texte (assistance de contenu)
## HTML
| Raccourci | Résultat |
| :---------: | :---------: |
|html:5|Template HTML|
|!|Template HTML|
|ul>li*2>span.class{li}|<ul><li><span class="class">li class="class"</span></li><li><span class="class">li class="class"</span></li></ul>|
|header+article|<header></header><article></article>|
|#id|<div id="id"></div>|
|.class|<div id="class"></div>|
|nav#navbar|<nav id="navbar"></nav>|
|||
## CSS
| Raccourci | Résultat |
| :---------: | :---------: |
|dn|display:none;|
|m10|margin:10px;|
|p10|padding:10px;|
|v|visibility:hidden;|
|-bdrs|border-radius|





nav#navbar                          <nav id="navbar"></nav>
article.post                        <article class="post"</article>
div.class1.class2.class3            <div class="class1 class2 class3">
img[src="image.png"]                <img src="image.png" alt="">
span[data-hint]                     <span data-hint=""></span>
section>h3+h4*3                     Créer une balise section contenant un titre h3 et 3 titres h4
lorem50                             Lorem text avec 50 mots
lorem*5                             Lorem sur 5 lignes
(p>lorem40)*2                       2x <p> avec 40 mots de lorem
