# Greensock 101 - Petr Tichy

## Selectors
- we can achieve a simple fade out and move down with the .to method

```javascript
var header = $('#header');

gsap.to(header, {opacity: 0, y: 50, duration: 1});
```
- we target the element we want
- then use the gsap.to
- duration has moved into the options

## TweenLite
- we now just include the gsap script instead of tweenLite
- we also don't need to add the CSS plugin as that now comes included in the core library
- using the .to method we can animate from the original CSS to our new css

```javascript
// animate scale down to 200px
gsap.to(img, {width: 200, duration: 1})

// animate to the left 200px
gsap.to(img, {x: 200, duration: 1})

// reverse the direction
gsap.to(img, {x: -200, duration: 1})

// animate to the left 200px
gsap.to(img, {x: '100%', duration: 1})
```
- with the .from method we animate in from our settings here to the original CSS

```javascript
// animate in from our settings to the original CSS with
gsap.from(img, {x: -200, duration: 1})
```
- or we can use the fromTo method to animate between settings

```javascript
gsap.fromTo(img, {x: -200}, {x: 200, duration: 1})
```
- you can use the .set method to simply set some CSS
```javascript
gsap.set(img, {x: -200, y: 300})
```
- we can animate two elements and use the delay option to stagger it

```javascript
var img = $('img'),
			h2 = $('h2');

	// Simple Tween
	gsap.from(img, {x: -200, duration: 1});
	gsap.from(h2, {autoAlpha: 0, delay: 1, duration: 1});
```

## Easing

- easing adds some more interest to out animations
- we don't need a plugin any more except for a few special easings like RoughEase or SlowMo
- [the visualiser it great](https://greensock.com/docs/v3/Eases)
- we add an easing as one of our options
  ease:Power0.easeNone
1. first we add ease
2. then we set the power 0 is none 4 is the max value
3. then the type of easing we want

```javascript
// starts fast then slows down as it comes in
gsap.from(img, {x: -200, duration: 1, ease:Power1.easeOut});

// starts slow then speeds up as it comes in
gsap.from(img, {x: -200, duration: 1, ease:Power1.easeIn});

// You can also write it as a string
// seems to be the new way to do it
gsap.from(img, {x: -200, duration: 1, ease: "power1.in"});

// Using elastic
gsap.from(img, {x: -200, duration: 3, ease: "elastic.out(1, 0.3)"});

// with Bounce
gsap.from(img, {x: -200, duration: 1, ease: "bounce.out"});
```
## Simple Callback Functions
- we can run callback functions at different points in our animation: onStart, onUpdate (during) & onComplete

```javascript
gsap.from(img, {
		x: -200,
		duration: 1,
		ease: "power1.in",
		onStart: onStart,
		onUpdate: onUpdate,
		onComplete: onComplete
	});

	function onStart() {
		console.log('animation started');
	}
	function onUpdate() {
		console.log('animation is in progress');
	}
	function onComplete() {
		console.log('animation completed');
	}
```
## Animating Multiple Objects

- you could do it with a .from each time but better to use a timeline
- first we setup our timeline
- then we can add our .to() or .from() methods

```javascript
var tl = gsap.timeline();
	// Animating Multiple Objects

	tl
		.from(h1,{y: -15, autoAlpha: 0, duration: 0.3, ease: "power1.in"})
		.from(intro, {y: -15, autoAlpha: 0,  duration: 0.3, ease: "power1.in"}, '-=0.15')
		.from(img, {y: -15, autoAlpha: 0,  duration: 0.3, ease: "power1.in"}, '-=0.15')
		.from(h2, {y: -15, autoAlpha: 0,  duration: 0.3, ease: "power1.in"}, '-=0.15')
		.from(listItem, {y: -15, autoAlpha: 0,  duration: 0.3, ease: "power1.in"}, '-=0.15');
```
- we can delay the animation slightly at the end '-=0.15' start 0.15 before the end of the previous item
- you can also save some repetition using defaults for some of the options

```javascript
var tl = gsap.timeline({ defaults: {duration: 0.3, ease: "power1.in"}});
	// Animating Multiple Objects

	tl
		.from(h1,{y: -15, autoAlpha: 0})
		.from(intro, {y: -15, autoAlpha: 0}, '-=0.15')
		.from(img, {y: -15, autoAlpha: 0}, '-=0.15')
		.from(h2, {y: -15, autoAlpha: 0}, '-=0.15')
		.from(listItem, {y: -15, autoAlpha: 0}, '-=0.15');
```
## Adding Tweens To A Timeline
- including the '-=0.15' to the end of a tween is called a relative positioning of tweens
- -=0.15 adds an offset that starts the tween early
- +=1 starts the tween 1 sec later, so adds a delay
- absolute positioning lets you add an exact time
  .from(img, {y: -15, autoAlpha: 0}, 3)
  - this starts 3 sec in
- you can apply absolute positioning at different points

```javascript
tl
		.from(h1,{y: -15, autoAlpha: 0})
		.from(intro, {y: -15, autoAlpha: 0}, '-=0.15')
		.from(img, {y: -15, autoAlpha: 0}, 3)
		.from(h2, {y: -15, autoAlpha: 0}, 2)
		.from(listItem, {y: -15, autoAlpha: 0}, 2);
```
- you can also add labels to reference in the timeline

```javascript
tl
		.from(h1,{y: -15, autoAlpha: 0})
		.add('intro')
		.from(intro, {y: -15, autoAlpha: 0}, '-=0.15')
		.from(img, {y: -15, autoAlpha: 0}, '-=0.15')
		.from(h2, {y: -15, autoAlpha: 0}, 'intro')
		.from(listItem, {y: -15, autoAlpha: 0}, '-=0.15');
```
- now the h2 comes in at that point in the timeline that we added the intro label
- you can also add a delay on the label reference
  .from(h2, {y: -15, autoAlpha: 0}, 'intro+=2')

## Controlling Timeline Playback
- we can control the timeline with functions

```javascript
$('#btnPlay').on('click', function() {
		tl.play();
  })


tl.play();
tl.pause();
tl.resume();
tl.reverse();
tl.timeScale(8); // speeds it up - by 8x
tl.timeScale(0.5); // slow it down by half

tl.seek(1); // 1 sec in
tl.seek('intro'); // go to intro label
tl.progress(0.5); // go to 50% of timeline 0 - 1
tl.restart(); // go to 50% of timeline 0 - 1

```
## Staggering Animations
- to add a stagger to our timeline we choose and element and add stagger as an extra option

```javascript
.from(buttons, {x: 200, ease: "power2.inOut", duration: 0.2, stagger: 0.1 });
```
- we can have multiple timelines on the page
- on our timeline we can add options:

```javascript
// pause timeline at the start
tl = gsap.timeline({paused: true})

// repeat infinitely
tlLoader = gsap.timeline({repeat: -1});

// repeat 2 times then run a callback function
tlLoader = gsap.timeline({repeat: 2, onComplete: loadContent});
```
