# Responsive web design

## Introduction

Nos modes d’accès à Internet on récemment beaucoup évolués. Nous accédons à nos sites et à nos applications préférées avec une variétés de plus en plus importante de terminaux aux capacités fort différentes: téléphones, tablettes, portables, desktops, télévisions, etc.

Les sites et les applications doivent donc offrir des expériences utilisateurs adaptées à ces différents terminaux. Le web design se sépare donc petit à petit de ses racines dans le monde de l’impression. Les notions de page, de canevas fixe et stable, d’expérience utilisateur constante, disparaissent petit à petit au profit d’une conception mouvante, adaptable et flexible du web.

Dans un article fondateur publié sur A List Apart, [Ethan Marcotte](unstoppablerobotninja.com/entry/on-being-responsive/) nous propose une approche faisant droit à cette dimension d’expérience utilisateur adaptable qu’il appelle [responsive web design](www.alistapart.com/articles/responsive-web-design/). Celle-ci se base sur trois composants:

- Layout fluides et grilles flexibles
- Media flexibles
- Media Queries

Attention, si l’utilisation des techniques décrites ici permet une plus grande flexibilité, elle ne vous dispense cependant pas de créer, dans certains de cas, une version du site ou de l’application dédiée au mobile.

En effet, ces changement n’interviennent qu’au niveau CSS. Pour des raisons de bande passante ou autre, le HTML et le contenu lui même doivent parfois être modifiés dans le cadre d’une version mobile.

Personnellement, je dirais que les techniques de Responsive Web Design fonctionnent bien pour les sites dont la mission première est la transmission de contenu. Pour les sites plus proches d’applications en ligne, d’autres solutions peuvent sans doute être envisagées.

Pour la presse par exemple, l’usage de ce genre de technologies permet de ne maintenir qu’une seule base de code et donc de réaliser des économies d’échelle. Les applications natives peuvent alors se concentrer sur la création de valeur ajoutée.

Cette approche, couplée à une approche [mobile first](www.abookapart.com/products/mobile-first) / [structured content first](www.slideshare.net/stephenhay/structured-content-first) permet également de réfléchir sur votre structure de données, de préserver ce qui est nécessaire et de se débarrasser du reste. Réfléchir d’abord à la version mobile de votre site vous permet également d’établir des priorités parmi les divers éléments composant votre site.

## Media Queries

Les Média Queries étendent les fonctionnalités des types de média. Elles permettent de servir des feuilles de styles ou certaines déclarations au sein de feuille de style en fonction de caractéristiques de la plateforme à l’aide de laquelle sont affichées les pages.

Ces Media Queries permettent de tester les caractéristiques suivantes: `aspect-ratio`, `device-height`, `monochrome`, `color`, `device-width`, `max-width`, `orientation`, `resolution`, `width`, `device-aspect-ratio`, `height`, `max-height`.

Elles sont utilisables avec des feuilles de styles liées

```
<link rel="stylesheet" media="screen and (max-width:970px)" href="css/medium.css" />
```

ou au sein de feuilles de styles existantes

```
@media screen and (max-width:970px)
{
	/*styles*/
}
```

