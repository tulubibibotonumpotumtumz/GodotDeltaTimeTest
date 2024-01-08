# GodotDeltaTimeTest
> The goal of this project is to demonstrate how delta time is used in Godot 4. The theory is applicable to all game engines that allow the use of delta time to their users (it would be a very bad one if it didn't).

## All displacements currently implemented
1. Linear
2. Quadratic
---
I like to start from the speed function when position doesn't matter and then do the integral to figure out how much I moved to add it to my position (x or y) variable.
I can also go the other way and decide how much I want to move in one second if that's important and then find the speed with the derivative if necessary.
  
Aside from helping with varying frame rates, Δt also helps with imitating a continuous function and get more accurate values.  
In fact, all the formulas/notations (integrals, derivatives) with _dt_ in them are here replaced with Δt, a more discrete value (we obviously can't get infinitely small with digital computers) that gets more and more accurate results as the frame rate increases but does a "good enough" job if it's low as well. It's all about using Riemann sums without going into crazy concepts like infinity which is way more intuitive and easy to play with.
  
I'm no math major but that's probably enough to get by with using delta time in most circumstances.

---
### Linear Movement
I decided to make the speed function _s(t) = 2_ which is implemented as such:  
```gdscript
#linear speed
var lin_speed = 2
func _process(delta):
	position.x += lin_speed*delta
```
By doing the integral we can get _S(t) = 2t_ and when it is displayed, this is what it returns for the position:
```
t=0: 0.23333333432674
t=1: 2.17475533485413
t=2: 4.17475366592407
t=3: 6.17475175857544
t=4: 8.1747522354126
```

### Quadratic Movement
The speed function is described by _s(t) = 2t + 2_ which gives:
```gdscript
var quad_speed = 2
var acceleration = 2
func _process(delta):
	position.x += quad_speed*delta
	quad_speed += acceleration*delta
```
To find how much we actually moved we find the integral _S(t) = t<sup>2</sup> + 2t_ with the values:
```
t=0: 0.30000001192093
t=1: 3.2512321472168
t=2: 8.37642574310303
t=3: 15.5016193389893
t=4: 24.6312084197998
```
There's also a way that might seem more intuitive to get the same result:
```gdscript
func quad(t):
	return t**2 + 2*t
func _process(delta):
	position.x = quad(quad_t)
	quad_t += delta
```
The returned values are similar so I guess they are both techniques to create movement. The only thing we can notice is that we have to set a value for the position instead of incrementing the values like the previous technique. If I had to guess it's probably because the definite integral _F_ represents a net change from the start of the interval. So, in order to get F's value at a specific point _t_, we have to sum up all the values up to _t_. With the new technique, we already have the _F_'s expression so we only need to plug the values in. The incrementing approach has its limits when, for example we want to model something that never derives to a constant (exponential function). This is why the other displacement functions that fit this criteria (or even polynomials but with too high of a degree) will use this new technique. This won't change much to our method, we can still find the speed first and then find the integral if we want to.  
  
Another solution is simply to write the speed function and increment it to the position. This will be closer to what we were already doing with the only exception being that we still increment time with delta.
```
func quad_speed(t):
	return 2*t + 2
var quad_t = 2
func _process(delta):
	position.x += quad_speed(t):
	quad_t += delta
```
  
The only issue I ran into with all these techniques is that if they didn't start from frame 0, they didn't work return the same results as expected. I can't figure out yet why this working this is behaving so strangely.

## Notes
--- 
There are a few details that I haven't talked about although you probably shouldn't take my word for most of what was or will be said here. It's just my own experience and nothing showed me that it didn't work.  
If we sum up all delta time during one second, we should get approximately 1. Some frames can take longer to render and some less time. Due to that fact, we can get less or more frames within one second.

---
