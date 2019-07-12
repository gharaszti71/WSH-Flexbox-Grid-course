# WSH CSS flexbox & grid practice

A grid koncepciója:

![flexbox koncepció](/img/grid1.png)

## Alap html/css létrehozása:

HTML markup létrehozása EMMET-el:

EMMET: ```.container>(.item.item--${$})*12```

1) ### Alap konfig:
    - container: ```display: grid;``` az inline-grid még experimental!
    - SCSS:
        ```css
        .container {
            display: grid;
            width: 90%;
            margin: 10rem auto;
            background-color: bisque;
            grid-gap: 1rem;
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
## Explicit grid definition

2) grid-template
   - grid-template-rows, grid-template-columns
   - új mértékegység: **fr** fraction unit
     - példa: *grid-template-columns: 3rem 25% 1fr 2fr* -- **1fr = ((width of grid) - (3rem) - (25% of width of grid)) / 3**
     - grid-template: 1fr 1fr 1fr / 1fr 1fr 1fr 1fr; 
     - grid-template: repeat(3, 1fr)  / repeat(4, 1fr); 
     - grid-template-columns: repeat(4, 1fr);
     - grid-template-columns: minmax(auto, 40rem) 1fr 8rem 12rem;

3) grid-gap
   - grid-row-gap **és** grid-column-gap => grid-gap
   - grid-gap: 100px 5rem;
   - grid-gap: 5rem;

---
## Positioning Items

![flexbox koncepció](/img/grid2.png)

4) grid-row, grid-column, grid-area
   - Minden egyes elemre meg lehet mondani, hogy mely osztási vonaltól meddig terjed.
     - grid-row-start:
     - grid-row-end:
     - grid-column-start:
     - grid-column-end:
   - Vagy ezek rövidre zárt verziói
     - grid-row: `grid-row-start / grid-row-end;`
     - grid-column: `grid-column-start / grid-column-end;`
   - Vagy a fentiek még rövidebbre zárt verziója
     - grid-area: `grid-row-start / grid-column-start / grid-row-end / grid-column-end;`
     - SCSS:
        ```css
        .item {
            ...
            &--1 {
                grid-area: 1/1/3/3;
            }

            &--2 {
                grid-column: 3 / -1;
            }

            &--3 {
                grid-row: 2 / span 4;
                grid-column: 4;
            }
        }
        ```
    - Természetesen át is fedhetik egymást az egyes területek, ilyenkor a szokásos *z-index* dönt a láthatóságról.

---
## Naming Grid Lines
A grid osztási vonalait el lehet nevezni, amely nevekre utána hivatkozni lehet a pozícionálásnál.

5) Név szerinti pozícionálás
   - head, main és foot régió kialakítása névvel
   - SCSS:
        ```css
        .container {
            ...
            grid-template-columns: repeat(4, 1fr);
            grid-template-rows: [head-start] 20rem [head-end main-start] 40rem [main-end foot-start] 10rem [foot-end];
        }

        .item {
            ...
            &--1 {
                grid-column: 1 / -1;
                grid-row: head-start / head-end;
            }

            &--2 {
                grid-column: 1 / -1;
                grid-row: main-start / main-end;
            }

            &--3 {
                grid-column: 1 / -1;
                grid-row: foot-start / foot-end;
            }
        }
        ```
    - Automatikus névadás repeat esetén
    - grid-template-columns: repeat(6, [col-start] 1fr [col-end]);
        ![grid auto-name](/img/grid4.png)
    - SCSS:
        ```css
        .container {
            ...
            grid-template-columns: repeat(6, [col-start] 1fr [col-end]);
        }

        .item {
            ...
            &--1 {
                grid-column: col-start / col-end 2;
            }

            &--2 {
                grid-column: col-start 3 / col-end 4;
            }

            &--3 {
                grid-column: col-start 5 / col-end 6;
            }
        }
        ```
---
## Naming and Positioning Items by Grid Areas

Ahogy a grid osztási vonalait el lehet nevezni, úgy komplett területeket is el lehet nevezni a gridben. A neveket az egyes pozíciók szerint kell kitölteni, területenként azonos névvel.

6) Terület "mátrix" megadása és hivatkozás rá
   - rid-template-areas: "első sor" "második sor";
   - SCSS:
        ```css
        .container {
            ...
            grid-template-rows: repeat(4, 1fr);
            grid-template-columns: repeat(3, 1fr);
            grid-template-areas: "head head head"
                                "main main main"
                                "main main main"
                                "foot foot foot";
        }

        .item {
            ...
            &--1 {
                grid-row-start:    head;
                grid-row-end:      head;
                grid-column-start: head;
                grid-column-end:   head;
            }

            &--2 {
                grid-area: main;
            }

            &--3 {
                grid-row:    foot;
                grid-column: foot;
            }
        }
        ```
---
## Implicit Grid

Ha nincs megadva explicit grid item megadás, akkor az item-ek implicit módon helyzekednek el.

7) grid-auto tulajdonságok
   - grid-auto-flow: **row** | column
   - grid-auto-columns, grid-auto-rows : 1db méret megadása, pl 10rem, 1fr...
   - Lehet keverni is template-el ha szükséges, akkor a template-en túlnyúló rész az implicit.
   - SCSS:
        ```css
        .container {
            ...
            grid-template-columns: 20rem 20rem;
            grid-auto-flow: column;
            grid-auto-columns: 1fr;
        }
        ```
---
## Implicit elnevezett grid területek és osztási vonalak

1) Implicit terület elnevezés
   - Ha vonalak elnevezésében következetesen használjuk a *-start* és *-end* utótagokat, valamint az elnevezéseket, akkor azok automatikusan területeket is kijelölnek, valamint ez vica-versa igaz, azaz a terület mátrix is létrehozza a *-start* és *-end* vonalakat, valamint sorszámozást.
   - Példa:
```css
grid-template-rows:    [outer-start] 1fr [inner-start] 1fr [inner-end] 1fr [outer-end];
grid-template-columns: [outer-start] 1fr [inner-start] 1fr [inner-end] 1fr [outer-end];
```

![grid auto-name](/img/grid5.png)

  - utána hivatkozás rá: `grid-area: inner` 
    ```css
        &--12 {
            grid-area: inner;
            background-color: lightblue;
        }
    ```

9) Implicit osztási vonal elnevezés
    - Ha a grid-template-areas -al adjuk meg a területeket, akkor a fent említett (*-start* és *-end*) séma szerint automatikusan elneveződnek az osztási vonalak.
   - SCSS:
        ```css
        .container {
            ...
            grid-template-rows: repeat(4, 1fr);
            grid-template-columns: repeat(3, 1fr);
            grid-template-areas: "head head head"
                                "main main main"
                                "main main main"
                                "foot foot foot";
        }

        .item {
            ...
            &--1 {
                grid-row-start:    head-start;
                grid-row-end:      head-end;
                grid-column-start: head-start;
                grid-column-end:   head-end;
            }

            &--2 {
                grid-area: main;
            }

            &--3 {
                grid-row:    foot;
                grid-column: foot;
            }
        }
        ```
---
## Aligning Grid Items (Box Alignment)
Lehetőség van a griden belül a tartalom grid cellán belüli igazítására.

10) Box Alignment Module
    - a konténeren vezessünk be egy új magasságot: `height: 80vh;`
    - Most csak 1 elemre lesz szükségünk.
    - A következő vezérlőket fogjuk használni: 
      - **justify-items** és **justify-self** sor-tengely mentén igazítás
      - **align-items** és **align-self** oszlop-tengely mentén igazítás
      - Lehetséges értékek: auto | normal | start | end | center | 
stretch | baseline | first baseline | last baseline
    - SCSS:
        ```css
        .container {
            ...
            height: 80vh;
            ...
            grid-template-rows: repeat(4, 1fr);
            grid-template-columns: repeat(3, 1fr);
            grid-template-areas: "main main main"
                                "main main main"
                                "main main main"
                                "main main main";
            justify-items: center;
            align-items: center;
        }

        .item {
            ...
            display: none;
            
            &--1 {
                display: block;
                grid-area: main;
                align-self: start; 
                justify-self: start;
            }
        }
        ```
    - Az item beállítása felülírja a konténer beállítását.
---
## Aligning Grid Tracks
Lehetőségünk van a `display: grid` -el jelölt konténeren belül az elemek különböző módon igazítására. Alapértelmezésben a bal felső sarok.

11) Grid track igazítás
    - a konténeren hagyjuk meg az új magasságot: `height: 80vh;`
    - ismét szükséges a 12 item. Elemméret 5x5 rem
    - A következő vezérlőket fogjuk használni:
      - **align-content** sor-tengely mentén igazítás
      - **justify-content** oszlop-tengely mentén igazítás
      - Lehetséges értékiek: normal | start | end |center | stretch | space-around | space-between | space-evenly | baseline | first baseline | last baseline
    - SCSS:
        ```css
        .container {
            ...
            height: 80vh;
            ...
            grid-template-rows: repeat(4, 5rem);
            grid-template-columns: repeat(3, 5rem);
            justify-content: end;
            align-content: center;
        }
        ```
---
## Hasznos linkek

### Grid

[A Complete Guide to Grid](https://css-tricks.com/snippets/css/complete-guide-grid/)

[Learn CSS Grid](https://learncssgrid.com/)

[GRID: A simple visual cheatsheet for CSS Grid Layout](http://grid.malven.co/)

### Általános

[JONAS' RESOURCES FOR BUILDING BEAUTIFUL 
WEBSITES WITH HTML, CSS AND JAVASCRIPT](http://codingheroes.io/resources/)

[Can I use?](https://caniuse.com/)

[CODEPEN](https://codepen.io/)

[CSS Selector Reference](https://www.w3schools.com/cssref/css_selectors.asp)

---
## Összefoglalásként

![grid összefoglaló](/img/grid3.png)