<html>
<head>
<style>
@import url('https://fonts.googleapis.com/css?family=Gochi+Hand');

table {
    border-collapse: collapse;
    border-style: hidden;
    width: 300;
    height: 300;
    text-align: center;
    vertical-align: center;
}

table td, table th {
    border: 4px solid black;
}

.button {
width: 90px;
height: 90px;
background-color: white;
border: none;
padding: 2px;
margin: 2px;
font-size: 78px;
text-transform: capitalize;
font-family: "Gochi Hand";
}

.text {
size: 20px;
font-family: verdana;
}

</style>

<meta name="viewport" content="width=device-width, initial-scale=1.0">
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.1.1/jquery.min.js"></script>
<script>

var tl ="o";
var tm ="o";
var tr ="o";
var ml ="o";
var mm ="o";
var mr ="o";
var bl ="o";
var bm ="o";
var br ="o";

var index;

var move;
var winningMove;
var blockingMove;

var over = false;
var winner = "none";

var moves = ["tl", "tm", "tr", "ml", "mm", "mr", "bl", "bm", "br"];

function start(move, player) {

	$("#text").text("Now playing!");
	removeMove(move);
	startReport();

	if (winner != "none") {
		$("#text").html("The game is over, doofus. " + winner + " won.");
		return; 

	} else if (move == "tl" && tl == "o") {	
	    tl = player;
		$("#b-tl").html(player);

	} else if (move == "tm" && tm == "o") {
	    tm = player;
		$("#b-tm").html(player);

	} else if (move == "tr" && tr == "o") {	
	    tr = player;
		$("#b-tr").html(player);

	} else if (move == "ml" && ml == "o") {	
	    ml = player;
		$("#b-ml").html(player);

	} else if (move == "mm" && mm == "o") {	
	    mm = player;
		$("#b-mm").html(player);

	} else if (move == "mr" && mr == "o") {
	    mr = player;
		$("#b-mr").html(player);

	} else if (move == "bl" && bl == "o") {
	    bl = player;
		$("#b-bl").html(player);

	} else if (move == "bm" && bm == "o") {	
	    bm = player;
		$("#b-bm").html(player);

	} else if (move == "br" && br == "o") {	
	    br = player;
		$("#b-br").html(player);

	} else {
		$("#text").html("Player <b>" + player + "</b> move at <b>" + move + "</b> is not allowed!");
		return;
	}

    var win = checkWin(player); 
    if (win) {
		$("#text").html("Player " + player + " won!");
		winner = player; 
	} else if (player != "O" && moves != "") {
		computerTakeTurn();
	} else if (moves == "") {
		$("#text").html("Game over!");
		winner = "Nobody";
	}

}

function startReport() {
	$("#text_start").html("<b>start:</b><br>moves: " + moves + " <br> moves.length: " + moves.length + "<br> move: " + move + "<br>winningMove: " + winningMove + "<br>blockingMove: " + blockingMove);
}

function computerTakeTurn() {
	move = null;
	winningMove = null;
    blockingMove  = null;

	winningMove = checkTwo("O");
	blockingMove = checkTwo("X");
    if (checkTwo("O") != null) { //checking for win move to make
        computerReport();
    	start(winningMove, "O"); //need to have it not pick moves that are taken
    } else if (checkTwo("X") != null) {
    	computerReport();
        start(blockingMove, "O");
    } else {
       index = Math.floor(Math.random() * (moves.length));
       move = moves[index];
       computerReport();
	   start(move, "O");
    }

}

function computerReport() {
	$("#text_computer").html("<b>computerTakeTurn:</b><br>moves: " + moves + "<br>moves[index]: " + moves[index] + " <br> move: " + move + "<br>winningMove: " + winningMove + "<br>blockingMove: " + blockingMove);
}

function computerFirst() {
	if (moves.length == 8 || winner != "none") {
		reset();
		computerTakeTurn();
    } else if (moves.length != 9) {
        $("#text").html("Reset the game first.");
    } else {
    	reset();
	    computerTakeTurn();
	}
}

function removeMove(move) {
	$("#text_remove_1").text("removeMove before for loop: move is " + move);
	for (i = 0; i < moves.length; i++) {
       if (move == moves[i]) {
       	    $("#text_remove_2").html("<b>removeMove after entering if:</b><br> move: " + move + "<br> moves[i]: " + moves[i] + "<br>moves: " + moves + "<br>i: " + i);
       	    moves.splice(i, 1);
       	    $("#text_remove").html("<b>removeMove after array splice:</b><br> move: " + move + "<br> moves: " + moves);
       	    return;
       } 
	}	
}

