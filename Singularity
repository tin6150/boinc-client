
# Singularity container definition for
# BOINC client (eg to run seti@home)
# Baded on Nvidia Cuda 8.0 
# 

# ref https://hub.docker.com/r/nvidia/cuda/
# ref http://singularity-hub.org/containers/712


BootStrap: docker
From: nvidia/cuda:8.0-runtime-centos6
#From: nvidia/cuda:8.0-runtime-ubuntu16.04
#From: nvidia/cuda:8.0-cudnn6-devel-ubuntu16.04


%help
This container is a Ubuntu with nvidia cuda runtime + BOINC client

%runscript
	echo "zsh from inside the container..."
	/bin/zsh
	# till find a way to run boinc for 1 work unit without going into daemon mode...
	# /opt/boinc/run_client
	# don't use the --daemon mode, should be good for running as batch job 


%post
	#echo "Hello from inside the container"
	touch 	 	  /THIS_IS_INSIDE_SINGULARITY
	echo "start"   >> /THIS_IS_INSIDE_SINGULARITY
	date           >> /THIS_IS_INSIDE_SINGULARITY
	#apt-get -qy install \
	yum -y install \
			vim bash zsh wget curl tar which \
			environment-modules telnet nc 
			#ipmitool \
			#pciutils \
			#epel-release  # sl6 may need diff mech to enable epel
			#libpng libpng-devel libpng-static \
			#openmotif openmotif-devl openmotif22 \
			# telnet client for troubleshooting license manager connection




	#apt-get -qy install \
	yum -y install \
			coreutils util-linux-ng man 
	# https://help.ubuntu.com/community/Cuda
	#apt-get -qy install \
	#		libxi-dev libxmu-dev freeglut3-dev build-essential binutils-gold	


	# ubuntu install with root priv, DONT like it
	# http://boinc.berkeley.edu/wiki/Installing_BOINC_on_Ubuntu
	#apt-get -qy install  boinc-client boinc-manager



	# https://boinc.berkeley.edu/wiki/Installing_BOINC#The_Berkeley_Installer
	# the Berkeley Installer may work more like a user program without daemon stuff?
	# at least no root priv so won't be creating services
	# probably don't need to add the boinc user, but just for it to host the file.
	#groupadd -f -g 29888 boinc
	#useradd -d /opt/BOINC -m -s /bin/false -o -g 29888 -u 29888 boinc 
	#test -d /opt/BOINC || mkdir -p /opt/BOINC
	#chmod 775 /opt/BOINC
	#cd /opt
	# this hopefully get all dependencies but not install, so won't execute scripts that add service...
	yum -y install  epel-release
	yum --setopt=tsflags=noscripts -y install boinc-client   # this works, don't add boinc user.  
	yum --setopt=tsflags=noscripts -y install boinc-manager
	#yum -q -y install --noplugins --setopt=tsflags=noscripts --downloadonly --downloaddir=/opt  boinc-client boinc-manager
	#rpm -i --noscripts /opt/boinc-client*rpm 
	#rpm -i --noscripts /opt/boinc-manager*rpm 
	#test -f boinc-64.sh || wget --no-verbose "https://boinc.berkeley.edu/dl/boinc_7.2.42_x86_64-pc-linux-gnu.sh" -O boinc-64.sh 
	#sh boinc-64.sh
	#rm boinc-64.sh


	echo "end"   >> /THIS_IS_INSIDE_SINGULARITY
	date         >> /THIS_IS_INSIDE_SINGULARITY


%labels
MAINTAINER  Tin Ho tin'at'lbl.gov


## sudo    /opt/singularity-2.4.1/bin/singularity build -w ./boinc.simg ./Singularity

