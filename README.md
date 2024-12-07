# QEMU-iOS 

# This is a great repo, I am adding an instruction that will fix errors on Apple Silicon macOS  Works for M2 Max.


Follow all original instructions VVV double check the authors links down below if errors are present at this point right here directly below in the (())

(( git clone, brew install  glib ninja pixman pkg-config sdl2, mkdir build && cd build, 
 ./configure --enable-sdl --target-list=arm-softmmu --disable-capstone --disable-pie --disable-slirp --extra-cflags=-I/opt/homebrew/opt/openssl@3/include --extra-ldflags='-L/opt/homebrew/opt/openssl@3/lib -lcrypto',  make -j6))

after running these you should be ready to run, 
original instructions say 
"./arm-softmmu/qemu-system-arm -M iPod-Touch,bootrom=<path to bootrom>,nand=<path to NAND directory>,nor=<path to NOR directory> -serial mon:stdio -cpu max -m 2G -d unimp"
we are simply adding a "-display sdl" flag resulting in

./arm-softmmu/qemu-system-arm -M iPod-Touch,bootrom=<path to bootrom>,nand=<path to NAND directory>,nor=<path to NOR directory> -serial mon:stdio -cpu max -m 2G -d unimp -display sdl

should work fine with that,
also ,for controls.. H=HOME, L=LOCK(?), P=POWER(?), Additional(?)

Steps to fix the original iPod 1 emulator linked on here is a little more involved and the instructions are there on my profile. Hope it fixed any issues.



QEMU-iOS is an emulator for legacy Apple devices.
Currently, the iPod Touch 1G and iPod Touch 2G are supported.

<img width="331" alt="it2g-qemu" src="https://github.com/devos50/qemu-ios/assets/1707075/9bf7f6c1-5918-47e9-bb3e-2e39ae15d519">

The schematic below shows the most important hardware components of the iPod Touch 2G and their interactions.
The schematic for the iPod Touch 1G is mostly similar.

<img width="80%" alt="it2g-schematic" src="https://github.com/devos50/qemu-ios/assets/1707075/4b8eca9a-74b0-4590-ad23-bc056acde434">

### Running the iPod Touch 1G

Instructions on how to run the iPod Touch 1G emulator can be found [here](https://devos50.github.io/blog/2022/ipod-touch-qemu-pt2/).
A technical blog post with more information about the peripherals and reverse engineering process is published [here](https://devos50.github.io/blog/2022/ipod-touch-qemu/).

### Running the iPod Touch 2G

Instructions on how to run the iPod Touch 2G emulator can be found [here](https://github.com/devos50/qemu-ios/blob/ipod_touch_2g/RUNNING.md).