function checkWin (player) {
	if (tl == player && tm == player && tr == player ||
		ml == player && mm == player && mr == player ||  // middle  hor
		bl == player && bm == player && br == player ||  //bottom hor
		tl == player && ml == player && bl == player ||  //left vert
		tm == player && mm == player && bm == player ||   //mid vert
		tr == player && mr == player && br == player ||   //rt vert
		bl == player && mm == player && tr == player ||   //diag
		br == player && mm == player && tl == player) { 
        	return true;
    } else {
    	return false;
    }
}

//check for two in a row for winning or blocking move
function checkTwo (player) {
	if (tm == player && tr == player && tl == "o" ||
	    ml == player && bl == player && tl == "o" ||
	    mm == player && br == player && tl == "o") {
	    return "tl";

	} else if (tl == player && tr == player && tm == "o" ||
	           mm == player && bm == player && tm == "o") {
	    return "tm";

    } else if (tl == player && tm == player && tr == "o" ||
	           mm == player && bl == player && tr == "o" ||
	           mr == player && br == player && tr == "o") {
	    return "tr";

	} else if (tl == player && bl == player && ml == "o" ||
	           mm == player && mr == player && ml == "o") {
	    return "ml";

	} else if (ml == player && mr == player && mm == "o" ||
	           tm == player && bm == player && mm == "o" ||
	           tl == player && br == player && mm == "o" ||
	           bl == player && tr == player && mm == "o") {
	    return "mm";

    } else if (ml == player && mm == player && mr == "o" ||
	           tr == player && br == player && mr == "o") {
	    return "mr";    

	} else if (tl == player && ml == player && bl == "o" ||
	           mm == player && tr == player && bl == "o" ||
	           bm == player && br == player && bl == "o") {
	    return "bl";

	} else if (bl == player && br == player && bm == "o" ||
	           tm == player && mm == player && bm == "o") {
	    return "bm";

	} else if (bl == player && bm == player && br == "o" ||
	           tl == player && mm == player && br == "o" ||
	           tr == player && mr == player && br == "o") {
	    return "br";
        
	} else {
		return null;
	}
}

function reset() {
	$("#text").text("Now playing!");

    $("#b-tl").html("");
    $("#b-tm").html("");
    $("#b-tr").html("");
    $("#b-ml").html("");
    $("#b-mm").html("");
    $("#b-mr").html("");
    $("#b-bl").html("");
    $("#b-bm").html("");
    $("#b-br").html("");

	 tl ="o";
	 tm ="o";
	 tr ="o";
	 ml ="o";
	 mm ="o";
	 mr ="o";
	 bl ="o";
	 bm ="o";
	 br ="o";

	 index = null;

	 move = null;
	 winningMove = null;
	 blockingMove = null;

	 winner = "none";

	 moves = ["tl", "tm", "tr", "ml", "mm", "mr", "bl", "bm", "br"];
}

</script>
</head>
<body>

<p class="text" id="text">Hi, let's play a game.</p>

<p></p>
<table>
    <tr>
        <td><button class="button" id="b-tl" onClick="start('tl', 'X')"></button></td>
        <td><button class="button" id="b-tm" onClick="start('tm', 'X')"></button></td>
        <td><button class="button" id="b-tr" onClick="start('tr', 'X')"></button></td>
    </tr>
    <tr>
        <td><button class="button" id="b-ml" onClick="start('ml', 'X')"></button></td>
        <td><button class="button" id="b-mm" onClick="start('mm', 'X')"></button></td>
        <td><button class="button" id="b-mr" onClick="start('mr', 'X')"></button></td>
    </tr>
    <tr>
        <td><button class="button" id="b-bl" onClick="start('bl', 'X')"></button></td>
        <td><button class="button" id="b-bm" onClick="start('bm', 'X')"></button></td>
        <td><button class="button" id="b-br" onClick="start('br', 'X')"></button></td>
    </tr>
</table>


 <br>


<p></p>
<button id="reset" onClick="reset()">Reset</button>

<button id="reset" onClick="computerFirst()">Computer goes first</button>

<p>Test results:</p>

<p id="text_start">start: </p>
<p id="text_remove_1">removeMove before for loop:</p>
<p id="text_remove_2">removeMove after if:</p>
<p id="text_remove">removeMove after for loop:</p>
<p id="text_computer">computerTakeTurn:</p>

</body>

</html>
