#Illumination map

###### *version :2.0.1beta   Update:2019-3-19*

Illumination mapping refers to the projection, shadow transition, lighting atmosphere, color influence between model material and material produced by 3D model in scene.

There are very few 3D game scenarios that rely on lighting and model rendering instantly to produce projection and color effects. This is a very expensive way of performance, especially mobile games. The function of the video card of mobile phones is not strong. All of them will become Caton very quickly with real-time video and shadow games.

Scene illumination mapping is to solve this problem. It simulates the light, shadow and color of game scenes in the way of mapping, and reduces a lot of real-time operations.

Illumination mapping is proposed to render and export the illumination mapping through the unity3D editor. When loading the scene, the engine will automatically load the illumination mapping to achieve better results.

If the lighting map is not rendered in Unity, the engine will not load the error after exporting, but the effect of the game will be discounted.

![] (img/1.png)<br> (Fig. 1) No light mapping was used

![] (img/2.png)<br> (Figure 2) uses light mapping



> ####About the inconsistent export effect, we can refer to the related support part of "Introduction Chapter of Scene Rendering Configuration".