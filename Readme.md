# Page Variables <!-- omit in toc -->

Sometimes it is necessary to temporarily store values on a page. This can be achieved by setting the *Text* property of a hidden *Label* control or by using this script. 

The values are stored in an attribute on the page. Browsers implement different limits regarding the possible amount of data that can be stored in this way. Do not use this method for storing large amounts of data. When users navigate away from the page, all variables and their values are lost. 

# Version
Initial 1.0

# Setup

## Global Script
1. Create a Global Script called "PageVariables"
2. Add the input parameters below to the Global Script
   1. Action
   2. Name
   3. Value
3. Drag a *JavaScript* action into the script
4. Add the Javascript below into the JavaScript code property
```javascript
/* Stadium Script v1.0 https://github.com/stadium-software/utils-page-variables */
let prefix = "stvar-";
let nm = prefix + ~.Parameters.Input.Name;
let val = ~.Parameters.Input.Value;
let action = ~.Parameters.Input.Action || "read";
action = action.toLowerCase();
let el = document.querySelector(".container");
if (action == "write" || action == "add") {
    writeVariable(nm, val);
} else if (action == "remove" || action == "delete") {
    deleteVariable(nm);
} else if (action == "readall" || action == "read-all") {
    return readAllVariables();
} else {
    return readVariable(nm);
}
function writeVariable(n, v) {
    el.setAttribute(n, v);
}
function readVariable(n) {
    return el.getAttribute(n);
}
function deleteVariable(n) {
    el.setAttribute(n, null);
}
function readAllVariables() {
    let retArr = [];
    for (let i = 0; i < el.attributes.length; i++) {
        if (el.attributes.item(i).name.indexOf(prefix) == -1) continue;
        let n = el.attributes.item(i).name.replace(prefix, "");
        let v = el.attributes.item(i).value;
        if (isJsonString(v)) {
            v = JSON.parse(v);
        }
        let obj = {};
        obj[n] = v;
        retArr.push(obj);
    }
   return retArr;
}
function isJsonString(str) {
    try {
        JSON.parse(str);
    } catch (e) {
        return false;
    }
    return true;
}
```

## Event Handler or Script
1. Drag the "PageVariables" to the event handler or script
2. Complete the input parameters
   1. Action - default is "read"
      1. write
      2. read
      3. remove
      4. readall
   2. Name: The name of the variable you wish to read, write or remove
   3. Value: The value you wish to write. 

**NOTE: Browsers implement different limits regarding the possible amount of data that can be stored in this way. Do not use this method for storing large amounts of data.**

## Upgrading Stadium Repos
Stadium Repos are not static. They change as additional features are added and bugs are fixed. Using the right method to work with Stadium Repos allows for upgrading them in a controlled manner. 

How to use and update application repos is described here: [Working with Stadium Repos](https://github.com/stadium-software/samples-upgrading)