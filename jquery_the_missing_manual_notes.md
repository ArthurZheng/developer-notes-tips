#jQuery the Missing Manual Book Notes

##Most jQuery programs involve three steps:
selecting an element on the page, attaching an event handler to that element, and then responding to that event by doing something.

###1. .prop() vs .attr()
As Tim said, the vast majority of the time, we want to be working with properties. Partially that's because their values (even their names) tend to be more consistent across browsers. We mostly only want to work with attributes when there is no property related to it (custom attributes), or when we know that for that particular attribute, the attribute and the property are not 1:1 (as with href and "href" above).

I'll summarize the main issues:

You usually want prop() rather than attr().
In the majority of cases, prop() does what attr() used to do. Replacing calls to attr() with prop() in your code will generally work.
Properties are generally simpler to deal with than attributes. An attribute value may only be a string whereas a property can be of any type. For example, the checked property is a Boolean, the style property is an object with individual properties for each style, the size property is a number.
Where both a property and an attribute with the same name exists, usually updating one will update the other, but this is not the case for certain attributes of inputs, such as value and checked: for these attributes, the property always represents the current state while the attribute (except in old versions of IE) corresponds to the default value/checkedness of the input (reflected in the defaultValue / defaultChecked property).
This change removes some of the layer of magic jQuery stuck in front of attributes and properties, meaning jQuery developers will have to learn a bit about the difference between properties and attributes. This is a good thing.

As an example of how properties are simpler to deal with than attributes, consider a checkbox that is initially checked. Here are two possible pieces of valid HTML to do this:
```
<input id="cb" type="checkbox" checked>
<input id="cb" type="checkbox" checked="checked">
So, how do you find out if the checkbox is checked with jQuery? Look on Stack Overflow and you'll commonly find the following suggestions:

if ( $("#cb").attr("checked") === true ) {...}
if ( $("#cb").attr("checked") == "checked" ) {...}
if ( $("#cb").is(":checked") ) {...}
```
This is actually the simplest thing in the world to do with the checked Boolean property, which has existed and worked flawlessly in every major scriptable browser since 1995:
```
if (document.getElementById("cb").checked) {...}
```
The property also makes checking or unchecking the checkbox trivial:

document.getElementById("cb").checked = false

In jQuery 1.6, this unambiguously becomes
```
$("#cb").prop("checked", false)
```
The idea of using the checked attribute for scripting a checkbox is unhelpful and unnecessary. The property is what you need.

It's not obvious what the correct way to check or uncheck the checkbox is using the checked attribute
The attribute value reflects the default rather than the current visible state (except in some older versions of IE, thus making things still harder). The attribute tells you nothing about the whether the checkbox on the page is checked. See http://jsfiddle.net/VktA6/49/.


###2. best way to disable button /link button in Twitter's Bootrap
You just need the $('button').prop('disabled', true); part, the button will automatically take the disabled class.
```
/* code for click checkbox to enable submit btn */
        // to disable btn, use prop('disabled', true);
        $('button[type="submit"]').prop('disabled', true);
           // to disable anchor use attr('disable', true);
        $('.anchor-submit-btn').attr('disabled', true);

        // check if the checkbox is checked or not, then disable/enable the submit btn accordingly;
        $(':checkbox[name="check-terms"]').change(function(){
            if ($(this).prop('checked')){
                alert('checkbox is now checked;');
                $('.btn-submit-btn').prop('disabled', false);
                $('.anchor-submit-btn').attr('disabled', false);
            } else {
              alert('checkbox is unchecked. Now we disable the anchor/btn submit btn;');
                $('.btn-submit-btn').prop('disabled', true);
                $('.anchor-submit-btn').attr('disabled', true);
            };
        });

```

###3. Toggle Hide/Show, SlideToggle, FadeToggle using True/False
Instead of using if true we slide, else we hide, we can compact the code using:

Given your existing code, you could therefore do this:
```
if(document.getElementById('isAgeSelected').checked) {
    $("#txtAge").show();
} else {
    $("#txtAge").hide();
}
However, there's a much prettier way to do this, using toggle:

$('#isAgeSelected').click(function() {
    $("#txtAge").toggle(this.checked);
});
```

