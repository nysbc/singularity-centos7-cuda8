BootStrap: yum
OSVersion: 7
MirrorURL: http://mirror.centos.org/centos-%{OSVERSION}/%{OSVERSION}/os/$basearch/
Include: yum wget
%environment
    export CUDA_HOME=/usr/local/cuda
    CUDA_LIB=$CUDA_HOME/lib64
    CUDA_INCLUDE=$CUDA_HOME/include
    CUDA_BIN=$CUDA_HOME/bin
    export LD_LIBRARY_PATH=$CUDA_LIB
    export PATH=$CUDA_BIN:$PATH
%runscript
# commands to be executed when the container runs 
echo "LD_LIBRARY_PATH: $LD_LIBRARY_PATH"
echo "PATH: $PATH"
echo "Arguments received: $*"
exec "$@"    
%post
    # commands to be executed inside container during bootstrap
    yum -y install epel-release
    yum -y install https://centos7.iuscommunity.org/ius-release.rpm
    yum -y install http://developer.download.nvidia.com/compute/cuda/repos/rhel7/x86_64/cuda-repo-rhel7-8.0.61-1.x86_64.rpm
    yum clean all && yum makecache
    yum -y install wget python35u python35u-pip libgomp cuda-runtime-8-0
