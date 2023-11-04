# Nvidia Cuda

it will install drivers and cuda

     wget https://developer.download.nvidia.com/compute/cuda/12.3.0/local_installers/cuda_12.3.0_545.23.06_linux.run
     sudo sh cuda_12.3.0_545.23.06_linux.run



another option is install via apt 

     sudo dpkg --add-architecture i386
     sudo apt-get update
     sudo apt-get install libnvidia-compute-535:i386 libnvidia-decode-535:i386 \
     libnvidia-encode-535:i386 libnvidia-extra-535:i386 libnvidia-fbc1-535:i386 \
     libnvidia-gl-535:i386


you can replace 535 for the driver recommended for your system

     