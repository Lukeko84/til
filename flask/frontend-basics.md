# Frontend Basics

There are some basic concepts that you should know about frontend development. There some very basic things around HTML, CSS, and JavaScript.

## Javascript

### Selecting elements
Operations mostly involve selecting elements and manipulating them. Here are some common ways to select elements in JavaScript:

```javascript
// Selecting by ID
// HTML: <div id="uniqueBox">Content</div>
const myElement = document.getElementById('uniqueBox');
// <div class="card">Card 1</div>
// <div class="card">Card 2</div>
const cards = document.getElementsByClassName('card'); // Returns both divs
// Selecting by tag
const allParagraphs = document.getElementsByTagName('p'); // Gets all <p> elements
// Selecting by query
// Can use complex CSS selectors
const element = document.querySelector('.container > div.special'); // First div with class 'special' inside element with class 'container';

```


### Waiting for DOM to be fully loaded

To make sure that your JavaScript code runs after the DOM is fully loaded, you can use the following code. If this is not done, your JavaScript code might run before the DOM is fully loaded, and you might not be able to access the elements you want to.

```javascript
document.addEventListener('DOMContentLoaded', function() {
    // Your code here
});
```

Alternatively, you can also use the html `defer` attribute. This is most often considered to be the preferred solution. The `defer` attribute tells the browser to execute the script after the document has been parsed.
```html
<head>
    <script defer>
        // This automatically waits for HTML
        const button = document.querySelector('#myButton');
    </script>
</head>
```

### Fetching data
The `fetch` API is a modern way to make network requests. It is a promise-based API that allows you to make requests to a server and handle the response. There are two ways how `fetch`can be used - with `.then()` and with `async/await`. `async/await` is a newer way to handle promises and is often considered more readable so I want to focus on this one:


```javascript
const submitForm = async (event) => {
    event.preventDefault();

    // Get form data
    const formData = new FormData(event.target);
    const userData = Object.fromEntries(formData);

    try {
        // Send data to server
        const response = await fetch('/api/submit', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(userData)
        });

        if (!response.ok) {
            // Server returned an error status
            throw new Error('Server responded with an error');
        }

        const result = await response.json();

        // Show success message
        alert('Data submitted successfully!');

    } catch (error) {
        // Handle network errors or server problems
        alert('There was a problem submitting the form');
        console.error('Error:', error);
    }
};
```

**Common Fetch Options**
```javascript
fetch(url, {
    method: 'POST',          // GET, POST, PUT, DELETE, etc.
    headers: {               // Request headers
        'Content-Type': 'application/json',
        'Authorization': 'Bearer your-token'  // For authenticated requests
    },
    body: JSON.stringify(data),  // Data to send
    mode: 'cors',           // Cross-Origin Resource Sharing settings
    cache: 'no-cache',      // Caching behavior
    credentials: 'include'  // Include cookies in the request
});
```
