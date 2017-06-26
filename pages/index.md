---
permalink: /
---

<link rel="stylesheet" href="/css/pusheen.css">


<div id="boarder">
    <div id="board"></div>
</div>

<div id="preload" style="display:none"></div>

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
    
    var preload = document.getElementById("preload");
    for (var i = 0; i < 15; i++) {
        preload.insertAdjacentHTML("beforeend", 
            `<img src="/img/pusheen/${imgArray[i]}">`
        )
    }

    var boardArray = imgArray.concat(imgArray);
    shuffle(boardArray);

    var board = document.getElementById("board");
    for (var i = 0; i < 30; i++) {
        board.insertAdjacentHTML("beforeend", 
            `<img data-index="${i}" src="/img/pusheen/card_back.jpg">`
        )
    }
    
    var flipNumber = 0;
    var firstFlip;
    var secondFlip;
    
    document.body.addEventListener("click", function (e) {
        var index = e.target.getAttribute("data-index");
        if(cardIsFacingDown(index)) {
            flipNumber++;
        }
        figurativelyFlipCard(index);
    });
   
   function cardIsFacingDown(index){
        return board.children[index].src.includes("card_back.jpg");
   }
   
    function figurativelyFlipCard(index) {
        var hiddenPusheen = boardArray[index];
        board.children[index].src = `/img/pusheen/${hiddenPusheen}`;
        // `/img/pusheen/${hiddenPusheen}` == "/img/pusheen/" + hiddenPusheen;
        
    }

    
    































function shuffle(a) {
    for (let i = a.length; i; i--) {
        let j = Math.floor(Math.random() * i);
        [a[i - 1], a[j]] = [a[j], a[i - 1]];
    }
}

</script>