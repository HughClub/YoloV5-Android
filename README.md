>   operations

1.   `python export.py --weight _wt`
2.   [`python -m onnxsim _model.onnx _model-sim.onnx`]
3.   `python onnx2ncnn _.onnx`



Notes for YoloV5 deployment to Android

> using [NCNN](https://github.com/Tencent/ncnn) framework

simple demo proj(ncnn) reference:<https://github.com/nihui/ncnn-android-yolov5/>

and PyTorch demo reference:<https://github.com/pytorch/android-demo-app/tree/master/ObjectDetection>

## Model Conversion

NCNN model files: .bin .param

PyTorch(.pt) -> onnx --onnx2ncnn-->ncnn
<https://github.com/Tencent/ncnn/wiki/use-ncnn-with-pytorch-or-onnx>

> PyTorch(.pt) --official tool(._export)-->.onnx--[onnxsim](https://github.com/daquexian/onnx-simplifier)-->.onnx--onnx2ncnn-->ncnn(.bin/.param)

```mermaid
graph LR
pt--export-->onnx-- onnx2ncnn-->ncnn
onnx-- onnxsim-->onnx
```

onnxsim:`python -m onnxsim _model.onnx _model-sim.onnx`


## demo

clone [YoloV5 demo project](https://github.com/nihui/ncnn-android-yolov5/)

using Android Studio to import the project after extracting the zip
> after this operation, you must promise the **ncnn_DIR** in CMakeLists.txt matched

download needed
- SDK:29.0.2
- NDK:23.1.7779620(the last when writing)
- Android Platform 24
- cmake 3.10 (maybe need **ninja**)

```cmake
# I don't extract the vulkan to `cpp/include/`
set(Vulkan ../../../../../ncnn-20210720-android-vulkan/)
set(ncnn_DIR ${CMAKE_SOURCE_DIR}/${Vulkan}/${ANDROID_ABI}/lib/cmake/ncnn)
```

then you can add `x86` support for avd in build.gradle

```gradle
ndk {
        moduleName "ncnn"
        abiFilters "armeabi-v7a", "arm64-v8a","x86" // add "x86"
    }
```

