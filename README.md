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
## Mouse movement effects

### 6. Mouse move interaction

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



