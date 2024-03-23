# GIF TOK

This project is a simple Vue.js application called GIF TOK, where users can search for GIFs using the GIPHY API and view them in a grid layout. Users can also load more GIFs and view individual GIFs in a modal.

## Overview

The SGBr services primarily utilize Vue 3, Quasar, Tailwind CSS, and Pinia technologies. My task is to create a web page that consumes data from the [Giphy API](https://developers.giphy.com/), featuring infinite scrolling of gifs, a search option, and an enlarged view of the selected gif.

## Features

- Search Bar:
  Users can enter search queries to find GIFs related to their interests.
  GIF Grid: Displays the search results in a grid layout with clickable GIF thumbnails.
  Load More: Allows users to load more GIFs if available.

- GIF Modal: Clicking on a GIF opens a modal to view the GIF in a larger size.
  Error Handling: Provides error messages for failed API requests or rate limit exceeded scenarios.

## How to Use Components

### App.vue

This component serves as the main container for the application. It includes the search bar, GIF grid, loading indicator, error message display, modal for individual GIFs, and a button to load more GIFs.

### SearchBar.vue

This component represents the search bar where users can input their search queries to find GIFs. It emits an `input` event with the search query text whenever the user types in the input field.

### GifGrid.vue

This component displays the search results in a grid layout. It receives an array of GIF objects as props and generates a grid of clickable GIF thumbnails. Clicking on a GIF thumbnail emits a `showGif` event with the selected GIF object.

### GifModal.vue

This component is responsible for displaying an individual GIF in a modal. It receives a GIF object as a prop and shows the GIF in a larger size within a modal dialog. Users can close the modal by clicking outside the modal or using the close button.

## Dependencies

[Axios](https://axios-http.com/docs/intro): For making HTTP requests to the GIPHY API.

[GIPHY API](https://developers.giphy.com/): Used to search for and fetch GIFs based on user queries.

## Getting Started

To get started with the project, follow these instructions:

1. Clone this repository to your local machine.

```bash
git clone https://github.com/juliafclima/gif-tok-vue.git
```

2. Navigate to the project directory.

```bash
cd gif-tok-vue
```

3. Install dependencies.

```bash
npm install
```

4. Run the development server.

```bash
npm run dev
```

5. Access the application in your browser at `http://localhost:8080` (or the appropriate port number if it's different).

## Author

This project was created by [Juliafclima](https://www.linkedin.com/in/juliafclima/). You can contact me at juliafclima@hotmail.com for any inquiries or feedback.

## License

This project is licensed under the [MTI License](https://mit-license.org/). See the LICENSE file for details.
