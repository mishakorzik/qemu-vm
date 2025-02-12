#!/usr/bin/bash

mkdir -p qemu
custom_install() {
    clear
    echo "Qemu boot: Custom (aarch64)"
    read -p "path to iso  : " iso
    read -p "max memory MB: " mem
    read -p "max cpu cores: " cpu

    if [ ! -f "$iso" ]; then
        echo "ISO file not found, exiting"
    else
        if [ ! -f "./qemu/uefi.fd" ]; then
            wget -q --show-progress "https://github.com/mishakorzik/qemu-vm/releases/download/linux/uefi.fd" -O "./qemu/uefi.fd"
        fi

        if [ ! -f "./qemu/custom.qcow2" ]; then
            qemu-img create -f qcow2 qemu/custom.qcow2 8G
        fi
        clear
        qemu-system-aarch64 \
          -machine virt,accel=tcg \
          -cpu cortex-a72 \
          -smp $cpu \
          -m $mem \
          -bios qemu/uefi.fd \
          -cdrom $iso \
          -drive if=virtio,file=qemu/custom.qcow2,format=qcow2 \
          -boot d \
          -netdev user,id=net0 \
          -device virtio-net-device,netdev=net0 \
          -nographic
    fi
}

alpine_install() {
    clear
    echo "Qemu boot: Alpine 3.21.1 (aarch64)"
    read -p "max memory MB: " mem
    read -p "max cpu cores: " cpu

    if [ ! -f "./qemu/uefi.fd" ]; then
        wget -q --show-progress "https://github.com/mishakorzik/qemu-vm/releases/download/linux/uefi.fd" -O "./qemu/uefi.fd"
    fi
    if [ ! -f "./qemu/alpine.qcow2" ]; then
        wget -q --show-progress "https://github.com/mishakorzik/qemu-vm/releases/download/linux/alpine.qcow2" -O "./qemu/alpine.qcow2"
    fi
    clear
    qemu-system-aarch64 \
      -machine virt,accel=tcg \
      -cpu cortex-a72 \
      -smp $cpu \
      -m $mem \
      -bios qemu/uefi.fd \
      -drive if=virtio,file=qemu/alpine.qcow2,format=qcow2 \
      -netdev user,id=net0 \
      -device virtio-net-device,netdev=net0 \
      -nographic
}

select_boot() {
    clear
    echo "1. boot custom"
    echo "2. boot alpine"
    read -p "select option: " option
    case $option in
        1)
            if [ ! -f "./qemu/custom.qcow2" ]; then
                echo "system not installed"
            else
                if [ ! -f "./qemu/uefi.fd" ]; then
                    echo "uefi not installed"
                else
                    qemu-system-aarch64 \
                      -machine virt,accel=tcg \
                      -cpu cortex-a72 \
                      -smp "$cpu" \
                      -m "$mem" \
                      -bios qemu/uefi.fd \
                      -drive if=virtio,file=qemu/custom.qcow2,format=qcow2 \
                      -netdev user,id=net0 \
                      -device virtio-net-device,netdev=net0 \
                      -nographic
                fi
            fi
            ;;
        2)
            if [ ! -f "./qemu/alpine.qcow2" ]; then
                echo "system not installed"
            else
                if [ ! -f "./qemu/uefi.fd" ]; then
                    echo "uefi not installed"
                else
                    qemu-system-aarch64 \
                      -machine virt,accel=tcg \
                      -cpu cortex-a72 \
                      -smp "$cpu" \
                      -m "$mem" \
                      -bios qemu/uefi.fd \
                      -drive if=virtio,file=qemu/alpine.qcow2,format=qcow2 \
                      -netdev user,id=net0 \
                      -device virtio-net-device,netdev=net0 \
                      -nographic
                fi
            fi
            ;;
        *)
            echo "unknown option!"
            ;;
    esac
}

clear
echo "1. install custom system"
echo "2. install alpine 3.21.1"
echo "3. boot installed system"
read -p "select option: " option

case $option in
    1)
        custom_install
        ;;
    2)
        alpine_install
        ;;
    3)
        select_boot
        ;;
    *)
        echo "unknown option!"
        ;;
esac
