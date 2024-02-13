Outline overview
================
Outline is used to draw a contour line around objects
- ## Outline using camera image, normal texture, depth buffer or some combination of these.
Simplier example of such outline is rendering 2D object. You can use just alpha channel from main texture to detect object edge and add outline.
For 3D object you can also detect color/normal/depth discontinuity, and if that is the case, draw outline.
- Outline using colors: this uses what is rendered without any post processing. It is useful for shadows outlining or small elements.
- Outline using normals: this uses normal texture of the object, to determin which pixels belong to edge. This method works only for models that have normal texture and gives smooth and uniform outline, but can be invisible on very flat surfaces.
- Outline using depth: this uses depth buffer, to determin which pixels belong to edge. This method works for both models and brush geometrh and gives sharp and contrast outline, but can be incorrect on very close objects.

More information about combination of this methods in [Edge Detection Outlines][1] and [Object Outline][2]
- ## Outline using [Rim Effects][3]
This outline uses `freshel effect` by calculating dot product between normalized normal vector and view direction, exponentiating the result. For front surfaces effect is near zero, but for side surfaces effect grows till one. Thats why this approach works well for objecst with smooth and round edges like spheres and capsules, but it breaks down for ojbecs like cubes or  more complex models that have sharp edges (because there is no side surface perpendicular to view direction).
- ## Outline using [Vertex Extrusion][4]
This outline uses `re-rendered/duplicate` version of original object to form outline. Depending on growing method has pros/cons:
- Using vertex position: simply scale it up. Outline depends on dimensions of object and convexity of its shape, for example such concave figure as empty moon will not have outline for internal edges at all.
- Using normal vector: by moving vertices along their normal vector. As in [Rim Effects][3] dependence from normals gives artifacts for any object with sharp angles.
- Using vertex color: by moving vetices along their vertex color. The logic behind this is that you can generate custom normals and store in color channels of object. Big downside is the manual setup involved since you need to generate custom normals for object.
- ## Outline using [Blurred Buffer][5]
This outline uses rendering silhouette of object to buffer, which is then blurred, which expands the silhouette, which then used to render outline. This outline is soft and glowy, but can have bigger impact on performance compared to other methods.
- ## Outline using [Jump Flood Algorithm][6]
This outline uses somethings like perpixel grid-propagation starting from border (which determined by mask edge at init stage), with multiple stages decreasing grid distance at each stage. This method is best for extrawide perfomant outlines

[1]: https://ameye.dev/notes/edge-detection-outlines/
[2]: https://medium.com/@erikkubiak/wip-part-2-object-outline-d9a420a4bc2c
[3]: https://ameye.dev/notes/rendering-outlines/#rim-effects
[4]: https://ameye.dev/notes/rendering-outlines/#vertex-extrusion
[5]: https://ameye.dev/notes/rendering-outlines/#blurred-buffer
[6]: https://bgolus.medium.com/the-quest-for-very-wide-outlines-ba82ed442cd9
