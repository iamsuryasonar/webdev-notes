## Grid
[Css grid in 13 minutes](https://www.youtube.com/watch?v=EiNiSFIPIQE&ab_channel=SlayingTheDragon)

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link href="./styles.css" rel="stylesheet">
</head>

<body>
    <div class="grid-container">
        <div class="grid-item grid-item-1">
            Lorem ipsum dolor sit amet consectetur adipisicing elit. Fugiat quas aut, dolor ullam dolorum nobis animi
            blanditiis obcaecati corporis, maxime delectus quam asperiores molestiae voluptates.
        </div>
        <div class="grid-item grid-item-2">grid-item-2</div>
        <div class="grid-item grid-item-3">grid-item-3</div>
        <div class="grid-item grid-item-4">grid-item-4</div>
    </div>
</body>

</html>
```

```css
.grid-item {
    background-color: aqua;
    height: 100%;
    width: 100%;
    border: solid 2px black;
}

.grid-container {
    display: grid;
    /* grid-template-columns: 200px 100px; horizontally 2 childrens 200px and 100px respectively*/
    /* grid-template-columns: 2fr 1fr; horizontally 2 childrens 2 fractions and 1 fraction respectively*/
    /* grid-template-columns: repeat(3, 100px); horizontally repeat 3 childrens of 100px each*/
    /* grid-template-rows: 200px 100px; vertically 2 childrens 200px and 100px respectively*/
    /* grid-auto-rows: 140px; vertically auto childrens if 140px is available*/
    /* grid-auto-rows: minmax(140px, auto); vertically auto childrens if minimum 140px is available and maximum can be content size*/
    /* grid-gap: 20px; gap between each grid*/
    /* grid-template-columns: 1fr 1fr;*/
    grid-auto-rows: minmax(140px, auto);
    grid-gap: 20px;
    grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));
    /* achieve responsiveness without media query */
}

.grid-item-1 {
    /* grid-column-start: 1;
    grid-column-end: 3; */
    /* where to start grid from and where to end */
    grid-column: 1/3;
    /* same as using grid-column-start or end */
}
```
