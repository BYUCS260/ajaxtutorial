```html
<!DOCTYPE html>
<html>
<title>XKCD</title>

<body>

  <p>Current comic</p>
  <div id="comic">No comic</div>
</body>

<script>
  const myurl = "https://xkcd.com/info.0.json";
  console.log(myurl);
  fetch(myurl)
    .then(function(response) {
      return response.json();
    }).then(function(json) {
      console.log(json);
      const alt = document.createTextNode(json["alt"]);
      const container = document.getElementById("comic");
      container.textContent = "";
      container.appendChild(alt);
    });
</script>
</html>
```
