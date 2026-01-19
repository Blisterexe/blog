+++
title = "Pourquoi est-ce que GNOME devrait prendre en charge les SSD?"
date = "2022-01-25"
lastmod = "2025-12-26"
author = "Blisterexe"
goimportpackage = true
cover = "cover.png"
+++

*This article is also [available in english](/posts/gnome-ssd).*

Cet article contient pas mal de termes techniques que j'expliquerai dans les paragraphes suivants. Ceux qui connaissent déjà ces termes peuvent passer à la section suivante. Une connaissance de base de Linux et de ses environnements de bureau est requise pour bien comprendre l'article.

Les décorations côté serveur (SSD) désignent le cas où la barre de titre de l'application est dessinée par le système, tandis que les décorations côté client (CSD) désignent le cas où l'application dessine sa propre barre de titre. Le bureau KDE privilégie la première option, tandis que GNOME opte pour la seconde. Par contre, KDE et la plupart des autres environnements de bureau prennent en charge les deux options, tandis que GNOME ne prend en charge que les CSD.

Étant donné que les SSD sont dessinés par le bureau plutôt que par l'application, les applications qui les utilisent ont une barre de titre distincte (dessous), tandis que les applications conçues autour du CSD ont généralement une barre de titre intégrée (dessus).

{{< figure src="csd-vs-ssd.png" alt="barre de titre distincte (dessous) vs barre de titre intégrée (dessus)" position="center" caption="barre de titre distincte (dessous) vs barre de titre intégrée (dessus)" captionPosition="center" >}}

Je ferai reference a GNOME, aux « système » et au applications GNOME au long du texte, et je serai réellement en train de parler du compositeur mutter, des compositeurs en général et des application gtk4/libadwaita, respectivement. Le choix d'utiliser des termes plus simple a été fait par souci de simplicité.

Sans plus attendre, passons à la question du titre.

## *Pourquoi* est-ce que GNOME devrait prendre en charge les SSD?

Eh bien, il y a plusieurs raisons :

#### Plusieurs application gèrent mal le manque de SSD.

Certaines applications fonctionnent mal sans SSD. Ces applications n'ont soit aucune barre de titre ou en ont une qui est bizarre. Le resultat est que l'utilisateur n'arrive pas à les déplacer ou les redimensionner facilement. Davinci Resolve en est un exemple notable, mais ce n'est pas le seul.

GNOME occupe une place suffisamment importante dans le monde de Linux pour que la plupart des applications conçues pour les décorations SSD mettent en œuvre un moyen de contournement pour qu'ils puissent bien fonctionner sur GNOME. Généralement, cette solution de contournement est [libdecor](https://github.com/neonkore/libdecor).

Probleme reglé, donc ? Non ! Libdecor est le pire des deux mondes, car elle à l'inefficacité d'espace et le manque d'intégration d'une barre de titre traditionnelle, avec toutes les incohérences et le manque de personnalisation d'une barre de titre intégrée.

{{< figure src="libdecor-inconsistency.png" alt="Trois barres de titre libdecor visuellement distinctes" position="center" caption="Trois barres de titre libdecor visuellement distinctes" captionPosition="center" >}}

Ce n'est pas la faute des développeurs de libdecor, c'est simplement parce qu'il s'agit d'une solution provisoire à un problème plus large.

