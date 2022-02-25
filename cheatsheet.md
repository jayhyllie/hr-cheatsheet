# **Cheatsheet**
## *FeedStuff*

### *Url Selector*
```
$("html").parent()[0].location.href
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

## *SEARCH*
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
https://explain.helloretail.com/DOum40Bd
```
## *RANDOM*
### *Set Timeout*
```
setTimeout(function(){
 *Add code here*
},1000)
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
