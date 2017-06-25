---
permalink: /
---

<link rel="stylesheet" href="/css/pusheen.css">


<div id="boarder">
    <div id="board"></div>
</div>

<script>

    var imgArray = [
        "autumn.jpg",
        "bake.gif",
        "balloons.jpg",
        "beach.gif",
        "burrito.gif",
        "busy.jpg",
        "christmas.gif",
        "christmas_tree.gif",
        "flowers.jpg",
        "halloween.gif",
        "mermaid.gif",
        "more_ice_cream.jpg",
        "pusheen_ice_cream.jpg",
        "shersheen.gif",
        "snowman.png"
    ];

    var boardArray = imgArray.concat(imgArray);
    shuffle(boardArray);

    var board = document.getElementById("board");
    for (var i = 0; i < 30; i++) {
        board.insertAdjacentHTML("beforeend", 
            `<img src="/img/pusheen/${boardArray[i]}">`
        )
    }
































function shuffle(a) {
    for (let i = a.length; i; i--) {
        let j = Math.floor(Math.random() * i);
        [a[i - 1], a[j]] = [a[j], a[i - 1]];
    }
}

</script>