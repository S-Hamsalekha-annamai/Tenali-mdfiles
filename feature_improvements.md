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