## Project Details

`Contributor name:` Kunal Agarwal   
`Mentors:` Paul Elder, Laurent Pinchart, Kieren Bingham  
`Organization:` libcamera   
`Project:` A GPU-based software ISP implementation

## Aim of the project

- To test and create an interface within the simple pipeline handler, replacing the existing converter, for GPU-based software ISP.
- To implement and add ISP functions/algorithms to the software ISP using OpenGL Compute Shaders

## Overview and Work done

- Interface for GPU processing was created by replacing the existing converter by my GL converter. It involved use of GBM for creating, exporting and allocating buffers.
    ### What is GBM?
    Generic Buffer Management (GBM) is an API that provides a mechanism for allocating buffers for graphics rendering tied to Mesa. GBM is intended to be used as a native platform for EGL on DRM or openwfd. The handle it creates can be used to initialize EGL and to create render target buffers

- Existing converter used V4L2 API for most of the task, which had to be and was replaced. Appropriate way to import and allocate buffers was researched and asked on upstream channels, which involved use of specific API calls.

- Appropriate OpenGL API and its version, for the project was explored and used, which was compatible with libcamera and the hardware I was working on(RaspberryPi 4). i.e. OpenGL ES
- Egl api was explored and used to create a non-windowing system and surface less context for rendering.

    ### What is OpenGL ES and EGL?
    OpenGL for Embedded Systems (OpenGL ES or GLES) is a subset of the OpenGL computer graphics rendering application programming interface (API) for rendering 2D and 3D computer graphics such as those used by video games, typically hardware-accelerated using a graphics processing unit (GPU). It is designed for embedded systems. OpenGL ES is the "most widely deployed 3D graphics API in history".
    We have used OpenGL ES3 in our GL converter

    EGL is an interface between Khronos rendering APIs (such as OpenGL, OpenGL ES or OpenVG) and the underlying native platform windowing system. EGL handles graphics context management, surface/buffer binding, rendering synchronization, and enables "high-performance, accelerated, mixed-mode 2D and 3D rendering using other Khronos APIs

- The goal of zero-copy of data was achieved by use of dmabufs, importing image and texture using dmabufs. It involved tackling issues like proper formats for buffers (limitation of GBM), creating egl image with proper configuration and exporting texture handles.
- Texture and shader classes were implemented for handling them, with proper classes and functions.
- Malvar-he-cutler demosaicing algorithm was understood in depth and changed for RAW-10 format instead of the existing one for RAW-8 format. Also got the shader for demosacing working with qcam. A patch got merged for the same
- Testing shaders were written using GLSL for debugging and getting the interface working

## Contributions

### Patches
All the submitted patches can be seen [here](https://patchwork.libcamera.org/project/libcamera/list/?series=&submitter=116&state=*&q=&archive=both&delegate=)

### Progress tracker

A [pad](https://pad.libcamera.org/code/#/2/code/edit/PQ4jhJAUUG+b97uPfqMfX8gR/) and [doc](https://docs.google.com/document/d/1TC_eCvXlilo2jxdJkw_JjTwy8ZhDDd55QYaZ9mEBpSo/edit) was made to track the progress.

### Github Repository
All the development was done in my [github repository](https://github.com/KunalA18/libcamera-gsoc/tree/kunal-dev)   
I have my implementation in `src/libcamera/pipeline/simple/`

Referring to [here](https://libcamera.org/getting-started.html), The dependent packages are required for building libcamera. I have put my GSoC work into the branch named kunal-dev. Then you can fetch the kunal-dev branch, build and install using below commands:

Build instructions:
```
git clone https://github.com/KunalA18/libcamera-gsoc.git
cd libcamera
ninja -C build install
```
Test instructions:
```
cd build/src/qcam
./qcam
```
### Hardware used
- RaspberryPi 4 + ov5647 camera module.


## Future prospects

- Create a library for the GPU based ISP to be used by any pipeline handler using the implemented GL converter and associated classes.
- Add other ISP algorithms like Black-level correction, Auto-white balance and so on to our GPU based ISP.

## Conclusion

I had a great time working with everyone at libcamera. I would like to specially thank my mentors, Laurent Pinchart and Paul ELder for their constant support and motivation. I'd also like to thank Kieren Bingham for guiding me during the last phase of the project. It was indeed an important and challenging project. I learned a lot during this journey, from writing good quality code to techniques for debugging code at low level. I appreciate the fact that mistakes made by me were returned with concise criticism rather than being reprimanded for them. I will continue to work on this project further and continue being a part of the libcamera community.
