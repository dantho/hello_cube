# Hello World with Ambient
### [Page0](index.md) **([Page1](page1_hello.md))** [Page2](page2_entities_components) [Page3]() [Page4]() [Page5]() 
## 1.1. Create hello-world Ambient program:
```PowerShell
ambient new hello_cube
cd hello_cube
ambient run
```
![hello_quad](README_assets/hello_quad.png)
## 1.2. Edit src/lib.rs and replace 'quad' with 'cube' in 2 places.
```PowerShell
ambient run
```
![hello_cube](README_assets/hello_cube.png)
## 1.3. Add some color with this line on the 2nd Entity:
```Rust
    .with(color(), random::<Vec3>().extend(1.0))
```
Here, `color()` is a "Component" and it has associated data.  The data is a Vec4 holding R, G, B which we want to be random, plus the alpha channel, W, which we want to be 1.0.

**Troubleshooting:** In the code below, `color()` has red squigglies under it. That is Visual Studio Code's Rust-Analyzer add-in letting me know I have a compile error. I will also get this compile error if I attempt to `ambient run`.  Instead, I will fix it by choosing `Quick Fix...`

![](README_assets/import_color.png)  

And this is the fix:

![](README_assets/import_color_quick_fix.png)  

Which will be added to the already complicated/nested `use` statement at the top of `lib.rs`:  

![](README_assets/import_color_quick_fix2.png)  

And now your cube will be colorful!
```PowerShell
ambient run
```

![](README_assets/hello_cube_color.png)

**Troubleshooting:** Is your cube still white?  You must add the random color() to the _second Entity_.  The first Entity is the camera, which is invisible.  Perhaps your camera is invisible _and_ colorful?  

## 1.4. Let's get wild!  How about a sphere!
Change the "cube" to a "sphere" in two places in the code.  When I run this new code, I get a blank screen.  🙁  

Try replacing this
```Rust
    .with_default(sphere())
```
with this
```Rust
    .with_merge(make_sphere())
```
And repair the compile error with the suggestion or via the ever-helpful `Quick Fix...`.

![](README_assets/hello_sphere.png)

I don't know why _quads_ and _cubes_ just show up, but _spheres_ need to be _made_.  Let's find out together.