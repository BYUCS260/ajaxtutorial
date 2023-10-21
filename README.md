# API tutorial
This tutorial shows you how to access a simple API with the JavaScript `fetch` command.
- Set up your basic page without any actions. Notice that we have put identifiers for each element.
```html
<html>
    <head>
        <title>City Finder</title>
    </head>
    <body>
    <form>
        Enter A Utah City: <input type="text" id="cityField" value=""><br>
        Suggestion: <span id="txtHint">Empty</span>
        <input id="weatherButton" type="submit" value="Submit">
    </form>
    <p>City</p>
    <textarea id="displayCity">No City</textarea>
    <p>Current Weather</p>
    <div id="weather">No weather</div>

    </body>
</html>
```

- Then make sure you can catch the <a href="http://api.jquery.com/keyup/">keyup event</a> and open an alert box. Put this code at the bottom of your page so that the buttons will be in the document before you try to catch the event.

```html
<script>
document.getElementById("cityField").addEventListener("keyup", function(event) {
  event.preventDefault();
  alert( "Handler for .keyup() called." );
});
</script>
```

- Now prove to yourself that you can modify the txtHint span
```js
document.getElementById("cityField").addEventListener("keyup", function(event) {
  event.preventDefault();
  const value = "Keyup";
  const txtHint = document.getElementById("txtHint");
  txtHint.textContent = "";
  txtHint.appendChild(document.createTextNode(value));
});
```
- Now show that you can get the value from the cityField form.
```js
document.getElementById("cityField").addEventListener("keyup", function(event) {
  event.preventDefault();
  const value = document.getElementById("cityField").value;
  const txtHint = document.getElementById("txtHint");
  txtHint.textContent = "";
  txtHint.appendChild(document.createTextNode(value));
});
```
- Now it is time to call a real RESTful service. Found here:
                                

[https://csonline.fhtl.org/](https://csonline.fhtl.org/)

                                
There are a lot of things that could go wrong, so it is a good idea to take baby steps. This service takes a query parameter following ? in the URL.  So lets start by passing it a "P" to get all cities that start with a P on the line with the "const url".

```html
<script>
document.getElementById("cityField").addEventListener("keyup", function(event) {
    event.preventDefault();
    const url = "https://csonline.fhtl.org/?q=P";
    fetch(url)
        .then(function(response) {
            return response.json();
        }).then(function(json) {
            console.log(json);
            console.log(json[0]);
            console.log("Got " + json[0].city);
            const everything = document.createElement("ul");
            for (let i = 0; i < json.length; i++) {
                const value = json[i].city;
                const listItem = document.createElement("li");
                listItem.appendChild(document.createTextNode(value));
                everything.appendChild(listItem);
            };
            const txtHint = document.getElementById("txtHint");
            txtHint.textContent = "";
            txtHint.appendChild(everything);
        });
});
</script>
```
- Now we want to pass it the real characters from the form to the REST service.  We will append the characters the user has typed to the end of the URL.

```js
const url = "https://csonline.fhtl.org/?q=" +
      document.getElementById("cityField").value;
```

- Now you should have a fully working Hint system.  Congratulations!
</ol>
