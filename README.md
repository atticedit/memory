	 	 	
Model:

-------Game---------

player --- String

numCards --- Number

card --- [Number]

gameTime --- Number

--------------------


don't need id because it's generated for us.

Thought we wanted these:

createdAt --- Date

completedAt --- Date

until we realized we could just use a timer

--------------------------------------------------------------------------------

	 	 	
GET '/' games.index

player inputs name and # of cards and clicks start

POST '/games/start' games.create
[URL will have a query string]

server:
Game object is created

browser:
- input for cards (but not player name – might want to play again and not enter it again) is cleared out
- all card children removed from parent div (meaningful once it’s run more than once between refreshes)
- clock starts counting up in seconds
- cards appear


--------------------------------------------------------------------------------

player clicks a card

GET '/games/:id' games.show

browser:
- ajax request sends card's data-position (which is the card's position on the page, left-to-right, top-to-bottom, starting at 0)

server:
- sends browser back the string in that position

browser (after just 1 card clicked):
- add a class of guess
- set the text of the div to what that number is, which:
- flips card

browser (after determining 2 cards have been clicked by using the length of elements with class of guess):
- add a timer so face of both cards remains visible for a second (Mike suggests set timeout)
- compares the text of cards with class of guess (there should be 2)

if not a match:
- removes guess class and text from both cards, which
   - visually flips both cards back over (because face of card is just back with number visible)

if a match:
- adds class of correct to both cards (so pseudoclass of :not on click event can prevent any action on click), which:
- keeps both card faces visible, possibly with different color background
- remove class of guess

--------------------------------------------------------------------------------

if all cards but 2 (because we're not making the player click the last 2 that inevitably match) have been matched; triggered when length of elements with a class of correct is equal to numCards - 2):


PUT '/games/:id' games.complete

server:
- update database property of gameTime with minutes and seconds from timer

browser:
- receive object
- display “you matched __ cards in ___ amount of time. Enter number of cards and hit start game to play again”

