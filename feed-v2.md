# **Feed V2 JS snippets**

### *isNew based on hours elapsed from when we saw the product / date provided by customer in feed*
```js
function hoursChecker(customerDate, hoursToConsiderNew) {
    const createdDate = new Date(customerDate).getTime();
    const currentDate = new Date().getTime();

    // Convert time from milliseconds to hours. Return true if elapsed hours is lower than provided number. 
    const hoursElapsed = Math.round((currentDate - createdDate) / (1000 * 60 * 60));
    return hoursElapsed < hoursToConsiderNew ? true : false;
}

function transform(item: any): TransformationResult {
    return {
        ...item,
        extraData: {
            isNewHours: hoursChecker(item.date, 720),
        }
    };
};
```

### *isNew based on days elapsed from when we saw the product / date provided by customer in feed*

```js
function daysChecker(customerDate, daysToConsiderNew) {
    const createdDate = new Date(customerDate).getTime();
    const currentDate = new Date().getTime();

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

### *How to handle extraDataList array data.*
```js
// Declare function that accepts an item parameter.
function arrayPusher(item){
    // Define an empty array.
    const pushedArray = [];
    // Push different values from the item parameter to the defined array.
    pushedArray.push(item.id + item.albumId);
    pushedArray.push(item.id);
    pushedArray.push(item.albumId);
    // Return the populated array to the property where it was invoked.
    return pushedArray;
}

function transform(item: any): TransformationResult {
    return {
        ...item,
        imgUrl: item.thumbnailUrl,
		price: item.id,
        extraDataList:{
            pushedArrayData: arrayPusher(item),
            simpleArrayData: [item.id,item.albumId]
        }
    };
};

```
