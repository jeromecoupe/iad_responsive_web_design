# Responsive web design

## Introduction

Nous accédons à nos sites et à nos applications avec une variétés de plus en plus importante de terminaux aux capacités fort différentes: téléphones, tablettes, laptops, desktops, télévisions, etc.

Les sites et les applications doivent donc offrir des expériences utilisateurs adaptées à ces différents terminaux. Les notions de page, de canevas fixe et stable, d’expérience utilisateur constante, disparaissent petit à petit au profit d’une conception mouvante, adaptable et flexible du web.

Dans un article fondateur publié sur A List Apart, [Ethan Marcotte](http://unstoppablerobotninja.com/entry/on-being-responsive/) nous propose une approche faisant droit à cette dimension d’expérience utilisateur adaptable qu’il appelle [responsive web design](http://www.alistapart.com/articles/responsive-web-design/). Celle-ci se base sur trois composants:

- Media Queries
- Layout et composants flexibles
- Media flexibles

Les techniques de Responsive Web Design suffisent en général pour les sites informatifs dont la mission première est la transmission de contenus. Pour les sites plus proches d’applications en ligne, d’autres solutions telles que des applications mobiles dédiées doivent parfois être envisagées en complément.

Pour la presse par exemple, l’usage du responsive web design permet de ne maintenir qu’une seule base de code et donc de réaliser des économies d’échelle. Les applications natives peuvent alors se concentrer sur la création de valeur ajoutée.

Cette approche, couplée à une approche [mobile first](http://www.abookapart.com/products/mobile-first) / [structured content first](http://www.slideshare.net/stephenhay/structured-content-first) permet également de réfléchir sur votre structure de données et d’établir des priorités parmi les divers éléments composant votre site.

## Document HTML

```html
<!DOCTYPE html>
<html class="no-js" lang="en">
  <head>
    <meta charset="utf-8">
    <title>My page</title>
    <meta name="description" content="My description">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="css/screen.css">
  </head>
  <body>
    <p>Hello world</p>
  </body>
</html>
```

L'utilisation du **meta tag viewport** permet de s'assurer de deux choses:

- la largeur du viewport est égale à la largeur du device utilisé
- Le zoom est remis à son niveau par défaut.

## Media Queries

Les [media queries](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Media_queries) étendent les fonctionnalités des types de média. Elles permettent de servir des feuilles de styles ou certaines déclarations au sein de feuille de style en fonction de caractéristiques de la plateforme à l’aide de laquelle sont affichées les pages.

Ces Media Queries permettent de tester les caractéristiques suivantes: `width`, `max-width`, `min-width`, `height`, `max-height`, `min-height`, `aspect-ratio`, `device-aspect-ratio`, `device-height`, `monochrome`, `color`, `device-width`, `orientation`, `resolution`, etc.

Les media queries sont utilisables avec des feuilles de styles liées

```html
<link rel="stylesheet" media="all and (min-width:970px)" href="css/medium.css" />
```

ou au sein de feuilles de styles existantes

```css
@media all and (min-width: 970px)
{
  /* selectors and rules */
}
```

Comme le mentionne Stéphanie Rieger sur Cloud Four [il peut-être avantageux de spécifier vos media-queries en em](http://blog.cloudfour.com/the-ems-have-it-proportional-media-queries-ftw/) pour donner plus de flexibilité à vos layouts. Ceux-ci vont en effet changer lorsque l'utilisateur change la taille de texte. Ceci étant dit, les navigateur supportent bien les media queries spécifiées en pixels également.

L’idée est d’utiliser les media queries pour permettre à l’expérience utilisateur d’être la meilleure possible quelle que soit la plateforme utilisée.

Pour ce qui est du choix des valeurs de breakpoints, je vous invite à [suivre le conseil de Stephen Hay](https://twitter.com/brad_frost/status/191977076000161793).

## Layouts & composants fluides

Les spécifications récentes que sont Flexbox et Grid ont été développées après l'avènement du responsive web design et permettent de travailler facilement avec des layout et des composants fluides.

### Layout responsive avec grid

CSS grid permet de réaliser facilement des layout resmonsives en redéfinissant des grilles à différents breakpoints à l'aide de media queries.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <link rel="stylesheet" href="css/main.css">
</head>
<body>
  <div class="page">
    <header class="page__header">header</header>
    <main class="page__content-primary">
      <h1>Title of my site</h1>
      <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Veniam saepe voluptatum, debitis voluptates error libero similique molestias tenetur doloribus praesentium placeat exercitationem quam qui. Magnam quod cumque ut ex cum.</p>
      <p>Ea ullam ex quam. Quisquam, ipsam facere. Quibusdam, animi necessitatibus. Eveniet hic nihil deleniti quo doloribus nisi esse ducimus voluptatibus voluptatum, voluptatem amet sapiente? Delectus provident eius quo quibusdam possimus.</p>
      <p>Amet, quod inventore. Enim voluptate quisquam facilis accusamus tempore eligendi illo illum minus? Minima aperiam iure laudantium fugiat nostrum, ut, ullam quisquam perferendis voluptatem, ducimus earum provident ex error culpa.</p>
    </main>
    <aside class="page__content-secondary">
      <h2>Secondary content</h2>
      <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Sint quia suscipit ullam neque amet. Voluptatibus enim facere error aliquid dolore repellat sed inventore. Unde ipsam tenetur ex fugit? Vitae, numquam!</p>
    </aside>
    <footer class="page__footer">footer</footer>
  </div>
</body>
</html>
```

```css
/* resets and unidirectional margins */
h1, h2, h3, h4, h5, h6,
p, blockquote,
ul, ol, dl,
pre,
figure,
table,
form,
hr
{
  margin-top: 0;
  margin-bottom: 1.5rem;
}

html
{
  font: 400 100%/1.5 "Helvetica", "Arial", sans-serif;
}

body
{
  margin: 0;
  padding: 0;

  background-color: #fff;
  color: #141414;
}

.page
{
  max-width: 1440px;
  min-height: 100vh;
  margin: 0 auto;
}

/* add a grid at medium breakpoint */
@media all and (min-width: 750px)
{
  .page
  {
    display: grid;
    grid-template-columns: 1fr 2fr;
    grid-template-rows: auto 1fr auto;
    grid-template-areas:
      "header header"
      "main sidebar"
      "footer footer";
  }
}

/* add a grid at large breakpoint */
@media all and (min-width: 1140px)
{
  .page
  {
    grid-template-columns: 1fr 4fr 2fr;
    grid-template-rows: 1fr auto;
    grid-template-areas:
      "header main sidebar"
      "header footer footer";
  }
}

.page__header
{
  grid-area: header;

  padding: 1.5rem;
  background-color: red;
}

.page__content-primary
{
  grid-area: main;

  padding: 1.5rem;
  background-color: greenyellow;
}

.page__content-secondary
{
  grid-area: sidebar;

  padding: 1.5rem;
  background-color: green;
}

.page__footer
{
  grid-area: footer;

  padding: 1.5rem;
  background-color: gray;
}
```

Exercices

- *Réaliser un composant responsive avec grid: title, quote, biography, skills*
- *Réaliser une gallerie d'images fluides avec grid*

### Composant responsive avec flexbox

Il est également assez facile d'utiliser flexbox pour certaines choses. Par exemple pour passer rapidement d'une interface de navigation verticale (small screens) à une interface horizontale (medium screens et plus).

```css
.c-mainnav {
  list-style: none;
  margin: 0;
  padding: 0;

  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: stretch;
}

@media all and (min-width: 750px) {
  .c-mainnav {
    flex-direction: row;
    justify-content: flex-end;
    align-items: center;
  }
}

.c-mainnav__item {
  border-bottom: 1px solid #cccccc;
}

.c-mainnav__item:first-child {
  border-top: 1px solid #cccccc;
}

@media all and (min-width: 750px) {
  .c-mainnav__item,
  .c-mainnav__item:first-child {
    border: none;
  }
}

.c-mainnav__link {
  display: block;
  padding: 0.5rem 0.25rem;
  font: 500 0.875rem/1 "Helvetica Neue", "Helvetica", "Arial", sans-serif;
  color: #050d0f;
  text-decoration: none;
  letter-spacing: 1px;
}

.c-mainnav__link:hover {
  color: #5cbee2;
}

@media all and (min-width: 750px) {
  .c-mainnav__link {
    padding: 0.5rem 1rem;
  }
}
```

Exercices

- *Réaliser un liste de blogposts avec flexbox (date et titres)*

## Media Flexibles: images

Commençons par les images. On supprime d’abord toute référence aux dimensions de l’image dans le HTML.

```html
<img class="o-fluidimage" src="img/monimage.png" alt="mon image">
```

Une simple modification de la CSS suffit ensuite à ce que les images prennent tout l’espace disponible dans leur bloc conteneur. C’est donc la taille du bloc conteneur qui va définir la taille de l’image.

```css
.o-fluidimage {
  display: block;
  max-width:100%;
  height: auto;
}
```

Ceci fonctionne tant que vous images restent toujours plus grandes que leurs blocs conteneurs. Si vous souhaitez que celles-ci s’étendent toujours pour occuper leur bloc conteneur, vous pouvez ajouter et utiliser les règles suivantes.

```css
.o-fluidimage--fullwidth {
  width:100%;
}
```

Pour éviter de faire consommer de la bande passante inutilement, il faut servir des images de tailles différentes suivant la plateforme. Les attributs `srcset` et `sizes` ainsi que l'élément `<picture>` permettent de résoudre ces problématiques.

Au niveau des images, assurez-vous tout d'abord d’utiliser des services tels que [ImageOptim](http://imageoptim.com/) ou [Smush.it](http://www.smushit.com/ysmush.it/) pour optimiser vos images.

Les outils de build tels que Grunt et Gulp que nous verrons l’année prochaine permettent également d’optimiser vos images automatiquement à l'aide de scripts.

### Images de background

En ce qui concerne les images de background, vous pouvez utiliser des media queries dans vos CSS pour servir une petite image par défaut et servir une plus grande image lorsque le layout l’exige.

Attention cependant, les deux images sont parfois téléchargées. Voir à ce sujet l'article très complet de Tim Kadlec ["Media Query & Asset Downloading Results"](http://timkadlec.com/2012/04/media-query-asset-downloading-results/).

```css
.banner {
  background-repeat: no-repeat;
  background-position: 50% 50%;
  background-size: cover;
}

.banner--home {
  background-image: url(../img/banners/banner-home-800.jpg);
}

@media all and (min-width: 800px) {
  .banner--home {
    background-image: url(../img/banners/banner-home-1024.jpg);
  }
}

@media all and (min-width: 1024px) {
  .banner--home {
    background-image: url(../img/banners/banner-home-1500.jpg);
  }
}
```

### Images de contenu: `srcset`, `sizes` et `<picture>`

La situation est un peu plus complexe au niveau des images de contenus. [Une solution idéale pour les images responsives](http://responsiveimages.org/) doit relever les [défis suivants](http://usecases.responsiveimages.org/):

1. Différentes images servies selon la densité d'écran
2. Différentes images servies selon la taille d'écran
3. Art direction (cadrage)

Cette solution est [implémentée dans la plupart des navigateurs aujourd’hui](http://responsiveimages.org/). Les navigateurs qui ne supportent pas `srcset`, `sizes` ou `picture` servent simplement l'image spécifiée par l'attribut `src`..

#### srcset and sizes

Les attributs `srcset` et `sizes` permettent de fournir au navigateur toutes les informations nécessaires pour choisir l'image à servir en fonction de la taille de l'écran ou de sa densité. Cette approche nécessite de connaître la façon dont les images vont s'afficher dans votre layout.

Ces attributs sont suffisants si vous ne devez pas prendre en compte de différence de cadrage mais que vous avez seulement affaire à des images identiques hormis en ce qui concerne la taille.

**Différentes tailles d'images:**

```html
<img src="small.jpg"
     srcset="large.jpg  1024w,
             medium.jpg 640w,
             small.jpg  320w"
     sizes="(min-width: 750px) 33.3vw,
            100vw"
     width="1024"
     height="768"
     loading="lazy"
     decoding="async"
     alt="alternative representation">
```

- `src` valeur par défaut pour les navigateurs ne supportant pas `srcset`. C'est la valeur de cette propriété de le navigateur va venir changer en fonction des informations passées pa `srcset` (images disponibles et taille) et pas `sizes` (information relatives à l'affichage).
- `srcset` spécifie différentes images et la largeur de chacune d'entre-elles. Les valeurs pour `w` font référence à la taille actuelle de l'image en pixels.
- `sizes` spécifie la largeur de l'image par rapport au viewport pour chacune des media-queries spécifiées dans les paires media query / valeur. La dernière valeur est une valeur par défaut.

Ces informations permettent aux navigateurs de choisir l'image adéquate en fonction à la fois de la taille d'affichage de l'image et de la densité de l'écran sur lequel elle est affichée.

Les attributs `loading` et `decoding` sont utiles pour la performance.

- `loading="lazy"`: donne l'instruction au navigateur de ne charger les images que lorsqu'elles sont afficher dans le viewport du navigateur. Attention à n'utiliser cet attribut que pour des images ou des iframe qui sont affichées hors écran.
- `decoding="async"`: donne l'instruction au navigateur de continuer à charger le contenu de la page, même si l'image n'est pas encore tout à fait chargée.

#### `<picture>` et art direction

Si vous devez servir des images différentes sur le plan de la composition (cadrage, orientation, art direction) vous pouvez alors utiliser les éléments `<picture>` et `<source>`. Voici un exemple simple:

```html
<picture>
  <source media="(min-width: 1024px)"
          srcset="obama-fullshot.jpg">
  <img src="obama-closeup.jpg"
       loading="lazy"
       decoding="async"
       alt="Obama seals the deal">
</picture>
```

Notez bien que `<picture>`, `<source>`, `srcset` et `sizes` peuvent être combinés avec ce dont nous avons parlé précédemment.

```html
<picture>
   <source media="(min-width: 750px)"
           srcset="large.jpg 1024w,
                   medium.jpg 640w,
                   small.jpg 320w"
           sizes="33.3vw" />
   <source srcset="large-cropped.jpg 1024w,
                   medium-cropped.jpg 640w,
                   small-cropped.jpg 320w"
           sizes="100vw" />
   <img src="small-cropped.jpg"
        loading="lazy"
        decoding="async"
        alt="alternative representation">
</picture>
```

Le sujet de images responsives est assez complexe. Je ne peux que vous recommander quelques ressources abordant le sujet en détail: un [article de Eric Portis pour Smashing Magazine](http://www.smashingmagazine.com/2014/05/14/responsive-images-done-right-guide-picture-srcset/), et deux autres excellents articles publiés sur Opera Dev.

- [Native Responsive Images](https://dev.opera.com/articles/native-responsive-images/) par Yoav Weiss
- [Responsive Images: Use Cases and Documented Code Snippets](https://dev.opera.com/articles/responsive-images/) par Andreas Bovens

A lire également, une série d'articles très complets de Jason Grigsby sur Cloudfour: [Responsive Images 101](http://blog.cloudfour.com/responsive-images-101-definitions/)

Exercices

- *Mélanger flexbox et grid en réalisant une grille fluide de cartes de produits (image, titre, description, prix) pour un site de e-commerce*

## Media Flexibles: videos

Il est également possible d’intégrer des vidéos à vos pages de façon fluide. Si vous travaillez en HTML5, la solution est assez simple et ressemble à celle adoptée pour les images.

```css
video {
  max-width: 100%;
  height: auto;
}
```

Afin de servir des videos adaptées à tous les terminaux, que ce soit sur le plan des formats ou de la taille d'affichage, des services tels que Youtube et Vimeo sont intéressants et très utilisés.

Dans le cadre de projets responsives, cla implique de disposer d'un container fluide, avec un ratio largeur / hauteur constant (16/9 ou 4/3). Cet [article sur A List Apart](http://www.alistapart.com/articles/creating-intrinsic-ratios-for-video/) ou encore [cet article sur css-tricks](http://css-tricks.com/NetMag/FluidWidthVideo/Article-FluidWidthVideo.php) détaillent une solution très efficace et facile à implementer qui repose sur le `padding` et les positionnements `absolute` et `relative`.

```css
.video-container {
  width: 100%;
  padding-top: 56.25%; /*1 6 by 9 ratio for the container */
  position:relative; /* positioning context */
}

.video-container > iframe {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  border: none;
}
```

## Contenus

Lorsque vous commencez un projet responsive, il est important de commencer par une réflexion en profondeur sur les contenus et les fonctionnalités de votre site ou de votre application.

Certains, comme Stephen Hay dans son livre ["Responsive Design Workflows"](http://responsivedesignworkflow.com/) préconisent de commencer par un prototype de votre site qui met l'accent uniquement sur le contenu. Voici également une vidéo d'[une présentation de Stephen Hay sur le sujet](http://vimeo.com/45915667).

Penser à vos contenus et à vos fonctionnalités dans le cadre d'un "tube de 320 pixels de large" est un excellent exercice pour établir des priorités.

### Stratégie de contenu

Développer une bonne stratégie de contenu est donc très important. A ce sujet, je ne peux que vous recommander l'ouvrage de Karen McGrane sur A Book Apart: ["Content strategy for mobile"](http://www.abookapart.com/products/content-strategy-for-mobile).

L'auteur aborde la conception de stratégies de contenu et de structuration de données adaptées à un web multi-plateformes. Ne vous laissez pas induire en erreur par le titre: les mots “for mobile” pourrait simplement en être effacés.

## Design responsive: nouveau workflows, nouveaux outils

### Présentation des designs

L’idée est ici de travailler de façon plus rapide et itérative, en utilisant des documents moins lourds à produire, permettant des cycles de feedback plus fréquents et plus rapides et créant de ce fait une dynamique dans laquelle le client / commanditaire se sent plus impliqué.

Dans une vidéo intitulée "[Design deliverables for a post-comp era](http://typecast.com/seminars/post-comp)" Dan Mall présente une méthode de travail intéressante et efficace.

#### Moodboards, Style tiles, style guides et elements collages

Plutôt que de fournir au client des “mockups” Photoshop dans lesquels les moindres éléments des pages sont désignés, il est plus facile et plus rapide d’explorer diverses pistes graphiques à l’aide de moodboards.

[Style tiles](http://styletil.es/), [styles guides](http://24ways.org/2011/front-end-style-guides/) et [elements collages](http://danielmall.com/articles/rif-element-collages/) peuvent ensuite être produits relativement rapidement pour réaliser [quelques explorations visuelles](http://www.clearleft.com/thinks/visualdesignexplorations/) autour de concepts intéressants et [d'éléments centraux du site](http://superfriend.ly/TechCrunch) / de l’application.

Photoshop est encore présent dans le processus, mais seulement pour le design de l’un ou l’autre composants graphiques et plus comme outil unique.

Style tiles et style guides peuvent facilement être produits en HTML/CSS/JS et peuvent également servir de documents de validation au niveau du style graphique du futur site.

##### Wireframes, prototypes papiers, prototypes keynote, InVision

Afin de tester les solutions pouvant être appliquées importants ou complexes du site / de l’application, les wireframes ont prouvés leur efficacité.

A nouveau, plutôt que de réaliser en wireframes l’ensemble des pages du site et de tous les éléments qui les composent, on va se concentrer sur les grands types de pages principaux et sur les éléments les plus problématiques ou les plus centraux (navigation, header, footer, profils utilisateurs, etc.)

A ce stade, les [“prototypes papier”](http://www.alistapart.com/articles/paperprototyping/) peuvent également être [intéressants au niveau de l’exploration de concepts](http://www.speckyboy.com/2010/06/24/10-effective-video-examples-of-paper-prototyping/). La démarche consiste à créer des wireframes papiers pour l’ensemble des éléments ou module à partir desquels les pages vont êtres composées, ce qui permet de les agencer facilement et de voir si les pages ainsi créées donnent satisfaction. Une démarche similaire peut être adoptée à l’aide d’[outils tels que keynote](http://www.edenspiekermann.com/en/blog/espi-at-work-the-power-of-keynote).

A nouveau, l’idée est ici de pouvoir rapidement tester des idées et de pouvoir ensuite travailler de façon itérative.

##### Prototypes en HTML/CSS/JS

La manière la plus efficace de tester et de présenter le graphisme final de votre site ou application, c’est de le faire dans son élément naturel: le browser et l’écran. Une fois un tel prototype HTML/CSS/JS construit, l’ensemble des solutions envisagées pour résoudre les divers problèmes posés peuvent être évaluées en “situation réelle”.

Les spécificités du medium sont présentées efficacement le produit final est représenté de façon réaliste.

Attention, il ne s’agit ici encore que d’un prototype, une “proof of concept” développée rapidement. L’ensemble du code HTML/CSS/JS développé à ce stade devra être codé en phase de production. De nombreuses agences web utilisent des frameworks HTML/CSS/JS préexistants tels que [Bootstrap](http://getbootstrap.com/) et [Foundation](http://foundation.zurb.com/) afin de développer rapidement ces prototypes.

> "[the design process] is about designing, prototyping and making. When you separate those, I think the final result suffers."

> Jonathan Ive, March, 2012

#### Communication interne et communication client

Le travail des designers web à grandement évolué: nous devons maintenant créer des systèmes modulaires et flexibles et plus des interfaces fixes dans photoshop. Le nombre d’inconnues et d’élements entrant en ligne de compte dans un site Internet ne cesse d’augmenter.

Cela requiert des changements dans les process et workflow utilisés mais aussi une plus grande collaboration et un dialogue plus étroit entre les différents intervenants.

> "You must also address the very human issue of communication. Earlier and more frequent collaboration among team members and the client must become the rule in your workflow, not the exception. Content, design, and development team members must review and collaborate regularly at every stage in the creation process until the site is live. We can’t ‘throw it over the wall’ anymore— at least, not if we want our sites to be excellent. There are simply too many moving parts now. Go forth and collaborate."

> Drew Clemens - [Design process in the responsive age](https://www.smashingmagazine.com/2012/05/design-process-responsive-age/) (Smashing Magazine)

## Techniques intéressantes

### Multi-column layouts

CSS permet de créer facilement des colonnes au sein d'un bloc conteneur, via les propriétés `column-count` et `column-width`, `column-gap` et `column-rule`. Etant assez facile à utiliser, ces propriétés sont également assez utiles dans le cadre de layouts responsive. Les colonnes peuvent contenir du texte comme des media et les navigateurs tentent toujours de créer des colonnes égales en hauteur.

- La propriété `column-count` permet de spécifier le nombre de colonnes à créer dans un bloc conteneur.
- La propriété `column-width` permet de spécifier la largeur minimale des colonnes. Si `column-count` n'est pas également spécifié, le navigateur va créer automatiquement le nombre de colonnes nécessaires en fonctione de l'espace disponible dans le bloc conteneur.

Certains navigateurs ne [supportent encore que les versions préfixées de ces propriétés](http://caniuse.com/#feat=multicolumn). Des outils tels que [prefixr](http://prefixr.com) ou [autoprefixer](https://github.com/postcss/autoprefixze) peuvent vous aider à automatiser la mise en place de ces derniers.


```html
<div class="columns">
  <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Et minima nulla obcaecati perspiciatis cumque totam aut, dicta natus molestias aperiam maiores dolore sequi fugit placeat nostrum architecto magnam possimus voluptate?</p>
  <p>Reprehenderit enim dignissimos at quia eum, vitae suscipit rerum nisi quos molestiae mollitia vero sapiente beatae, porro non illo numquam! Nemo doloribus!</p>
</div>
```

```css
@media all and (min-width: 1200px) {
  .columns {
    column-count: 2;
  }
}
```

ou

```css
.columns {
  column-width: 30rem;
}
```

- la propriété `column-gap` permet de spécifier la taille des espaces entre les colonnes.
- la propriété `column-rule` permet de spécifier les caractéristiques d'un séparateur de colonnes. Ses caractéristiques sont calquées sur celles de la propriété `border`

```css
.columns {
  column-count: 2;
  column-gap: 2.5em;
  column-rule: 1px solid red;
}
```

### Icônes en SVG

Etant donné la prolifération d'écran de haute résolution, il devient intéressant de travailler avec SVG, un format vectoriel, plutôt qu'avec des formats comme PNG ou JPEG pour vos icônes ou pour certains visuels.

De nombreux programmes de création (Illustrator, Sketch) permettent d'exporter facilement des fichiers SVG. Ceux-ci peuvent ensuite être optimisés ([SVGO](https://github.com/svg/svgo), [SVGO GUI](https://github.com/svg/svgo-gui), [SVGCleaner](http://sourceforge.net/projects/svgcleaner/) avant d'être intégrés à vos fichiers HTML / CSS. L'idée est ici de créer un système permettant d'inclure facilement des icones dans votre design.

De nombreuses solutions existent pour créer des systèmes d’icônes en SVG en utilisant des sprites. L’inévitable Sara Soueidan détaille ces solutions dans un article pour 24ways: "[An overview of SVG sprite creation techniques](https://24ways.org/2014/an-overview-of-svg-sprite-creation-techniques/)"

Une autre solution, sans doute plus flexible que des sprites, consiste à intégrer directement le code SVG de vos icônes dans votre fichier CSS. Cela vous permet d'éviter une requête HTTP et de ne pas devoir créer et maintenir vos sprites. Les outils de build ou les CMS rendent cela facile à réaliser et à maintenir. De plus, des icones en SVG sont facilement manipulables à l'aide de classes et de styles CSS (couleurs, transitions, etc).

SVG devient un format de plus en plus populaire et s'y intéresser de près devient nécessaire. Si vous souhaitez vous documenter sur le sujet, Chris Coyier propose [une excellente introduction sur CSS Tricks](http://css-tricks.com/using-svg/), ainsi qu'une [série de ressources](http://css-tricks.com/mega-list-svg-information/). Willian Justen propose également une [liste impressionnante de ressources](https://github.com/willianjusten/awesome-svg) sur Github. Voir aussi le ["practical SVG" de Chris Coyier sur A Book Apart](https://abookapart.com/products/practical-svg).

Exercice

*Créer une icône SVG et manipuler les couleurs en utilisant `currentColor`*

### Interfaces de navigation

Il existe différentes techniques / patterns pour réaliser des interfaces de navigation responsives.

- [Dogstudio](https://dogstudio.co/): Navigation hamburger permanente avec variantions de layout pour le menu suivant les breakpoints
- [Leapforward](https://leapforward.be/): Hambuger menu (small et medium) et navigation horizontale (large)

Il existe énormément de variantes de ces grands patterns. Voyons ensemble comment réalser deux exemples simples en code.

Exercices
- *Réaliser une navigation hamburger permanente*
- *Réaliser une navigation hamburger / horizontale*
- *Réaliser une navigation side menu off canvas / push page*

## Ressources

- Blogpost ["Maximally optimizing image loading for the web in 2021"](https://www.industrialempathy.com/posts/image-optimizations/) de Malte Ubl;
- L’article ["Responsive Web Design"](http://www.alistapart.com/articles/responsive-web-design/) de Ethan Marcotte sur A List Apart;
- ["Responsive Web Design"](http://www.alistapart.com/articles/responsive-web-design/) Le livre de Ethan Marcotte publié par A Book Apart;
- ["This is Responsive Ressources"](http://bradfrost.github.io/this-is-responsive/resources.html) par Brad Frost: une mine d’or pour les ressources sur le responsive web design.
- ["Guidelines for Responsive Web Design"](http://www.smashingmagazine.com/2011/01/12/guidelines-for-responsive-web-design/) sur Smashing Magazine;
- ["Responsive Web Design Techniques, Tools and Design Strategies"](http://www.smashingmagazine.com/2011/07/22/responsive-web-design-techniques-tools-and-design-strategies/) sur Smashing Magazine;
- [“More responsive Web Design”](http://www.jasonthings.com/2011/03/626/): un article de fond résumant bien la problématique, ses tenants et aboutissants.
- ["Envisionning a Responsive Future"](http://www.wsol.com/White_Board/Topics/Design_Advice/Envisioning_a_Responsive_Future/): par Dennis Kardys pour WSOL
- ["Mobile First Responsive Web Design"](http://www.bradfrostweb.com/blog/web/mobile-first-responsive-web-design/) par Brad Frost
- ["Responsive Navigation Patterns"](http://www.bradfrostweb.com/blog/web/responsive-nav-patterns/) et ["Complex Navigation Patterns for Reposnive Design"](http://www.bradfrostweb.com/blog/web/complex-navigation-patterns-for-responsive-design/): par Brad Frost
- ["Multi-Device Layout Patterns"](http://www.lukew.com/ff/entry.asp?1514) par Luke W.
- ["Responsive Responsive Design"](http://www.24ways.org/2012/responsive-responsive-design/) par Tim Kadlek
- [Southstreet](https://github.com/filamentgroup/Southstreet): Une boite à outils mises à votre disposition par Filement Group pour vous aider dans votre implémentation de mobile first responsive web designs.
- ["Create fluid Videos"](http://www.netmagazine.com/tutorials/create-fluid-width-videos) - Chris Coyer
- ["Responsive Images Done Right: A Guide To <picture> And srcset"](http://www.smashingmagazine.com/2014/05/14/responsive-images-done-right-guide-picture-srcset/) - Eric Portis
- ["Native Responsive Images"](https://dev.opera.com/articles/native-responsive-images/) - par Yoav Weiss
- [Responsive Images in Practice](http://alistapart.com/article/responsive-images-in-practice) - par Eric Portis
- [Responsive Images 101 series](http://blog.cloudfour.com/responsive-images-101-definitions/) - par Jason Grigsby
- ["A pixel is not a pixel is not a pixel"](http://www.quirksmode.org/blog/archives/2010/04/a_pixel_is_not.html) de Peter-Paul Koch: différence entre pixels CSS pixels écran, retina etc.
- ["Media Query & Asset Downloading Results"](http://www.timkadlec.com/2012/04/media-query-asset-downloading-results/) par Tim Kaldec: chargement des média et autres fichier externes lors de l’utilisation de media-queries
- [Design deliverables for a post-comp era](http://typecast.com/seminars/post-comp) par Dan Mall
- [“Design Process in the Responsive Age”](http://uxdesign.smashingmagazine.com/2012/05/30/design-process-responsive-age/) par Drew Clemens
- ["Collaborative Development for a Responsively Designed Web"](http://www.24ways.org/2011/collaborative-development-for-a-responsively-designed-web/) par Paul Robert Lloyd
- ["For a Future-Friendly Web"](http://www.bradfrostweb.com/blog/web/for-a-future-friendly-web/) par Brad Frost
- ["A richer Canevas"](http://www.markboulton.co.uk/journal/a-richer-canvas) par Mark Boulton
- ["Design Systems"](http://www.24ways.org/2012/design-systems/) par Laura Kalbag
- "[Understanding SVG Coordinate Systems & Transformations (part 1)](http://sarasoueidan.com/blog/svg-coordinate-systems/)" par Sara Soueidan
- "[Understanding SVG Coordinate Systems & Transformations (part 2)](http://sarasoueidan.com/blog/svg-transformations/)" par Sara Soueidan
- "[http://sarasoueidan.com/blog/nesting-svgs/ (part 3)](http://sarasoueidan.com/blog/svg-coordinate-systems/)" par Sara Soueidan
- "[Making SVGs Responsive with CSS](http://tympanus.net/codrops/2014/08/19/making-svgs-responsive-with-css/)" Sara Soueidan
- "[Using SVG](http://css-tricks.com/using-svg/)" par Chris Coyier
- "[A Compendium of SVG Information](http://css-tricks.com/mega-list-svg-information/)" par Chris Coyier
- "[Awesome SVG](https://github.com/willianjusten/awesome-svg)": Une liste de ressources concernant SVG par Willian Justen
- [Web fundamentals](https://developers.google.com/web/fundamentals/) par Google, particulièrement la partie concernant les [layouts Multi-Device](https://developers.google.com/web/fundamentals/layouts/)
- Mes propres [ressources archivées sur Pinboard](https://pinboard.in/search/u:jeromecoupe?query=rwd).
- “[Practical SVG](https://abookapart.com/products/practical-svg)” par Chris Coyier
