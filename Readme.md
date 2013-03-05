# Anima
CSS animations with a soul

With Anima it's easy to animate over a hundred objects at a time.
And it's only **4k** when gzipped.

**[Examples](#examples)**

### Browser support
Chrome, Safari, Firefox, Internet Explorer 10

## Motivation
CSS animations have some limits, the main is that you can't really have full control over them. And it's near impossible to stop transitions without dirty hacks.

Another problem is calculating percents for keyframes. People create animations with time in mind, not percents. You always think of _"it should fly and rotate for a half of a second, then stand still for another second and continue flying"_, and not `0% start 50% fly 70% stop 90% fly`.

Anima gives you the ability to use delays and durations normally, even for pure CSS animations. It has two modes: `JS` and `CSS`. The first one doesn't really use CSS transitions or `@keyframes`. On the contrary, it uses CSS transforms and 3d-transforms together with Javascript to create animation. You have full control over the flow, so you can start, stop, cancel animations and even create event-based stuff. `CSS` mode allows you to generate pure CSS animations, but it has limitations for parallel animations.

_**Note**: `CSS` mode is experimental for now, not everything works as expected._

## API

[Single](#single-animation) | [Sequence](#sequential-animations) | [Parallel](#parallel-animations) | [Infinite](#infinite-animations) | [Control methods](#taking-control) | [Events](#animation-events) | [Easings](#timing-functions) | [Timeline](#timeline)

At first you have to initialize the World, so the frame loop will start (so called `JS` mode)
```js
var world = anima.js()
```
You may also get the world without frame loop for pure css animations (`CSS` mode)
```js
var world = anima.css()
```
then you have to add items, you want to animate later
```js
var item = world.add(document.querySelector("div"))
```
so the world is looping now, waiting for item transformations to animate

API remains the same in `CSS` mode, but you have to call the `.css()` method explicitly at the end of desired `.animate`'s
```js
item.animate(...).css()
```

### Single animation
Arguments:
1. The map of transformations to apply. Only `translate`, `rotate`, `scale` and `opacity` are currently supported, but the list will expand.
2. Animation duration
3. Easing function
4. Animation delay
```js
item.animate({translate: [x, y, z]}, 500, 'ease-in-out-quad', 100)
```
It's also possible to pass everything in a single object
```js
item.animate({
  translate: [x, y, z],
  opacity: .5,
  duration: 500,
  ease: 'ease-in-out-quad',
  delay: 100
})
```
_**Note**: transformations' values are relative to the last known `Item` state, its initial state or the state after previous animation. Angles for `rotate` should be in degrees._

### Sequential animations
You can create sequential animations with ease :)
```js
item.animate(...).animate(...).animate(...)
```

### Parallel animations
Sometimes you need to transform something in parallel
```js
item.animate([{
	translate : [x,y,z],
	duration: 500,
	ease: 'ease-in-out-quad',
	delay: 100
},{
	rotate : [angleX,angleY,angleZ],
	duration: 1000,
	ease: 'ease-in-expo',
	delay: 400
}])
```
So you basically pass an array of transformations to create parallel animation.

### Infinite animations
You can call `.infinite()` at the end of `.animate`'s chain, to make the animation infinite.

### Taking control
#### `JS` mode
Animations start automatically as soon as you call `.animate()` on the item.
there are three control methods available
`pause`, `resume` and `stop`
they can be called on an item, or on the whole world
```js
item.pause()
world.stop()
```

#### `CSS` mode
To start your animations you have to call `.css()` after all you desired `.animate`'s, it will return a custom CSS object, that has `pause`, `resume` and `stop` methods.
```js
var animation = item.animate(...).animate(...).css()
animation.pause()
animation.stop()
```

### Animation events
Every animation has it's own `start` and `end` events.
```js
item.animate(...).on('start', callback).on('end', callback)
```

### Timing functions
Here's the list of al supported timing functions
`linear`

`ease-in-quad` `ease-in-cubic` `ease-in-quart` `ease-in-quint` `ease-in-sine` `ease-in-expo` `ease-in-circ` `ease-in-back`

`ease-out-quad` `ease-out-cubic` `ease-out-quart` `ease-out-quint` `ease-out-sine` `ease-out-expo` `ease-out-circ` `ease-out-back`

`ease-in-out-quad` `ease-in-out-cubic` `ease-in-out-quart` `ease-in-out-quint` `ease-in-out-sine` `ease-in-out-expo` `ease-in-out-circ` `ease-in-out-back`

You can learn more about them at [easings.net](http://easings.net)

### Timeline
Timeline is a separate `world` that is useful for debug and development. It has `play` `pause` and `stop` methods available like other worlds, but add `seek` method to seek animations.
```js
var world = anima.timeline()
world.add(...)
world.seek(500) // seek to 500ms
```
You can use [timeline example](http://lvivski.github.com/anima/example/timeline.html) as a reference

## Examples
### requestAnimationFrame
- [keyboard control](http://lvivski.github.com/anima/example/keyboard.html) (use `↑` `↓` `←` `→` and `W` `A` `S` `D` to transform)
- [animation chain](http://lvivski.github.com/anima/example/bounce.html)
- [parallel animations](http://lvivski.github.com/anima/example/parallel.html)
- [infinite animation](http://lvivski.github.com/anima/example/infinite.html)
- [delayed animation](http://lvivski.github.com/anima/example/delay.html)

### pure CSS
- [animation chain](http://lvivski.github.com/anima/example/bounce_css.html)
- [infinite animation](http://lvivski.github.com/anima/example/infinite_css.html)
- [delayed animation](http://lvivski.github.com/anima/example/delay_css.html)
- [parallel animations](http://lvivski.github.com/anima/example/parallel_css.html) (do not support custom `timing-functions` for now)

### mixed
uses both `JS` and `CSS` world at the same time

- [keyboard control](http://lvivski.github.com/anima/example/keyboard_mixed.html) (use `↑` `↓` `←` `→` and `W` `A` `S` `D` to transform)
