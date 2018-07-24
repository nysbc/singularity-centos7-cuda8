### A Singularity Container with CentOS7 and Cuda8

We are using this at SEMC to run latest version of motioncor2 on a CentOS6 cluster. This is based on [this](https://docs.hpc.arizona.edu/display/UAHPC/Singularity+-+CentOS7%2C+Tensorflow1.2%2C+Python3.5%2C+Cuda8.0%2C+cuDNN5.1) document from docs.hpc.arizona.edu. 

* To get started install Singularity following documentation for admins from https://www.sylabs.io/docs/ 
* Create Singularity image:
```bash
singularity create --size 4096 centos7_cuda8.img
```
* Download https://raw.githubusercontent.com/nysbc/singularity-centos7-cuda8/master/Singularity and bootstrap your image:
```bash
wget https://raw.githubusercontent.com/nysbc/singularity-centos7-cuda8/master/Singularity
singularity bootstrap centos7_cuda8.img Singularity
```
* Download and unzip MotionCor2: http://msg.ucsf.edu/em/software/motioncor2.html
* Copy MotionCor2_1.1.0-Cuda80 to Singularity image:
```bash
singularity shell -w centos7_cuda8.img
Singularity centos7_cuda8.img:~> cp ~/Downloads/MotionCor2_1.1.0-Cuda80 /usr/local/bin/
chmod +x /usr/local/bin/MotionCor2_1.1.0-Cuda80
```
* Create MotionCor2_1.1.0-Cuda80 helper script as follow:
```bash
# more /gpfs/sw/bin/MotionCor2_1.1.0-Cuda80
#!/bin/bash
singularity  exec --nv -B /gpfs:/gpfs /gpfs/sw/bin/centos7_cuda8.img /usr/local/bin/MotionCor2_1.1.0-Cuda80 "$@"
```
Replace /gpfs with the drive that you use to store movie frames.
We have tested MotionCor2 running from Singularity container vs MotionCor2 compiled natively. There was no difference in timing, meaning that Singularity provides no overhead.
