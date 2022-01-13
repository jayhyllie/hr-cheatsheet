# **Cheatsheet**
## **Liquid**
### *Standard Liquid*
```
{% if %}{% else %}{% endif %} // A conditional if statement. Example: {% if product.inStock == true %}<div>Product is in stock!</div>{% endif %}. {% elsif %} Can also be used.
{% for %}{% endfor %}
```
### *Condition to check if product contains a specific extraData*
```
{% if product.extraData and product.extraData != "" and product.extraData != "undefined" and product.extraData != "null" %}
{% endif %}
```
### *Same as above, but haven't figured out how it works 100% yet*
```
{% if product.extraData != blank %}
{% endif %}
```
### *Calculate the savings percentage*
```
{{ product.oldPrice|minus:product.price|times:100.0|divided_by:product.oldPrice|round }}
```
### *Calculate the savings percentage and only show if percentage is above or below "X"*
```
{% assign saving = product.oldPrice|minus:product.price|times:100.0|divided_by:product.oldPrice|round %}
{% if saving >= X %}
	<div class="hr-sales-splash">-{{ saving }}%</div> // Replace this with their own badge
{% endif %}
```
### *Running a for loop with sizes and checken stock status in a seperate list*
```
{% for size in product.extraDataList.sizes %}
    {% assign index = forloop.index | minus: 1 %}
    <span class="aw-size{% if product.extraDataList.sizesStockStatus[index] == 'false'%} aw-{% endif %}">{{ size }}</span>
{% endfor %}
```
### *CurrentProduct is used to call data outside of the for loop*
```
{% if currentProduct.extraData.isStrikkeKit == 'true' %}
```
## **Regex // Regular expressions**
### *Match all HTML*
```
/<[^>]*>/g
```
### *Match specific word/phrase and everything around*
```
/.*(Word).*/g
```
## **Useful feed and selector stuff**
### *Standard jQuery methods / Homebrew methods*
```
.text() // Grabs the value of the selector as a string. Example: $("selector").text()
.fns() // Creates a list of items acording to the parameter and the selector. Example: $("selector, selector").fns("text")
.split() // Splits a string into a list on a specific character. Example $("selector").text() == "Hello|World", $("selector").text().split("|") == ["Hello", "World"]
.join() // Joins a list into a string placing specific character in between. Example: $("selector").text().split("|").join(" ") == "Hello World"
.pop() // Grabs the last item in a list
.shift() // Grabs the first item in a list
.slice() // Grabs a specific interval of a list
.replace("", "") // Replaces the first parameter with the second parameter. Very powerfull with Regex. Example: $("selector").text().replace("Hello", "World")
.matches() // Gives a boolean value compared to the parameter
.unescapeHtml() // Should remove all HTML and special characters. Doesn't always work.
.removeMatching("") // Removes the parameter you put in from a list. Can also be empty to remove empty list items
```
### *Standard CSS*
```
:first-child() // Selects the first match of the selector
:last-child() // Selects the last match of the selector
:nth-child()
:contains() // Looking for a selector that contains a specific value. Example: .parent .child:contains("I am a child element")
:has() // Only works if selected element has a selected child element. Example: .parent:has(.child)
:eq() // Equal to (x). Example .category-list:has(.product:eq(7)) .hr-category-box-1 // This way you can see if there is 8 products in a category before inserting our recom. Some customers does not was it if there is less than fx 4.
```
### *Remove all HTML tags and special characters*
```
.unescapeHtml().replace(/<[^>]*>/g, "")
```

