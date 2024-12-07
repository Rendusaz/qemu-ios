# QEMU-iOS 

# This is a great repo, I am adding an instruction that will fix errors on Apple Silicon macOS  Works for M2 Max.


Follow all original instructions VVV double check the authors links down below if errors are present at this point right here directly below in the (())

(( git clone, brew install  glib ninja pixman pkg-config sdl2, mkdir build && cd build, 
 ./configure --enable-sdl --target-list=arm-softmmu --disable-capstone --disable-pie --disable-slirp --extra-cflags=-I/opt/homebrew/opt/openssl@3/include --extra-ldflags='-L/opt/homebrew/opt/openssl@3/lib -lcrypto',  make -j6))

after running these you should be ready to run, 
original instructions say 
"./arm-softmmu/qemu-system-arm -M iPod-Touch,bootrom=<path to bootrom>,nand=<path to NAND directory>,nor=<path to NOR directory> -serial mon:stdio -cpu max -m 2G -d unimp"
we are simply adding a "-display sdl" flag resulting in

# ./arm-softmmu/qemu-system-arm -M iPod-Touch,bootrom=<path to bootrom>,nand=<path to NAND directory>,nor=<path to NOR directory> -serial mon:stdio -cpu max -m 2G -d unimp -display sdl

should work fine with that,
also ,for controls.. H=HOME, L=LOCK(?), P=POWER(?), Additional(?)

Steps to fix the original iPod 1 emulator linked on here is a little more involved and the instructions are there on my profile. Hope it fixed any issues.
# edit, instructions for the iPod/iOS 1g emulator fixes are on this readme, follow below.

after cloning and following all the instructions from orginal author, you might have issues with a few things,
# the file at audio/jackaudio.c this edit will be made, this is an improvement to allow both macOS AND linux as the original only allows linux

// Replace this line:
pthread_setname_np(*thread, "jack-client");

// With this platform-specific code:
#ifdef __APPLE__
    pthread_setname_np("jack-client");
#else
    pthread_setname_np(*thread, "jack-client");
#endif

# the at hw/arm/ipod_touch_sha1.c 
// Change this function to accept uint64_t directly instead of void*
static uint64_t swapLong(uint64_t x) {
    // Implement byte swapping for uint64_t
    return ((x & 0xFF00000000000000ull) >> 56) |
           ((x & 0x00FF000000000000ull) >> 40) |
           ((x & 0x0000FF0000000000ull) >> 24) |
           ((x & 0x000000FF00000000ull) >> 8)  |
           ((x & 0x00000000FF000000ull) << 8)  |
           ((x & 0x0000000000FF0000ull) << 24) |
           ((x & 0x000000000000FF00ull) << 40) |
           ((x & 0x00000000000000FFull) << 56);
}

// Then modify the problematic line to:
uint64_t data_length = swapLong(((uint64_t *)s->buffer)[s->buffer_ind / 8 - 1]) / 8;

Also, add 

#include <openssl/evp.h>

// Replace the SHA1 calls with:
EVP_MD_CTX *mdctx = EVP_MD_CTX_new();
const EVP_MD *md = EVP_sha1();
EVP_DigestInit_ex(mdctx, md, NULL);
EVP_DigestUpdate(mdctx, s->buffer, data_length);
EVP_DigestFinal_ex(mdctx, s->hashout, NULL);
EVP_MD_CTX_free(mdctx);

The complese section should look like this 

uint64_t data_length = swapLong(((uint64_t *)s->buffer)[s->buffer_ind / 8 - 1]) / 8;
if (data_length > 0) {
    EVP_MD_CTX *mdctx = EVP_MD_CTX_new();
    const EVP_MD *md = EVP_sha1();
    EVP_DigestInit_ex(mdctx, md, NULL);
    EVP_DigestUpdate(mdctx, s->buffer, data_length);
    EVP_DigestFinal_ex(mdctx, s->hashout, NULL);
    EVP_MD_CTX_free(mdctx);
}


This will fix the errors, from here you can run make or make -j8


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
