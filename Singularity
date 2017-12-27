
# Singularity container definition for
# BOINC client (eg to run seti@home)
# Baded on Nvidia Cuda 8.0 

# ref https://hub.docker.com/r/nvidia/cuda/
# ref http://singularity-hub.org/containers/712


BootStrap: docker
From: nvidia/cuda:8.0-runtime-centos7   # Berkeley Devel need glibc2.4, not in RHEL6
#From: nvidia/cuda:8.0-runtime-centos6
#From: nvidia/cuda:8.0-runtime-ubuntu16.04
#From: nvidia/cuda:8.0-cudnn6-devel-ubuntu16.04


%help
This container has a BOINC client for running things like SETI@home.
is a CentOS 6 with nvidia cuda runtime + BOINC client

It doesn't quite work as desired yet.
 
To use this container:
singularity pull --name boinccmd shub://tin6150/boinc-client
mkdir /tmp/boinc
#mkdir /tmp/boinc-lib
singularity exec -B /tmp/boinc:/var/log,/tmp/boinc:/var/lib/boinc ./boinccmd.simg /bin/bash
/usr/bin/boinc --check_all_logins --redirectio --dir /var/lib/boinc &
## no, did something here that triggered it to work...
## unless someone blocked boinc connection at the network...  RPC auth fail... TCP 31416...
## give time for server to get ready (run_benchmarks)
## stangely can't get to setiathome, route blocked to 208.68.240.110 ??
#/usr/bin/boinccmd --set_proxy_settings  # to what though??
/usr/bin/boinccmd --project_attach http://setiathome.berkeley.edu 824003_a8aa1de0c75802fc1651a1015133624f
/usr/bin/boinccmd --project_attach http://www.gpugrid.net 518173_5f7f1ae63651a14444ca08429fe16ae6
##/usr/bin/boinccmd --project http://www.gpugrid.net detach 
/usr/bin/boinccmd --get_state
/usr/bin/boinccmd --set_run_mode always 86400 # 86400 seconds = 24 hours
/usr/bin/boinccmd --set_gpu_mode always 86400 # 86400 seconds = 24 hours
#https://www.systutorials.com/docs/linux/man/1-boinccmd/

### method 1a: boinc runs, but gui_rpc authentication always fail, cuz of namespace?
singularity exec -B /tmp/boinc:/var/log,/tmp/boinc:/var/lib/boinc ./boinccmd-r2-c6.simg /usr/bin/boinc --check_all_logins --redirectio --dir /var/lib/boinc &
singularity exec -B /tmp/boinc:/var/log,/tmp/boinc:/var/lib/boinc ./boinccmd-r2-c6.simg /usr/bin/boinccmd --get_state

# r3 build in c7 didn't work, get auth error -155 often.

## method 2, with Berkeley Install bin in /opt/BOINC
export LD_LIBRARY_PATH=/lib64:/lib:/usr/lib64:/usr/lib:$LD_LIBRARY_PATH


#singularity exec -B /tmp/boinc:/var/log -B /tmp/boinc-lib:/var/lib/boinc ./boinccmd.simg /opt/BOINC/bin/run_client --run_benchmarks
#singularity exec -B /tmp/boinc:/var/log -B /tmp/boinc-lib:/var/lib/boinc ./boinccmd.simg /usr/bin/boinccmd --project_attach http://setiathome.berkeley.edu 824003_a8aa1de0c75802fc1651a1015133624f

## ??

## service start this process as user boinc (ubuntu):

## /usr/bin/boinc --check_all_logins --redirectio --dir /var/lib/boinc-client

# singularity exec -B /tmp/boinc:/var/log -B /tmp/boinc-lib:/var/lib/boinc ./boinccmd.simg /usr/bin/boinccmd --project_attach http://setiathome.berkeley.edu 824003_a8aa1de0c75802fc1651a1015133624f

# singularity exec -B /tmp/boinc:/var/log  ./boinccmd.simg /usr/bin/boinccmd --run_benchmarks
# output will be directed to /tmp/boinc/boinc*.log 


%runscript
	echo "zsh from inside the container..."
	/bin/zsh
	# till find a way to run boinc for 1 work unit without going into daemon mode...
	# /opt/boinc/run_client
	# don't use the --daemon mode, should be good for running as batch job 


%post
	#echo "Hello from inside the container"
	touch 	 		  /THIS_IS_INSIDE_SINGULARITY
	echo "build start"     >> /THIS_IS_INSIDE_SINGULARITY
	date                   >> /THIS_IS_INSIDE_SINGULARITY
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
			coreutils util-linux-ng man strace
	# https://help.ubuntu.com/community/Cuda
	#apt-get -qy install \
	#		libxi-dev libxmu-dev freeglut3-dev build-essential binutils-gold	


	# http://boinc.berkeley.edu/wiki/Installing_BOINC_on_Ubuntu
	#apt-get -qy install  boinc-client boinc-manager

	# this will get all dependencies that boinc needs, 
	# without running pre-install script like adding user and service 
	yum -y install  epel-release openssl-devel 
	ln -s /usr/lib64/libcrypto.so /usr/lib64/libcrypto.so.1.0.0   # not sure why isn't there, hope works
	ln -s /usr/lib64/libssl.so    /usr/lib64/libssl.so.1.0.0      # really .so.1.0.2k
		#openssl-devel for libcrypto libssl 
		# boincmgr will require lots of GUI libs, like libnotify-devel 


	# the daemon version need (at least prefer) to have the boinc user
	groupadd -f -g 29888 boinc
	useradd -d /opt/BOINC -m -s /bin/false -o -g 29888 -u 29888 boinc 
	test -d /opt/BOINC || mkdir -p /opt/BOINC
	chmod 775 /opt/BOINC
	yum --setopt=tsflags=noscripts -y install boinc-client
	#yum --setopt=tsflags=noscripts -y install boinc-manager

	# the Berkeley Installer may work more like a user program without daemon stuff?
	# https://boinc.berkeley.edu/wiki/Installing_BOINC#The_Berkeley_Installer
	cd /opt
	test -f boinc-64.sh || wget --no-verbose "https://boinc.berkeley.edu/dl/boinc_7.2.42_x86_64-pc-linux-gnu.sh" -O boinc-64.sh 
	sh boinc-64.sh
	#rm boinc-64.sh


	echo "build end"  >> /THIS_IS_INSIDE_SINGULARITY
	date       	  >> /THIS_IS_INSIDE_SINGULARITY


%labels
MAINTAINER  Tin Ho tin'at'lbl.gov


## sudo    /opt/singularity-2.4.1/bin/singularity build -w ./boinc.simg ./Singularity

