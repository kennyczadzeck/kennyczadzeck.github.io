---
layout: post
title:  "All I Know is That I Don't Know Nothing"
date:   2014-11-30 17:29:16
categories: technology, life, makersquare, san francisco, javascript, code
---
I’ve just finished the first week at Makersquare and I can sufficiently say that I’ve learned/re-learned at least as much (read: more than) about JavaScript as I have since beginning to really study web development and programming back in May/June. Some concepts are more solid than others, but I’m using all of them in some way or another.

At the end of the week, we were assigned an assignment to build a JavasScript checkers game over the weekend that ran in both the browser and the console simultaneously. It seemed simple enough, but it was an undertaking that took up most of my weekend. One thing I read over and over again in other developers’ blogs was that you can study and complete tutorials until you think you understand everything, but you don’t know how to code until you really start building your own applications. For better or for worse, I can say that this is 100% true. I thought I had a very solid grasp on basic JavaScript functionality and how to use JQuery to manipulate the DOM, but I definitely found myself struggling to apply my knowledge when I needed to do something more than complete a tutorial exercise and/or pass a quiz. However, I did come to fully embrace the concept of modularity in JavaScript development and it’s now fully ingrained in how I will approach everything I build from this point forward.

Here is an example of my first attempt at constructing the engine. It’s a mess.

{% highlight javascript %}
var makeMove = function (row1, col1, row2, col2) {
  // Executes the movement of the piece. Helper function called by attemptMove
  // White player execution block. First is movement to empty space. Second is movement for jumping a red piece. Both set currentPlayer to opposite after completion.
    if ((currentPlayer === 'wht') && (board[row2][col2] === ' X ')) {
    board[row2][col2] = currentPlayer;
    board[row1][col1] = ' X ';
    currentPlayer = 'red';
    console.log("Movement made! Now it's " + currentPlayer+ "'s turn.")
    // White jumping red. Adds an extra space to distance and empties out original destination.
  } else if ((currentPlayer === 'wht') && (board[row2][col2] === 'red')) {
    var jumpRow = row2+1;
    console.log(jumpRow);
    var jumpCol = 0;
      if (col2 < col1) { jumpCol = col2 - 1 } else {jumpCol = col2 + 1};
    console.log(jumpCol);
    board[row1][col1] = ' X ';
    board[row2][col2] = ' X ';
    board[jumpRow][jumpCol] = currentPlayer;
    currentPlayer = 'red';
    console.log("Movement made! Opponent jumped! Now it's " + currentPlayer + "'s turn.")
  }
  // Red player execution block. First is movement to empty space. Second is movement for jumping a white piece.
    else if ((currentPlayer === 'red') && (board[row2][col2] === ' X ')) {
    board[row2][col2] = currentPlayer;
    board[row1][col1] = ' X ';
    currentPlayer = 'wht';
    console.log("Movement made! Now it's " + currentPlayer + "'s turn.")
  } else if ((currentPlayer === 'red') && (board[row2][col2] === 'wht')) {
    var jumpRow = row2-1;
    console.log(jumpRow);
    var jumpCol = 0;
      if (col2 < col1) { jumpCol = col2 - 1 } else {jumpCol = col2 + 1};
    board[row1][col1] = ' X ';
    board[row2][col2] = ' X ';
    board[jumpRow][jumpCol] = currentPlayer;
    currentPlayer = 'wht';
    console.log("Movement made! Opponent jumped! Now it's " + currentPlayer + "'s turn.")
  } else {
    console.log("Something went wrong :(");
  }
  displayBoard();
};

var removePiece = function (row, col) {
  // Remove piece at the gievn coordinate. Helper function called by attemptMove

};
{% endhighlight %}

I knew I was going to need to refactor this heavily. After speaking to one of my instructors he told me to trust him and try making a lot of little functions that I could pass around to different parts of my program. I agreed and started re-writing my game engine, and ended up with this.

 {% highlight javascript %}
// Red Player
//Is red player moving forward?
var checkRedMove = function (row1, col1, row2, col2) {
  if (board[row1][col1] === 'RK') {
    return true
  } else {
    return (row1 > row2)
  }
};

var redMove = function(row1, col1, row2, col2){
  movePiece(row1, col1, row2, col2); // sets the destination to be the red piece
  removePiece(row1, col1, row2, col2); // removes the red piece from its existing coordinates
  displayBoard();
  console.log("Successfully moved " + currentPlayer + "'s piece.");
  currentPlayer = 'wht'; // sets the game to be white's turn next
  console.log("It's " + currentPlayer + "'s turn.");

};


// Move Functions
var movePiece = function (row1, col1, row2, col2) {
  board[row2][col2] = board[row1][col1];
  console.log(board[row2][col2]);
  kingPiece();
};

var removePiece = function (row1, col1, row2, col2) {
  // Remove piece at the gievn coordinate. Helper function called by attemptMove
  board[row1][col1] = ' . ';
};
{% endhighlight %}

One is definitely a little more readable and digestible than the other, wouldn’t you agree?

These are the kinds of reasons why I knew I needed to attend a bootcamp program. The problem with learning on your own is that what you don’t know, is what you don’t know. You can gain a basic understanding of how all the pieces work, but without a mentor guiding you on how to best use them together, you’re going to be rebuilding your work over and over again until you figure out.
