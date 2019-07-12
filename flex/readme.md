# WSH CSS flexbox & grid practice

A flexbox koncepciója:

![flexbox koncepció](/img/flex1.png)

## Alap html/css létrehozása:

HTML markup létrehozása EMMET-el:

EMMET: ```.container>(.item.item--${$})*12```

1) ### Alap konfig:
    - container: ```display: flex; vagy display: inline-flex```
    - SCSS:
        ```css
        .container {
            display: flex;
            width: 90%;
            margin: 10rem auto;
            background-color: bisque;
        }

        .item {
            padding: 1rem;
            margin: 1rem;
            font-size: 4rem;
            background-color: orangered;
            text-align: center;
        }
        ```
---
## Ordering & Orientation

2) ### flex-direction
    - flex-direction: **row** | row-reverse | column | column-reverse

3) ### flex-wrap
    - flex-wrap: **nowrap** | wrap | wrap-reverse

4) ### flex-flow (container)
   - A *flex-direction* és *flex-wrap* összevonása.
   - flex-flow: row wrap | column nowrap | ...

5) ### order
    - Alapesetben mindegyik **0**
    - order: -1
        ```css
        .item {
            ...

            &--3 {
                order: -1;
            }
        }
        ```
---
## Alignment

![flexbox tengelyek](/img/flex2.png)

Alapvetően két tengely mentén lehet az igazítást megadni. Van egy alap irány a 'main axis', amit a *felx-direction* szab meg, valamint egy kereszt irány.

6) ### justify-content - A main-axis mentén igazítás
   - justify-content: **flex-start** | flex-end | center |
space-between | space-around | space-evenly
    - flex-wrap: wrap; // Ha elfogy a hely, töri

7) ### align-items - A cross axis mentén igazítás
   - align-items: **stretch** | flex-start | flex-end |
center | baseline
    - flex-direction: column;

8) ### align-self - A cross axis mentén igazítás elemenként
    - Az egyes elemeken külön-külön is állítható a konténeren állított *align-items*.
    - align-self: **auto** | stretch | flex-start | flexend
| center | baseline
    - SCSS:

        ```css
        .item {
            ...

            &--5 {
                align-self: flex-end;
            }
        }
        ```

9) ### align-content - A main-axis igazítása a konténerben
    - align-content: **stretch** | flex-start | flex-end |
center | space-between | space-around
    - flex-wrap: nowrap esetén nincs értelme és hatása se!

---
## Flexibility

Mind a 3 beállítás elemenkénti működést szabályoz.

10)  ### flex-grow - növekedési faktor
    - Szabályozza, hogy a konténerben - main-axis irányban - a maradék helyet hogy töltse ki az elem. 
    - flex-grow: **0** | `<integer>`
    - Az alap 0 a tartalomnak megfelelően, a pozitív egész pedig egy szorzószám. Hasonlóan, mint WPF esetén a *
    - SCSS:
        ```css
        .item {
            ...

            &--5 {
                flex-grow: 1;
            }

            &--8 {
                flex-grow: 2;
            }
        }
        ```
11)  ### flex-shrink - zsugorodási faktor
   - Szabályozza, hogy a konténerben - main-axis irányban -, ha nem férnek el az elemek hogy zsugorodjon az adott elem.
   - 0: ne zsugorodjon
   - 1: zsugorodjon
   - flex-shrink: **1** | `<integer>`
   - SCSS:
        ```css
        .item {
            ...

            width: 20rem;

            &--2 {
                flex-shrink: 0;
            }

            &--8 {
                flex-shrink: 0;
            }
        }
        ```

12) flex-basis - kezdő méret megadása
    - flex-basis: **auto** | `<length>`
    - beállítja az adott elem kezdőméretét, melyről a zsugorodási és növekedési faktor indul. Normál css méretmegadás.

13) flex - A grow, shrink és basis összevonása
    - flex: **0 1 auto** | `<int> <int> <len>`

---
## Hasznos linkek

### Flexbox

[A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)


[Flexbox Cheatsheet](https://yoksel.github.io/flex-cheatsheet/)

### Általános

[JONAS' RESOURCES FOR BUILDING BEAUTIFUL 
WEBSITES WITH HTML, CSS AND JAVASCRIPT](http://codingheroes.io/resources/)

[Can I use?](https://caniuse.com/)

[CODEPEN](https://codepen.io/)

[CSS Selector Reference](https://www.w3schools.com/cssref/css_selectors.asp)


---
## Összefoglalásként

![flexbox tengelyek](/img/flex3.png)