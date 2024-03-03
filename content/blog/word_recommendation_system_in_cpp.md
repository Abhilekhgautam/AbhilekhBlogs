+++
title = "Writing a Word Recommendation system in C++"
date = 2024-01-14
+++

I always wondered, how my search engine most of the time exactly guessed what
I wanted to search by looking at the few words, I type in the search bar.
In this article I'd try to implement similar feature myself in C++.

## Objective
To guess next word(s) of a given word in a sentence.

## How?
First up we'll need lots of data, yup a lot of data to make correct predictions. But we will just work with small amount of dummy data.

We'll work by using a very well known algorithm that works on the basis of probability and it is 

#### The Naive-Bayes Algorithm
Let's say we have two sentences in our data.

* I love you my sweet heart.
* I love you my maiyya.

Now what we'll do is just calculate the probability of occurence of each words after the given word.

If we have to guess the word(s) after the word 'I' we have to calculate the probability of occurence of every word that occurred after 'I' and the one above certain threshold is recommended.

For our above data, the probability of occurence of the word "love" after the word "I" is 100% because it appears every time after the word "I", so the word "love" will be recommended by our system. But we won't stop there because we want to guess the entire sentence, so we'll again then calculate the probability of occurence of every word after the word "love", in this case the word "you" has the highest probability (100%), we will continue this same process until we don't find the word having probability greater or equal to certain threshold value.

In this case, "love you my" will be recommended by the system if we typed in the word "I".

### The Data Structure Setup
#### Requirement

We need to keep track of words that appear after every word and their frequency as well.

For every new word we encounter in our data set, we assign a `std::unordered_map<std::string, int>` that holds the frequencies of the next word appearing after that word which is stored in a `std::vector`. The index of the `std::vector` is stored in another `std::unordered_map<std::string, int>` where the key is the word we encounter and value is the index.

### Implementation
Assuming we have our data set in the file `data.txt` which is pre-processed as per our requirement.

We start by defining a `struct` 
```cpp
struct Recommendation{
 std::string word;
 float probability;
};
```
and then we also need some globals,
```cpp
std::unordered_map<std::string, long> Index;
std::vector<std::unordered_map<std::string, long>> LookUp;
```
Now we can start defining our function `RecommendeWord`. This function
takes a single argument, a word whose preceeding word(s) is to predicted.

```cpp
Recommendation RecommendWord(const std::string& word){
    auto Found = Index.find(word);
    if(Found == Index.end()) return Recommendation{};
    else{
        auto LookUpIndex = Index[word];
	    int TotalOccurence{};
        for(const auto& [word, count]: LookUp[LookUpIndex]){
          TotalOccurence += count;  
        }
     	for(const auto& [word, count]: LookUp[LookUpIndex]){
       	  float Probability = (float)count / (float)TotalOccurence;
	      if(Probability >= PROBABILITY_THRESHOLD){
	       return Recommendation{word, Probability}; 
	      }
	    }
        return Recommendation{};
      }
}
```
We start by checking if we have the input word in our data set, if not we exit early.
If the word is found, we will take its lookup index for the `LookUp` vector.
