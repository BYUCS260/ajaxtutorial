# ajaxtutorial
This tutorial shows you how to access a simple REST service with ajax
- Set up your basic page without any actions. Notice that we have put identifiers for each element.
```
<html>
<head>
<title>City Finder</title>
<script
  src="http://code.jquery.com/jquery-3.2.1.min.js">
</script>
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

- Then make sure you can catch the <a href="http://api.jquery.com/keyup/">keyup event</a> and open an alert box.

```
<script>
$(document).ready(function() {
  $( "#cityField" ).keyup(function() {
    alert( "Handler for .keyup() called." );
  });
});
</script>
```

- Now prove to yourself that you can modify the txtHint span
```

$( "#cityField" ).keyup(function() {
  $("#txtHint").text("Keyup");
});

```
- Now show that you can get the value from the cityField form.
```

$( "#cityField" ).keyup(function() {
  $("#txtHint").text("Keyup "+$("#cityField").val());
});

```
- At this point you have a dynamic page, but you would like to get some real data to put into the Suggestion span.  Lets start with a baby step and create some fake <a href="http://www.json.org/">JSON</a> data that we can pretend came from a <a href="https://github.com/tfredrich/RestApiTutorial.com/raw/master/media/RESTful%20Best%20Practices-v1_2.pdf">REST</a>ful service.
<p>
Create a file <a href="http://students.cs.byu.edu/~clement/CS360/jquery/staticCity.txt">staticCity.txt</a> with the following content:

```
[
{"city":"Provo"},
{"city":"Lehi"}
]
```

You will want to make sure you can read this  array of two city entries before you talk to a live service.
We are going to show you the <a href="http://api.jquery.com/jquery.getjson/">getJSON</a> shorthand Ajax function 

```
jQuery.getJSON( url [, data ] [, success ] )
```

which is equivalent to 

```
$.ajax({
  dataType: "json",
  url: url,
  data: data,
  success: success
});
```

You ought to be familiar with using console.log in conjunction with the <a href="https://developer.chrome.com/devtools">javascript console</a> in your browser to debug your code.

```
$( "#cityField" ).keyup(function() {
  $.getJSON("staticCity.txt",function(data) {
    console.log(data);
    console.log(data[0]);
    console.log("Got "+data[0].city);
  });
  $("#txtHint").text("Keyup "+$("#cityField").val());
});
```
Open the console in your chrome debugger to see the data that is returned from the Ajax call.
Notice that we are passing getJSON an anonymous function that will be called when the Ajax response is received.
- Now lets write the response as an unordered list into the Suggestion span with id #txtHint.

```
  var everything;
  everything = "<ul>";
  $.each(data, function(i,item) {
    everything += "<li> "+data[i].city;
  });
    
  everything += "</ul>";
  $("#txtHint").html(everything);
```
- Notice that we are calling the Jquery "each" function.  You can tell when Jquery is being used by the $.  You can find details in the [Jquery documentation](http://api.jquery.com/jquery.each/).
- Now it is time to call a real RESTful service.  There are a lot of things that could go wrong, so it is a good idea to catch errors so that you can figure out what is wrong. This service takes a query parameter following ? in the URL.  So lets start by passing it a "P" to get all cities that start with a P.

```
<script>
$(document).ready(function() {
$( "#cityField" ).keyup(function() {
  $.getJSON("http://bioresearch.byu.edu/cs260/jquery/getcity.cgi?q=P",function(data) {
    var everything;
    everything = "<ul>";
    $.each(data, function(i,item) {
      everything += "<li> "+data[i].city;
    });
    everything += "</ul>";
    $("#txtHint").html(everything);
  })
  .done(function() { console.log('getJSON request succeeded!'); })
  .fail(function(jqXHR, textStatus, errorThrown) { 
    console.log('getJSON request failed! ' + textStatus); 
    console.log("incoming "+jqXHR.responseText);
  })
  .always(function() { console.log('getJSON request ended!');
  });
});
});
</script>
```

You can chain these callback functions to select the <a href="http://api.jquery.com/jquery.ajax/">events</a> you would like to be notified of.  
Notice that the fail callback allows you to access the responseText directly.
- Now we want to pass it the real characters from the form to the REST service.  We will append the characters the user has typed to the end of the URL.

```
var url = "http://bioresearch.byu.edu/cs260/jquery/getcity.cgi?q="+$("#cityField").val();
$.getJSON(url,function(data) {
```

- Now you should have a fully working Hint system.  Congratulations!
</ol>
