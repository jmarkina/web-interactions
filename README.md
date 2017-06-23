# A collection of useful code snippets, libraries, and plugins for web interactions that have been used in our projects 

### 1. Slick

**URL:** http://kenwheeler.github.io/slick/

**Usage:** Slideshows

**Dependencies:** jQuery 1.7

**Pros (for our projects):** has infinite looping, ability to control width of slides and show 1.5 slides for example. Ability to control how many slides to show and scroll.

In order to create a slider that displays 1,5 slides I used these options:

```
arrows: true,
infinite: true,
variableWidth: true
```

Note: not possible to use variableWidth with centerMode set to true (false by default)

**Cons:** challenging to deal with nested divs. The simplier the structure, the better.
Since I needed to have image and content side by side (where having heights adjusted by flexbox would be ideal but not possible in this case), I ended up using this straightforward structure and fixed height for image and content:

```
<div class="slider-container">
    <div class="slider-item">
      <div>image</div>
      <div>content</div>
    </div>
    <div class="slider-item">
      <div>image</div>
      <div>content</div>
    </div>
</div> 
```

## Scrolling effects

### 2. Reveal anything on scroll

**Dependencies:** jQuery

```
.fadeInBlock {
    opacity: 0;
}

$(document).ready(function(){   
    $(window).scroll( function(){
        $('.fadeInBlock').each(function(){
            
            var bottom_of_object = $(this).offset().top + $(this).outerHeight() / 2;
            var bottom_of_window = $(window).scrollTop() + $(window).height();
            
            /* Adjust the "200" to either have a delay or that the content starts fading a bit before you reach it  */
            bottom_of_window = bottom_of_window - 100;  
          
            if( bottom_of_window > bottom_of_object ){
                $(this).animate({
                    'top': '0',
                    'opacity':'1',
                    'easing': 'easeInExpo'
                }, 600);       
            }
        }); 
    });
});

[reference](http://www.ordinarycoder.com/jquery-fade-content-scroll/)

```

### 3. Srollify

**URL:** https://projects.lukehaas.me/scrollify/

**Usage:** to create a snap scroll effect on web pages

**Dependencies:** jQuery

**Pros (for our project)** Lightweight - 9Kb, scrolls one screen at a time no matter how fast user scrolls. Possibility to have the plugin resize window (make it full screen) or turn this option off.

**Cons** One of the reasons I picked this plugin is that there is a possiblity to keep sections with standart scrolling behaviour, like header and footer. Hovewer, in my case with tall header and footer as a side effect I got flickering in between those sections. I ended up having all sections as a part of snap scroll and full screen.

### 4. Parallax effect

**Usage** The following snippet will create a nice background movement on fixed backgrounds as user scrolls. Handy when there is no complex parallax effects.
Works by changing background-position with defined speed.

```
 var prlxBg = function() {
        var prlxItem = $('.prlx-bg');
        var speed = 0.02;
        $(window).scroll(function(){ 
            prlxItem.each(function(){
                var windowYOffset = $(window).scrollTop(),
                elBackgrounPos = "0 " + (windowYOffset * speed) + "px";
          
                $(this).css('background-position', elBackgrounPos);
            });
        });
    };
    prlxBg();

``` 

### 5. Pure CSS Parallax effect

**Usage** The following snippet will create a nice background movement on fixed backgrounds as user scrolls. Handy when there is no text on background and when it's OK if background gets zoomed in a bit.

**Cons** Zooming in on background and blur on text on top of background because of the scale property used.

```
.prlx-bg {
    transform: translateZ(1.5px) scale(1.1);
}

```
### 6. Modular parallax effect 

