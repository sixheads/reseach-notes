# ScrollMagic 101 - Petr Tichy

- we can use the debug.addIndicators.min.js script as a helper to see trigger clues on our page while developing
- first up we need to init a new controller
- this lets us use the scroll bar to trigger our animation
- we always define the animation in a scene

```javascript
// Init ScrollMagic
var controller = new ScrollMagic.Controller();

// build a scene
var ourScene = new ScrollMagic.Scene({
  triggerElement: "#project01" //set our trigger element
})
  .setClassToggle("#project01", "fade-in")
  .addIndicators({
    name: "fade scene",
    colorTrigger: "black",
    indent: 200,
    colorStart: "#75c695"
  }) // need a plugin for this
  .addTo(controller);
```

- this animation triggers a class on #project01
- we can add our indictors to help see when an animation will trigger
- you can add options to the trigger to change position and the look of it

## Duration & Triggerhook

- you can set the duration of your animation after the trigger element
- this sets when the animation ends and goes back

```javascript
var ourScene = new ScrollMagic.Scene({
  triggerElement: "#project01",
  duration: 300
});
```

- you can also set the color of the end trigger marker:
  colorEnd: "pink"
- with our 300px duration the class is removed again 300 px down
- the fade-in animation duration is set with our transition value in our CSS
- you can also use percentages for the duration which is better for responsive.
- so setting it to "100%" makes it 100% of the viewport height

- with **triggerHook** we can set where the trigger starts
- by default its in the middle of the viewport
- 0 puts it at the very top
- 1 puts it right at the bottom of the viewport
- 0.9 puts it 90% down

- we can also choose a different trigger element

```javascript
var ourScene = new ScrollMagic.Scene({
  triggerElement: "#project01 img",
  // duration: 300
  duration: "90%",
  triggerHook: 0.9
});
```

## Reverse and each loop

- if you remove the duration all together the animation fades in but then stays
- but if you scroll back up past the start it will fade out again
- to avoid that you can use the reverse: false option

```javascript
var ourScene = new ScrollMagic.Scene({
  triggerElement: "#project01 img",
  triggerHook: 0.9,
  reverse: false
});
```

- for multiple scenes you could write extra scenes for each project section, each with a different name. eg. ourScene2
- not very flexible, too much code - especially if you add extra projects
- a better option is a loop

```javascript
// Init ScrollMagic
var controller = new ScrollMagic.Controller();

// create our loop
$(".project").each(function() {
  // build a scene
  var ourScene = new ScrollMagic.Scene({
    triggerElement: this.children[0],
    triggerHook: 0.9
  })
    .setClassToggle(this, "fade-in")
    .addIndicators({
      name: "fade scene",
      colorTrigger: "black",
      colorStart: "#75c695",
      colorEnd: "pink"
    })
    .addTo(controller);
});
```

- target **this** and the first child element which is the image

## A Simple Pinning

- to pin an element we can use the setPin() function

```javascript
var pinIntroScene = new ScrollMagic.Scene({
  triggerElement: "#intro",
  triggerHook: 0
})
  // .setPin("#intro")
  .setPin("#intro", { pushFollowers: false })
  .addTo(controller);
```

- this pins the whole intro section from the start & lets the projects scroll over the top
- adding pushFollowers: false when a duration is set means that as soon as the intro hits the next project it gets pushed up & unpins

- to pin the intro a second time you can add another scene

```javascript
var pinIntroScene = new ScrollMagic.Scene({
  triggerElement: "#intro",
  triggerHook: 0,
  duration: "30%"
})
  // .setPin("#intro")
  .setPin("#intro", { pushFollowers: false })
  .addTo(controller);

var pinIntroScene2 = new ScrollMagic.Scene({
  triggerElement: "#project01",
  triggerHook: 0.4
})
  // .setPin("#intro")
  .setPin("#intro", { pushFollowers: false })
  .addTo(controller);
```

## A Simple Parallax Effect

- to create this parallax effect we're login Greensock and the Scrollmagic Greensock plugin
- with our parallax scene

```javascript
// parallax scene
var slideParallaxScene = new ScrollMagic.Scene({
  triggerElement: ".bcg-parallax",
  triggerHook: 1,
  duration: "150%"
})
  .setTween(TweenMax.from(".bcg", 1, { y: "-50%", ease: Power0.easeNone }))
  .addTo(controller);
```

- you need to be careful with the y offset to avoid gaps - we've set the height of the image to 150% in the box so the largest we can go is -50%
- our css looks like this:

```css
.bcg-parallax {
  padding: 150px 0;
  color: #fff;
  background-color: #000;
  text-align: center;
  position: relative;
  overflow: hidden;
}
.bcg {
  background: url("../img/img_background.jpg") no-repeat;
  background-size: cover;
  position: absolute;
  width: 100%;
  height: 150%;
  top: 0;
  z-index: 1;
  opacity: 0.7;
}
.content-wrapper {
  width: 90%;
  margin: 0 auto;
  max-width: 1140px;
  position: relative;
  z-index: 2;
}
```

- our final version uses a timeline instead as part of the setTween

```javascript
// parallax scene
var parallaxTl = new TimelineMax();
parallaxTl
  .from(".content-wrapper", 0.4, { autoAlpha: 0, ease: Power0.easeNone }, 0.4)
  .from(".bcg", 2, { y: "-50%", ease: Power0.easeNone }, 0);

var slideParallaxScene = new ScrollMagic.Scene({
  triggerElement: ".bcg-parallax",
  triggerHook: 1,
  duration: "100%"
})
  .setTween(parallaxTl)
  .addTo(controller);
```

- we also add offsets so the image starts immediately and the content wrapper comes in a bit later but faster
