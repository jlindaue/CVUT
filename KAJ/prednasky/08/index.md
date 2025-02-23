# KAJ 08: CSS transitions, animations, efekty

---

# Obsah
  1. Transitions
  1. Animations
  1. Filtry
  1. Ostatní efekty

---

# CSS transitions

  - V rámci životního cyklu stránky často dochází ke změnám CSS hodnot
  - Změny hodnot jsou okamžité a skokové
  - Pomocí transition lze tyto změny provádět plynule (animovaně)

---

# CSS transitions: ukázka

```css
div {
	background-color: blue;
	width: 200px;
	height: 200px;
	transition: all 3s;
}
div:hover {
	width: 600px;
	background-color: red;
}
```

<div id="example1"></div>

<style>
.slide:not(.current) #example1 { display: none; }
#example1 {
	background-color: blue;
	width: 200px;
	height: 200px;
	transition: all 3s;
	-moz-transition: all 3s;
	-webkit-transition: all 3s;
	position: absolute;
	right: 0;
	top: 3em;
}
#example1:hover {
	width: 600px;
	background-color: red;
}
</style>

---

# CSS transitions: vlastnosti

  - Meta-vlastnost `transition`
  - Individuálně lze upřesnit `transition-property`, `transition-duration`, `transition-timing-function`, `transition-delay`
  - Občas nutnost prefixování
  - Žádný JavaScript

---

# CSS transitions: transition-property

  - Plynule lze měnit řadu vlastností, ne však všechny
  - Je nutné dbát na kompatibilní hodnoty (`px` vs. `%` vs. `auto`)

---

# CSS transitions: transition-duration

  - Jak dlouho plynulá změna trvá
  - Povolené jednotky `s` a `ms`

---

# CSS transitions: transition-timing-function

  - Jak interpolovat hodnoty v závislosti na čase
  - Interpolační funkci možno zadat klíčovým slovem nebo předpisem
  - [Přehled na MDN](https://developer.mozilla.org/en-US/docs/CSS/timing-function)

---

# CSS transitions: steps

```css
div {
	background-image: url(sprite.png);
	background-position: 0 0;
	transition: background-position 500ms steps(11);
}
div:hover {
	background-position: -8800px 0;
}
```

<div id="example-steps"></div>
<style>
	#example-steps {
		width: 800px;
		height: 480px;
		background-image: url(sprite.png);
		background-position: 0 -100px;
		transition: background-position 500ms steps(11);
	}
	#example-steps:hover {
		background-position: -8800px -100px;
	}
</style>

---

# CSS transitions: transition-delay

  - Zpoždění animace po změně vlastnosti

---

# CSS transitions: pozor na implementaci vykreslování!

```js
var div = document.createElement("div");
div.style.transition = "all 3s";
div.style.color = "red";
document.body.appendChild(div);

div.style.color = "blue";
/* ??? */
```

---


# CSS transitions: pozor na implementaci vykreslování!

```js
div.style.color = "red";
div.offsetWidth;
div.style.color = "blue";
```

```js
div.style.color = "red";
setTimeout(function() {
	div.style.color = "blue";
}, 0);
```

---

# CSS animations

  - Vylepšení techniky transitions
  - Navazování změn
  - Možnost vykonávání ve smyčce
  - Řádově složitější syntaxe

---

# CSS animations: syntaxe

  - Definice rozdělena na dvě komponenty
  - V první části popis metadat animace (jak rychle, opakování, směr, časování, &hellip;)
  - V druhé části popis jednotlivých vizuálních vlastností v průběhu animace

---

# CSS animations: ukázka

<div id="example2"></div>

```css
div {
	animation: posun 4s linear infinite;
}

@keyframes posun {
	0%, 100% { right: 0px;   top: 100px; }
	25%      { right: 200px; top: 100px; }
	50%      { right: 200px; top: 300px; }
	75%      { right: 0px;   top: 300px; }
}
```

<style>
	#example2 {
		animation: posun 4s linear infinite;
		-moz-animation: posun 4s linear infinite;
		-webkit-animation: posun 4s linear infinite;
		background-color: blue;
		width: 200px;
		height: 200px;
		position: absolute;
	}
	@keyframes posun {
		0%, 100% { right: 0px; top: 100px;}
		25% { right: 200px; top: 100px;}
		50% { right: 200px; top: 300px;}
		75% { right: 0px; top: 300px;}
	}
	@-moz-keyframes posun {
		0%, 100% { right: 0px; top: 100px;}
		25% { right: 200px; top: 100px;}
		50% { right: 200px; top: 300px;}
		75% { right: 0px; top: 300px;}
	}
	@-webkit-keyframes posun {
		0%, 100% { right: 0px; top: 100px;}
		25% { right: 200px; top: 100px;}
		50% { right: 200px; top: 300px;}
		75% { right: 0px; top: 300px;}
	}
</style>

---

