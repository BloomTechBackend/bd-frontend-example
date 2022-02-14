## Project Description and Purpose

This is a two part project intended to show students how they can create a simple static frontend capable of sending and receiving CRUD requests to an API. This project uses a slightly updated version of the Unit 4 project, where we've added a new Lambda function and endpoint to GET all playlists from `/playlists`.

While you are under no constraints to follow the solution provided in this repo, we want to make sure students get a chance to see a read and a write (in this case we have a GET and a POST) request made with axios, as well as to see a page dynamically load HTML elements using JavaScript.

The provided solution repo has an index page which GETs all of the `playlists` and uses them to construct a list of links. Following any link send the user to the playlist page which then has a GET to all of the `album_tracks` on the list. The `album_tracks` populate a table which has some CSS to make it look a bit nicer. Finally, there are two `<form>` elements: One that allows users to POST to `/playlists/{id}/songs` on submit, and one on `index.html` that allows users to POST a new playlist.

## Key Objectives

### Part 1: HTML/CSS

- Create multiple HTML pages with navigation links between them
- Use several basic HTML tags: `<p>`, `<h>`, `<ul/ol>`, `<li>`, `<table>`, `<head/body>`, `<a>`
- How to add a CSS file
- How to use selectors: single/multiple
- divs, classes, and ids
- Basic CSS: color, background, text-align, margin/padding/border

**Stretch**
- <img>
- Additional CSS

### Part 2: JS and Axios

- JS basics:
  - dynamically typed
  - var/let/const
  - functions
  - anonymous functions
  - objects
- Events: onload, onsubmit
- Reference, Add, and Remove elements with JS
- Axios: get and post
  - `.then()`

## Guided Project

This should span at least two modules. There is too much material to cover since we do not assume Backend students have ever used HTML, CSS, or JavaScript.

The first class should be focused on setting up the project for axios with HTML and CSS. We'll create a list of playlists and a playlist page with a table of album tracks. For part 1 we'll make up fake data which we'll remove for part 2. Additionally we can show how to host the website using AWS Amplify, or simply using an S3 bucket.

0. In this project, students should use your API key so they can access the same backend. The backend that we are using is mostly the same as their final Unit 4 project, but has an additional Lambda for getting all playlists. Students would either need the updated code for the backend, or should just use the same API key as the instructor.

1. Make an `index.html` page
  - First demonstrate basic HTML tags: `<head>` and `<body>`, paragraphs and headers, etc.
  - This page will eventually hold a list of all playlists from our database. For now it can hold dummy data in a `<ul>`.
  - Make the `<li>`s in the list links. These will all link to a new playlist.html page.
    - Each link will however have a query parameter with the playlist id which we can use when it is time to GET the playlist's data.

2. Make a `playlist.html` page
  - This time instead of a list, we will make a table of `album_tracks`.
  - The table looks pretty bad without CSS, and this can motivate us to use it.
  - It could be a good time to introduce the devtools console on chrome, show how we can apply CSS to elements with it.

3. Create a `styles.css` document
  - Show how selectors can apply CSS styling to elements.
  - Make the table pretty
    - text align
    - border
    - background color to the table head
    - `border-collapse: collapse;` to get rid of the double border lines
  - Students can reference websites such as W3Schools to learn more about available CSS properties.

4. Demonstrate divs, ids, and classes
  - We'll need id's for the JavaScript portion of this project
  - Classes and divs are useful to know, feel free to add any that might be useful to you in the base project, or just show one-off examples.

5. Show students how to use a local image. You can save this for the second part where we make use of an animated loading icon while awaiting axios requests.

6. Finish class by showing how to upload the website either to an S3 bucket with website hosting turned on, or better using AWS Amplify.

### Part 2 JavaScript and Axios

We now have the skeleton of our example website build. Now we'll use JavaScript to dynamically modify the HTML elements when necessary. We'll use the axios library to make GET and POST requests, and then populate the tables and lists with the retrieved data.

1. Introduce JavaScript. We assume students have only ever used Java before.
  - dynamically typed language: `var`, `let`, `const`.
  - functions are declared using the `function` keyword.

2. We use events to trigger JavaScript functions.
  - onload being an entry point for sample javascript code
  - some ideas for examples:
    - show console.log.
    - do a scoping example comparing let and var.
    - select and log an HTML element, then browse it's contents in the console.
    - set a text element to a random color
  - show how simply making `index.js` does not cause the script to run, we need to include it on the html page.

3. Now that we are familiar with JavaScript it's time to bring in the axios library
  - We'll use a CDN link to include the library
  - Describe what axios is and what we'll use it for

4. First we can make a GET request to our API
  - We are going to call this method from within the `window.onload` event
  - discuss axios syntax:
    ```
    async function getPlaylistData() {
      axios.get(url, headers).then((res) => {
        console.log(res);
      })
    }
    ```
  - `async`, `then`, fat arrow `=>`
  - Try with an empty header and show how to get the url for `/playlists`.
    - you should get a permissions error due to lack of an API key.
  - Show how to create an API key on API Gateway.
    - Explain that while we are going to put our API key straight into our code, this is not good practice. Discuss possible alternatives.
    - Create a header object with the API key:
    ```
    let headers = {
      authorization: {
        'x-api-key': 'your-API-key'
      }
    }
    ```
    - Now we can see the response object in the console. Show the data property contains the object data.

5. Populate the playlists list on index.html with the playlist data.
  - Note that it can take a long time before the Lambda "wakes up" and returns the data.
  - We can talk about eventually adding a loading icon here.
  - Each of these playlists needs to be a link to `playlist.html` with an added query parameter containing the id of the playlist.

6. Next we can create an onload event on the playlists page to GET all of the album tracks for the playlist identified in the query parameter
  - Show how to access query params using
  ```
  const urlParams = new URLSearchParams(window.location.search);
  const id = urlParams.get('id');
  ```
  - This time, instead of populating a list, we are populating a table. There are multiple strategies for populating a table, the sample code demonstrates a simple method.

7. Finally show how we can also make a POST request.
  - To do this we'll need a request body, which we can build using form data.
  - It might be best to first create a request body object with sample data and show how to post that. Then use this to motivate the need for a `<form>` so that the user can write their own data.
  - Create a form to POST new album_tracks to a playlist
    - NOTE: The way this works requires the user to provide an existing `asin` and matching `trackNumber` of an album track that exists in the database already. It's not a great system but it's what we have from the unit 4 project.

8. Extras
  - With any additional time, it great to discuss how we can add a loading animation with students. In the provided sample project, there is an animated `.gif` grabbed from a Google image search.
  - We can also have students think about displaying helpful information such as displaying "No Album Tracks Added to this Playlist" instead of just an empty table.
    - Or perhaps we can display error messages such as "No playlist with id {id} found!"
