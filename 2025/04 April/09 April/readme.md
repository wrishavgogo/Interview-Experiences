Phone pe machine coding round 

I was given a question to code on machine coding round , and on 11th there will be evaluations on my code 

Question :

Parallel System
It’s a system that is able to give all the similar sentences for a given sentence.

The system is given synonym word pairs, and at any point the system can be asked for all the synonym sentences for a given sentence. The system is expected to give these sentences, as fast as possible.

Problem Statement
You need to design a system that is able to give synonym sentences.
There are 3 parts to this problem statement.
addSynonymPair, deleteSynonymPair and getSentences.
When a synonym is added or deleted, you will be provided word pairs that are synonym of each other. When sentences are being retrieved, you will be provided with a sentence, for which similar sentences need to be returned.

The following is a guide for the interfaces you may have in your system. Your code might not have the same function syntax and parameters, but it should be capable of providing all the below requirements.

1- An interface to add synonym pair of words.

void addSynonymPair (String word1, String word2);
where,
word1 = first word from the synonym pair
word2 = second word from the synonym pair

2- An interface to remove synonym pairs of words.

void removeSynonymPair (String word1, String word2)
where,
word1 = first word from the synonym pair.
word2 = second word from the synonym pair.

3- An interface to get similar sentences.

List<String> getSentences(String sentence)
where,
sentence = sentence for which unique similar sentences need to be returned in lexicographically sorted manner.

Sample
The following is just a sample for your understanding.
Please remember: You are expected to write the system which mirrors production quality code, rather than just implementing these functions

Examples for add/remove synonyms and getting sentences

addSynonymPair("hello", "hey"); 
addSynonymPair("world", "earth"); 
addSynonymPair("planet", "earth");
addSynonymPair("planet", "planet");


getSentences("hello world")   
Output: 
hello earth
hello planet
hey earth
hey planet
hey world

getSentences("world earth")
Output:
earth earth
earth planet
earth world
planet earth
planet planet
planet world
world world
world planet

removeSynonymPair("planet", "earth");

getSentences("hello world")
Output: 
hello earth
hey earth
hey world
Requirements P0
Implement the above with appropriate assumptions for the example shown above.
Optimize your solution for time/space complexity taking reasonable tradeoffs.
You can simulate the operations of adding synonyms and getting similar sentences as shown in the sample, using a main function or test cases
You should have a working code that demonstrates the same.
Handle error scenarios appropriately
Requirements P1
Handle concurrency cases for different scenarios.
Black list a word, in this case all the synonym pairs involving that word will become inactive.
Things to keep in mind
You are only allowed to use in-memory data structures.
You are NOT allowed to use any databases.
You are NOT required to have a full-fledged web service or APIs exposed.
A working code is ABSOLUTELY NECESSARY. Evaluation will not be done if your code is not running. So ensure you time yourselves accordingly.
You are required to showcase the working of the above concept.
Just a main class that simulates the above operations is enough.
Should you have any doubts, you are allowed to make appropriate assumptions, as long as you can explain them during the evaluation.
You are allowed to code on your favorite IDEs as long as you paste the code back into the tool within the allotted time frame
How you will be evaluated
You are expected to write production quality code while implementing the requirements.
We look for the following:

Separation of concerns
Abstractions
Application of OO design principles
Testability
Code readability
Language proficiency
[execution time limit] 3 seconds (java)

[memory limit] 2g




My Soln : 

package com.example.machinecoding;
import java.io.*;
import java.util.concurrent.ConcurrentHashMap;
import java.util.*;

import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.core.JsonProcessingException;
import io.micrometer.common.util.StringUtils;

// Main class should be named 'Solution' and should not be public.
public class Solution {

    Map<String , Set<String>> wordToSynonymSetMap = new ConcurrentHashMap<>();
    SynonymAddingStrategy synonymAddingStrategy;
    SynonymRemovingStrategy synonymRemovingStrategy;
    GetSentenceStrategy getSentenceStrategy;
    SynonymHelper synonymHelper;
    final String SEPARATOR = " ";
    public static void main(String[] args) {



        Solution solution = new Solution();

        solution.synonymAddingStrategy.addSynonymPair("hello", "hey");
        solution.synonymAddingStrategy.addSynonymPair("world", "earth");
        solution.synonymAddingStrategy.addSynonymPair("planet", "earth");
        solution.synonymAddingStrategy.addSynonymPair("planet", "planet");

        List<String> ans = solution.getSentenceStrategy.getSentences("hello world");
        ans.stream().forEach(System.out::println);

        ans = solution.getSentenceStrategy.getSentences("world earth");
        ans.stream().forEach(System.out::println);

        solution.synonymRemovingStrategy.removeSynonymPair("planet", "earth");


        ans = solution.getSentenceStrategy.getSentences("hello world");
        ans.stream().forEach(System.out::println);






    }