Below is Jun's code for demo;
```
$('element').toggle(checkbox is Checked/not checked Boolean);
$('element').Slidetoggle(statusTrue/False);
$('element').Fadetoggle(statusTrue/False);


/* code for checkbox to hide and show age form */

        // first, let's disabled the link btn and button submit btn;
        $('.link-submit-btn').attr('disabled', true);
        $('.submit-btn').prop('disabled', true);

        //we also hide the age input field;
//        $('input[name="age"]').hide();
        $('.age-checkbox').hide();

        // then we check if the checkbox is check;
        $(':checkbox[name="age-checkbox"]').change(function(){
            var checkedStatus = $(this).prop('checked');
            console.log('age checkbox is checked:'), checkedStatus;

            /* this slideToggle is very clever, it toggles base on checkbox checked True or False
               Note: it's better than the normal if checkbox checked, slideDown, else slideUp;
             */
//            $('input[name="age"]').slideToggle(checkedStatus);
            //$('.age-checkbox').slideToggle(checkedStatus);
            $('.age-checkbox').slideToggle($(this).checked); // or even better use this;

            // if the age checkbox is checked, we enabled the submit btns;
            if (checkedStatus){
                $('.link-submit-btn').attr('disabled', false);
                $('.submit-btn').prop('disabled', false);
            } else {
                // if they are unchecked, we disable the submit btns again;
                $('.link-submit-btn').attr('disabled', true);
                $('.submit-btn').prop('disabled', true);
            }

        }); // end age checkbox change;

    });
```

###4. jQuery hover is actually a function not event (a combination of mouse enter and mouse leave events)

jQuery’s .hover() function is a bit wild looking when you’ve finished adding all of its programming (see step 12). However, it’s really just a function that accepts two functions as arguments. It’s a good idea to add the “shells” of the anony- mous functions first—so you can make sure you’ve set it up correctly—before adding all of the programming inside the functions. 


###5. jQuery stop() functions stops the current animateion for the new animation
What’s happening in the code for this tutorial is that each time you mouse onto and off of the div, an animation is added to the queue; so, rapidly mousing over the div creates a long list of effects for jQuery to perform: Animate the div into view, animate the div out of view, animate the div into view, animate the div out of view, and so on. The solution to this problem is to stop all animations on the div before performing a new animation. In other words, when you mouse over the div, and that div is in the process of being animated, then jQuery should stop the current animation, and proceed with the animation required by the mouseEnter event. Fortunately, jQuery supplies a function—the stop() func- tion—for just such a problem.

$(this).stop().animate(.....)


###6. setTimeout() function
```
function greet(greeting){
  console.log(greeting);
}

function getRandom(arr){
  return arr[Math.floor(Math.random()*arr.length)];
}

var greetings = ["Hello", "Bonjour", "Guten Tag"],
    randomGreeting = getRandom(greetings);

setTimeout(function(){
  greet(randomGreeting);
}, 1000);
```


###7. setInterval() function
```
setInterval(function() {
      // Do something every 5 seconds
}, 5000);
```

###7. :input vs input
:input is pseudo selector by jQuery which includes <input>, <select>, <textarea>, <buttons>, form elements e.t.c

input is a tag match which strictly matches <input>.

This additional note about :input is informative:

Because :input is a jQuery extension and not part of the CSS specification, queries using :input cannot take advantage of the performance boost provided by the native DOM querySelectorAll() method. To achieve the best performance when using :input to select elements, first select the elements using a pure CSS selector, then use .filter(":input").


###8. At its core
At its core, jQuery is passed DOM elements or a string containing a CSS selector. It returns a jQuery object, which is an array-like collection of DOM nodes. One or more methods can then be chained to this set of nodes.



###9. jQuery form selectors

TABLE 8-1 jQuery includes lots of selectors to make it easy to work with specific types of form fields
￼
SELECTOR
EXAMPLE
WHAT IT DOES
:input
$(':input')
Selects all input, textarea, select, and button elements. In other words, it selects all form elements.
:text
$(':text')
Selects all text fields.
:password
$(':password')
Selects all password fields.
:radio
$(':radio')
Selects all radio buttons.
:checkbox
$(':checkbox')
Selects all checkboxes.
:submit
$(':submit')
Selects all submit buttons.
:image
$(':image')
Selects all image buttons.
:reset
$(':reset')
Selects all reset buttons.
:button
$(':button')
Selects all fields with type button.
:file
$(':file')
Selects all file fields (used for uploading a file).
:hidden
$(':hidden')
Selects all hidden fields.


