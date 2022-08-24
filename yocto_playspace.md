# YOCTO
## AGENDA
1. Motivation
	https://www.yoctoproject.org
	### Study
	Layers index
	https://layers.openembedded.org/layerindex/branch/master/layers/

	How do we auto-provision
	- We would write a software to do the provisioning?
	- We just write  a recipe to include the software in the OS. YOCTO PROJECT
2. Requirements
	1. We would need a linux running on Ubuntu version 16, 18.
	2. We would need a cloud VM.
	3. We need install some dependencies on the VM
	 i. python3.8 
	 ii. The following packages
	```
	sudo apt-get update -y
	sudo apt-get install -y gawk diffstat texinfo build-essential gcc-multilib chrpath socat libsdl1.2-dev xterm tmux
	```
	4. Raspberry pi OR QEMU Simulator
	5. Install python3.8 on the VM
Help set-up VMs
4. TODOs 
    i. Set-up VMs
   ii. Go through the yoctoproject website  https://docs.yoctoproject.org/what-i-wish-id-known.html https://www.yoctoproject.org/is-yocto-project-for-you/
   iii. Go through QEMU	Quick start https://www.qemu.org/docs/master/system/quickstart.htmli
   iv. Go through https://github.com/Tob-iee/Embedded_helper/blob/main/yocot_intro.md


5. Next meeting agenda? Same time next week??? 
   i. Run first embedded linux (Salama and Tobe) - core-image-minimal
  ii. How to run a QEMU simulation? Emma 
  iii. A story of linux in Aviation? Ikenna

6. Upper meeting.
 i. test image
 ii. Include python3

7. Upper upper meeting
i. Write our own custom recipe
