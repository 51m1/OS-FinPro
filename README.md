Basic Kernel
==========

The final product of this project is a simple kernel written in c and x86 Assembly capable of supplying text on the screen, with soma basic extra features
that can be run either by using an emulator (such as qemu) or on real hardware.

For this project we needed some basic knowledge and understanding of C, functions, pointers, computer architecture and the difference between ROM
and RAM memories.

The first step for creating a kernel is to build a cross-compiler, which will allow us to compile and link code, this is very important because
before this step is taken care of we cant do anything, since a standard C compiler won’t understand how to generate the machine code we need. 

To aid us in this first step we follow the instructions provided this link:  

'''
      https://hastebin.com/imasobixop.bash 
'''

This list of commands helped us install dependencies, create temporal data, define environment variables, to build binutils and to build a custome gcc.

After the cross compiler is raady, we get to move on to step two. We created following three files:
- starts.s: This file containa the x86 assembly code that starts our kernel and sets up the x86 and set everything up for the other two files to do their work.
- kernel.c: This file contains the majority of the kernel code, written in C and where the features of the kernel are added (Color cange in our case).
- linker.ld: This file will give the compiler information about how it should construct our kernel executable. It is used to define how bits of the final kernel
executable will be stitched together.

For the next stepwe have to compile the code within the files we created. This can be acomplished with  this commands:

'''

	i686-elf-gcc -std=gnu99 -ffreestanding -g -c start.s -o start.o

	i686-elf-gcc -std=gnu99 -ffreestanding -g -c kernel.c -o kernel.o

	//This creates two object files that are going to be linked by the linker file.

	i686-elf-gcc -ffreestanding -nostdlib -g -T linker.ld start.o kernel.o -o mykernel.elf -lgcc

	//Links the previously compliled files and creates a OS image.
'''

For the three commands above to work, this line must be introduced first: 

'''
       export PATH="$HOME/opt/cross/bin:$PATH"
'''

After this, using the SO image created by the last command (mykernel.elf), we can run the kernel. To run the kernel we need to install virtualizarion software
such as VirtualBox or Qemu. We opted for Qemu (and we suggest you do as well).

To run the iso using qemu the following command is used:

'''
    qemu-system-i386 -kernel mykernel.elf
'''

The additional feature that we decided to add to the kernel is the color support one. The color of both the text and the background of the kernel is selected
in a variable within the kernel.c file, using a hexadecimal 8 bit number. After the welcoming message of the kernel a sample of every color combination 
possible is displayed.