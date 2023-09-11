  
# Hydral 
Hydral is a mini-notation for [Hydra](hydra.ojack.xyz/) inspired by [Tidal mini-notation](https://tidalcycles.org/docs/reference/mini_notation/).

## Principles:

The notation enables time and spatial patterns.

The language aims to be intuitive and simple.

The language aims to be compact.

## Learn Hydral

### Color

Use HTML basic color expressions:

```javascript
"black"
// or "rgb(0,0,0)"
// or #000, or #000000, or ...
```

### Time

Use commas to tell what happens first, and what follows.

```javascript
"black, white"
```

### Space

Use spaces to create vertical separations

```javascript
"black white black white black"
```

This creates 5 alternating black and white vertical stripes.

And use  `\` or multi-line strings for horizontal separations:

```javascript
" black white \ white black"
```

```javascript
" black white \
  white black"
```

### Repetition

Repeat your patterns using `*` operator, and wrap your repeated patterns in brackets (as in tydal's mini notation).

```javascript
// 8 vertical black and white stripes
"[black white]*4"

// 8 horizontal black and white stripes
"[black \
  white]*4"
```

As in these examples, the repetition operator `*` repeats the outermost space expression (vertical or horizontal). You can also use `|*` or `\*` for explicit vertical or horizontal repetitions:

```javascript
"[[black white]*4 \
  [white black]*4]*4"
```

Inspired by hydra, default repetitions are 3 times horizontal and 3 time vertical:

```javascript
// 6 vertical stripes
// alternating black stripes
// and stripes alternating blue and red colors 3 times 
"[black blue \
  black red]*" // same as [black [blue \ red]*3]*3

// same, but repeating the pattern 5 times vertically and 2 times horizontally
"[black blue \
  black red]*(5,2)"
```

Other advanced patter notations also available and inspired in tydal:

|operator | function|
|--|--|
|  \| 	| Create a random choice |
| <  > |	Alternate between patterns |
| ! 	| Replicate a pattern |
| _ 	| Elongate a pattern |
| @ 	| Elongate a pattern |
| ? 	| Randomly remove events from pattern|

```javascript
// black blue black red
"black  <blue red>"

// black black black blue
"black!3 blue"
```

### Rythm

```javascript
// one beat black, next beat white
"black, white"
```

Group events using brackets, and use operators to create rhythm:

```javascript
// blue beat, followed by half black beat and half white beat
"blue, [black, white]"

// alternating between black and white four times per beat
"[black, white]*2"
		  
// two black beats followed by a half white beat
"black / 2, white * 2"
```

### Transformations
The following table summarizes the available transformations, with examples bellow.

| operator|transformation|
| --|-- |
| < | rotation|
| ~ | transparency |
| < | kaleidoscope |
| <- | scroll |
| << | scroll with speed param |


#### Rotation
 
use `<` operator for rotations, ()default value 90):

```javascript
// black and white horizontal stripes
"[black white]>"

// black and white screen, with a 30º slope
"[black white]>30"
```

#### Transparency

Use `~` or follow a color with `.amount` for transparency

```javascript
// black and gray screen
"[black white~.5]"
// or
"[black white.5]"
```

#### Kaleidoscope

Use `>` for kaleidoscopic reflections:

```javascript
// a white triangle over a background using a kaleidoscopic transformation
"[black white]<3"
```

#### Scroll

scroll using `<- scroll` (default <-.5) or `<< speed`, (default <<1);

```javascript
// white black
"[black white]<-"
		  
// "black white / 2 black"
"[black white]<-.25"
		  
// black and white screen scrolling left
"[black white]<<.1"
		  
// 2 horizontal white and black stripes using scroll and rotation
// "white \
//  black "
"[black white]<->90"

// "black \
//  white /2 \
//  black"
"[black \
  white]<-.25>" // note that 90 is the default rotation param
```

### Blending and Mixing

There are many ways of combining visuals. Use `+ - x / % ;` to add, substract, multiply, divide, blend or layer patterns.

```javascript
// gray screen blending black and white
"black % white"
		  
// a darker gray
"black %.25 white"
		  
// and a lighter one
"black %.75 white"
		  
// "white white \
// black white" using rotation and addition
"[black white] + [black white]>90"

// "black black \
// black white" using rotation and substraction
"[black white] - [black white]>90"
		  
// "black red \
//  white red" using transparency and layering
"[black white] ; [~ red]>90"
		  
// "black dark-red \
//  white pink" using transparency and layering
"[black white] ; [~ red.5]>90"
```

### Effects

Use first letter of hydra color functions, or `^` for shift as a shorthand.

| operator | function |
|--|--|
| p | posterize |
| px | pixelate |
| ^ | shift |
| i | invert |
| c | contrast |
| b | brightness |
| l | luma |
| t | thresh |
| c | color |
| s | saturate |
| z *for zoom* | scale |
| h | hue |
| c | colorama |
| r | r |
| g | g |
| b | b |
| a, or ~ | a |

```javascript
// black, using invert function
"white.i"
// or
"white.i()"
	  
// gray, using brightness parameter of white
"white.b(.4)"
```

### Other visual sources

So far we have only used color and transparency as the source of our visuals (as well as some transformations), but hydra offers different video sources.

| operator | source |
|---|---|
| n | noise |
| v | voronoi |
| o | osc |
| s | shape |
| g | gradient |
| ic | initCam |
| iv | initVideo |
| is | initScreen |
| i | init |

```javascript
// oscilator
"o"
		  
// noise( scale = 10, offset = 0.1 )
"n(10,0.1)"
```

```javascript
// the webcam input blended with black
s0 = ic();
"s0 % black"
```

### Modulations

Hydra offers visual modulations, that apply transformations with an intensity that depends on another visual sour

ce. We will use `.m` function, with the operator as the first parameter, followed by the texture and remaining parameters.

```javascript
// Modulate repeat a pink triangle with an oscilator
// shape(3).modulateRepeatX(noise()).out(o0)
"s(3).c('pink').m(*,o)"
```

### Using patterns for parameters
 
Until now, hydra used simple pattern expressions for parameters, for instance:

```javascript
// smoothly move a triangle back and forth horizontally
shape(3).scrollX([-0.2,0.2].smooth()).out(o0)
```

First things firsts, we use `$` for that sweet *smooth operator*

```javascript
// smoothly move a triangle back and forth horizontally
"s(3)<-[-0.2,0.2]$"
		  
// and use the smooth operator for hydral patterns too
// painting a vertical gradient from black to white
"[black white]#"
```
now, mini-notation refers not only to colors and visual functions, but to numbers too; and we can combine them, as they should only refer to one type of pattern at a time:

```javascript
// half screen black, the other with a pink shape that changes from a triangle to a square and back
"black s([3,4]#).c(pink)"

// a pink shape that transform from triangle to square and back, and then from triangle to penthagon
"s([3,<4,5>]#).c(pink)"
```

#### Speed

We can use `*` and `/` operators for speed.
nomeric expressions should be put in brackets to avoid ambiguous evaluations: `3/3` is still 1, and not the value 3 for 3 beats.

```javascript
// a pink triangle for the first 3 beats, then changing to a square and a triangle in the following beat
"s([[3]/3,4]#).c(pink)"
```
