+++
authors = ["RebertiCS"]
title = "Compilando Kernel Android"
date = "2024-01-12"
description = "Guia para Compilação usando CLang11"
+++

# Requisitos

Clang-11:
 - [Prebuilts](https://android.googlesource.com/platform//prebuilts/clang/host/linux-x86/+/refs/heads/android11-qpr3-release/clang-r383902b1/)

Needed to build the kernel
``` bash
sudo apt-get install git-core gnupg flex bison build-essential zip curl zlib1g-dev libc6-dev-i386 x11proto-core-dev libx11-dev lib32z1-dev libgl1-mesa-dev libxml2-utils xsltproc unzip fontconfig clang axel xz-utils make ccache openssl libssl-dev bc
```

## Codigo Fonte
``` bash
git clone <codigo_fonte> ./kernel
cd kernel

# Caso queira KernelSU
curl -LSs "https://raw.githubusercontent.com/tiann/KernelSU/main/kernel/setup.sh" | bash -s main
```

## Compilando
### Defconfig
Configurações do modelo estarão disponiveis em `arch/aarch64/config` ou `arch/aarch64/config/vendor`

### Procedimento
Imagens do kernel(`Image`, `Image-dtbo`, ...) estarão na pasta `out/arch/aarch64/boot/`
``` bash
# Set env
CLANG=$HOME/android/toolchains/clang_11/bin
PATH="$CLANG:$PATH"
JOBS=-j$(nproc --all)
MAKE_ENV=-j$(nproc --all) CC=clang CLANG_TRIPLE=aarch64-linux-gnu- CROSS_COMPILE=aarch64-linux-android- \
    CROSS_COMPILE_ARM32=arm-linux-androidebi- O=out ARCH=arm64 CONFIG_SECTION_MISMATCH_WARN_ONLY=y CONFIG_FRAME_WARN=0

make $MAKE_ENV mrproper
make $MAKE_ENV <DEF_CONFIG>
make $MAKE_ENV
```

### Instalação
O kernel pode ser instalado via [AnyKernel3](https://github.com/osm0sis/AnyKernel3)
ou substituindo o kernel no pacote original da frabricante.