# CSS animations: definice animace

  - Meta-vlastnost `animation`
  - Individuálně lze upřesnit:
    - `animation-name`
    - `animation-duration`
    - `animation-timing-function`
    - `animation-delay`
    - `animation-iteration-count`
    - `animation-direction`
    - `animation-fill-mode`
    - `animation-play-state`

---

# CSS animation: animation-iteration-count

  - Počet iterací
  - Hodnota `infinite` značí nekonečnou smyčku

---

# CSS animation: animation-direction

  - Směr vykonávání jednotlivých iterací
  - `normal`, `reverse`, `alternate`, `alternate-reverse`

---

# CSS animation: animation-fill-mode

  - Jak aplikovat styly pokud animace neběží
  - Povolené hodnoty:
    - `none`
    - `forwards` &ndash; styly z posledního aplikovaného keyframe
    - `backwards` &ndash; styly z prvního aplikovaného keyframe
    - `both` &ndash; obojí

---

# CSS animation: animation-play-state

  - running/paused &ndash; možnost pozastavit animaci

---

# CSS animation: @keyframes

Definice vzhledu v jednotlivých krocích animace

```css
@keyframes test {
	from, 33% { /* ... */ }
	to        { /* ... */ }
}
@-moz-keyframes {}
@-webkit-keyframes {}
```

---

# Události pro transitions a animations

  - Nastávají na těch HTML prvcích, na které se aplikuje transition/animace
  - `transitionend`, `animationstart`, `animationend`, `animationiteration`
  - Dříve existovaly prefixované varianty
  - Hodí se, když synchronizujeme JS logiku s CSS vizuálem

---

# CSS efekty: průhlednost

```css
div {
	background-color: white;
	opacity: 0.5;
}

div {
	background-color: rgba(255, 255, 255, 0.5);
}
```

---

# CSS efekty: zaoblené rohy

```css
div {
	border-radius: 5px;
	border-radius: 50% 0 0 0;
}
```

---

# CSS efekty: stín písma

```css
div {
	text-shadow: 2px 1px 5px red;
}
```

  - Možno zadat více stínů naráz
  - Nulové rozostření = kopie textu

---

# CSS efekty: stín HTML uzlu

```css
div {
	box-shadow: 2px 1px 5px red;
	box-shadow: 2px 1px 5px 10px red;  /* spread */
	box-shadow: inset 2px 1px 5px red; /* dovnitř */
}
```

  - Možno zadat více stínů naráz
  - Respektuje zaoblené rohy
  - [Ukázka](https://developer.mozilla.org/en-US/docs/Web/CSS/box-shadow)

---

# CSS efekty: barevné přechody

  - Lineární (opakující se) barevné přechody
  - Kruhové (opakující se) barevné přechody
  - Možno zadat více přechodů naráz
  - Respektuje zaoblené rohy
  - Pouze jako forma obrázku na pozadí

---

# CSS efekty: barevné přechody

```css {id=example3}
background-image: linear-gradient(45deg, red, yellow, green, blue, violet);
```

```css {id=example4}
background-image: radial-gradient(red, yellow, rgb(30, 144, 255));
```

```css {id=example5}
repeating-linear-gradient(to right, #f88, #f88 5px, #fff 5px, #fff 10px);
```

<style>
	#example3 {
		background-image: linear-gradient(45deg, red, yellow, green, blue, violet);
	}
	#example4 {
		background-image: radial-gradient(red, yellow, rgb(30, 144, 255));
	}
	#example5 {
		background-image: repeating-linear-gradient(to right, #f88, #f88 5px, white 5px, white 10px);
	}
	#example3, #example4, #example5 {
		line-height: 80px;
		text-align: center;
		display: block;
	}
	#example3 div::before, #example4 div::before, #example5 div::before { display: none; }
</style>

---

# CSS filtry

  - Návrat kdysi proprietární vlastnosti `filter`
  - [Dokumentace na MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/filter)
  - Aplikace základních grafických filtrů na jednotlivé HTML prvky

---

# CSS filtry &ndash; ukázka

```css
filter: blur(5px);
filter: brightness(0.4);
filter: contrast(200%);
filter: drop-shadow(16px 16px 20px blue);
filter: grayscale(50%);
filter: hue-rotate(90deg);
filter: invert(75%);
filter: opacity(25%);
filter: saturate(30%);
filter: sepia(60%);
filter: url("filters.svg#filter-id");

filter: contrast(175%) brightness(3%);
```

---

# CSS efekty: ukázky

[Ukazatel načítání](https://jsfiddle.net/wk3y941x/)

[Lea Verou: CSS3 Patterns](https://lea.verou.me/css3patterns/)

[Prezentace bez JS](https://ondras.github.io/pure-css-slides/)

[Stín a přechod jako dekorace textu](http://www.shadycharacters.co.uk/)

---

# Prostor pro otázky

? { .questions }
