# **Cheatsheet**
## *FeedStuff*

### *Url Selector*
```
- $("html").parent()[0].location.href
- $("link[rel='canonical']").attr("href")
```
### *HTML Unescape*
```
.unescapeHtml().replace(/<[^>]*>/g, "")
```
### *Remove from end of title*
```
title: $("g\\:title, title").text().split("-")[0]
```
### *extraData to get products on sale*
```
extraData.isOnSale: [$("main_variant > price").text(),$("main_variant > compare_at_price, main_variant > price").first().text()].subtract().matches(/^-/)
```
### *SORT on largest saving*
```
extraData.saving: [[$("oldPrice").text(),
$("price").first().text()].subtract().multiply(100),
$("oldPrice").text()].divide().round().replace(/^/, "000").replace(/\d*(0\d{2})$/g, "$1")
```
### *Magento 2 out of stock if price is 0*
```
inStock: $("price, instock").text().matches(/^[^0].*(true)/)
```
### *DANDOMAIN sort out variants*
```
$("items > name, isVariantMaster").text().replace(/true$/, "").replace(/.*false$/, "")
```
### *SHOPIFY inStock selector*
```
inStock: $("inventory_quantity").fns("text").sum().matches(/^[1-9]/)
```
### *SHOPIFY hasVariants selector*
```
extraData.hasVariants: $("variants > sku").fns("text").fns("replace", /.*/, "1").sum().notMatches(/^1$/)
```
### *SHOPIFY ORDER FEED remove if POS*
```
orderNumber: $("source_name, order_number").fns("text").join("||").replace(/^pos.*/, "").split("||").pop()
```
### *Change image size*
```
.replace(/(\.[a-z]{3,4}\?)/i, "_300x$1")
```
### *Change the image size on SMARTWEB webP*
```
imgUrl: [$("g\\:image_link").text().replace(/(.*)\/.*$/, "$1"), $("item > g\\:image_link").text().split("/").pop().replace(/^/, "/_thumbs/")].join("").replace(/(.*)\./, "$1.w293.h293.fill.")
```
## *HIERARCHY*
### *Remove from hierarchies*
````
.fns("removeMatching", /Alla produkter/ig)
````
### *Recom on last hierarchy not first*
```
extraData.hasMoreThanOneLevel: $(".m-breadcrumb li.page span").fns("text").slice(2).join("||").matches(/.*\|\|.*/)
Switch out the classes and number of slice maybe
```
### *Hierarchy selecter for category pages*
```
aw$(".breadcrumbs .items .item").fns("text").fns("trim").slice(1).asHierarchy()
Switch out the classes
```
### *Clean out categories in search*
```
url: $("hasProducts, url").text().replace(/^false.*/, "").replace(/^true/, "")
title: $("isVisibleInMenu, name").fns("text").join("||").replace(/^0.*/, "").split("||").pop()
```
## *RECOMS*
### *Category page NO BREADCRUMBS from customer*
```
https://addwish.com/supervisor/app/678784/websites/107e93d5-5ff6-4c75-896a-857b3f5df1b3/designs/edit/135110
```
### *Category page don't show on more than 8 products and show not on filter active*
```
.catalog-category-view:has(.item:eq(8)):not(:has(.filter-active)) .column.main .category-product-actions
```
### *STRECHED recom fix*
```
Find customers parent element
Add this to design:
document.querySelector('#aw-box-{{ key }}').style.width=document.querySelector("#Content_Productlist").offsetWidth+"px";
```
### *FREE SHIPPING USA*
```
var heading = jQuery(".aw-heading"),
basketAmount = parseFloat( jQuery(".cart-total-wrapper .money").text().trim().split("$").pop().trim().replace(",", "") );

if( !isNaN(basketAmount) ) {
    if(basketAmount < 50) {
       var amountLeft = 50 - basketAmount;
       heading.text("Add $"+ amountLeft.toFixed(2).replace(".00", "") +" to reach FREE shipping");
    }
    else {
        heading.html("You've reached FREE shipping! <br>Others also bought");
    }
}
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
### *Trigger Upsell Dropdown after page reload*
```
var viewportWidth = window.matchMedia("(min-width: 700px)");