###10. jQuery form userful filters: :checked;

var checkedValue = $('input[name="shipping"]:checked').val();
The selector—$('input[name="shipping"]')—selects all input elements with the name shipping, but adding the :checked—$('input[name="shipping"] :checked')—selects only the one that’s checked. The val() function returns the value stored in that checkbox—USPS, for example.


:selected

 var selectedState=$('#state :selected').val();
Notice that unlike in the example for the :checked filter, there’s a space between the ID name and the filter ('#state :selected'). That’s because this filter se- lects the <option> tags, not the <select> tag. To put it in English, this jQuery selection means “find all selected options that are inside the <select> tag with an ID of state.” The space makes it work like a CSS descendant selector: First it finds the element with the proper ID, and then searches inside that for any elements that have been selected.


###11. DOM element properties: checked is a property of the checkbox/radio btn element, disabled is a property of text box. 
```
For DOM properties, you use jQuery’s prop() method, like this:
    if ($('#news').prop('checked')) {
      // the box is checked
    } else {
      // the box is not checked
}
The code $('#news').prop('checked') returns the value true if the box is checked. If it’s not, it returns the value false. 
```

###12. form field property defaultValue, text field has a property of defaultValue which represents the text inside the field when the page first loads.

Then instead of forcing the visitor filling out the form to erase all that text herself, you can erase it when she focuses on the field, like this:

```
1
2
3
4 5} 6 });
$('#username').focus(function() {
  var field = $(this);
  if (field.val()==field.prop('defaultValue')) {
            field.val('');

```


###13. jQuery check if an element exists using elment.length ==== 0

```
if($('#fail').length === 0) {
	// do sth;
}
```
One way to check if an element already exists on the page is to try to use jQuery to select it. You can then check the length attribute of the results. If jQuery couldn’t find any matching elements, the length attribute is 0.


###14. Ajax post data to server example
```
$(document).ready(function() {
$('#login').submit(function() {
  var formData = $(this).serialize();
  $.post('login.php',formData,processData);
  function processData(data) {
    if (data=='pass') {
       $('.main').html('<p>You have successfully logged on!</p>');
    } else {
       if ($('#fail').length === 0) {
       		$('#formwrapper').prepend('<p id="fail">Incorrect ↵ login information. Please try again</p>');
 		} 
  }
 return false;
}); // end submit
}); // end ready
```


###15. jQuery tips and information

15.1 $() Is the Same as jQuery()
15.2 Saving Selections into Variables
15.3 Adding Content as Few Times as Possible
The bottom line is that if you want to inject a chunk of HTML into a spot on the page, do it in one single operation (or at least in as few as you can get away with) rather than add the HTML in parts using multiple inserts.

###16. Optimizing Your Selectors
16.1 Use ID selectors if at all possible. The fastest way to select a page element is to use an ID selector.

16.2 Use IDs first, as part of adescendant selector. 
It just so happens that all of those tags are also inside a div tag with an ID of main. It’s faster to use this selector:

```
$('#main a')
than this selector:
$('a')
```
16.3 Usethe.find()function.jQuery includes a function for finding elements within a selection. It works kind of like a descendant selector in that it locates tags inside of other tags. 
In other words, you could write $('#main a') like this:
$('#main').find('a');
In fact, in some situations, using .find() instead of a descendant selector is
over two times as fast!

16.4 Avoid too much specificity. 
If possible, either use a descendant selector that’s shorter and more refined— $('.sidebar .nav a'), for example — or use the .find() function mentioned in the previous point: $('#main').find('.sidebar').find('.nav a').

