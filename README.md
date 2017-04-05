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
font-size: 50px;
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

var size;
var moves = [];
var locations = {};

function start(move) {
	//reset everything if there was already a game in progress
	moves = [];
    $( "#grid_here" ).empty();
    size = $('#size').val(); 
    //reset all v and h arrays 

	$("#text").html("Now playing on " + size + " x " + size + " board."); 
	
	//generates a table with rows and cols equal to size
	//rows are r1, r2, r3, cols are c1, c2, c3 etc
	//to do: how to assign -1/1 value ? or just count x/o as values
	//each cell has a button that calls makeMove, passes in its location on the grid
    var grid;
	grid = "<table id='grid'>"
	
	for (i = 0; i < size ; i++) {
		grid = grid + "<tr>";
		for (j = 0; j < size ; j++) {
		grid = grid + "<td> <button id='" + i + "" + j + "' class='button' Onclick='makeMove(\"" + i +  + j + "\")'></button> </td>";
		//adds all the moves into the moves array
		moves.push(i + "" + j);
		//add all the rows and cols properties to the locations object
        locations["row" + i] = 0;  //empty space is worth 0
        locations["col" + i] = 0;
		//creates empty arrays for each row and col. uh, if i knew how it would 
		//should the arrays be empty or should they have the locations for the row/col

		//how the f do i make these accessible outside? add them all to a global var?
		}
		
	grid = grid + "</tr>";
	}
	
	grid = grid + "</table>";
    $('#grid_here').append(grid);	 
}

function makeMove(m) {
	if ($("#" + m).html() == "") {
	    $("#" + m).html("X");
		removeMove(m);
		checkWin();
		//to do: check for a win
		computerMove();
		$("#text_move").html("<b>move</b><br>moves: " + moves + "<br>move: " + m); 
	} else {
		$("#text").html("Move not allowed."); 
	}
}	

function checkWin() {
	//check if someone won
	var win = size;
	$("#text_win").html("<b>checkWin</b><br>size: " + size + "<br>win: " + win);
	//catch cats game before all moves are used? separate checkCats function?
}

function removeMove(move) {
    for (i = 0 ; i < moves.length ; i++) {
        if (move == moves[i]) {
            moves.splice(i, 1);
       	    $("#text_remove").html("<b>removeMove</b><br>moves: " + moves + "<br>move: " + move);
		}
	}		
}

function computerMove() {
	if (moves == "") {
		$("#text").html("No moves left. Cat's game.");
	} else if (0 == 1) {
	    //make a winning move
	} else if (0 == 2) {
	    //make a blocking move    	
	} else {
        //make random move
	    index = Math.floor(Math.random() * (moves.length));
	    cm = moves[index];
		$("#" + cm).html("0");
		removeMove(cm);
		$("#text_computer").html("<b>computerMove</b><br>moves: " + moves + "<br>move: " + cm);
	}
}

</script>
</head>
<body>

<p class="text" id="text">Hi, let's play a game.</p>

<input id="size" value="3"/>

<button id="reset" onClick="start()">Set</button>  

<p></p>
<div id="grid_here"></div>

<p class="text" id="text_move">fd</p>
<p class="text" id="text_remove">fd</p>
<p class="text" id="text_win">fd</p>
<p class="text" id="text_computer">fd</p>
<p class="text" id="test1">test1</p>

</body>
</html>
