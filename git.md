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
    const id = document.getElementById("user").value;
    console.log("id is",id);
    const fullURL = "https://api.github.com/users/" + id;
    console.log(fullURL);
    fetch(fullURL)
    .then(function(response) {
      return response.json();
    }).then(function(json) {
      console.log(json);
      const idResponse = json["id"];
      const strong = document.createElement("strong");
      strong.appendChild(document.createTextNode("ID=" + idResponse));
      
      const container = document.getElementById("github");
      container.textContent = "";
      container.appendChild(strong);
    });
  });
</script>
</html>
```
