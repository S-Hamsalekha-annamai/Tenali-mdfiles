# Existing feature improvements

# Improvement #1 4/6/26
percent-api/question : Populate and feed the values for tier and type correctly

Steps
1.For the percent-api/question api, at the client side, make required changes to populate and send  the following params to the server
(Take a look at how it is done /pythag-api/question. Use it as reference)
req.query.q : question number index
req.query.difficulty  : difficulty level (easy, medium, hard, extrahard)
req.query.first : '1' for the first questions of a type else '0'


2. Use the following logic to populate tier and type in the function /percent-api/question
  In the Non adaptive modes ( easy: 'Easy — Find %', medium: 'Medium — Increase/Decrease', hard: 'Hard — Reverse %', extrahard: 'Extra Hard — Compound')
    Type : ( 1: 'Easy — Find %', 2: 'Medium — Increase/Decrease', 3: 'Hard — Reverse %', 4: 'Extra Hard — Compound')
    Tier :( question index < 3 : Tier = 1 )
          ( 3 < question index < 6 : Tier = 2 )
          ( 6 < question index < 9 : Tier = 3 )
          ( question index >= 9  : Tier = 4 )

  In adaptive mode 
   Type : Rotates sequentially across all the 4 modes ( easy: 'Easy — Find %', medium: 'Medium — Increase/Decrease', hard: 'Hard — Reverse %', extrahard: 'Extra Hard — Compound')

   Tier  is derived based on difficulty level ( req.query.difficulty)
   Tier  = { easy: 1, medium: 2, hard: 3, extrahard: 4 };

3. update percent route comments and other relevant documentation with these changes.


# Improvements #2 4/6/26
PercentApp : Update the labels displayed on UI to reflect the new changes

1. In PercentApp, Update the labels for different types of questions (diffLabels) as follows
   easy: 'Easy — Find Percentage of a Number'
   medium: 'Medium — Express as Percentage'
   hard: 'Hard — Find the Whole'
   veryhard: 'Very Hard — find Percentage Change' 
   extrahard: 'Extra Hard — Shopping Discounts & Tax' 

2. On the server side , /percent-api/question , Update the mapping of these labels to "type" as follows

  In the non adaptive modes
   easy: type = 1
   medium: type = 2
   hard: type = 3
   veryhard:  type = 4 
   extrahard:  type = 5

   In the adaptive mode
   Rotate the questions across 5 types based on question index.


# Improvements #3 5/6/26
   SqrtApp : Add two more question types in the same quiz

1. SqrtApp : Send the question number to the server using req.query.q
2. In /sqrt-api/question,  Add two more question types 
Type 1  : Find the square root of the number X ( cieling or floor accepted) (Existing question)
Type 2  : Find the square root of a perfect square X (Hint : Heard of  prime factorization ?) (new)
Type 3  : Find a number when multiplied with X , makes X a perfect square. (new)
3. Based on question number , rotate across all three types.
4. Add this to all the modes(easy , medium , hard , extrahard)
5. In the case of type 2 , make sure the number is a perfect square. After you obtain the number , take the squareroot 
and then square it again. that makes it a perfect square.
6. In the case of type 3, make sure the number is neither perfect square nor prime.
7. Modify prompt of the question based on the type. Use prompts mentioned above.
8. Update the font size to match with other sections  while displaying prompts.


# Improvements #4 12/6/26
   SqrtApp : Ensure questions dont repeat

1. Modify /sqrt-api/question, to generate a question ID for every question. This ID should be based on final number ${q} used in the question .
   Use generatePercentQuestion as  reference to see how question ID can be generated. Send this to the client along with the question.
2. At the client side, maintain a list of  all the question ID's received. Keep the apis and the logic similar to  that in function  '/percent-api/question . 
3. Use the req.query.seen parameter to send the list of seen qids to the server along with question api.
4.  At the server side,in /sqrt-api/question , After every question is generated , make sure its not repeated by checking against the list previous questions ID's 
given by the client (upto 50). Use /percent-api/question as reference.
5. In '/sqrt-api/question', Modify the prompt in Type 3  to "`Which is the smallest number to be multiplied with ${q} to make it a perfect square`"
6. In '/sqrt-api/question' , for questions of type 3 , add another check to ensure the number in question has repeated prime factors.


# Improvements #5 12/6/26
SqrtApp : Make the UI keyboard friendly
1.In the SqrtApp (App.jsx), Update the UI so that the Solution box automatically receives focus whenever a question is displayed. Users should be able to start typing their answer immediately without needing to click inside the input field.