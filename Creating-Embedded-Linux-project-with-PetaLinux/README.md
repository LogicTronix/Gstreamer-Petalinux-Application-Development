# Creating Embedded Linux project with PetaLinux
In this post, you will see the instruction for generating embedded linux project using Petalinux.

    * Creating Petalinux Project on Petalinux 2019.2 version

    * Implementing various subtasks using PetaLinux as guided by PetaLinux Tool Reference Guide UG 1144

# Lets now start on "Creating Petalinux Project"
<B> 1.  Creating PetaLinux Project using BSP for Xilinx ZCU102 </B>

  <i> petalinux-create -t project -s <path-to-bsp> </i>

<B> 2. Creating PetaLinux Project using template and and hardware description </B>

<i> petalinux-create -t project --template zynqMP -name <project-name> </i>

<i> petalinux-config --get-hw-description=<PATH-TO-HDF/XSA DIRECTORY> </i>

<B> 3. Doing the first build to fetch and compile the linux </B>


<i> petalinux-build </i>

<B> 4. Booting the petaLinux image on QEMU </B>


<i> petalinux-boot --qemu --kernel </i>

<B> 5. BSP packaging for distributing project between team and customers </B>

<B> 5.1 Simple BSP packaging </B>

<i> petalinux-package --bsp -p <plnx-proj-root> --output MY.BSP </i>

<B> 5.1 Additional BSP packaging along with hardware source </B>

<i> petalinux-package --bsp -p <plnx-proj-root> --hwsource <hw-projectroot> --output MY.BSP </i>

<B> 5.1 Adding linux packages like python, gstreamer, QT </B>

<i>petalinux-config -c rootfs </i>

This will lead to menuconfig from which you can add multiple packages from the list.

# PetaLinux Project Structure
hw-description and configuration of kernel and roofs are available at <i> project-spec/hw-description and projecct-spec/configs </i> folders these are auto-generated when we use above petalinux commands to configure the project. Similarly auto-generated device tree files are present in <i> /components/plnx_workspace/device-tree/devicetree/ </i> folder, it has base device tree for the required hardware from which other project specific device tree is configured Project Specific device tree is located at <i> /project-spec/meta-user/recipes-bsp/devicetree/files/ system-user.dtsi </i> is used to describe the hardware description in the device tree.

# Adding custom/user application in linux rootfs
For creating application in user level, one can add recipe at meta-user folder by running following command:

<i> petalinux-create -t apps --template c --name myapp --enable </i>

This will create a application recipe folder myapp in the meta-user folder. It consist of bitbake recipe file to compile and load the executable in rootfs, /usr/bin

To build the recipe, use following command:

<i> petalinux-build -c myapp </i>

Then to load the executable in rootfs, build the rootfs running petalinux-build -c rootfs

# Linux Kernel Folder Structure and Location in PetaLinux
It is located at <i> firstProject/build/tmp/work/zcu102_zynqmp-xilinx-linux/linux-xlnx/4.19-xilinx-v2019.2+gitAUTOINC+b983d5fd71-r0/linux-zcu102_zynqmp-standard-build/source/ </i>

Also how does linux boot works: FSBL UBoot Kernel – start_kernel – source code located at <i> source/init/main.c </i>
