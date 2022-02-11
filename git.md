```html
<!DOCTYPE html>
<html>
<title>GIT Demo</title>

<body>
<h1>
Github IDs
</h1>
<form id="userform">
  <input id="user">
  <input type="submit" value="Go">
</form>
<div id="github">

</div>
</body>

<script>
document.getElementById("userform").addEventListener("submit", function(event) {
    event.preventDefault();
    var id = document.getElementById("user").value;
    console.log("id is",id);
    var fullURL = "https://api.github.com/users/" + id;
    console.log(fullURL);
    fetch(fullURL)
    .then(function(response) {
      return response.json();
    }).then(function(json) {
      console.log(json);
      var id = json["id"];
      var strong = document.createElement("strong");
      strong.appendChild(document.createTextNode("ID=" + id));
      
      var container = document.getElementById("github");
      container.textContent = "";
      container.appendChild(strong);
    });
  });
</script>
</html>
```
