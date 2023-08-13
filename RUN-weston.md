# setup env

1. clone source code
    ```shell
    git clone https://gitlab.freedesktop.org/wayland/wayland.git wayland
    git clone https://gitlab.freedesktop.org/wayland/wayland-protocols.git wayland-protocols
    git clone git://anongit.freedesktop.org/wayland/libinput libinput
    git clone https://gitlab.freedesktop.org/wayland/weston.git weston
    ```
2. install dependency
    ```shell
    sudo apt install python3-pip
    sudo pip3 install -i http://mirrors.aliyun.com/pypi/simple/ --trusted-host mirrors.aliyun.com meson
    sudo apt-get install pkg-config
    sudo apt-get install cmake
    sudo apt-get install libffi-dev
    sudo apt-get install libxml2-dev
    sudo apt-get install ninja-build
    sudo apt-get install libudev-dev
    sudo apt-get install libmtdev-dev
    sudo apt-get install libevdev-dev
    sudo apt-get install libwacom-dev
    sudo apt-get install libgtk-3-dev
    sudo apt-get install check
    sudo apt-get install libdrm-dev
    sudo apt-get install libpam0g-dev
    sudo apt-get install libseat-dev
    sudo apt-get install libsystemd-dev
    sudo apt-get install libgbm-dev
    ```
3. set env var
    ```shell
    export WLD=/your_code_path/out/weston/install
    export LD_LIBRARY_PATH=$WLD/lib/x86_64-linux-gnu
    export PKG_CONFIG_PATH=$WLD/lib/x86_64-linux-gnu/pkgconfig/:$WLD/share/pkgconfig/
    export ACLOCAL_PATH=$WLD/share/aclocal
    export PATH=$WLD/bin:$PATH
    ```
4. install wayland-scanner
    ```shell
    meson scanner_build/ -Dscanner=true -Dtests=false -Ddocumentation=false
    sudo ninja -C scanner_build/ install
    sudo rm -rf /usr/local/lib/x86_64-linux-gnu/
    # this way can install waykand-scanner to system dir
    ```
5. start build weston
    ```shell
    # wayland
    cd wayland
    meson build/ --prefix=$WLD -Ddocumentation=false
    ninja -C build/ install
    cd ..

    # wayland-protocols
    cd wayland-protocols
    meson build/ --prefix=$WLD
    ninja -C build/ install

    # libinput
    cd libinput
    meson build/ --prefix=$WLD 
    ninja -C build/ install
    cd ..

    # weston
    cd weston
    meson --reconfig build/ --prefix=$WLD -Drenderer-gl=false -Dbackend-rdp=false -Dbackend-x11=false -Dbackend-drm=true -Drenderer-gl=true -Dxwayland=false -Dremoting=false -Dpipewire=false -Dimage-webp=false -Dcolor-management-lcms=false -Dbackend-drm-screencast-vaapi=false -Dbackend-pipewire=false
    ninja -C build/ install
    cd ..
    ```
6. run weston
    ```shell
    # the weston.ini in build dir.
    find . -name weston.ini
    ./weston/build/ivi-shell/weston.ini

    cp ./weston/build/ivi-shell/weston.ini out/

    # run weston

    export WLD=/your_code_path/out/weston/install
    export LD_LIBRARY_PATH=$WLD/lib/x86_64-linux-gnu
    export PKG_CONFIG_PATH=$WLD/lib/x86_64-linux-gnu/pkgconfig/:$WLD/share/pkgconfig/
    export ACLOCAL_PATH=$WLD/share/aclocal
    export PATH=$WLD/bin:$PATH

    weston -c /your_code_path/out/weston.ini --log=/your_code_path/out/weston.log
    ```
7. run weston with drm-backend

    weston.ini
    ```shell
    [core]
    shell=ivi-shell.so
    modules=hmi-controller.so
    backend=drm-backend.so
    ```
    run weston with drm-backend, create run.sh script file.
    ```shell
    # run.sh
    export WLD=/your_code_path/out/weston/install
    export LD_LIBRARY_PATH=$WLD/lib/x86_64-linux-gnu
    export PKG_CONFIG_PATH=$WLD/lib/x86_64-linux-gnu/pkgconfig/:$WLD/share/pkgconfig/
    export ACLOCAL_PATH=$WLD/share/aclocal
    export PATH=$WLD/bin:$PATH

    weston -c /your_code_path/out/weston.ini --log=/your_code_path/out/weston.log
    ```
    Ctrl+Alt+ F4/F3 switch to virtual terminal, then can run with drm-backend.
    ```
    ./run.sh
    ```
8. tips: https://blog.csdn.net/yangchao315/article/details/123189373
