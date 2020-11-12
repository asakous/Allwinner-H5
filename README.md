# Allwinner-H5

Howw to build Retroarch on H5 platform</p>

sudo apt install cmake mc quilt libasound-dev zlib1g-dev libudev-dev libfreetype6-dev libdrm-dev libgbm-dev libx11-dev fonts-droid-fallback libwayland-client0</p>


1:image from armbian 5.4.75 or 5.8 . it dosen't matter. because It will be replaced by custom build kernel.</p>
2:armbian build environment(select legacy sunxi64).</p>
  kernel config</p>
  remove lima driver</p>
  enable EXPERT mode</p>
  enable DRM_FBDEV_LEAK_PHYS_SMEM</p>
  after build finished . copy to H5 board and install it. </p>
  edit /boot/armbianEnv.txt  add a line [extraargs=drm_kms_helper.drm_fbdev_overalloc=200 drm_leak_fbdev_smem=1 video=HDMI-A-1:1280x720-24@60]</p>
  and don't forget reboot once.</p>
3: mali kernel driver </p>
   https://github.com/mripard/sunxi-mali . </p>
   I use r9p0</p>
4: mali blob</p>
    https://github.com/superna9999/amlogic-meson-mali</p>
    I use m450/r7p0/wayland/drm</p>
    copy libMali.so to /usr/lib/aarch64-linux-gnu </p>
    copy amlogic-meson-mali/lib/* (links) to /usr/lib/aarch64-linux-gnu</p>
    and create a link ln -sf libMali.so libgbm.so.1.0.0 in /usr/lib/aarch64-linux-gnu/ folder</p>
    and copy amlogic-meson-mali/include/* to /usr/include/</p>
5: compile SDL2</p>
-DSDL_STATIC=OFF \
                         -DLIBC=ON \
                         -DGCC_ATOMICS=ON \
                         -DALTIVEC=OFF \
                         -DOSS=OFF \
                         -DALSA=ON \
                         -DALSA_SHARED=ON \
                         -DJACK=OFF \
                         -DJACK_SHARED=OFF \
                         -DESD=OFF \
                         -DESD_SHARED=OFF \
                         -DARTS=OFF \
                         -DARTS_SHARED=OFF \
                         -DNAS=OFF \
                         -DNAS_SHARED=OFF \
                         -DLIBSAMPLERATE=OFF \
                         -DLIBSAMPLERATE_SHARED=OFF \
                         -DSNDIO=OFF \
                         -DDISKAUDIO=OFF \
                         -DDUMMYAUDIO=OFF \
                         -DVIDEO_WAYLAND=OFF \
                         -DVIDEO_WAYLAND_QT_TOUCH=ON \
                         -DWAYLAND_SHARED=OFF \
                         -DVIDEO_MIR=OFF \
                         -DMIR_SHARED=OFF \
                         -DVIDEO_COCOA=OFF \
                         -DVIDEO_DIRECTFB=OFF \
                         -DVIDEO_VIVANTE=OFF \
                         -DDIRECTFB_SHARED=OFF \
                         -DFUSIONSOUND=OFF \
                         -DFUSIONSOUND_SHARED=OFF \
                         -DVIDEO_DUMMY=OFF \
                         -DINPUT_TSLIB=OFF \
                         -DPTHREADS=ON \
                         -DPTHREADS_SEM=ON \
                         -DDIRECTX=OFF \
                         -DSDL_DLOPEN=ON \
                         -DCLOCK_GETTIME=OFF \
                         -DRPATH=OFF \
                         -DRENDER_D3D=OFF \
                         -DVIDEO_X11=OFF \
                         -DVIDEO_OPENGLES=ON \
                         -DVIDEO_MALI=ON \
                         -DVIDEO_VULKAN=OFF \
                         -DVIDEO_KMSDRM=ON \
                         -DKMSDRM_SHARED=OFF \
                         -DPULSEAUDIO=ON</p>
                         
make sure kmsdrm option is on after configuration</p>
 
5: compile retroarch</p></p>
./configure --disable-opengl1 --enable-udev  --enable-opengles --disable-videocore --disable-pulse --disable-oss --prefix=/usr/local --disable-x11 --enable-kms --disable-qt \
--enable-alsa \
--disable-opengl \
--enable-egl \
--disable-wayland \
--disable-x11 \
--enable-zlib \
--enable-freetype \
--disable-vg \
--disable-sdl \
--enable-sdl2 \
--enable-video-kmsdrm \
--enable-ffmpeg  \

6: run retroarch -v </p></p>
you should be  able to see </p></p>
root@orangepipc2:~/RetroArch-1.9.0# ./retroarch -v</p>
[INFO] === Build =======================================</p>
[INFO] Capabilities:  ASIMD</p>
[INFO] Built: Nov 12 2020</p>
[INFO] Version: 1.9.0</p>
[INFO] =================================================</p>
[INFO] [Environ]: SET_PIXEL_FORMAT: RGB565.</p>
[INFO] [Overrides]: Redirecting save file to "/root/.config/retroarch/saves/.srm".</p>
[INFO] [Overrides]: Redirecting save state to "/root/.config/retroarch/states/.state".</p>
[INFO] Version of libretro API: 1</p>
[INFO] Compiled against API: 1</p>
[INFO] [Audio]: Set audio input rate to: 48000.00 Hz.</p>
[INFO] [Video]: Video @ 960x720</p>
[INFO] [DRM]: Found 1 connectors.</p>
[INFO] [DRM]: Connector 0 connected: yes</p>
[INFO] [DRM]: Connector 0 has 6 modes.</p>
[INFO] [DRM]: Connector 0 assigned to monitor index: #1.</p>
[INFO] [DRM]: Mode 0: (1280x720) 1280 x 720, 60 Hz</p>
[INFO] [DRM]: Mode 1: (1024x768) 1024 x 768, 60 Hz</p>
[INFO] [DRM]: Mode 2: (800x600) 800 x 600, 60 Hz</p>
[INFO] [DRM]: Mode 3: (800x600) 800 x 600, 56 Hz</p>
[INFO] [DRM]: Mode 4: (848x480) 848 x 480, 60 Hz</p>
[INFO] [DRM]: Mode 5: (640x480) 640 x 480, 60 Hz</p>
[INFO] [GL]: Found GL context: kms</p>
[INFO] [GL]: Detecting screen resolution 1280x720.</p>
[INFO] [EGL] Falling back to eglGetDisplay</p>
[INFO] [EGL]: EGL version: 1.4</p>
[INFO] [EGL]: Current context: 0x40000001.</p>
[INFO] [KMS]: New FB: 1280x720 (stride: 5120).</p>
[INFO] [GL]: Vendor: ARM, Renderer: Mali-450 MP.</p>
[INFO] [GL]: Version: OpenGL ES 2.0.</p>
[INFO] [GL]: Using resolution 1280x720</p>
[INFO] [GL]: Default shader backend found: glsl.</p>






    
