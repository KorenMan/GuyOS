# $@ = target file
# $< = first dependency
# $^ = all dependencies

# First rule is the one executed when no parameters are fed to the Makefile
all: run

# Link the kernel binary from the entry and kernel object files
kernel.bin: kernel-entry.o kernel.o
	ld -m elf_i386 -o $@ -Ttext 0x1000 $^ --oformat binary

# Assemble the entry point from assembly
kernel-entry.o: boot/kernel-entry.asm
	nasm $< -f elf32 -o $@

# Compile the kernel from C source
kernel.o: kernel/kernel.c
	gcc -m32 -ffreestanding -c $< -o $@ -fno-pic -fno-stack-protector

# Assemble the boot sector
mbr.bin: boot/mbr.asm
	nasm $< -f bin -o $@

# Create the OS image from the boot sector and kernel
os-image.bin: mbr.bin kernel.bin
	cat $^ > $@

# Run the OS image using QEMU
run: os-image.bin
	qemu-system-i386 -fda $<

# Clean up generated files
clean:
	rm -f *.bin *.o
