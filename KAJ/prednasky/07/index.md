# KAJ 07: 2D a 3D transformace, Flexible Boxes

---

# Obsah
  1. 2D transformace
  1. 3D transformace
  1. CSS Flexible Box Module
  1. Písma

---

# CSS transformace

  - CSS vlastnosti pro afinní geometrické transformace HTML prvků
  - Aplikují se po vykreslení, tj. neovlivňují layout ostatních prvků
  - Mohou být akcelerované (a proto např. rychlejší než absolutní pozicování)
  - Občas nutnost použití vendor prefixů

---

# CSS transformace: dostupné funkce

[Dokumentace na MDN](https://developer.mozilla.org/en-US/docs/CSS/transform)

```css {.hover}
div {
	transform: translate(100%, 50px);
	transform: rotate(45deg);
	transform: scale(0.2);
	transform: skewX(30deg);
	transform: matrix(a, b, c, d, tx, ty);
}
```

<style>
code.hover { display: block; }
code.hover:hover {
	transform: scale(0.8) rotate(10deg);
	-webkit-transform: scale(0.8) rotate(10deg);
}
</style>

---

# CSS transformace: ostatní

Vlastnost `transform-origin` určuje počátek souřadného systému (výchozí: 50% 50%)

```css
div {
	transform-origin: 0 0;
	transform: rotate(45deg);
}
```

Transformace lze kombinovat

```css
div {
	transform: rotate(45deg) translate(50px, 50px);
}
```

---

# CSS transformace: pořadí

```css
.blue { transform: translate(100%, 0) rotate(45deg) }
```

<div id="order1">
	<div></div>
	<div></div>
</div>

```css
.blue { transform: rotate(45deg) translate(100%, 0) }
```

<div id="order2">
	<div></div>
	<div></div>
</div>


<style>
	#order1, #order2 {
		height: 9em;
	}
	#order1 div, #order2 div {
		position: absolute;
		left: 50%;
		width: 200px;
		height: 200px;
		opacity: 0.5;
	}
	#order1 div:first-child, #order2 div:first-child {
		background-color: red;
	}
	#order1 div:nth-child(2), #order2 div:nth-child(2) {
		background-color: blue;
	}
	#order1 div:nth-child(2) {
		transform: translate(100%, 0) rotate(45deg);
		-webkit-transform: translate(100%, 0) rotate(45deg);
	}
	#order2 div:nth-child(2) {
		transform: rotate(45deg) translate(100%, 0);
		-webkit-transform: rotate(45deg) translate(100%, 0);
	}
</style>

---

# 3D CSS transformace

  - Rozšíření transformačních funkcí
  - `translateZ`, `translate3d`, `scaleZ`, `scale3d`
  - `rotate3d`, `rotateX`, `rotateY`, `rotateZ`
  - Nejistá podpora prohlížečů (závisí i na hardware)

<div id="rotate"><div></div></div>
<style>
#rotate {
	transform-style: preserve-3d;
	-webkit-transform-style: preserve-3d;
	perspective: 500px;
	-webkit-perspective: 500px;
	position: absolute;
	right: 10px;
	top: 100px;
}
#rotate div {
	width: 190px;
	height: 145px;
	background-image: url(img/cvut.png);
	border-radius: 20%;
	box-shadow: 0 0 3px 3px #000;
}
</style>

---

# 3D CSS transformace

  - Aby docházelo k prespektivnímu zkreslení, je nutno definovat parametry 3D prostoru
  - Vlastnost `perspective` na rodičovském prvku určuje míru zkreslení
  - Vlastnost `transform-style` určuje, jak se chovají 3d transformace [na potomcích již transformovaných prvků](https://jsfiddle.net/1rzpe641/)

---

# 3D CSS transformace

<div id="parent"><div></div></div>
<style>
#parent {
	-webkit-perspective: 500px;
	perspective: 500px;
}
#parent div {
	transform: rotateY(45deg);
	-webkit-transform: rotateY(45deg);
	width: 40%;
	height: 150px;
	background-color: red;
	margin: auto;
}
</style>

