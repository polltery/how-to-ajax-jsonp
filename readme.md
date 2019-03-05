# Network Applications : AJAX and JSONP lab exercise

## Pre-requisites
- install [Git](https://git-scm.com/).
- install [Node.js](https://nodejs.org/en/) (comes with node package manager).
- Keep your favourite IDE ready, I like to use [visual studio code](https://code.visualstudio.com/) for JavaScript developments.


## Making AJAX requests

1. Open terminal/cmd/git bash 
2. Run the command `git clone https://github.com/acucciniello/xhr-post-example.git`
3. Go to the project `cd xhr-post-example`
4. Run the server `node server.js`
5. Realise you are missing the dependency and install the dependency by running `npm install express`
6. Perform step 4
7. Open your browser and visit `http://localhost:3000`
8. Open developer console by hitting `F12`
9. Check the **Network** tab
10. Hit the **send** button on the webpage
11. Read the message on the console
12. Pat yourself

## AJAX and CORS

1. Update the AJAX request so that it sends a `GET` request to `"http://www2.macs.hw.ac.uk:8080/demo/spellhelp.jsp?prefix=" + value` (value is what we enter in textbox)
2. Try sending the request now and check your developer console, under the **console** tab, you should be getting an error. Realise that the server is trying to make a cross domain request to `http://www2.macs.hw.ac.uk:8080` from `http://localhost:3000` which is not allowed. It's like going to McDonalds and ordering KFC.
3. Good thing our server supports CORS (Cross-Origin Resource Sharing), update the url `"http://www2.macs.hw.ac.uk:8080/demo/spellhelp.jsp?prefix=" + value + "&CORS"`, the server is configured so that when `CORS` query parameter is passed it will send an additional header `Access-Control-Allow-Origin : *`. This header will tell the browser to relax the security and continue the request. In real life you have to tell the guy who manages the server for your website to send these headers if it's trying to make cross domain requests.
4. (OPTIONAL) You can check how CORS can be enabled on server https://enable-cors.org/server.html

## JSONP

1. Update the AJAX request again so that it sends a `GET` request to `"http://www2.macs.hw.ac.uk:8080/demo/getScript.jsp?user=imcc&fn=callback"`.
2. Try sending this request now and expect failure.
3. Open a new tab/window on your browser and visit this URL http://www2.macs.hw.ac.uk:8080/demo/getScript.jsp?user=imcc&fn=callback
3. Realise the response from the server is in JSONP, i.e. JSON with padding, the method it will invoke is `callback`.
```
// Normal JSON
{"foo" : "bar"}

// JSONP
callback({"foo" : "bar"})
```
4. Add a new JavaScript function called `callback` that will take a response `data` and log it on the console.
5. Update the `send` function to dynamically generate a `script` tag (given below) instead of using `XHR`. 
```
          var s = document.createElement("script");
          s.type = "text/javascript";
          s.src = "http://www2.macs.hw.ac.uk:8080/demo/getScript.jsp?user=imcc&fn=callback";
          document.head.appendChild(s);
```
6. Run the request again and celebrate success.
7. (OPTIONAL) This website will explain from a different prespective what we just did https://medium.freecodecamp.org/use-jsonp-and-other-alternatives-to-bypass-the-same-origin-policy-17114a5f2016
