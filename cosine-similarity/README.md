#Cosine Similarity

A node implementation of the Cosine Similarity algorithm (for finding text similarity) with some added functionality for filtering and querying. Based on the [cosine](https://www.npmjs.org/package/cosine) module but with some sexier stuff.

The Cosine Similarity module is optimized for finding similarity between an input string against a large array of custom objects like those saved in a document-oriented databases (like CouchDB or MongoDB). 

#Example

`CosineSimilarity.findSimilar(input, instructions)` returns the `corpus` array with its elements re-ordered by score with the first element being most similar to `input`.

```
/* Corpus must be an array of custom objects.
[
	{
	   "date": "2011-03-12 11:56:29", 
	   "to": "Me",
	   "from": "Heather Parker", 
	   "text": "yeah, I feel a LOT better"
  	}, etc...
] 
*/

var CosineSimilarity = require('cosine-similarity'),
fs = require('fs');

//load corpus file, or make a query to a document-style database
fs.readFile('corpus.json', function(err, data){
		
	//array of objects
	var corpus = JSON.parse(data);
	var cosineSimilarity = new CosineSimilarity(corpus);

	var input = 'What time is the show?';

	//returns an array of all results ordered by a score property
	var similar = cosineSimilarity.findSimilar(input, { use: "text" });

	//logs the "text" property from where is most similar to "What time is the show?" 
	console.log(similar[0].text);
}
```

#Filters

The second parameter passed into `findSimilar()` is an `instructions` object that gives the user added control with custom queries. Use the `scoreOnly`, `dontScore`, `preference`, `minScore`, and `limit` properties to build an elaborate query.

```
/* Corpus structure...
	{
	   "date": "2011-03-12 11:56:29", 
	   "to": "Me",
	   "from": "Heather Parker", 
	   "text": "yeah, I feel a LOT better"
  	} 
*/

var CosineSimilarity = require('cosine-similarity.js'),
fs = require('fs');

//load corpus file, or make a query to a document-style database
fs.readFile('corpus.json', function(err, data){
		
	//array of objects
	var corpus = JSON.parse(data);
	var cosineSimilarity = new CosineSimilarity(corpus);

	var input = 'What time is the show?';

	//returns an array of all results ordered by a score property
	var similar = cosineSimilarity.findSimilar(input, {
		use: text, //specifies which property to run the cosine similarity algorithm on
		onlyScore: { //only score items that 
			to: "Brannon Dorsey"  //can be single value
			from: ["Barney", "Thomas Waits", "Albert Grothsburnger"] //or an array of possible values
		},
		minScore: .66,
		limit: 15
	}

	//logs the 15 most similar text messages to "What time is the show?"
	//from Barney, Thomas Waits, or Albert Grothsburnger to Brannon Dorsey that 
	//have a similarity score of .66 or higher
	for (var result in similar) {
		console.log(similar[result].text);
	}
}
```
Below is a complete list of all `instruction` properties.

###use property (required) 

__Type__: `string`

The `use` value tells the program which property to score against `input`.

###minScore

__Type__: `int`

Only returns results with a score greater than or equal to the value of `minScore`.

###limit

__Type__: `int`

Limits the number of results returned. 

###onlyScore

__Type__: `Object`

`onlyScore` is a custom object with properties that __you define__. These properties must match some properties present in the objects that populate your corpus array. The values of these properties tell the program to __only__ score objects if their specified property (which again, you define) matches the string (or regex) defined by the property name.

The values of the `onlyScore` object's properties can be a string or an array of strings. Here is an example:

```javascript
onlyScore {
	date: "2011-03-12*",
	to: "Brannon Dorsey"
}
```

This `onlyScore` object would only score messages sent on February 12, 2011 __and__ (not or) any messages to Brannon Dorsey and all other message objects would receive a score default of `0`.

###dontScore

__Type__: `Object`

`dontScore` is the opposite of `onlyScore`. The object uses the values of the properties that you define to give any matches in your corpus an automatic score of `0`.

```javascript
dontScore {
	date: "2011-03-12*",
	to: "Brannon Dorsey"
}
```
This `dontScore` object will automatically give any messages sent or received on February 12, 2011 __and__ (not or) all messages to Brannon Dorsey a score of `0`.

##Other methods

##License
MIT