La prise en charge des SSD résoudrait ce problème, car toutes les applications qui ne souhaitent pas utiliser CSD auraient une belle barre de titre native avec un ombrage, des couleurs et un arrondissement des coins cohérents, et ça [faciliterait la vie des développeurs](https://factorio.com/blog/post/fff-408).

#### La prise en charge des SSD rendrait les applications « plus natives ».

Sous Windows et macOS, n'importe quelle application peut implémenter les CSD et respecter l'espacement, la position et l'apparence des boutons de la barre de titre fournis par les équivalents SSD de ces plateformes. 

Par contre, on ne peux pas faire la même chose sur linux, parce que la barre de titre peut varier tellement d'un environnement à l'autre qu'il est pratiquement impossible d'obtenir un CSD qui semble natif à l'environment. Offrir l'option SSD pour les applications non-GNOME sur GNOME et vice versa contribuerait grandement à donner une apparence plus native aux environments des utilisateurs qui apprécient ce genre de choses.

#### De nombreux utilisateurs d'autres ordinateurs de bureau souhaitent que leur SSD soit respecté.

De nombreux utilisateurs d'autres bureaux sont frustrés par le fait que les applications GNOME ne prennent pas en charge les SSD et cessent de fonctionner si ceux-ci sont activés de force.

Bien qu'il ne soit évidemment pas de la faute du développeur si une application cesse de fonctionner lorsque les utilisateurs font des choses qui ne sont pas prisent en charge, beaucoup d'utilisateurs seraient ravis si les applications GNOME prennent en charge optionnellement les SSD. 

Si vous vous demandez pourquoi un utilisateur souhaiterait que le SSD soit appliqué à une application GNOME, sachez que certaines personnes accordent plus d'importance à la cohérence des barres de titre qu'au design de chaque application individuelle. [Certains utilisateurs](https://discuss.kde.org/t/i-believe-that-server-side-decorations-are-an-accessibility-feature-which-we-are-on-the-verge-of-losing/9529) considèrent également les SSD comme une fonctionnalité d'accessibilité.

#### Le fait que GNOME ne prenne pas en charge les SSD nuit au bureau linux.

Linux est déjà un marché restreint, et le fait que GNOME ne prenne pas en charge la norme de facto qu'est `xdg-decoration` ne fait que nuire au bureau Linux, parce que ça augmente la fragmentation.

Cette fragmentation rend le bureau Linux moins attrayant pour les développeurs et, surtout, rend les développeurs d'applications **moins enclins à adopter des normes plus modernes telles que Wayland**.

## Les arguments contre la prise en charge des SSD

De nombreuses discussions sur les raisons pour lesquelles GNOME devrait implémenter les SSD s'arrêtent aux arguments en faveur de cette implémentation, mais il y a évidemment aussi des arguments contre la prise en charge des SSD, sinon GNOME les aurait déjà implémentés.

Des arguments tels que :

#### « Le SSD est pas conforme aux spécifications »

Ceci est l'argument principal utilisé par les développeurs GNOME pour justifier leur exclusion des SSD. Malgré le fait que ça soit vrai que `xdg-decoration` est un protocole « instable », et que Wayland a été initialement conçu en tenant compte uniquement du CSD. Cependant, Wayland a aussi été initialement conçu sans partage d'écran ni raccourcis clavier globaux, des normes que GNOME a depuis adoptées.

La norme ne pose pas non plus les mêmes problèmes de sécurité ou de conception que des normes telles que celle relative à la barre d'état système.

Le fait est que cette norme est adopté par [tous les compositeurs de bureau prêts à la production](https://wayland.app/protocols/xdg-decoration-unstable-v1), excepté le mutter de GNOME, et qu'il est utilisé par les développeurs d'applications et les développeurs de bureaux, ce qui en fait une norme de facto qui est largement adoptée.

Autrement dit, bien que `xdg-decoration` soit techniquement hors spécifications, la seule chose qui le distingue des protocoles Wayland « conformes aux spécifications » est l'absence de prise en charge par GNOME.

#### « Les décorations de fenêtre font partie de l'application et ne devraient donc pas relever du domaine du bureau. »

De nombreux développeurs et utilisateurs considèrent la barre de titre comme quelque chose qui fait partie du bureau, tandis que d'autres ne sont pas pensent que ça fait partie de l'application. Ce n'est pas un problème, juste une différence de philosophie.

Le vrai problème, c'est l'idée que le projet GNOME ne devrait pas répondre aux besoins du premier groupe. Ce serait comme si GNOME ne prenait pas en charge `xdg-file-chooser` et affirmait que chaque application *devrait* fournir son propre sélecteur de fichiers. Mais GNOME le prend en charge, et seules les applications qui souhaitent implémenter leur propre sélecteur de fichiers le font.

Étant donné que les deux approches sont utilisées et appréciées, les avantages et inconvénients divers de chacune d'elles sont sans importance, tout comme les autres arguments relatifs à la conception. C'est la raison que je n'en ai pas parlé.

## Solution

J'espère avoir réussi à démontrer pourquoi il serait intéressant pour GNOME d'adopter les décorations côté serveur, mais il reste une question.

#### À quoi cela ressemblerait-il ?

J'ai évoqué à plusieurs reprises dans cet article à quoi ressemblerait la mise en œuvre des SSD, sans l'expliquer en détail. C'est ce que je vais maintenant faire.

GNOME mettrait évidement en place les protocoles pertinents et activerait les décorations côté serveur sur toutes les applications qui ne demandent pas explicitement le SSD. Ceci serait fait afin d'appliquer le SSD aux applications qui ne fournissent que le CSD comme solution de secours pour les compositeurs qui ne prennent pas en charge le SSD.

Pour être clair, cela ne s'appliquerait pas aux applications conçues pour une barre d'en-tête unifiée, comme les applications GNOME.

Sur d'autres bureaux, cependant, les applications GNOME demanderaient encore que les SSD ne soient pas appliqué, une préférence respectée par tous les bureaux Linux populaires. La seule différence, c'est qu'ils permettraient aux utilisateurs de forcer l'affichage d'une barre de titre sur les applications GNOME. Dans ce scénario, l'application GNOME supprimerait le titre et les commandes intégrés à la fenêtre.

Ce serait le meilleur des deux mondes, car les utilisateurs de GNOME bénéficieraient de barres de titre et d'ombres de fenêtres plus cohérentes sur les applications de projets tiers, et les **utilisateurs** d'autres bureaux auraient la possibilité d'utiliser SSD sur les applications GNOME s'ils le souhaitent.

À mon avis, le fait que GNOME ne prenne pas en charge des décorations côté serveur est actuellement le plus gros problème de l'environnement de bureau GNOME. J'espère donc sincèrement que cet article aidera a mettre en place une bonne solution.

Si vous voulez m'aider avec ça, vous pouvez **partager cet article**.
