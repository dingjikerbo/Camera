# glsurfacepreview2工程

本工程和glsurfacepreview相比，区别在于glsurfacepreview拿到相机帧后直接绘制到屏幕，而本工程需要先对相机帧做一些处理，再绘制到屏幕。

由于中间多了一层处理的过程，所以这里创建了一个FBO和一个纹理，将纹理挂载在FBO上，先通过YUVProgram将相机帧离线绘制到该纹理上，然后再通过FilterTextureProgram滤镜将该纹理绘制到屏幕上。这个滤镜效果只是简单的给图像染红。

这个过程非常常见，相机帧要经过一些处理才能展现给用户，如做一些滤镜效果。通常的做法就是先创建一个FBO，然后挂载一个离线纹理到该FBO。绘制时先绑定到该FBO上，那么接下来所绘制的东西都在该FBO下挂载的纹理上了。当所有的离线绘制完成后，再将该纹理绘制到屏幕上即可，注意此时要解除绑定该FBO，这样绘制才会回到默认的屏幕上。