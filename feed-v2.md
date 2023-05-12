# **Feed V2 JS snippets**

### *isNew based on days elapsed from when we saw the product / date provided by customer in feed*
```js
function dateChecker(customerDate){
    let createdDate = new Date(customerDate).getTime();
    let currentDate = new Date().getTime();
    /*let secondsElapsed = Math.round((currentDate - createdDate) / 1000);
    let minutesElapsed = secondsElapsed / 60;
    let hoursElapsed = minutesElapsed / 60;*/

    //Convert time from milliseconds, to seconds, to hours.
    let compactHoursElapsed = Math.round((((currentDate - createdDate) / 1000) / 60) / 60);
    // Products are considered new if they were created within the last NUMBER hours.
    return compactHoursElapsed > 48 ? true : false;
}

function transform(item:any): TransformationResult {
    return {
        ...item,
        extraData:{
            isNew: dateChecker("2023-05-10") 
        }
    };
}
```