```css
#parent {
	perspective: 500px;
}

#parent div {
	transform: rotateY(45deg);
}
```

---

# 3D CSS transformace

  - [CSS Blockout](https://ondras.github.io/blockout/)
  - [Rubikova kostka](https://ondras.github.io/rubik/)

---

# CSS3 Flexible Box

  - Nový pozicovací algoritmus do CSS
  - Pro polohování boxů vedle sebe / pod sebe
  - Náhrada za (slabý) float

---

# CSS3 Flexible Box: základ

```html
<div id="parent">
	<div id="child1"></div>
	<div id="child2"></div>
	<div id="child3"></div>
</div>
```

```css
#parent {
	display: flex;
	flex-direction: row;
	justify-content: space-between;
}
```

[výsledek](https://jsfiddle.net/cu4ykbxL/1/)

---

# CSS3 Flexible Box

  - Komplexní systém mnoha CSS vlastností
  - Popisuje vždy jen rozložení několika sourozenců v rámci rodiče
  - Terminologie a *big picture* [na MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout/Using_CSS_flexible_boxes)

---

# CSS3 Flexible Box: relativní rozměry boxů

  - `flex-grow` je relativní *síla růstu*
  - `flex-shrink` je relativní *síla zmenšení*
  - `flex-basis` je základní rozměr (výška/šířka)

---

# CSS3 Flexible Box: pořadí

  - Vlastnost `order` pro změnu pořadí prvků
  - Podobný princip jako u `z-index`
  - Dovoluje zachovat obsah první
  - Často v kombinaci s media queries
  - [Ukázka](http://jsfiddle.net/ondras/tGK7p/)

---

# CSS3 Flexible Box: další vlastnosti

  - `justify-content` pro zarovnání v hlavní ose (*main axis*)
  - `align-items` a `align-self` pro zarovnání v kolmé ose (*cross axis*)
  - `flex-wrap` pro povolení/zakázání zalamování

---

# CSS3 Flexible Box: patička

  - Velmi obvyklý požadavek: patička na spodu, i když je málo obsahu
  - [Řešení na jsfiddle.net](https://jsfiddle.net/fLwn0w1w/)

---

# CSS3 Flexible Box: další čtení

  - [A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
  - [Flexbox Generator](http://the-echoplex.net/flexyboxes/)

---

# CSS @font-face {style="font-family: the girl next door"}

  - Definice vlastního písma
  - Podporováno od IE 4 (!)
  - Nutnost dodat soubor(y) s definicí písma
  - Pozor na CORS
  - Vhodné pro typografii, dříve pro ideogramy (nyní lépe SVG)

---

# CSS @font-face &ndash; ukázka

```css
@font-face {
	font-family: MujFont;
	src: local("Muj Font"), url(MujFont.ttf);
}

body {
	font-family: MujFont;
}
```

---

# CSS @font-face &ndash; formáty

  - EOT proprietární od Microsoftu
  - TTF, OTF
  - WOFF / WOFF2: Web Open Font Format
  - Více info viz caniuse.com

---

# CSS @font-face &ndash; Data URI

```css
@font-face {
    font-family: "MujFont";
    src: url(data:application/x-font-woff;charset=utf-8;base64,d09......AAA==) format("woff"),
         url("MujFont.ttf") format("truetype");
}
```

---

# CSS @font-face &ndash; FOUT

  - [Flash Of Unstyled Text](http://paulirish.com/2009/fighting-the-font-face-fout/)
  - Co má prohlížeč dělat, než se písmo načte?
  - Čekat vs. zkusit jiné?
  - Velký problém u HTML canvasu (později v semestru)
  - Budoucnost: [Font Loading API](https://developer.mozilla.org/en-US/docs/Web/API/CSS_Font_Loading_API)

---

# CSS @font-face &ndash; poznámky

  - Vlastní písma navyšují objem přenášených dat
  - Kvalita písma je často subjektivní
  - Odlišné renderování v různých prohlížečích a operačních systémech
  - <a target="_blank" href="http://www.google.com/webfonts">Google Web Fonts</a>

---

# Prostor pro otázky

? { .questions }
