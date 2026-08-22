## Target: Port Swigger Web Security Academy - Shopping Application
## Platform: PortSwigger
## Date: 08/22/26
## Difficulty: Practitioner (DOM XSS document.write()) Apprentice (jQuery anchor href, jQuery hashchange)
## Tools: 
- Browser 
- DevTools 




### DOM XSS in `document.write` sink using source `location.search` inside a select element 

Navigate to the product page and observe the page has a stock checker functon. 

![Home-Page](dom-images/product-page.jpeg)


#### Identify the Source and Sink 

Inspecting the live DOM in Chrome DevTools, we find that JavaScript is responsible for implementing and updating the form's location.

![Pre-DOM](dom-images/pre-exploit-dom.jpeg)

A statically defined array containing the locations "London", "Paris", and "Milan" is declared. A for loop then iterates over the array to generate an `<option>` for each element. The JavaScript writes these locations using `document.write()`. Further down, we see the result of the JavaScript execution in the form of the `<select>` and the location `<option>` elements.


![original-execution](dom-images/original-js-execution.jpeg)

This output context will be important for successfully injecting the payload in the future.

Directly below `stores` arra, we find the source:

```javascript
var store = (new URLSearchParams(window.location.search)).get('storeId');

```

`window.location.search` extracts the query string from the URL. `URLSearchParams` then parses that query string, allowing `.get('storeId')` to retrieve the value associated with the `storeId` parameter. The value associated with that parameter is then saved to the variable `store`. 

We then identify the sink: 

```javascript
document.write('<select name="storeId">');

if (store) {
    document.write('<option selected>'+store+'</option>');
}
```

The `document.write()` sink takes the extracted parameter input `store` and writes it to the web page without sanitization.



#### Prove Input Reaches Sink 

The first thing to test is whether attacker-controlled input from the URL query reaches the `document.write()` sink and is reflected in the dropdown option.

![Input-Insertion](dom-images/storeid-insertion.jpeg)

We then verify successful insertion through viewing the location dropdown.

![Updated-Dropdown](dom-images/parameter-added.jpeg)
> Insertion Confirmed 


#### Payload Injection

After confirming successful injection, we craft and inject a payload that calls the `alert` function. As noted earlier, the sink `document.write()` places input within an `<option>` element inside a `<select>` element. To prevent the HTML parser from treating the injected JavaScript as content within the restricted context, we need to break out of the context using a closing `</select>` tag first. 

```javascript 
</select><script>alert('Hacked By Ian`)</script>
```

![Successful-Payload](dom-images/select-alert.jpeg)
> Successful injection execution reflected in the page


#### Room Completion 

![Success-Banner](dom-images/select-solved-banner.jpeg)








### DOM XSS in jQuery anchor href attribute sink using location.search source

### DOM XSS jQuery in selector sink using a hashchange event 



