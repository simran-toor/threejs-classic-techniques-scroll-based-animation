# 20 SCROLL BASED ANIMATION 
- how to use Three.js as a background of a classic HTML page 
- make camera translate to follow the scroll 
- tricks to make it more immersive 
- add parallax animation based on cursor position 
- trigger animations when arriving at the corresponding sections 
## HTML SCROLL
- to reactivate scroll remove the **overflow** from css 
- to keep canvas behind when scrolling:
```
.webgl
{
    position:fixed
}
```
### ELASTIC SCROLL
- fix elastic scroll (when scroll too far get elastic animation with white at the bottom when page goes beyond its limit)
- not every user has this 
- make clear colour transparent and only set background colour on the page 
- set alpha property to true on WebGLRenderer 
- add background colour to the html in CSS 
## OBJECTS
- going to create object for each sections and use Three.js primitives
- create one material and use for all meshes (better for performance)
- using MeshToonMaterial 
    - only appears when there is light
### TEXTURES
- gradient is very small images composed of 3 pixels 
- instead of choosing one of those 3 pixels according to the light, WebGL tries to merge them 
- to solve this set the `magFiler` of the texture to `THREE.NearestFilter`
### POSITION
- in Three.js field of view is vertical
- if you put 1 object at the top and 1 on the bottom and resize the window the objects will stay at the top and bottom 
### PERMANENT ROTATION
- give more life to the experience by adding a permanent rotation to the objects
- add objects to a `sectionMeshes` array
- in  `tick` function, loop through the `sectionMeshes` array and apply a slow rotation by using `elapsedTime` 
- don't make objects move too fast, otherwise experience is not enjoyable for user 
## CAMERA
### SCROLL
- want to make camera move with the scroll
- retrieve scroll value with `window.scrollY`
- need to update value when scroll is happening by listening to the `scroll` event on `window`
- in `tick` function, use `scrollY` to make the camera move (before doing the render)
- camera is too sensitive to scroll and moving too fast 
- `scrollY` is positive when scrolling down but the camera should be negative 
    - change camera position to be negative 
- `scrollY` contains the amound of pixels that have been scrolled 
- each section has exactly the same sizes as the viewport, so when we scroll the distance of one viewport height, the camera should reach the next object 
    - to do this need to divide `scrollY` by the height of the viewport which is `sizes.height`
- camera is now going down by 1 unit but objects are seperated by 4 units
    - multiply by `objectDistance` 
### POSITION OBJECT HORIZONTALLY 
- position onkects to left and right to match titles 
### PARALLAX
- parallax is the action of seeing one object through different observation points
- this is done naturally by our eyes and it's how we feel the depth of things 
- going to apply a parallax effect by making the camera move horizontally and vertically according to the mouse movements
- retrieve cursor position 
    - create `cursor` object with `x` and `y` properties
- listen to the `mousemove` event on `window` and update those values
- can use these values directly, but it's always better to adapt them to the context 
- currently the amplitude depends on the size of the viewport and users with different screen resolutions will have different results
    - normalise the value (from`0` to `1`) by dividing them by the size of the viewport 
- instead of a value going from `0` to `1` it's better to have a value going from `-0.5` to `0.5`
    - do this by subrating `0.5`
- add to `tick` function by crating `parallaxX/Y` variables and putting `cursor.x/y` in them
- x and y not in sync and camera scroll gone 
    - invert `cursor.y`
- the scroll issue is caused by the updating of the camera position y twice and the second one replaces the first 
    - to fix, put camera in a `Group` and apply parallax on the group and not the camera itself
### EASING
- parallax animation feels too mechanic and it's not realistic
- add "easing" (aka "smoothing" or "lerping") 
- on each frame, instead of moving the camera straight to the target, going to move it a 10th closer to the target and a 10th closer on next frame etc. 
- calculate the distance from the actual position to the destination 
- if you test the experience on a high frequency screen, the `tick` function will be called more often and the camera will move faster towards the target, this isn't accurate and it's preferable to have the same result across devices
    - need to use the time spend between each frame
    - `deltaTime` is in seconds 
- lower amplitude of effect 
## PARTICLES
- good way to make the user experience more immersive and help feel the depth
- b/c positioning particles ourselves need to create custom `BufferGeometry`
- create a loop and add random coordinates to the `positions` array
- instantiate the `BufferGeometry` and set the `position` attribute
- create materials using `PointsMaterial`
- position particles on 3 axis
- can improve them with random sizes, random alpha and even animate them (like previous lesson)
## TRIGGERED ROTATIONS
- make objects do a small spin when we arrive at the corresponding section in addition to the permanant rotation 
### KNOW WHEN TO TRIGGER THE ANIMATION 
- need to know when section reached 
- use `scrollY` and maths 
### ANIMATING THE OBJECTS
- using `GSAP` library
- add to project and import
## GO FURTHER
- add more content to HTML
- animate other properties like the material
- animate the HTML text
- improve the particles
- add more tweaks to Debug UI
- test other colours
- Etc. 