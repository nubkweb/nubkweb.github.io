---
layout: default
permalink: /pusheen-memory
---

<link rel="stylesheet" href="/css/pusheen.css">

<div id="ultradiv">
    <div id="board"></div>
    <div id="gameStatus">
        <p id="moves"> Moves: <span id="numberOfMoves">0</span></p>
        <p id="score"> Score: <span id="totalScore">0</span></p>
        <img id="pusheenMood" src="/img/pusheen/mood/neutral.gif">
    </div>
</div>

<div id="preload" style="display:none"></div>

<div id="victoryDiv">
    <div id="megaDiv">
        <h1>Your score: <span id="finalScore">0</span></h1>
        <img src="/img/pusheen/mood/victorypusheen.gif"> 
    </div>
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
        "more_ice_cream.gif",
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

    var pusheenMoodDisplay = document.getElementById("pusheenMood")
    var numberOfMovesDisplay = document.getElementById("numberOfMoves")
    var totalScoreDisplay = document.getElementById("totalScore")
    var victoryModal = document.getElementById("victoryDiv")
    var finalScoreDisplay = document.getElementById("finalScore")
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
        secondFlip: {index: null, img: null},
        moves: 0,
        misses: 0,
        hits: 0,
        score: 0,
        flips: 0
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
            game.moves++;
            numberOfMovesDisplay.textContent = game.moves;
            game.secondFlip = hiddenPusheen;
            if (game.firstFlip.img !== game.secondFlip.img) {
                setTimeout(function() {
                    flipCardBack(game.firstFlip.index);
                    flipCardBack(game.secondFlip.index);
                    reset();
                }, 1000);
                game.hits = 0;
                game.misses++
            } else { // if cards match
                game.misses = 0;
                game.hits++;
                game.score = game.score + game.hits * game.hits;
                game.flips++;
                if (victory()) {
                    showVictoryDance();
                }
                reset();
            }
            setTimeout(updateGameStatus, 200);
        }
        console.log(game.moves)
        
        
    });
    
    function victory() {
        return game.flips === 15;
    }
    
    function showVictoryDance() {
        finalScoreDisplay.textContent = game.score;
        victoryModal.style.display = "block";
    }
    
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
    
    function updateGameStatus() {
        totalScoreDisplay.textContent = game.score;
        if (game.misses === 1) {
           pusheenMoodDisplay.src = "/img/pusheen/mood/fail1.png";
        } else if (game.misses === 2) {
            pusheenMoodDisplay.src = "/img/pusheen/mood/fail2.png";
        } else if (game.misses >= 3) {
            pusheenMoodDisplay.src = "/img/pusheen/mood/fail3.png";
        }
        if (game.hits === 1) {
           pusheenMoodDisplay.src = "/img/pusheen/mood/happy1.png";
        } else if (game.hits === 2) {
            pusheenMoodDisplay.src = "/img/pusheen/mood/happy2.png";
        } else if (game.hits >= 3) {
            pusheenMoodDisplay.src = "/img/pusheen/mood/happy3.png";
        }
        setTimeout(function() {
            pusheenMoodDisplay.src = "/img/pusheen/mood/neutral.gif";
        }, 1500);
    }
    
    function reset() {
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