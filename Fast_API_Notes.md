###### These are the notes I made while studying how to use FastAPI by following this short video:https://youtu.be/iWS9ogMPOI0?si=NVdPOg_mrEpdu9I0

   from fastapi import FastAPI
    app=FastAPI()
    @app.get('/')
    def root():
      return {"Hello":"World"}

on the terminal
    pip install fastapi
    pip install uvicorn
    uvicorn main:app --reload

This will run a basic app

Uvicorn is used because FastAPI requires ASGI (Asynchronous Server Gateway Interface) to handle requests and serve the application

**Asynchronous Server Gateway Interface (ASGI)** facilitates asynchronous communication because web servers and our Python web appplications, enabling support for high concurrency and real-time protocols. ASGI supports both HTTP and web sockets. ASGI is typically callable with 3 parameters, they are: scope (connection details), receive ( read client messages) and send (server response). 

concurrency = performing multiple tasks simultaneously. 


We create routes to handle different interactions. 

We create a new endpoint for our app.

    @app.post("items")
    def create_item(item: str):
      items.append(item):
      return items

To add an item to the list, you use curl and add the parameter at the end of the URL of the app like this:

curl -X POST -H "Content-Type: application/json" 'http://xxx.x.x.x:8000/items?item:milk'

To view a particular item on the list we create a new end-point.

    @app.get("/items/{item_id}")
    def get_item(item_id:int) -> str:
    items=items[item_id]
    return item

Every time we make a change, the server will reload and the items in the array will be erased.

We can use HTTP extension to display errors


#### Pydantic
A base model is the starting point for a Pydantic model. A pydantic model has strict validation rules and all pydantic models inherit their essentail behavior from the base model.   
Pydantic models are used to define schemas for structured data.   
You write your data requirements as annotated fields in a class. Pydantic ensures that the incoming data matches your requirements and notifies you of errors.  
Strict mode or Pedantic model does not aloow coercion, data must exactly fit the required data type.

    from pydantic import BaseModel

    class Item(BaseModel):
      text : str = None
      is_done bool = False


    @app.get("/items):
    def list_items(limit : int =10):
    return items[0:limit]


To add the item, we cannot use the same curl request, we need to add the item in the JSON payload of the request.

   curl -X POST -H "Content-Type: application/json" -d '{"text":"apple"}' 'http://127.0.0.1:8000/items'

is_done will output False because we have given false as the default value. For it to output True, we will have to pass is_done=True.


We can add *response model* to structure the response. 

    @app.get("/items/{item_id}", response_model = Item)
    def get_item(item_id:int) -> str:
    items=items[item_id]
    return item

We can go to our local fast API server and add docs/ or redoc at the end of the url to view all our endpoints and which http methods they accept, and the type of parameters they take. 