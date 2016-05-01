CSS Tips

1. Style the width of inline elements

inline-elements like 'span', 'a', 'img' only takes up as much width as necessary, whereas block level elements like 'h1 - h6', 'p', 'div', 'footer', 'header', 'section' start on a new line and take the entire width, as far left and right as possible. 

css display has properties like: display:block, display:inline, display:none, display: inline-block;

display: block is the default for block level elements (h1-h6, div, section, footer, header, p)
display: inline is the default for inline level elements ('a', 'img', 'span'). This can be used to make a block level list element to display as inline to make list elements a horizontal nav bar.
display: none is used to hide an element, it doesn't take up space in the page, but exists in the DOM.
visibility: hidden is used to make an element not visible on the page, but it still takes up the space on the page.
display: inline-block is used to give inline elements a width and heigth since inline element don't have a width and height. 

inline-block elements are like inline elements but they can have a width and a height.

Give inline element: display: inline-block, or float: left/right, to be able to give it a width. You can't assign a width to an inline element, only padding/margin so you'll need to make it float so that you can give it a width.

<style type="text/css">
ul 
{
    list-style-type: none;
    padding-left: 0px;
}

ul li span { 
    float: left;
    width: 40px;
}
</style>

OR Better Solution

In an ideal world you'd achieve this simply using the following css

<style type="text/css">

span {
  display: inline-block;
  width: 50px;
}

</style>


2. Vertical align anything with just 3 lines of CSS

The CSS property transform is usally used for rotating and scaling elements, but with its translateY function we can now vertically align elements. Usually this must be done with absolute positioning or setting line-heights, but these require you to either know the height of the element or only works on single-line text etc.

So, to vertically align anything we write:

.element {
  position: relative;
  top: 50%;
  transform: translateY(-50%);
}

To make it even more simple, we can write it as a mixin:

/* Mixin */
@mixin vertical-align($position: relative) {
  position: $position;
  top: 50%;
  -webkit-transform: translateY(-50%);
  -ms-transform: translateY(-50%);
  transform: translateY(-50%);
}

.element p {
  @include vertical-align();
}


3. display:none vs visibility:hidden

Property  Description
display Specifies how an element should be displayed on the page; 
visibility  Specifies whether or not an element should be visible, but still occupies the space on page;

display:none means that the tag in question will not appear on the page at all (although you can still interact with it through the dom). There will be no space allocated for it between the other tags.

visibility:hidden means that unlike display:none, the tag is not visible, but space is allocated for it on the page. The tag is rendered, it just isn't seen on the page.

They are not synonyms. 
display:none removes the element from the normal flow of the page, allowing other elements to fill in.
visibility:hidden leaves the element in the normal flow of the page such that is still occupies space. 


4. Make 2 divs that are side by side same height

<div class="row">
    <div class="col">...</div>
    <div class="col">...</div>
</div>

With flexbox, same height is just one declaration:

.row {
    display: flex; /* equal height of the children */
}

.col {
    flex: 1; /* additionally, equal width */
}


5. How to center image in div horizontally

Simply set display: block for the img inside your div, and also set margin-left: auto, margin-right: auto, or margin: 0 auto. That's all. Note, that you'll also have to set an initial min-width for your outer div.

img {
  display: block;
  margin-left: auto;
  margin-right: auto;
  margin: 0 auto; // one line says margin-left: auto, margin-right: auto;
}


6. How to make bootstrap button same width

You can also use the .btn-block class on the button, so that it expands to the parent's width.

If the parent is a fixed width element the button will expand to take all width. You can apply existing markup to the container to ensure fixed/fluid buttons take up only the required space.

<div class="span2">
<p><button class="btn btn-primary btn-block">Save</button></p>
<p><button class="btn btn-success btn-block">Download</button></p>
</div>


7. Bootstrap 3 Media Query

@media(max-width:767px){} xs
@media(min-width:768px){} sm
@media(min-width:992px){} md
@media(min-width:1200px){} lg 

Here are the selectors used in BS4. There is no "lowest" setting in BS4 because "extra small" is the default. I.e. you would first code the XS size and then have these media overrides afterwards.

@media(min-width:34em){}
@media(min-width:48em){}
@media(min-width:62em){}
@media(min-width:75em){}




