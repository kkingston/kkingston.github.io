---
layout: post
title:      "A Game of Fetch"
date:       2020-10-26 16:50:17 +0000
permalink:  a_game_of_fetch
---



Using fetch in javascript can be quite a useful tool to use while using data from an API.  It can also be quite confusing if you don't quite understand the process or the meanings behind each action that's going on.

I will brush on my current project for our examples. The idea is that we are using a rails API backend to communicate with our frontend using javascript. The project implements a random Portuguese word generator, and the user is able to login and create a journal entry for that particular word. This was an idea for the user to build their vocabulary in a simple way, and can open conversations about that word if you have friends or family who speak that language.

So, back to fetch. What is a fetch request anyways? Or how will we be using it in our application?

To begin with we have three models in our database schema. Words, Entries, and Users. With our rails API we will seed our database with a few premade users, entries, and words. In this article I will not go over how to set up your rails API, but if you need some help you can check this article out: https://medium.com/datadriveninvestor/create-a-rails-api-from-scratch-fa81bcba5422

A fetch request basically is what's allowing you to talk with your database. It's like if your dog had a collection of toys in their house, and you wanted the red ball, so, you send your dog to his house to fetch the red ball for you.
Only in this case we have a dog house full of Portuguese words.. and we'd like to access them on our frontend for easier use.


` 
document.addEventListener('DOMContentLoaded', callOnLoad);

  //to DOM contentLoaded
  function callOnLoad(){
    Word.fetchWords();
    Word.wordWrapper().addEventListener('click', Word.randomWord);
  }

const baseURL = 'http://localhost:3000/api/v1'
Word.wordsArray = [];

class Word {

static fetchWords() {
    fetch(`${baseURL}/words`)
    .then(resp => resp.json())
    .then(words => {
      words.forEach(word => {
        let newWord = new Word(word)
        Word.wordsArray.push(newWord)
      })
      Word.randomWord();
    })
  }
	
static randomWord() {
    const currentWordIndex = Math.floor(Math.random() * Word.wordsArray.length)
    Word.wordDisplay().innerText = Word.wordsArray[currentWordIndex].word
    Word.posDisplay().innerText = 'Part of Speech: ' + Word.wordsArray[currentWordIndex].pos
    Word.definitionDisplay().innerText = 'Definition: ' + Word.wordsArray[currentWordIndex].definition
    Word.entryWord().innerText = Word.wordsArray[currentWordIndex].word
    Word.wordId().innerText = Word.wordsArray[currentWordIndex].id
  }
	}
	`

If you take a look, you'll see that we have two variables. An empty array, and a URL. We also have a fetchWords function and a randomWord function and a couple different event listeners. For our fetch to work, we need an endpoint URL, this is where we are sending our dog to get his red ball.

Because I know that I am going to use this same endpoint for different kinds of fetches it is easier to assign it to a variable called baseURL . Because of this, we have the benefit to interpolate the given model that we want our endpoint to go to. So, if we look at our `fetchWords` function the line `${baseURL}/words` will be the exact same as http://localhost:3000/api/v1/words. (this is our endpoint, plus the model that we are trying to access)

But because we're grabbing our words from an API, things aren't formatted how we want them quite yet.. As of now, all of our data is formatted in JSON but because in our frontend we are using javascript we need to translate that JSON into a javascript object. So, if we were to look at our `fetchWords` function again you'll see we are taking our response and we're going to parse that into a JSON using the .json() function.  
That line will look like this: ` .then(resp => resp.json())`

Here is an example of what that data will look like parsed into JSON:
`[
  {
    "id": 9,
    "word": "Sol",
    "definition": "Estrela em torno da qual gira a Terra. Sun",
    "pos": "noun"
  },
  {
    "id": 10,
    "word": "Palavra",
    "definition": "Letra ou conjunto de letras com sentido. Word",
    "pos": "noun"
  },
  {
    "id": 11,
    "word": "Pêssego",
    "definition": "Fruto de pele aveludada com caroço. Fuzzy fruit that is good in pies. Peach",
    "pos": "noun"
  }
	]
	`

Next, we are going to take that javascript object and do something with it.  We want to take that JSON and instanciate all of those words into javascript objects, so that we can use it later on in our project.  

`  .then(words => {
      words.forEach(word => {
        let newWord = new Word(word)
        Word.wordsArray.push(newWord)`
				
What we are doing here is taking all the words from our response (formatted like the example above) and for each individual word we want to create an instance of that word.  Then we are taking our `wordsArray` and pushing each new word into that array.  Now we are able to access that data in our program for later use!

`Word.wordWrapper().addEventListener('click', Word.randomWord);`

If you look at our code snippet above, you'll see that we have an event listener.  Once all our words have been fetched our event listener will wait for someone to come along and click the 'new word button'.  Once that button has been clicked the event listener has been triggered and will move along to our `randomWord` function.

Our complete fetch and use of an event listener is complete!  If you have any additions, edits, or questions please let me know since I am still learning as well :)  



