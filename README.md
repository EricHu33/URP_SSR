# Screen Space Reflection For Unity 6 And Unity 2022 [URP]

The project impl SSR shader that works on Unity 6 and Unity 2022.

It provide 3 methods to perform SSR tracing, mostly for my own references.
 - #### view space trace
   > (has oversampling and undersampling issue due to perspetive nature)
 - #### screen space trace
   > (based on Morgan McGuire's fast Screen Space Ray Tracing paper )
 - #### Hi-Z trace
   > (based on GPU Pro 5' SSR, also heavly modified from JoshuaLim007's impl, see references)

   > for rougher surface pixel, the Hi-Z method will fallback to use screen space trace, which imporves performance.

## Both deferred and forward path are support.
When using the forward path, it creates a "thin" G-buffer that contains the scene's metallic and smoothness properties from each renderer's standard lit shader.


> ## If TAA(or STP in Unity 6) is enabled, it will mixed the SSR result with previous frame. So it gives a extra bounce of reflection.

It also proivide option to trace the reflection in half scale (0.25 scale has aliasing issue, I should fix it in the future)
![{031FE112-CBA8-43EF-896E-D40C387018B5}](https://github.com/user-attachments/assets/6f8a1d6d-934f-4100-89f3-bb58408cae37)

In Unity 6, I also make it support render graph API.
So the shader can run either on compatible mode or the new render graph.
With render graph enable, we can also use the STP (Spatial-Temporal Postprocessing) to downscale the native render scale. This way it improve performance by a lot.

![{3EDF0B1A-939A-4142-A76A-3EE6E5CF391A}](https://github.com/user-attachments/assets/77bc82df-16ba-4af8-b1d0-d5d190c17c8f)
![{672BBA13-F723-48CF-A1C4-36DD1526EE9E}](https://github.com/user-attachments/assets/e9b94cde-2101-43ac-a713-98d7f8a417df)

#### Known Issue
Somtimes the depth texture used by the SSR will act in undefiend behaviour if you swtich between different reder scale, turn on/off STP. 

Remove the SSR renderer feature and re-add it can solve it. I plan to solve the issue in the future.

## References

[JoshuaLim007's SSR](https://github.com/JoshuaLim007/Unity-ScreenSpaceReflections-URP)

[Screen Space Reflections : Implementation and optimization – Part 1 : Linear Tracing Method – Sugu Lee (wordpress.com)](https://sugulee.wordpress.com/2021/01/16/performance-optimizations-for-screen-space-reflections-technique-part-1-linear-tracing-method/)

[bitsquid: development blog: Notes On Screen Space HIZ Tracing](https://www.jpgrenier.org/ssr.html)

[Stochastic Screen-Space Reflections (Frostbite Engine) - EA](https://www.ea.com/frostbite/news/stochastic-screen-space-reflections)

[Low Complexity, High Fidelity: The Rendering of INSIDE](https://www.youtube.com/watch?v=RdN06E6Xn9E)
