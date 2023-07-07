# **Fetch snippets**

### *How to structure a Pages REST-API request url*
```js
fetch("https://core.helloretail.com/serve/pages/{key}", {
  "method": "POST",
  "mode": "cors",
  "credentials": "include",
    "headers":{
       'Content-Type':'application/json'
    },
    "body":JSON.stringify({
        "id":"{pages id}",
        "url":"{url to category page}",
        "layout":"true",
        "firstLoad":"true",
	"format":'json',
        "products":{
            "start":0,
            "count":4000,
            "filters":[
                "extraDataList.colorFilter:Silver",
                "price":"100,500",
                "brand":"nike"
                
            ],
            "sorting":[
                "title desc"
            ],
            "params":{
                "filters":"{'hierarchies':['Strömbrytare och vägguttag']}"
            },
            "trackingUserId":"{hello_retail_id cookie}"
        }
    })
}).then((res)=>{ return res.json() }).then((data)=>{ console.log(data) });
```
### *How to structure a TrackingUserId REST-API request url*
```js
fetch('https://core.helloretail.com/serve/trackingUser',{
    method: "POST", 
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
        "websiteUuid": "6a3b6823-a7d9-429c-95fe-b34cff1548bd"
    }), 
}).then((res) => {
    return res.json();
}).then((data) => {
    console.log(data)
});
```
### *How to structure a Page-view REST-API request url*
```js
fetch('https://core.helloretail.com/serve/collect/pageview',{
    method: "POST", 
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
    "location": "https://b2b.jbs.dk/da/",
    "trackingUserId": "6426929526c7b13a8a1566f9",
    "websiteUuid": "6a3b6823-a7d9-429c-95fe-b34cff1548bd",
    "referrer": "https://b2b.jbs.dk/da/",
    "url": "https://b2b.jbs.dk/da/produkt/023008750000000000101",
    "productNumber": "0230087500000000001"
}), 
}).then((res) => {
    return res.json();
}).then((data) => {
    console.log(data)
});
```
### *How to structure a Click REST-API request url*
```js
fetch('https://core.helloretail.com/serve/collect/click',{
    method: "POST", 
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
    "trackingUserId": "6426929526c7b13a8a1566f9",
    "websiteUuid": "6a3b6823-a7d9-429c-95fe-b34cff1548bd",
    "source": "32ef3ce4-19c6-4469-a69f-bb1ce3b450f5"
}), 
}).then((res) => {
    return res.json();
}).then((data) => {
    console.log(data)
});
```
### *[Managed Fetch Recommendation API request example](https://developer.helloretail.com/api/recoms/)* 
```js
fetch('https://core.helloretail.com/serve/recoms', {
    "method": 'POST',
    "mode": 'cors',
    "credentials": 'include',
    "headers": {
        'Content-Type': 'application/json'
    },
    "body": JSON.stringify({
        "websiteUuid":"6afc16a0-6657-4f10-859e-54ea03a6759b",
        "trackingUserId":"64538f21e1c2815a658631dd",
        "requests":[
            {
                "key": "k62a1aa2f5c5f7e7b1b50458f",
                "format":"json",
                "fields": ["title","url"], // fields can be removed in order to return ALL data.
                "context": {
                    "hierarchies":[["Clothing","Women"]],
                    "brand": "Nike",
                    "urls": ["https://product-urls-for-context"],
                    "price":20,
                    "extraData":{
                        "color":"black"
                    },
                    "extraDataList":{
                        "d9995dTargetGroups":["590"]
                    }
                }
            }
            
        ]
    })
}).then((res) => {
    return res.json();
}).then((data) => {
    console.log(data)
});
```
### *[Unmanaged Fetch Recommendation API request example](https://developer.helloretail.com/api/recoms/)* 
```js
fetch('https://core.helloretail.com/serve/recoms', {
    "method": 'POST',
    "mode": 'cors',
    "credentials": 'include',
    "headers": {
        'Content-Type': 'application/json'
    },
    "body": JSON.stringify({
        "websiteUuid":"3ced6ff1-d6a1-4263-9e0d-34cdfd9b3056",
        "trackingUserId":"644fa63d74a13834ab841abc",
        "requests":[
            {
                "trackingKey":"frontpage-12",
                "fields":["title","url"], // fields can be removed in order to return ALL data.
                "hideAdditionalVariants":true,
                "count":8,
                "sources":[
                    {
                        "type":"TOP",
                        "limit":4,
                        "filters":{
                            "hierarchies": {"$in": [["Legetøj","Klassisk Legetøj","Sjovt legetøj til lave priser"]]}
                        }
                    },
                    {
                        "type":"RETARGETED",
                    }
                ]
            }
        ]
    })
}).then((res) => {
    return res.json();
}).then((data) => {
    console.log(data)
});

```