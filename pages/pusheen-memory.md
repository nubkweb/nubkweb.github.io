---
layout: default
permalink: /pusheen-memory
---

<link rel="stylesheet" href="/css/pusheen.css">

<div id="board"></div>

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
    
    var game = {
        locked: false,
        flipNumber: 0,
        firstFlip: {index: null, img: null},
        secondFlip: {index: null, img: null}
    }
    
    document.body.addEventListener("click", function (e) {
        var index = e.target.getAttribute("data-index");
        
        if (index === null || game.locked || !cardIsFacingDown(index)) {
            return; // nothing can happen after return
        }
        
        game.locked = true;
        game.flipNumber++;
        var hiddenPusheen = figurativelyFlipCard(index);
        
        if (game.flipNumber === 1) {
            game.firstFlip = hiddenPusheen;
            game.locked = false;
        } else if (game.flipNumber === 2) {
            game.secondFlip = hiddenPusheen;
            if (game.firstFlip.img !== game.secondFlip.img) {
                setTimeout(function() {
                    flipCardBack(game.firstFlip.index);
                    flipCardBack(game.secondFlip.index);
                    reset();
                }, 1000);
            } else {
                reset();
            }
        }
        console.log(game)
    });
   
   function cardIsFacingDown(index){
        return board.children[index].src.includes("card_back.jpg");
   }
   
    function figurativelyFlipCard(index) {
        var hiddenPusheen = boardArray[index];
        board.children[index].src = `/img/pusheen/${hiddenPusheen}`;
        return {index: index, img: hiddenPusheen};
    }
    
    function flipCardBack(index) {
        board.children[index].src = "/img/pusheen/card_back.jpg";
    }
    
    function reset(){
        game.flipNumber = 0;
        game.firstFlip = null;
        game.secondFlip = null;
        game.locked = false;
    }

    
    




























function shuffle(a) {
    for (let i = a.length; i; i--) {
        let j = Math.floor(Math.random() * i);
        [a[i - 1], a[j]] = [a[j], a[i - 1]];
    }
}

</script>