    public Solution() {

        this.synonymAddingStrategy = new CrossJoinSynonymAddingStrategy();
        this.synonymRemovingStrategy = new CrossJoinSynonymRemovingStrategy();
        this.synonymHelper = new BFSSynonymHelper();
        this.getSentenceStrategy = new CartesianProductGetSentenceStrategy(synonymHelper);
    }





    /**
     1. Following interface Segration principle here
     to allow flexibility of implementation of any method as and when required

     2. following strategy design pattern
     as in future adding of synonym might happen in a different way

     */
    interface SynonymAddingStrategy {
        public void addSynonymPair (String word1, String word2);
    }

    interface SynonymRemovingStrategy {
        public void removeSynonymPair (String word1, String word2);
    }

    interface GetSentenceStrategy {
        List<String> getSentences(String sentence);
    }

    class CrossJoinSynonymAddingStrategy implements SynonymAddingStrategy {

        @Override
        public void addSynonymPair (String word1, String word2) {


            if(word1.equals(word2)) {
                return ;
            }

            // creating bidirectionalGraph

            wordToSynonymSetMap.computeIfAbsent(word1 , (String key) -> new HashSet<>());
            wordToSynonymSetMap.get(word1).add(word2);

            wordToSynonymSetMap.computeIfAbsent(word2 , (String key) -> new HashSet<>());
            wordToSynonymSetMap.get(word2).add(word1);
        }
    }

    class CrossJoinSynonymRemovingStrategy implements SynonymRemovingStrategy {

        @Override
        public void removeSynonymPair (String word1, String word2) {

            if(wordToSynonymSetMap.containsKey(word1)) {
                wordToSynonymSetMap.get(word1).remove(word2);
            }

            if(wordToSynonymSetMap.containsKey(word2)) {
                wordToSynonymSetMap.get(word2).remove(word1);
            }

        }
    }

    class CartesianProductGetSentenceStrategy implements GetSentenceStrategy {


        SynonymHelper synonymHelper;

        public CartesianProductGetSentenceStrategy(SynonymHelper synonymHelper) {
            this.synonymHelper = synonymHelper;
        }
        Set<String> ans = new HashSet();

        @Override
        public List<String> getSentences(String sentence) {

            // flush previous answer
            ans.clear();
            /**
             Step1 -> extract each word from sentence
             Step2 -> get synonym set for each of the word
             Step3 -> do cartesian product of all the words in the synonym list
             Step4 -> after merging them return all the sentence except the one given in input


             */


            String[] words = sentence.split(" "); // get separation by space

            // for each word find its synonym set
            List<Set<String>> synonymSetList = new ArrayList<>();
            for(int i = 0 ; i < words.length ; i ++ ) {
                synonymSetList.add(synonymHelper.getSynonyms(words[i]));
            }



            generateSentences(0 , synonymSetList , "");

            ans.remove(sentence);


            List<String> sortedAnswer = ans.stream().toList();



            sortedAnswer = sortedAnswer.stream().sorted().toList();

            return sortedAnswer;
        }

        private void generateSentences(int position , List<Set<String>> synonymSetList , String sentence ) {

            int n = synonymSetList.size();

            if(position >= n ) {
                ans.add(sentence);
                return;
            }

            Set<String> synonymsForThisPosition = synonymSetList.get(position);

            for(Iterator it = synonymsForThisPosition.iterator() ; it.hasNext() ; ) {

                String sentence1 = sentence + it.next();

                if(position < n - 1) {
                    // last word no need to add space
                    sentence1 = sentence1 + SEPARATOR;
                }

                generateSentences(position + 1 , synonymSetList , sentence1);

            }




        }


    }

    interface SynonymHelper {

        public Set<String> getSynonyms(String word);

    }

    class BFSSynonymHelper implements SynonymHelper {

        // helps me get synonyms for the word using bfs over the connected component

        @Override
        public Set<String> getSynonyms(String word) {


            Set<String> synonymSet = new HashSet<>();
            Map<String , Integer > visited = new HashMap<>();

            Queue<String> q = new LinkedList<>();

            q.offer(word);
            visited.putIfAbsent(word , 1 );


            // doing a bfs to find all the nodes which are synonyms of each other
            while(!q.isEmpty()) {

                String node = q.poll();
                synonymSet.add(node);

                Set<String> childSet = wordToSynonymSetMap.getOrDefault(node , new HashSet<>());
                Iterator it = childSet.iterator();

                while(it.hasNext()) {
                    String child = (String) it.next();
                    if(visited.getOrDefault(child, 0) == 0 ) {
                        q.offer(child);
                        visited.put(child , 1);
                    }
                }
            }



            return synonymSet;
        }






    }
}