###17. jQuery Traversing
```
 .find().Finds particular elements inside the current selection.

 .children()is similar to.find().It also accepts a selector as an argument, but it limits its selection to immediate children of the current selection. 

.parent().Whereas.find()locates elements inside the current element, .parent() travels up the DOM locating the parent of the current tag.

.closest()finds the nearest ancestor that matches a particular selector. Unlike .parent(), which finds the immediate parent of the current tag, .closest() accepts a selector as an argument and finds the nearest ancestor that matches.

.siblings()comes in handy when you wish to select an element that’s at the same level as the current selection.

.next()finds the next sibling of the current selection.

.prev()works just like.next(), except that it selects the immediately preceding sibling.

.end -- you can use jQuery’s .end() function to “rewind” a changed selection back to its original state.

 $('div').click(function() {
       $(this).fadeTo(250,1)
              .find('h2')
              .css('color','#F30')
              .end()
              .find('p')
              .('backgroundColor','#F343FF');
    }); // end click

```
Notice the .end() after the .css('color', '#F30); this code returns the jQuery selection back to the div, so the following .find('p') will find all <p> tags inside the div.


###18. replaceWith(), remove(), wrap(), wrapInner(), unwrap(), empty()

.replaceWith() completely replaces the selection( including the tag and everyhing inside it) with whatever you pass the function. 

.remove()removes the selection from the DOM; essentially erasing it from the page. 

.wrap() wraps each element in a selection in a pair of HTML tags.

 .wrapInner() wraps the contents of each element in a selection in HTML tags.

 .unwrap() simply removes the parent tag surrounding the selection.

.empty() removes all of the contents of a selection, but leaves the selection in place. As with .unwrap(), .empty() takes no arguments.


###19. Working with Number, Number, and the +, parseInt(), parseFloat(), 
If your goal is to add two numbers, but they’re strings, then use Number() or the + operator. However, if you want to extract a number from a string that might include letters, like '200px' or '1.5em', then use parseInt() to capture whole numbers (200, for example) or parseFloat() to capture numbers with decimals (1.5, for example).


###20. Writing more efficient JavaScript

####20.1 Putting Preferences in Variables
One important lesson that programmers learn is how to extract details from scripts so they are more flexible and easier to update.
A better approach is to place the color into a variable at the beginning of your script and then use that variable throughout your script:

You could make the above code even more flexible by uncoupling the connection between the color used for the <p> tags and <td> tags. Currently, they’re both set to the same color, but if you want to make it so you could eventually assign different colors to each, you could add an additional variable to your code:

$(document).ready(function() { 
var pColor='#F60';
var tdColor=pColor;

Now, the click and hover events use the same color—#F60 (because the tdColor variable is set to the value of pColor in line 3). However, if you later decide that you want the table cells to have a different color, just change tdColor="some color";


When writing a JavaScript program, identify values that you explicitly name in your code and turn them into variables. Likely candidates are colors, fonts, widths, heights, times (such as 1,000 milliseconds), filenames (such as image files), message text (such as alert and confirmation messages), and paths to files (such as the path for a link or an image). For example:

   ```
    var highlightColor = '#33A';
    var upArrow = 'ua.png';
    var downArrow='da.png';
    var imagePath='/images/';
    var delay=1000;
```

Put these variable definitions at the beginning of your script (or if you’re using jQuery, right inside the .ready() function).
 TIP  It’sparticularlyusefultoputtextthatyouplanonprintingtoapageintovariables.Forexample,error messages like “Please supply a valid email address,” or confirmation messages like “Thank you for supplying your mailing information” can be variables. When these messages are grouped together as variables at the begin- ning of a script, it’s easier to edit them later (and to translate the text if you ever need to reach an international audience).


####20.2 Putting Preferences in Objects
There’s a slightly more advanced way to store options: using a JavaScript object. 

you can group all of these values into a single object, and then reference individual properties using the dot syntax (page 56).
For example, the list of 5 variables in the previous section could be stored in a single object, like this:

```
    var siteSettings = {
      highlightColor: '#33A',
      upArrow: 'ua.png',
      downArrow: 'da.png',
      imagePath: '/images/',
      delay: 1000
}
```

For example, to retrieve the value of the highlightColor property, you’d write this: siteSettings.highlightColor


###21. jQvery array.join(), string.split() to convert to string and from string to array
```
 .join() method. This array method takes all of the items in the array and turns them into a single string. Each item in the array is separated by either a comma or another separator you specify.
 var days = ['Monday','Tuesday','Wednesday','Thursday','Friday','Saturday',
    'Sunday'];

 var weekdays = days.join(', '); // Monday, Tuesday, Wednesday...
```

 On the other hand, you can take a string and turn it into an array using the split() method as long as the string has some kind of separator (or delimiter) that indicates where one item ends the next begins. For example, say you have this string:
    var weekdays = 'Monday,Tuesday,Wednesday,Thursday,Friday,Saturday,Sunday';
You could take that string and break it up into an array like this:
    var dayList = weekdays.split(','); // dayList now is array of 7 items

