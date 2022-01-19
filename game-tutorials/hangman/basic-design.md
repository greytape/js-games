This gives a basic run through of the code in the hangman game. It's not a step-by-step tutorial (although I could adapt it into one).

The basic logic of the game runs whenever the user presses a key.

### Constants

At the top of the file you'll see a couple of constants.

```js
const words = [
  "acid",
  "blade",
  "country",
  // lots of other words...
]
const alphabet = ["A","B","C","D","E","F","G","H","I","J","K","L","M","N","O","P","Q","R","S","T","U","V","W","X","Y","Z"];
```

These are really important to the logic of the game. The purpose of the word list is pretty clear. We need to randomly choose one of the words from this list when we start the game.

The list of letters in the alphabet is perhaps less obvious. We need a list of all the letters in the alphabet to check that the user has pressed a letter key, rather than some other irrelevant key (eg `7` or `Ctrl`).

### Variables

Also at the top of the file, we declare a number of variables. You can think of this as the information that the game needs to keep track of in order to carry out its logic. 

```js
let word = null;
let currentGuessedLetters = [];
let currentWordContainer = null;
let imageContainer = null;
let guessedLettersContainer = null;
let incorrectGuessNumber = 0;
```

Three of the variables are HTML elements (`currentWordContainer`, `imageContainer`, `guessedLettersContainer`). These are parts of the page that we might need to update each time the player makes a guess.

The other three are key data, which we use to answer questions like:

- what is the current word the user is trying to guess?
- which letters has the player guessed already?
- how many incorrect guesses has the player used so far?

### Starting the game

When the page loads, we run the following code. This uses the `getElementById` method to find the HTML elements that we're going to be updating, and then assign them to the variables that we declared at the top of the file.

```js
document.addEventListener('DOMContentLoaded', () => {
  currentWordContainer = document.getElementById('current-word-container');
  imageContainer = document.getElementById('image-container');
  guessedLettersContainer = document.getElementById('guessed-letters-container');
  document.addEventListener('keydown', newGuess);
  startNewGame();
});
```

We then call the function `startNewGame`.

```js
function startNewGame() {
  word = getNewWord().toUpperCase();
  currentGuessedLetters = [];
  incorrectGuessNumber = 0;
  renderContent();
};
```

Here we are resetting all the data from previous games. We empty the array of `currentGuessedLetters`, we choose a new word from the list etc.

We then call the `renderContent` function. This updates the HTML elements (the various `containers`) with the current state of the game. We will call `renderContent` after each guess the user makes. 

```js
function renderContent() {
  currentWordContainer.textContent = wordWithGuessedLetters().join(' ');
  guessedLettersContainer.textContent = incorrectlyGuessedLetters().join(' ');
  setLatestImage();
}
```

### Making a guess

The core logic of the game is triggered whenever a user makes a guess by pressing a key.

You can see this in the code below. We're adding an `EventListener` to the page, so that whenever a user presses a key, it invokes the `newGuess` function.

```js
document.addEventListener('keydown', newGuess);
```

Let's have a look at `newGuess`. 

```js
function newGuess(event) {
  key = event.key.toUpperCase();
  if (invalidKey(key)) return null;

  currentGuessedLetters.push(key);
  checkLatestGuess(key);
  renderContent();
  checkEndGameConditions();
}
```

- We first convert the key to a capital letter (it easier for us to compare these letters against previous guesses if they are all in the same case.). 
- We then check that the key that has been pressed is a valid one (eg a letter key) using the `invalidKey` function. 
- We add the new guessed letter to `currentGuessedLetters` array.
- We `checkLatestGuess` to see whether it is a correct guess or not.
- We `renderContent` to update the page with the new information we have received.
- We check to see if we can end the game yet with `checkEndGameConditions`.

This is a quick overview of the code. Keep digging to see if you can track down each part of it, until you fully understand how the game works.