### *Dandomain variants instock*
```
inStock: $("variantsInStock > stockCount:first, root > stockCount").first().text().matches(/[1-9]/)
```
### *Shopify API instock*
```
inStock: $("root > main_variant > inventory_quantity").text().matches(/^[1-9]/)
```
### *Magento 2 out of stock if price is 0*
```
inStock: $("price, instock").text().matches(/^[^0].*(true)/)
```
## **Javascript**
### *Category page slider dynamic headline*
```
<script>
	// Put this in the box title <span class="hr-dynamic-header"></span>
	document.querySelector('.hr-dynamic-header').textContent = document.querySelector('{% input category_header_selector %}').textContent.trim();
</script>
```
### *Cart page slider dynamic headline*
```
<script>
	// Put this in the box title <span class="hr-dynamic-header"></span>
	const freeShippingAmmount = {% input free_delivery_amount %};
	const totalCartAmmount = parseInt(document.querySelector("{% input cart_total %}").textContent.replace(".", "").replace(/,.*/, ""));
	document.querySelector(".hr-dynamic-header").textContent = totalCartAmmount >= freeShippingAmmount ? "Du har opnået gratis fragt! Måske du også var interesseret i disse?" : `Du mangler stadig ${freeShippingAmmount - totalCartAmmount} kr for at få fri fragt.`;
</script>
```
### *Overlay search results headline*
```
post_render: function(data, first_render){
    if(first_render){
        let searchWord = document.querySelector('.aw-search-overlay-input').value;
        let header = document.createElement("h2");
        header.classList.add("aw-overlay-search-heading");
        header.textContent = `Søgningen på "${searchWord}" gav ${data.results} resultater`;
        document.querySelector(".aw-search-overlay-selected-filters-container").append(header);
    }
}
```
### *Overlay search link to full search*
```
post_render: function(data, first_render){
    if(first_render){
        let searchWord = document.querySelector('.aw-search-overlay-input').value;
        document.querySelector(".aw-search-overlay-input").addEventListener("keypress", function(e){
            if(e.keyCode === 13){
                e.preventDefault();
                window.location.href = `https://www.test.dk/soegeresultat?q=${searchWord}`;
            }
        });
    }
}
```
### *Overlay search button link to full search*
```
post_render: function(data, first_render){
	if(first_render){
        let btn = document.createElement("a");
        btn.classList.add("aw-overlay-search-results-btn");
        btn.textContent = `Show all results (${data.results})`;
        btn.href = `https://www.test.dk/soegeresultat?q=${searchWord}`;
        document.querySelector(".aw-search-overlay-filters-toggle-container").append(btn);
    }
}
```
## **Honorable mentions**
### *Sometimes this needs to be added if you are updating an old search template. Put inside _.search.util.instant(key, input, {});*
```
engine_options: engine_options
```
### *Old overlay search template. This allows to show products when initializing the search*
```
allow_blank_search: true
```
### *Instant search is always relative to the input. This can change that, so you decide the placement. Put inside _.search.util.instant(key, input, {});*
```
let container = document.querySelector(".selector");
insert_results: function(res) {
    res.insertAfter(container);
}
```
### *Add free delivery to cart page*
```
// Put this in the box title <span class="hr-dynamic-header"></span>
const freeShippingAmmount = {% input free_delivery_amount %};
const totalCartAmmount = parseInt(document.querySelector("{% input cart_total_selector %}").textContent.replace(",", ".").replace(/\./, "").replace(/[^0-9\.]/g, ""));
document.querySelector(".hr-dynamic-header").textContent = totalCartAmmount >= freeShippingAmmount ? "{% input free_delivery_title %}" : `{% input not_free_delivery_before_text %} ${freeShippingAmmount - totalCartAmmount} {% input not_free_delivery_after_text %}`;
```
### *Make different shapes with Clip path*
```
https://bennettfeely.com/clippy/
```
### *HTML Symbols*
```
https://www.toptal.com/designers/htmlarrows/arrows/
```
### *Recom on last hierarchy not first*
```
extraData.hasMoreThanOneLevel: $(".m-breadcrumb li.page span").fns("text").slice(2).join("||").matches(/.*\|\|.*/)
Switch out the classes and number of slice maybe
```
### *Box selecter for category page*
```
aw$(".breadcrumbs .items .item").fns("text").fns("trim").slice(1).asHierarchy()
Switch out the classes
```
### *Remove from hierarchies*
````
.fns("removeMatching", /Alla produkter/ig)
````
### *Clean out categories in search*
```
url: $("hasProducts, url").text().replace(/^false.*/, "").replace(/^true/, "")
title: $("isVisibleInMenu, name").fns("text").join("||").replace(/^0.*/, "").split("||").pop()
```
### *Category page don't show on more than 8 products and show not on filter active*
```
.catalog-category-view:has(.item:eq(8)):not(:has(.filter-active)) .column.main .category-product-actions
```
### *Add fade on last product in slider*
```
.aw-slider-{{ key }} .swiper-slide.swiper-slide-next + .swiper-slide + .swiper-slide {
    opacity: .2;
}
```
### *URL : Display none on recom*
```
var searchParams = new URLSearchParams(window.location.search);
	console.log(typeof searchParams.get('p'));
	if (searchParams.get('p') !== null){
		document.querySelector("#aw-box-{{ key }}").style.display="none"
	}