Comme le mentionne Stéphanie Rieger sur Cloud Four [il est avantageux de spécifier vos media-queries en em](http://blog.cloudfour.com/the-ems-have-it-proportional-media-queries-ftw/), pour donner plus de flexibilité à vos layouts, ceux-ci vont en effet changer lorsque l'utilisateur change la taille de texte.

L’idée est d’utiliser les media queries pour créer permettre à l’expérience utilisateur d’être la meilleure possible quelle que soit la plateforme utilisée.

Un Polyfill Javascript existe si vous devez supporter des media queries simples dans IE: [respond.js de Scott Jehl](https://github.com/scottjehl/Respond). Si vous utilisez [selectivizr.js](https://github.com/keithclark/selectivizr) et respond.js, vérifiez que vous télécharger la dernière version de selectivizr.js sur github et que votre document charge d’abord selectivizr.js avant respond.js.

## Layouts fluides & grilles flexibles

Les layouts fluides existent depuis longtemps mais étaient jusqu’ici moins utilisés, en partie parce que les layouts fixes (spécifiés en pixels) sont techniquement plus faciles à réaliser, en partie aussi parce que le web était principalement accédé au départ de desktops. L’avènement d’autres terminaux d’accès à changé la donne.

Les grilles fluides sont soit basées sur des proportions harmonieuses telles que le nombre d’or, soit proportionnelles, soit créées à partir de grilles fluides en utilisant la formule suivante:

**target / context = result**

Voici un petit exemple pour illustrer le propos. Imaginons un layout de 960px de large, avec un zone de contenu principale de 520px et une zone de contenu secondaire de 400px. Traduisons cela en css flexibles en utilisant des pourcentages.

```
.page
{
	margin:0 auto;
	width:80%;
	min-width:760px;
	max-width:1140px;
}

.main
{
	float:left;
	width:54.1666667%; /*520/960*/
}

.secondary
{
	float:right;
	width:41.6666667%; /*400/960*/
}

.pagefooter
{
	clear:both;
}
```

Cette approche fonctionne sans problème avec l’ensemble des navigateurs modernes. Attention cependant au fait que les navigateurs ont [des façons différentes de traiter les décimales et le sub-pixel rounding](http://ejohn.org/blog/sub-pixel-problems-in-css/) avec des valeurs en pourcentages.

La même formule peut également être utilisée au niveau des tailles de polices. Si vous visez une taille de police de 24 pixels, avec une taille de corps de texte de 14 pixels par exemple:

```
html
{
	font:normal 87.5%/1.5 Helvetica, Arial, sans-serif;
}

h1
{
	font:normal 1.714285714em/1.2 Helvetica, Arial, sans-serif; /*24/14*/
}
```

Vous pouvez également travailler avec des [tailles de polices spécifiées en rem](http://snook.ca/archives/html_and_css/font-size-with-rem) qui sont toujours relative à la taille de texte sur l''élément html. Veillez simplement dans ce cas à spécifier une taille de police en px juste avant pour les navigateurs ne supportant pas rem.

```
html
{
	font:normal 87.5%/1.5 Helvetica, Arial, sans-serif;
}

h1
{
	font-size:24px;
	font-size:1.714285714rem; /*24/14*/
}
```

## Media Flexibles

Dans le cadre d’une approche fluide, les media tels que les images ou les vidéos dont les dimensions sont fixes, peuvent poser problème. Il est cependant possible, à l’aide de différentes techniques, de rendre ces media fluides.

Commençons par les images. On supprime d’abord toute référence aux dimensions de l’image dans le HTML.

```
<img src="img/monimage.png" alt="mon image" />
```

Une simple modification de la CSS suffit ensuite à ce que les images prennent tout l’espace disponible dans leur bloc conteneur. C’est donc la taille du bloc conteneur qui va définir la taille de l’image.

```
img
{
	max-width:100%;
}
```

Ceci fonctionne tant que vous images restent toujour plus grandes que leurs blocs conteneurs.

Idéalement, pour éviter de faire consommer de la bande passante inutilement, il faut servir des images de tailles différentes suivant la plateforme. Plusieurs solutions existent pour éviter de servir à un téléphone des images de grande taille, alors que l’affichage ne nécessite que de petites images. [Jeremy Keith](http://adactio.com/journal/4997/), [Smashing magazine](http://mobile.smashingmagazine.com/2013/07/08/choosing-a-responsive-image-solution/) et [Chris Coyier](http://css-tricks.com/which-responsive-images-solution-should-you-use/) ont réalisé de très bons résumés des différentes techniques utilisables aujourd’hui pour servir des images différentes en fonctions des caractéristiques de la plateforme. Lire également à ce sujet [l'excellent article de HTML5 Doctor](http://html5doctor.com/responsive-images-end-of-year-report/).

Il est également posible d’intégrer des vidéos à vos pages de façon fluide. Si vous travaillez en HTML5, la solution est assez simple et ressemble à celle adoptée pour les images.

```
video
{
	max-width:100%;
}
```

Des solutions telles que [FitVids.js](http://fitvidsjs.com/) peuvent également être envisagées.

Si vous devez intégrer des vidéos provenant de services tels que Youtube ou Vimeo, examinez la technique proposée dans [cet article sur A List Apart](http://www.alistapart.com/articles/creating-intrinsic-ratios-for-video/) ou encore [cet article sur css-tricks](http://css-tricks.com/NetMag/FluidWidthVideo/Article-FluidWidthVideo.php).

```
.video-container
{
	width:100%;
	padding-top:56.25%; /*16 by 9 ratio for the box*/
	position:relative; /*positioning context*/
}

.video-container iframe
{
	position:absolute;
	top:0;
	left:0;
	width:100%;
	height:100%;
	border:none;
}
```

## Problématiques importantes

### Performance

Dans une approche mobile first / responsive, l’optimisation et la performance est un paramètre encore plus important que lorsque seul le desktop était considéré, ce qui n’est plus possible aujourd’hui.

Commencez par vous concentrer sur des mesures rapides et efficaces:

#### Performance et videos

Vos images, vos vidéos, etc doivent être optimisés autant que possible.

Si vous utilisez un service vidéo comme Youtube ou Vimeo, cette optimisation est déjà faite pour vous. C’est l’un des grands avantages de cette solution.

Vous pouvez également appliquer directement des media queries à vos éléments `<source>`

```
<video controls="controls">
	<source src="large.mp4" type="video/mp4" media="all and (min-width:46.875em)" />
	<source src="small.mp4" type="video/mp4" />
</video>
```

#### Performance et images

Au niveau des images, assurez-vous tout d'abord d’utiliser des services tels que ImageOptim ou CodeKit pour optimiser vos images, ainsi que SpriteMe pour créer vos sprites.

##### Images de background

En ce qui concerne les images de background, vous pouvez utiliser des media queries pour servir une petite image par défaut et servir une plus grande image lorsque le layout l’exige.

Attention cependant, les deux images sont parfois téléchargées. Voir à ce sujet l'article très complet de Tim Kadlec ["Media Query & Asset Downloading Results"](http://timkadlec.com/2012/04/media-query-asset-downloading-results/).

##### Images de contenu

La situation est un peu plus complexe au niveau des images de contenus. [Une solution idéale pour les images responsives devrait relever les défis suivants](http://responsiveimages.org/):

1. Différentes images servies selon la densité d'écran
2. Différentes images servies selon la taille d'écran
3. Art direction (cadrage)

###### Compressive images

Une solution intermédiaire baptisée ["Compressive Images"](http://filamentgroup.com/lab/rwd_img_compression/) est intéressante. Elle consiste à optimiser une image extrèmement fortement mais de l'exporter à deux fois la taille d'affichage prévue. Cela donne des images très légères mais qui sont d'une bonne qualité même sur les écrans retina.

###### `<picture>` and picturefill

[Un nouvel élément <picture> a également été proposé](https://dvcs.w3.org/hg/html-proposals/raw-file/9443de7ff65f/responsive-images/responsive-images.html), qui permet de servir différentes sources en utilisant les media queries et l’élément srcset suivant la taille ou la résolution de l’écran de vos visiteurs. Filament Group (encore eux) proposent [un polyfill baptisé PictureFill](https://github.com/scottjehl/picturefill) pour émuler cet élément.

###### Server side solutions

Ces solutions mélangent front end et back end et sont [basées sur une proprosition de Luke Wroblewski](http://www.lukew.com/ff/entry.asp?1392).

A noter si vous voulez préserver votre markup actuel, [Adaptive Images](http://adaptive-images.com/), une solution dévelopée par Matt Wilcox.

###### Services

A noter également l'émergence de quelques services tels que [sencha.io](http://www.sencha.com/products/space/), [ReSRC.it](http://www.resrc.it/), [Capturing/Mobify.js 2.0](http://www.mobify.com/mobifyjs/v2/docs/capturing/)

##### Scripts

[HiSRC](http://hisrcjs.com/) est un plugin jQuery qui peut vous rendre service pour les images de contenu.

#### Perfomance et Scripts

Combiner et minifier vos scripts est la base. Ensuite des scripts de loading en parallèle tels que [require.js](http://requirejs.org/) ou [yepnope](http://yepnopejs.com/) (intégré dans [Modernizr](http://modernizr.com/)) permettent de loader vos scripst en parallèle.

#### Performance et CSS

Des outils commes [Sass](http://sass-lang.com/) permettent de combiner et de minifier les fichiers CSS. La compression gzip au niveau serveur aide à encore diminuer le poids des fichiers à télécharger.

#### HTML conditional loading

Vos fichiers de base peuvent rester très légers et du contenu peut être injecté via Javascript lorsque l’espace utile devient plus important. [Jeremy Keith vous explique les principes de base](http://24ways.org/2011/conditional-loading-for-responsive-designs/) et Filement Group vous donne un script à utiliser: [Ajax-include Pattern](http://filamentgroup.com/lab/ajax_includes_modular_content/).

### Content

Dnas des projets responsive, il est important de connaître autant que possible les contenus à présenter sur les différentes pages. Les contraintes d'écrans plus petits vous obligent à donner des priorités à vos contenus, ce qui nécessite d'avoir une bonne idée de ce en quoi ces contenus consistent.

#### Content strategy

Développer une bonne stratégie de contenu est donc très important. Plus vous aurez une bonne vision du contenu de l'ensemble de vos pages, meilleures seront les décisions que vous pourrez prendre au niveau de votre data structure et de l'organisation de vos pages.

A ce sujet, je ne peux que vous recommander l'ouvrage de Karen McGrane sur A Book Apart: ["Content strategy for mobile"](http://www.abookapart.com/products/content-strategy-for-mobile). L'auteur aborde la conception de stratégies de contenu et de structuration de données adaptées à un web multi-plateformes.

#### Content first

Lorsque vous commencez un projet responsive, il est donc important de commencer par une réflexion en profonndeur sur les contenus de votre site ou de votre application. Certains, comme Stephen Hay dans son livre ["Responsive Design Workflows"](http://responsivedesignworkflow.com/) préconisent de commencer par un prototype de votre site qui met l'accent uniquement sur le contenu. Voici également une vidéo d'[une présentation de Stephen Hay sur le sujet](http://vimeo.com/45915667).

### workflows

#### Présentation des designs
L’idée est ici de travailler de façon plus rapide et itérative, en utilisant des documents moins lourds à produire, permettant des cycles de feedback plus fréquents et plus rapides et créant de ce fait une dynamique dans laquelle le client / commanditaire se sent plus impliqué.##### Moodboards, Style tiles, style guides et elements collagesPlutôt que de fournir au client des “mockups” Photoshop dans lesquels les moindres éléments des pages sont designés, il est plus facile et plus rapide d’explorer diverses pistes graphiques à l’aide de moodboards.
[Style tiles](http://styletil.es/), [styles guides](http://24ways.org/2011/front-end-style-guides/) et [elements collages](http://danielmall.com/articles/rif-element-collages/) peuvent ensuite être produits relativement rapidement pour réaliser [quelques explorations visuelles](http://www.clearleft.com/thinks/visualdesignexplorations/) autour de concepts intéressants et d'éléments centraux du site / de l’application. Photoshop est encore présent dans le processus, mais seulement pour le design de l’un ou l’autre composant graphique et plus comme outil unique.Style tiles et style guides peuvent facilement être produits en HTML/CSS/JS et peuvent également servir de documents de validation au niveau du style graphique du futur site.##### Wireframes, prototypes papiers, prototypes keynoteAfin de tester les solutions pouvant être appliquées importants ou complexes du site / de l’application, les wireframes ont prouvés leur efficacité.
A nouveau, plutôt que de réaliser en wireframes l’ensemble des pages du site et de tous les éléments qui les composent, on va se concentrer sur les grands types de pages principaux et sur les éléments les plus problématiques ou les plus centraux (navigation, header, footer, profils utilisateurs, etc.)A ce stade, les [“prototypes papier”](http://www.alistapart.com/articles/paperprototyping/) peuvent également être [intéressants au niveau de l’exploration de concepts](http://www.speckyboy.com/2010/06/24/10-effective-video-examples-of-paper-prototyping/). La démarche consiste à créer des wireframes papiers pour l’ensemble des éléments ou module à partir desquels les pages vont êtres composées, ce qui permet de les agencer facilement et de voir si les pages ainsi créées donnent satisfaction. Une démarche similaire peut être adoptée à l’aide d’[outils tels que keynote](http://www.edenspiekermann.com/en/blog/espi-at-work-the-power-of-keynote).A nouveau, l’idée est ici de pouvoir rapidement tester des idées et de pouvoir ensuite travailler dessus de façon itérative.##### Prototypes en HTML/CSS/JSLa manière la plus efficace de tester et de présenter le graphisme final de votre site ou application, c’est de le faire dans son élément naturel: le browser et l’écran. Une fois un tel prototype HTML/CSS/JS construit, l’ensemble des solutions envisagées pour résoudre les divers problèmes posés peuvent être évaluées en “situation réelle”.Les spécificités du medium sont présentées efficacement le produit final est représenté de façon réaliste.Attention, il ne s’agit ici encore que d’un prototype, une “proof of concept” développée rapidement. L’ensemble du code HTML/CSS/JS développé à ce stade devra être codé en phase de production. De nombreuses agences web utilisent des frameworks HTML/CSS/JS préexistants tels que [Boostrap](http://getbootstrap.com/) et [Foundation](http://foundation.zurb.com/) afin de développer rapidement ces prototypes.
<blockquote><p>""[the design process] is about designing, prototyping and making. When you separate those, I think the final result suffers."</p><p>Jonathan Ive, March, 2012</p>
</blockquote>

#### Communication interne et communication client

Le job des designers web à grandement évolué: nous devons maintenant créer des systèmes modulaires et flexibles et plus des interfaces fixes dans photoshop. Le nombre d’inconnues et d’élements entrant en ligne de compte dans un site Internet ne cesse d’augmenter.Cela requiert des changements dans les process et workflow utilisés mais aussi une plus grande collaboration et un dialogue plus étroit entre les différents intervenants.

<blockquote><p>You must also address the very human issue of communication. Earlier and more frequent collaboration among team members and the client must become the rule in your workflow, not the exception. Content, design, and development team members must review and collaborate regularly at every stage in the creation process until the site is live. We can’t ‘throw it over the wall’ anymore— at least, not if we want our sites to be excellent. There are simply too many moving parts now. Go forth and collaborate.</p><p><em>Drew Clemens - Smashing Magazine</em></p>
</blockquote>
## Ressources

- L’article [“Responsive Web Design”](http://www.alistapart.com/articles/responsive-web-design/) de Ethan Marcotte sur A List Apart;
- ["Responsive Web Design"](http://www.alistapart.com/articles/responsive-web-design/) Le livre de Ethan Marcotte publié par A Book Apart;
- ["Guidelines for Responsive Web Design"](http://www.smashingmagazine.com/2011/01/12/guidelines-for-responsive-web-design/) sur Smashing Magazine;
- ["Responsive Web Design Techniques, Tools and Design Strategies"](http://www.smashingmagazine.com/2011/07/22/responsive-web-design-techniques-tools-and-design-strategies/) sur SMashing Magazine;
- [“More responsive Web Design”](http://www.jasonthings.com/2011/03/626/): un article de fond résumant bien la problématique, ses tenants et aboutissants.
- ["Envisionning a Responsive Future"](http://www.wsol.com/White_Board/Topics/Design_Advice/Envisioning_a_Responsive_Future/): par Dennis Kardys pour WSOL
- ["This is Responsive Ressources"](http://bradfrost.github.io/this-is-responsive/resources.html) par Brad Frost: une mine d’or pour les ressources sur le responsive web design.
- ["Mobile First Responsive Web Design"](http://www.bradfrostweb.com/blog/web/mobile-first-responsive-web-design/) par Brad Frost
- ["Responsive Navigation Patterns"](http://www.bradfrostweb.com/blog/web/responsive-nav-patterns/) et ["Complex Navigation Patterns for Reposnive Design"](http://www.bradfrostweb.com/blog/web/complex-navigation-patterns-for-responsive-design/): par Brad Frost
- ["Multi-Device Layout Patterns"](http://www.lukew.com/ff/entry.asp?1514) par Luke W.
- ["Responsive Responsive Design"](http://www.24ways.org/2012/responsive-responsive-design/) par Tim Kadlek
- [Southstreet](https://github.com/filamentgroup/Southstreet): Une boite à outils mises à votre disposition par Filement Group pour vous aider dans votre implémentation de mobile first responsive web designs.
- ["Create fluid Videos"](http://www.netmagazine.com/tutorials/create-fluid-width-videos) - Chris Coyer
- ["A pixel is not a pixel is not a pixel"](http://www.quirksmode.org/blog/archives/2010/04/a_pixel_is_not.html) de Peter-Paul Koch: différence entre pixels CSS pixels écran, retina etc.
- ["Media Query & Asset Downloading Results"](http://www.timkadlec.com/2012/04/media-query-asset-downloading-results/) par Tim Kaldec: chargement des média et autres fichier externes lors de l’utilisation de media-queries
- [“Design Process in the Responsive Age”](http://uxdesign.smashingmagazine.com/2012/05/30/design-process-responsive-age/) par Drew Clemens
- ["Collaborative Development for a Responsively Designed Web"](http://www.24ways.org/2011/collaborative-development-for-a-responsively-designed-web/) par Paul Robert Lloyd
- ["For a Future-Friendly Web"](http://www.bradfrostweb.com/blog/web/for-a-future-friendly-web/) par Brad Frost
- ["A richer Canevas"](http://www.markboulton.co.uk/journal/a-richer-canvas) par Mark Boulton
- ["Design Systems"](http://www.24ways.org/2012/design-systems/) par Laura Kalbag
- Mes propres [ressources archivées sur Pinboard](https://pinboard.in/search/u:jeromecoupe?query=rwd).
