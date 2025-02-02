# Backend - Trivia API

## Setting up the Backend

### Install Dependencies

1. **Python 3.7** - Follow instructions to install the latest version of python for your platform in the [python docs](https://docs.python.org/3/using/unix.html#getting-and-installing-the-latest-version-of-python)

2. **Virtual Environment** - We recommend working within a virtual environment whenever using Python for projects. This keeps your dependencies for each project separate and organized. Instructions for setting up a virual environment for your platform can be found in the [python docs](https://packaging.python.org/guides/installing-using-pip-and-virtual-environments/)

3. **PIP Dependencies** - Once your virtual environment is setup and running, install the required dependencies by navigating to the `/backend` directory and running:

```bash
pip install -r requirements.txt
```

#### Key Pip Dependencies

- [Flask](http://flask.pocoo.org/) is a lightweight backend microservices framework. Flask is required to handle requests and responses.

- [SQLAlchemy](https://www.sqlalchemy.org/) is the Python SQL toolkit and ORM we'll use to handle the lightweight SQL database. You'll primarily work in `app.py`and can reference `models.py`.

- [Flask-CORS](https://flask-cors.readthedocs.io/en/latest/#) is the extension we'll use to handle cross-origin requests from our frontend server.

### Set up the Database

With Postgres running, create a `trivia` database:

```bash
createbd trivia
```

Populate the database using the `trivia.psql` file provided. From the `backend` folder in terminal run:

```bash
psql trivia < trivia.psql
```
`
### Run the Server

From within the `./src` directory first ensure you are working using your created virtual environment.

To run the server, execute:

```bash
flask run --reload
```

The `--reload` flag will detect file changes and restart the server automatically.

## To Do Tasks

These are the files you'd want to edit in the backend:

1. `backend/flaskr/__init__.py`
2. `backend/test_flaskr.py`

One note before you delve into your tasks: for each endpoint, you are expected to define the endpoint and response data. The frontend will be a plentiful resource because it is set up to expect certain endpoints and response data formats already. You should feel free to specify endpoints in your own way; if you do so, make sure to update the frontend or you will get some unexpected behavior.

1. Use Flask-CORS to enable cross-domain requests and set response headers.
2. Create an endpoint to handle `GET` requests for questions, including pagination (every 10 questions). This endpoint should return a list of questions, number of total questions, current category, categories.
3. Create an endpoint to handle `GET` requests for all available categories.
4. Create an endpoint to `DELETE` a question using a question `ID`.
5. Create an endpoint to `POST` a new question, which will require the question and answer text, category, and difficulty score.
6. Create a `POST` endpoint to get questions based on category.
7. Create a `POST` endpoint to get questions based on a search term. It should return any questions for whom the search term is a substring of the question.
8. Create a `POST` endpoint to get questions to play the quiz. This endpoint should take a category and previous question parameters and return a random questions within the given category, if provided, and that is not one of the previous questions.
9. Create error handlers for all expected errors including 400, 404, 422, and 500.

## Documenting your Endpoints

You will need to provide detailed documentation of your API endpoints including the URL, request parameters, and the response body. Use the example below as a reference.

### Documentation Example

`GET '/api/v1.0/categories'`

- Fetches a dictionary of categories in which the keys are the ids and the value is the corresponding string of the category
- Request Arguments: None
- Returns: An object with a single key, `categories`, that contains an object of `id: category_string` key: value pairs.

```json
{
  "1": "Science",
  "2": "Art",
  "3": "Geography",
  "4": "History",
  "5": "Entertainment",
  "6": "Sports"
}
```
`GET '/categories'`

Fetches a dictionary of categories in which the keys are the ids and the value is the corresponding string of the category

Request Arguments: None
Returns: An object with a single key, categories, that contains an object of id: category_string key:value pairs.
Example: curl http://localhost:5000/categories

{
	'1' : "Science",
	'2' : "Art",
	'3' : "Geography",
	'4' : "History",
	'5' : "Entertainment",
	'6' : "Sports"
}

`GET '/questions'`
Fetches a dictionary of questions, paginated in groups of 10.
Returns JSON object of categories, questions dictionary with answer, category, difficulty, id and question.
Example: curl http://localhost:5000/questions

{
    "categories": [
        "Science",
        "Art",
        "Geography",
        "History",
        "Entertainment",
        "Sports"
    ],
    "current_category": [],
    "questions": [
        {
            "answer": "Apollo 13",
            "category": 5,
            "difficulty": 4,
            "id": 2,
            "question": "What movie earned Tom Hanks his third straight Oscar nomination, in 1996?"
        }
        ... # omitted for brevity 
    ],
    "success": true,
    "total_questions": 33
}

`DELETE '/questions/int:question_id'`

Deletes selected question by id
Returns 200 if question is successfully deleted.
Returns 404 if question did not exist
Returns JSON object of deleted id, remaining questions, and length of total questions
Example: curl -X DELETE http://localhost:5000/question/2

{
    "deleted": 2,
    "questions": [
        {
            "answer": "Tom Cruise",
            "category": 5,
            "difficulty": 4,
            "id": 4,
            "question": "What actor did author Anne Rice first denounce, then praise in the role of her beloved Lestat?"
        }    
        ... # omitted for brevity 
    ],
    "success": true,
    "total_questions": 32
}
`POST '/questions'`
Creates a new question posted from the react front end.
Fields are: answer, difficulty and category.

Returns a success value and ID of the question.
If search field is present will return matching expressions
Example (Create): curl http://localhost:5000/questions -X POST -H "Content-Type: application/json" -d '{"question":"Who is Tony Stark?", "answer":"Iron Man", "category":"4", "difficulty":"2"}'

{
... # shortened for brevity
  "success": true, 
  "total_questions": 35
}

Example (Search):
curl http://localhost:5000/questions -X POST -H "Content-Type: application/json" -d '{"searchTerm":"Lestat"}'

{
  "questions": [
    {
      "answer": "Tom Cruise", 
      "category": 5, 
      "difficulty": 4, 
      "id": 4, 
      "question": "What actor did author Anne Rice first denounce, then praise in the role of her beloved Lestat?"
    }
  ], 
  "success": true, 
  "total_questions": 1
}
`GET '/categories/<cat_id>/questions'`

Returns JSON response of current_category, and the questions pertaining to that category
Example: curl http://localhost:5000/categories/1/questions

{
 "current_category": {
    "id": 1, 
    "type": "Science"
  }, 
  "questions": [
    {
      "answer": "The Liver", 
      "category": 1, 
      "difficulty": 4, 
      "id": 20, 
      "question": "What is the heaviest organ in the human body?"
    }, 
   ... # omitted for brevity
  ], 
  "success": true, 
  "total_questions": 6
}
`POST '/quizzes'`

Generates a quiz based on category or a random selection depending on what the user chooses.
Returns a random question
Example: curl http://localhost:5000/quizzes -X POST -H "Content-Type: application/json" -d '{"previous_questions":[], "quiz_category":{"type":"Art","id":2}}'

{
  "question": {
    "answer": "One", 
    "category": 2, 
    "difficulty": 4, 
    "id": 18, 
    "question": "How many paintings did Van Gogh sell in his lifetime?"
  }, 
  "success": true
}
Error Handlers
When an error occurs a JSON response is returned

Returns these error types when the request fails
400: Bad Request
404: Resource Not Found
422: Not Processable
500: Internal Server Error Example "Resource Not Found":
{
	"success": False,
	"error": 404,
	"message": "Resource Not Found"
}
## Testing

Write at least one test for the success and at least one error behavior of each endpoint using the unittest library.

To deploy the tests, run

```bash
dropdb trivia_test
createdb trivia_test
psql trivia_test < trivia.psql
python test_flaskr.py
```
