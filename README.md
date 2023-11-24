# cicv-r4l-alexjunyuexuli
cicv-r4l-alexjunyuexuli created by GitHub Classroom
作业1-编译Linux内核
git clone git@github.com:cicvedu/cicv-r4l-alexjunyuexuli.git
cd /cicv-r4l-alexjunyuexul 
cd linux
make x86_64_defconfig
make LLVM=1 menuconfig
General setup
        ---> [*] Rust support
make LLVM=1 -j$(nproc)
ls vmlinux

作业2-对Linux内核进行配置编译e1000网卡驱动
cd src_e1000
chmod 777 ./build_image.sh
make LLVM=1 menuconfig
Device Drivers 
    ---> Network device support
        ---> Ethernet driver support
            ---> Intel devices, Intel(R) PRO/1000 Gigabit Ethernet support

make LLVM=1 -j$(nproc)
./build_image.sh
ip link set eth0 up
ip addr add 10.0.2.15/255.255.255.0 dev eth0
ip route add default via 10.0.2.1
ping 10.0.2.2
问题1、编译成内核模块，是在哪个文件中以哪条语句定义的？
答：Kbuild中的obj-$ +=****.o这条
问题2、该模块位于独立的文件夹内，却能编译成Linux内核模块，这叫做out-of-tree module，请分析它是如何与内核代码产生联系的？
答：通过makefile中的obj$ +=那个文件，内核的构建系统通过M 来定位独立模块的源代码


作业3-使用rust编写一个简单的内核模块并运行

进入到Linux目录下samples/rust文件夹
添加一个rust_helloworld.rs文件
修改Makefile
obj-$(CONFIG_SAMPLE_RUST_HELLOWORLD)	+= rust_helloworld.o
修改Kconfig
config SAMPLE_RUST_HELLOWORLD
	tristate "Print hello world module"
	help
	  To compile this as a module, choose M here:

	  If unsure, say N.

Kernel hacking
  ---> Sample Kernel code
      ---> Rust samples
              ---> <M>Print Helloworld in Rust (NEW)

make LLVM=1 -j$(nproc)

insmod rust_helloworld.ko



