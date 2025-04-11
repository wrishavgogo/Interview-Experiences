Phone Pe
Code Signal Rounds discussion , how i coded and they want to understand my soln
Will note down the experience here 


So the interviewer asked me to explain the code , gave a few test cases and asked to explain the complexity 


        word1 , word2 , word3 , word4

                word1 -> { word1 , word2 , word3 , word4},

        word -> 50 * 2 bytes = 100B
            1 M =10 ^6 *2 * 100 b , 2 * 10 ^ 8 => 0.2 gb


            0.2 * 10^9 * 1O^6 = 0.2 PB

            Max 36 GB
            key ->
                    kEY -> VALUE
                    word1 -> locked



        { 22 to 25 qps }


        { word1 -> word2 }  hello -> hey
                            hey -> earth

        word -> synonym list


  Interviewer asked me in case of production systems what database will you use and why and what schema will you use ? 

  Here i did i mistake of storing 

  word1 -> word2 as a key value pair 
  correct answer is storing the adjacency list 
  word1 -> { wordi , wordj , wordk , ..... } 
  word2 -> { word1 , word3 , .....  } 

  so here i recommended no sql db 
  I did the size estimation wrong , 
  the size estimation is like 

  i assumed word 1 - > word2 mapping 
  total number of words = 10^6 , 

  i assumed each word = 50 character , 2 bytes for each character  = 50 * 2 = 100 bytes 
  100 bytes * 10 ^ 6 * 2 , i assumed this as the size 

  But in reality size if 
  n*n , each word can have mapping with each word

  total number of words in 1 row can be N ,
  each key is mapped to each word in a row 
  N word -> { n other words} 
  like that for all words

  I row has N words , *N 
  N^2 , 
  ( 10^6 * 10 ^6 ) * 100 Bytes , that about peta bytes 

  


  Things that i have to learn .

  1. I have to learn about aeroSpike db , how it is a NOsql and support very fast operations due to SSDs
  2. PostGreSql , how it supports ACID properties  , different isolation properties
       Read_Committed , Read_Uncommitted, etc etc


One more thing i got to know abt PhonePe , 
they have started blinkit like services , they are delivering grocery item , but assigning delivery partners part they have outsourced it to 
ShadowFax 
ShadowFax is a company like porter , parcel service 






  Things i need to learn here 