if(sessionStorage.getItem("buyButtonClicked")){

if(sessionStorage.getItem("buyButtonClicked") === "true"){
setTimeout(function(){
document.querySelector("#slider-{{ key }}").style.overflow = "visible";
if(viewportWidth.matches){
document.querySelector("#slider-{{ key }}").style.height = "660px";
document.querySelector("#{{ key }}").style.height = "715px";
}
else{
document.querySelector("#slider-{{ key }}").style.height = "680px";
document.querySelector("#{{ key }}").style.height = "820px";
}
sessionStorage.setItem("buyButtonClicked", "false");
},1000);

}
}
Array.from(document.querySelectorAll(".button-primary.button-icon")).pop().addEventListener("click",function(){
sessionStorage.setItem("buyButtonClicked","true");
});
```
### *Free shipping Price Range Products in Cart*
```
1) Make a selector to grab the cart total: https://explain.helloretail.com/xQuqz0nn (make sure the price is formatted right).
2) Call this variable "price": https://explain.helloretail.com/d5u9NeAY (the reason being, that this treats it as an actual number) in the box-selector-section.
3) Use the variable "price" (which is now the crawled value from the basket page, and not "some value of products"), to set up conditions, like: a) "if below free shipping, do X", "if above, do Y".
4) Use this, in the first section (where the price (value of basket) is still below free shipping), to specify, that the products found in this step, have a certain price-range:
 
$price|freeShipping:495, 100
The explanation of this would be:
$price(this is still the crawled value of the basket-total)|freeShipping:495(here you set the treshold for when free shipping has been applied), 100(this is the margin, with witch the price are allowed to go the range).
So put into a concrete example this:
(200|freeShipping:495, 100)
would mean "show products, that have a price between 195 and 395".
like so: https://explain.helloretail.com/BluodB1q - the behaviour on the page should be like: https://explain.helloretail.com/6quZEB8k
```
## *SEARCH*
### *MOBILE GRID SEARCH*
```
1. .hr-tab-wrapper {
	display: flex;
	flex-wrap: wrap;
	justify-content: center;
}
.hr-tab-body[data-tab='category'] .hr-tab-wrapper {
	display: block;
}
2. .hr-overlay-search .hr-search-overlay-product {
    padding: 10px;
    position: relative;
    background-color: #ffffff;
    border-radius: 12px;
    -webkit-border-radius: 12px;
    -moz-border-radius: 12px;
    flex-basis: 42%;
    flex-direction: column;
    display: flex;
    margin: 5px auto;
}
3. .hr-row {
	display: flex;
	flex-direction: column;
	width: 100%;
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
### *Fix scroll in search*
```
max-height: 100vh;
overflow: scroll;
```
### *Add filters in full search*
```
dubuy.dk, from all templates!!
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
### *Add hierachypath to category content template*
```
<span class="hr-search-overlay-content-hierarchy-wrapper">
    {% for level in ctnt.hierarchy limit : 1 %}
    {% if level != "" and level != current_content_item.title %}
    <span class="hr-search-overlay-content-category-hierarchy-category">{{ level }}</span>
    {% unless forloop.last %}
    <span class="hr-search-overlay-content-hierarchy-divider">›</span>
    {% endunless %}
    {% endif %}
    {% endfor %}
</span>
(Remove limit : 1, to show all hierachies)

.hr-search-overlay-content-hierarchy-wrapper {
    display: block;
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
    font-size: .9em;
    color: #777;
}
https://explain.helloretail.com/DOum40Bd
```
## *PAGES*
### *Test pages div*
```
let html = `<div id="helloretail-category-page-624421fb2d64b76dbff35627" data-filters=" { &quot;hierarchies&quot;: [&quot;Overlocker&quot;, &quot;Bernina Overlocker&quot;] } "></div>`

document.querySelector("SELECTOR TO BE POSITIONED RELATIVE TO").insertAdjacentHTML("beforebegin",html);

The &quot; represents a literal quote: "
But has to remain  &quot; in order for the html to recognize and format it correctly.

Once above code has been run in the console of the browser, run the following piece of code (WHILE THE PAGES DRAFT IS ENABLED)

ADDWISH_PARTNER_NS.api.reload()
```
## *RANDOM*
### *Set Timeout*
```
setTimeout(function(){
 *Add code here*
},1000)
```
### *Add getCookie method to window object*
```
if(!window.getCookie){
     window.getCookie = function(name) {
          var match = document.cookie.match(new RegExp('(^| )' + name + '=([^;]+)'));
          if (match) return match[2];
     }
}
```
### *How to add input fields to triggered email designs*
```
https://addwish.com/supervisor/triggeredemail/designEdit.html?designId=8924

{# text priceColor = "#CA0303" #}

<h3 style="color:{{ priceColor }};font-weight:700;"></h3>
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
### *Make different shapes with Clip path*
```
https://bennettfeely.com/clippy/
```
### *HTML Symbols*
```
https://www.toptal.com/designers/htmlarrows/arrows/
```
### *IP for whitelist*
```
46.137.110.51
```