```
### *Set up instant grid search*
```
change the "var input" to the right class
.attr("action", "/pages/search-results")
Then in the HTML :
<div class="aw-grid-search-results__submit-wrapper">
		<a href="https://shenanigans.toys/pages/search-results?q={{query}}" class="aw-grid-search-results__submit-link">
			{{ show_more_results_text }} ({{ products.totalResults }})
		</a>
	</div>
```
### *Take away from end of title*
```
title: $("g\\:title, title").text().split("-")[0]
```
### *Fix scroll in search*
```
max-height: 100vh;
overflow: scroll;
```
### *Add filters in full search*
```
dubuy.dk, from all templates!!
```
### *Make a diagonal line-through*
```
.aw-oldPrice{
	position: relative;
}
.aw-line {
	position: absolute;
    width: 80%;
    left: 20px;
    height: 1px;
    border: none;
    background: #185398;
    transform: rotate(342deg) translate(-50%, -50%);
}
```
### *Change the image size on SMARTWEB webP*
```
imgUrl: [$("g\\:image_link").text().replace(/(.*)\/.*$/, "$1"), $("item > g\\:image_link").text().split("/").pop().replace(/^/, "/_thumbs/")].join("").replace(/(.*)\./, "$1.w293.h293.fill.")
```
### *STRECHED recom fix*
```
Find customers parent element
Add this to design:
document.querySelector('#aw-box-{{ key }}').style.width=document.querySelector("#Content_Productlist").offsetWidth+"px";
```
### *If shipping price is within the Total Price*
```
var basketAmount = 0;
	document.querySelectorAll(".webshop-showbasket .product_price").forEach((item)=>{
	console.log(item.textContent.split("NOK").shift().trim());
	basketAmount += parseFloat(item.textContent.split("NOK").shift().trim());
	});
	
	var heading = jQuery(".aw-heading");
    
if( !isNaN(basketAmount) ) {
    if(basketAmount < 199) {
       var amountLeft = 199 - basketAmount;
       heading.text("Kjøp for "+ amountLeft.toFixed(2).replace(".", ",").replace(",00", "") +" kr mer og få fri frakt");
    }
    else {
        heading.text("Du har oppnådd fri frakt! Andre kunder kjøpte også");
    }
}
```
### *extraData to get products on sale*
```
extraData.isOnSale: [$("main_variant > price").text(),$("main_variant > compare_at_price, main_variant > price").first().text()].subtract().matches(/^-/)
```
### *Remove CROSS in desktop search*
```
input[type=search]::-ms-clear { 
	display: none; 
	width : 0; 
	height: 0; 
}
input[type=search]::-ms-reveal { 
	display: none; 
	width : 0; 
	height: 0; 
}
input[type="search"]::-webkit-search-decoration,
input[type="search"]::-webkit-search-cancel-button,
input[type="search"]::-webkit-search-results-button,
input[type="search"]::-webkit-search-results-decoration { 
	display: none; 
}
```
### *Reset form event listener SEARCH*
```
var searchBtn = document.querySelector(".header-search");
searchBtn.replaceWith(searchBtn.cloneNode(true));
```