```
$(document).ready(function () {    
    
    var prlxUp = $('.prlx-up');
    var prlxUpSlow = $('.prlx-up-slower');
    var prlxInside = $('.prlx-up-inside');
    
    // A positive value moves the element down the page or infront of the x axis, 
    // a negative value moves it up the page or behind the x axis.

    var prlxUpValueSlower = -.9;
    var prlxUpValue = -.10;
    var prlxInsideValue = 0.01;
  
    var prlxUpStr;
    var prlxUpSlowerStr;
    var prlxInsideStr;
    // set a variable flag which will be used to check weather to run the animations or not.
    var requesting = false;
    
    // Use the debounce() function to kill the animations 100 milliseconds after the last scroll event using the requesting flag.
    var killRequesting = debounce(function () {
        requesting = false;
    }, 100);

    window.addEventListener("scroll", onScroll, false); // Start the parallax animations on the scroll

    function onScroll() {
        if (!requesting) {  // checks to see that the requesting flag is false before running the animations.
            requesting = true;
            requestAnimationFrame(parallax);  // using requestAnimationFrame browser API to perfomr cheaper animations
        }
        killRequesting();
    }
        
    function parallax() {  
        // prlxUp
        prlxUpStr = "translate3d(0, " + getYOffsetValue(prlxUpValue) + "px, 0)";
        $(prlxUp).css({
            "transform":prlxUpStr,
            "-ms-transform":prlxUpStr,
            "-webkit-transform":prlxUpStr
        });

        //prlxUpSlower 
        prlxUpSlowerStr = "translate3d(0, " + getYOffsetValue(prlxUpValueSlower) + "px, 0)";
        $(prlxUpSlow).css({
            "transform":prlxUpSlowerStr,
            "-ms-transform":prlxUpSlowerStr,
            "-webkit-transform":prlxUpSlowerStr
        });

        //prlxInside
        prlxInsideStr = "center " + getYOffsetValue(prlxInsideValue) + "%";
        $(prlxInside).css({
            "background-position":prlxInsideStr
        });

        function getYOffsetValue(prlxValue) {
            // setting the speed/Value ation effect to be a multiple of the windows Y scroll position
            return ( window.pageYOffset * prlxValue ).toFixed(2)
        }

        if (requesting) {  // check the flag before calling itself again
            requestAnimationFrame(parallax);
        }
    }

    // A browser polyfill for requestAnimationFrame 
    // http://paulirish.com/2011/requestanimationframe-for-smart-animating/
    // http://my.opera.com/emoller/blog/2011/12/20/requestanimationframe-for-smart-er-animating
    // requestAnimationFrame polyfill by Erik Möller. fixes from Paul Irish and Tino Zijdel
    // MIT license
    (function () {
        var lastTime = 0;
        var vendors = ['ms', 'moz', 'webkit', 'o'];
        for ( var x = 0; x < vendors.length && !window.requestAnimationFrame; ++x) {
            window.requestAnimationFrame = window[vendors[x]+'RequestAnimationFrame'];
            window.cancelAnimationFrame = window[vendors[x]+'CancelAnimationFrame']
                || window[vendors[x]+'CancelRequestAnimationFrame'];
        }

        if (!window.requestAnimationFrame)
            window.requestAnimationFrame = function(callback, element) {
                var currTime = new Date().getTime();
                var timeToCall = Math.max(0, 16 - (currTime - lastTime));
                var id = window.setTimeout(function() { callback(currTime + timeToCall); },
                    timeToCall);
                lastTime = currTime + timeToCall;
                return id;
            };

        if (!window.cancelAnimationFrame)
            window.cancelAnimationFrame = function(id) {
                clearTimeout(id);
            };
    }());

    // debounce is taken from _underscore.js
    // http://underscorejs.org/#debounce
    function debounce(func, wait, immediate) {
        var timeout, args, context, timestamp, result;
        return function() {
            context = this;
            args = arguments;
            timestamp = new Date();
            var later = function() {
                var last = (new Date()) - timestamp;
                if (last < wait) {
                    timeout = setTimeout(later, wait - last);
                } else {
                    timeout = null;
                    if (!immediate) result = func.apply(context, args);
                }
            };
            var callNow = immediate && !timeout;
            if (!timeout) {
                timeout = setTimeout(later, wait);
            }
            if (callNow) result = func.apply(context, args);
            return result;
        };
    }
});

```
## Mouse movement effects

### 7. Mouse move interaction

**Usage:** For making things move as user moves mouse (an example would be eyes of a cat looking at cursor)

**Dependencies:** jQuery, but can be easily rewritten with vanilla JS

```
    var bg = $('.moveBlock'); // element which will move
    var windowWidth = $(window).innerWidth()/10; 
    // increasing this number (10) will encrease distance in which elements will move 
    var windowHeight = $(window).innerHeight()/10;
    
    //element that will trigger the movement. Can be the same one or a whole document
    $('.moveBlock').on('mousemove', function(e){ 
        var mouseX = (e.clientX / windowWidth).toFixed(2);
        var mouseY = (e.clientY / windowHeight).toFixed(2);
        bg.css("transform","translate3d(" + mouseX +"px ,"+ mouseY + "px , 0px)");
    }); 
```