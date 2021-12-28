# Install [gocv](https://github.com/hybridgroup/gocv) "gocv.io/x/gocv" on Windows

## 1. Download and extract both to `D:\opencv`.
- https://github.com/opencv/opencv/archive/4.5.4.zip
- https://github.com/opencv/opencv_contrib/archive/4.5.4.zip

## 2. Have `MSYS2` and its `make` and `cmake` properly installed.
- Assuming you've already installed `go` and `git`.
- Install `MSYS2`.
- Run `pacman -S make cmake mingw-w64-x86_64-gcc` from your `MSYS2` terminal.
- Uncomment `MSYS2_PATH_TYPE=inherit` in `msys2.ini`, `mingw64.ini` and `mingw32.ini`.
- Add `D:\msys64\mingw64\bin` and `D:\msys64\usr\bin` to your `Path` environment variable.

## 3. Run the following in `cmd.exe`.
```cmd
cd /D D:\opencv\build
set enable_shared=ON
cmake D:\opencv\opencv-4.5.4 -G "MinGW Makefiles" -BD:\opencv\build -DENABLE_CXX11=ON -DOPENCV_EXTRA_MODULES_PATH=D:\opencv\opencv_contrib-4.5.4\modules -DBUILD_SHARED_LIBS=%enable_shared% -DWITH_IPP=OFF -DWITH_MSMF=OFF -DBUILD_EXAMPLES=OFF -DBUILD_TESTS=OFF -DBUILD_PERF_TESTS=OFF -DBUILD_opencv_java=OFF -DBUILD_opencv_python=OFF -DBUILD_opencv_python2=OFF -DBUILD_opencv_python3=OFF -DBUILD_DOCS=OFF -DENABLE_PRECOMPILED_HEADERS=OFF -DBUILD_opencv_saliency=OFF -DBUILD_opencv_wechat_qrcode=OFF -DCPU_DISPATCH= -DOPENCV_GENERATE_PKGCONFIG=ON -DWITH_OPENCL_D3D11_NV=OFF -DOPENCV_ALLOCATOR_STATS_COUNTER_TYPE=int64_t -Wno-dev
mingw32-make -j%NUMBER_OF_PROCESSORS%
mingw32-make install
rmdir D:\opencv\opencv-4.5.4 /s /q
rmdir D:\opencv\opencv_contrib-4.5.4 /s /q
chdir /D %GOPATH%\src\gocv.io\x\gocv
```
This should install `opencv` under directory `D:\opencv\`.

## 4. Set environment variables in order to provide `gcc` with global include paths to your `opencv`.
- `CPLUS_INCLUDE_PATH`=`D:\opencv\build\install\include`
- Add `D:\opencv\build\install\x64\mingw\bin` to `Path` environment variable.

If you want to find out your gcc's default include path:
```bash
$ echo | gcc -v -x c++ -E -
```
```txt
#include <...> search starts here:
 D:\opencv\build\install\include
```

## 5. Go get it.
Either way you prefer.
- `go get -v gocv.io/x/gocv`
- `git clone https://github.com/hybridgroup/gocv %GOPATH%\src\gocv.io\x\gocv`

## 6. Test it.
- `go get -v gocv.io/x/gocv/...`
