# GodotDeltaTimeTest
> The goal of this project is to demonstrate how delta time is used in Godot 4. The theory is applicable to all game engines that allows the use of delta time.

_⚠️Disclaimer: these techniques aren't necessarily mathematically exact or rigourous because digital computers cannot produce infinitely small values. This is "good enough" for what we're trying to achieve and in comparison to not using it._

## All displacements currently implemented
1. Linear
2. Quadratic

<!---
### Linear Movement
### Quadratic Movement

## What is delta time ?
Delta time or Δt represents a difference between two values t<sub>0</sub> and t<sub>1</sub> (w/ t<sub>0</sub> < t<sub>1</sub>) on the time axis.  
We could say that _Δt = t<sub>1</sub> - t<sub>0</sub>_

Now, the first reason to use Δt is for :
### Frame rate independence
Let's say we want to move a 100 units in one second with a game running at 30 frames per second. It could look like this:
``` 
#simulates number of frames per second (fps)
var expected_fps = 30
var speed = 100/expected_fps
var actual_fps = 30
#game running
for i in range(actual_fps):
  position.x += speed/expected_fps
```
> I've made an assumption for simplicity that in one second we will always have 30 frames (hence why actual_fps = expected_fps) .

The position will gain 100/30 ≈ 3.3 units every frame. Now we will end up moving approximately a 30\*3.3 ≈ 100 units in one second.  
But what if someone with a better computer could run the game at 60 fps? What if a low end computer could only run it at 15?  
What we expected our computer to do doesn't work anymore because they will actually be running at a different frame rate:  
3.3\*60 ≈ 200 units, 3.3*15 ≈ 50 units.  
This essentially is what is done when we have this in Godot:
``` 
var expected_fps = 30
var speed = 100
func _process(delta):
  position.x += speed/expected_fps
```
And delta represents our `actual_fps` variable from earlier.
---!>

