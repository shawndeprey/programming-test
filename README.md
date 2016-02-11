Consider the following models. Assume these models exist in a PostgreSQL database with the ORM ActiveRecord siting on top.

Company
  - id
  - name (string)
  - description (text)
  - logo_url (string)
  - score (int)
  - has many funding rounds

FundingRound
  - id (int)
  - funding_in_usd (float)
  - company_id

1) When using an ORM what techniques could you use to ensure the fastest possible lookup time of the above models and the relationships between those models?

Example Answers
- First and foremost you will want to index the relationships between the models in SQL. This will ensure that SQL can quickly lookup a model based on that relational attribute. Since ORM libraries make SQL queries on your behalf, usually there are more queries happening than you might think. Having relational attributes indexed ensures this background operation isn't spinning too much, increasing your overall response times.
- Next you will want to consider other attribute you expect to be looking up data on such as the attribute name on Company. You will want to index this data in SQL as well to ensure queries on this attribute match as quickly as possible.
- When using ActiveRecord, you can force the ORM to combine queries to save on overall lookup times using the .includes functionality. For instance, in the above example the model Company has_many FundingRounds. When looking up a company you can eager load that relationship to perform it in a single query vs. multiple queries when looping through the list later.
- Moving beyond SQL, you can also use some kind of in-memory indexing system such as ElasticSearch. Being that ElasticSearch is optimized for super fast lookup queries, you can perform fairly complex queries in much less time than those queries would take in SQL. So if you really needed a lookup time decrease you could push your lookup data into ElasticSearch. Like anything, you should use the right tool for the right job, so in-memory indexing isn't always the answer, but it's certainly a powerful tool to help decrease your overall load times.

2) Design a RESTful JSON CRUD API for the 2 models above. Include expected input, what the controller might look like as well as the HTTP response code & JSON response object. Be sure to include how to obtain a list of each model type. Assume there are no security considerations expected in the solution. An example is provided below to give context as to what we are expecting in this question.

GET /funding_rounds/:id
  Controller Process:
    - Lookup Model, 404 on fail
    - Render Model as JSON, 200 success
  Input Example:
    - {id: 123}
  Output Example:
    - ```javascript
    {
      funding_round: {
        id: 123,
        funding_in_usd: 123.25,
        company_id: 321
      }
    }
    ```


3) Using HTML & CSS, take the following mock and implement a list of company models. The JSON output you have on the front-end should be the JSON output for the `/companies` API route you created in question 2. You are not expected to be tying data in javascript objects to HTML elements in this question. You are only expected to create an HTML file that implements the mocks.

Mock:
https://s3.amazonaws.com/ryse-static/programming-test/mock.png

Company Logo Examples:
https://s3.amazonaws.com/ryse-static/programming-test/facebook.png
https://s3.amazonaws.com/ryse-static/programming-test/google.png
https://s3.amazonaws.com/ryse-static/programming-test/reelio.png
https://s3.amazonaws.com/ryse-static/programming-test/twitter.jpg

Typography:
font-family: Open Sans, sans-serif;
color: #727272;

Background:
background-color: #f1f1f1;