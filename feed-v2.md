# **Feed V2 JS snippets**

### *isNew based on hours elapsed from when we saw the product / date provided by customer in feed*
```js
function hoursChecker(customerDate) {
    const createdDate = new Date(customerDate).getTime();
    const currentDate = new Date().getTime();
    /*let secondsElapsed = Math.round((currentDate - createdDate) / 1000);
    let minutesElapsed = secondsElapsed / 60;
    let hoursElapsed = minutesElapsed / 60;*/

    // Convert time from milliseconds, to seconds, to hours.
    const compactHoursElapsed = Math.round((((currentDate - createdDate) / 1000) / 60) / 60);
    // Products are considered new if they were created within the last NUMBER hours.
    return compactHoursElapsed > 48 ? true : false;
};
```

### *isNew based on days elapsed from when we saw the product / date provided by customer in feed*

```js
function daysChecker(customerDate, daysToConsiderNew) {
    const createdDate = new Date(customerDate);
    const currentDate = new Date();

    // Convert time from milliseconds to days. Return true if elapsed days is lower than provided number. 
    const daysElapsed = Math.round((currentDate - createdDate) / (1000 * 60 * 60 * 24));
    return daysElapsed < daysToConsiderNew ? true : false;
}

function transform(item: any): TransformationResult {
    return {
        ...item,
        extraData: {
            isNewHours: hoursChecker("2023-05-10"),
            isNewDays: daysChecker(item.date, 30),
        }
    };
};
```
