# **Browser console JS snippets**

### *Test pages div*
```js
let html = `<div id="helloretail-category-page-624421fb2d64b76dbff35627" data-filters=" { &apos;hierarchies&apos;: [&apos;Overlocker&apos;, &apos;Bernina Overlocker&apos;] } "></div>`

document.querySelector("SELECTOR TO BE POSITIONED RELATIVE TO").insertAdjacentHTML("beforebegin",html);
```

### *Get array of checked inputs in the Search filter & sorting column*
```js
let checkbox = [];

document.querySelectorAll("tr td:last-of-type input[type='checkbox']").forEach(function(item,index){
    if(item.checked){
        checkbox.push(index);
    }
});
console.log(checkbox);
```
### *Use array generated from snippet above to check same inputs on a duplicate domain in supervisor*
```js
[0, 1, 2, 3, 4, 5, 6, 9, 10, 11, 12, 13, 14].checkbox.forEach(function(item,index){
    document.querySelectorAll("tr td:last-of-type input[type='checkbox']")[checkbox[index]].click()
});
```