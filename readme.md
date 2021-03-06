# NTX-Ventures - REST Service & DB Service
Library crate housing both the REST client service and MySQL client service.

## DB Service Requirements
-   Provide a setup function to establish a connection with credentials.
-   Hold a persistent connection to db.
-   Provide a query function for sending raw queries to db.
-   Provide tests for testing query function for SELECT, ADD, UPDATE, and DELETE functionality.
-   Run setup tests to verify that
-   connection has been successfully setup
-   Insert function is working
-   Read function is working
-   Delete function is working

### Implementation
``docker-compose`` is used to spin up a MYSQL instance and populate a static database for testing. There is a setup function that returns a pooled, asynchronous connection. There are two functions ``query`` and ``select`` query used to handle any raw query type. Environment variables via ``dotenv`` are used to house the database url that contains default credentials from the docker image.

## REST Requirements 
-   A constant variables for the purpose of calling a designated API
-   An async reqwest Http client that will be used for making all calls.
-   A setup function that accepts a config object, creates the reqwest::Client instance, and returns the service as a struct.
-   An async function that will call an api with a provided bearer token and parameters, then return the response as a reqwest::Response object.
-   A standardized way to strip out and return the body of response in a way that can be easily converted to a custom data object.
-   Unit tests for setup and convert functionality.

### Implementation
Since this is to be a service I decided to structure it as a library and develop the tests as if I was an external program utilizing the functions. I am new to rust so bear with me on the language conventions and such. A tokio runtime is used to support asynchronous operations. A struct was used ``ServiceConfig`` to house the root url instead of a constant variable. 
\
More testing is necessary to take to production, as the requirements only specified convert and functionality testing; the ``request`` function remains programmatically untested (I manually tested the ``GET`` endpoint). I'm sure there are many optmizations that can be made here, and it would likely need to be tested in the context of the actix webserver.

#### Assumptions
-   I developed a ``strip`` function that extracts the request body into a json hashmap with generic values. To me, this is standardized, but other functions could be added to support text extraction, or some other requirements.
-   REST methods supported include ``GET`` and ``POST``. I kept it to the two most used methods as this was not specified further, but other methods could be added here.
-   Testing for instantiation and conversion is implicit in the function, otherwise they would throw an error. For example, if a function instantiates a new struct, the fact that it succeeds is a test in itself.

#### TODO
-   Test ``request`` function
    -   Authorized requests
    -   POST request
    -   Multiple headers
-   Implement query parameters for ``GET`` function
