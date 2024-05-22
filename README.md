# FSCJ-Full-Stack-Development
codecademy exam
FSCJ Full-Stack Development
In the code editor, we have provided you with some incomplete code that creates an Express server and defines callback functions for RESTful API endpoints. Your task is to complete the code in app.js. You will need to:

Import the necessary modules
Initialize an Express server
Inject the environment variables
Define the PORT and PROGRAM_NAME variables
Finally, you’ll need to use the provided callback functions to define the following endpoints:

Get all movies
Get a single movie by its ID
Create a new movie
Find a movie by its ID and update its title
Find a movie by its ID and delete it

```javascript
  const express = require("express");
  const movies = require("./movies");
  const bodyParser = require("body-parser");
  const dotenv = require("dotenv");

  // Initialize express server
  const app = express();

  // Middleware
  app.use(bodyParser.json());

  // Inject environment variables

  function getValueFromEnv(variable, defaultValue) {
    return process.env[variable] || defaultValue;
  }

  dotenv.config();

  process.env.PORT = process.env.PORT || 4001;
  process.env.PROGRAM_NAME = process.env.PROGRAM_NAME || 'Movie App';



  app.use(bodyParser.json());

  // Callback functions
  const getAllMovies = (req, res, next) => {
    res.send(movies);
  };

  const getOneMovie = (req, res, next) => {
    const foundMovie = movies.find((movie) => movie.id == req.params.id);

    if (foundMovie) {
      res.send(foundMovie);
    } else {
      res.status(404).send();
    }
  };

  const createMovie = (req, res, next) => {
    if (req.body.title) {
      // Use req.body instead of req.params for POST requests
      const newMovie = {
        title: req.body.title,
        release_year: req.body.release_year,
        id: movies.length,
      };
      movies.push(newMovie);
      res.send(newMovie);
    } else {
      res.status(400).send();
    }
  };

  const updateMovieTitle = (req, res, next) => {
    const movieIndex = movies.findIndex((movie) => movie.id == req.params.id); // Use findIndex instead of find
    if (movieIndex !== -1) {
      movies[movieIndex].title = req.body.title; // Use req.body instead of req.params for PUT requests
      res.send(movies[movieIndex]);
    } else {
      res.status(404).send();
    }
  };

  const deleteMovie = (req, res, next) => {
    const movieIndex = movies.findIndex((movie) => movie.id == req.params.id); // Use findIndex instead of find
    if (movieIndex !== -1) {
      movies.splice(movieIndex, 1);
      res.status(200).send(movies);
    } else {
      res.status(404).send();
    }
  };

  // Define API endpoints
  app.get("/movies", getAllMovies);
  app.get("/movies/:id", getOneMovie);
  app.post("/movies", createMovie);
  app.put("/movies/:id", updateMovieTitle);
  app.delete("/movies/:id", deleteMovie);

  // Start the server
  app.listen(PORT, function () {
    console.log(`${PROGRAM_NAME} listening on port ${PORT}!`);
  });

  // Leave this in for testing purposes
  module.exports = { app };
</details>
```
