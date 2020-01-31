# Simple jQuery sections slider

A very simple implementation of the sections slider in jQuery.

Try it live on this [demo](https://epranka.github.io/sections-slider)

Yes, it is very basic. It can be greatly improved, but it just an example how fast it can be implemented. If you need a full-featured slider, use this awesome library [fullpage.js](https://alvarotrigo.com/fullPage/) by [Alvaro Trigo](https://twitter.com/IMAC2)

Follow me on [![](http://i.imgur.com/wWzX9uB.png) Twitter](https://twitter.com/epranka) and [dev.to](https://dev.to/epranka)

![](https://i.ibb.co/wBPqWY3/sections-slider-min.gif)

The HTML code

```html
<div id="section1" class="section">
  <span>1. Viewport height section</span>
</div>

<div id="section2" class="section">
  <span>2. Long section</span>
</div>

<div id="section3" class="section">
  <span>3. Short section</span>
</div>

<div id="section4" class="section">
  <span>4. Viewport height section</span>
</div>
```

The CSS code

```css
.section {
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 800;
  font-size: 120%;
  font-weight: 800;
  position: relative;
}

#section1 {
  height: 100vh;
  background: #6699cc;
}

#section2 {
  height: 150vh;
  background: #ff8c42;
}

#section3 {
  height: 60vh;
  background: #ff3c38;
}

#section4 {
  height: 100vh;
  background: #a23e48;
}
```

The JavaScript code

```js
(function($) {
  var selector = ".section";

  var $slides = $(selector);

  var currentSlide = 0;
  var isAnimating = false;

  var stopAnimation = function() {
    setTimeout(function() {
      isAnimating = false;
    }, 300);
  };

  var bottomIsReached = function($elem) {
    var rect = $elem[0].getBoundingClientRect();
    return rect.bottom <= $(window).height();
  };

  var topIsReached = function($elem) {
    var rect = $elem[0].getBoundingClientRect();
    return rect.top >= 0;
  };

  document.addEventListener(
    "wheel",
    function(event) {
      var $currentSlide = $($slides[currentSlide]);

      if (isAnimating) {
        event.preventDefault();
        return;
      }

      var direction = -event.deltaY;

      if (direction < 0) {
        // next
        if (currentSlide + 1 >= $slides.length) return;
        if (!bottomIsReached($currentSlide)) return;
        event.preventDefault();
        currentSlide++;
        var $slide = $($slides[currentSlide]);
        var offsetTop = $slide.offset().top;
        isAnimating = true;
        $("html, body").animate(
          {
            scrollTop: offsetTop
          },
          1000,
          stopAnimation
        );
      } else {
        // back
        if (currentSlide - 1 < 0) return;
        if (!topIsReached($currentSlide)) return;
        event.preventDefault();
        currentSlide--;
        var $slide = $($slides[currentSlide]);
        var offsetTop = $slide.offset().top;
        isAnimating = true;
        $("html, body").animate(
          {
            scrollTop: offsetTop
          },
          1000,
          stopAnimation
        );
      }
    },
    { passive: false }
  );
})(jQuery);
```
