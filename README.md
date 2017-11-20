# HPC cluster setup guide
This setup guide explains what I've done to make tensorflow work under the Cambridge gpu cluster.

### Tensorflow installation

First, source ```run.sh``` in this directory (**comment out the source virtualenv part**), this loads a number of required modules such as cudnn and python.

I installed a tensorflow-gpu in virtualenv.
The installation follows the official documentation of tensorflow, nothing tricky.
I installed my virtualenv at ```~/tensorflow-env-py36```.

Now lets test your installation using the Interactive session.

### Some useful commands:
1. ```sacct```: shows what jobs are running under your account
2. ```mybalances```: as the name suggests, shows your balance
3. ```module list```: shows all the loaded modules

### Interactive session:
```sintr -A MULLINS-SL2-GPU -p pascal --gres=gpu:2 --time=01:00:00 --nodes=1 --ntasks=4```

You can check this [manual](https://slurm.schedmd.com/sbatch.html) for the parameters.
You need to load some modules now to make the virtualenv environment work.
Same as before, simply source ```run.sh```

You have then loaded some modules.
Type ```module list``` to make sure modules are loaded correctly:
```
1) dot                                6) singularity/current               11) rhel7/default-gpu
2) slurm                              7) rhel7/global                      12) cudnn/6.0_cuda-8.0
3) java/jdk1.8.0_45                   8) cuda/8.0                          13) python-3.6.1-gcc-5.4.0-xk7ym4l
4) turbovnc/2.0.1                     9) gcc-5.4.0-gcc-4.8.5-fis24gg
5) vgl/2.5.1/64                      10) openmpi-1.10.7-gcc-5.4.0-jdc7f4f
```

Make sure your virtualenv is active, now run:
```
# Creates a graph.
a = tf.constant([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], shape=[2, 3], name='a')
b = tf.constant([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], shape=[3, 2], name='b')
c = tf.matmul(a, b)
# Creates a session with log_device_placement set to True.
sess = tf.Session(config=tf.ConfigProto(log_device_placement=True))
# Runs the op.
print(sess.run(c))
```
This should print out the device placements which include gpus.

### Datasets
In our normal practice, we make use of the imagenet dataset.
An easier way to get access to the dataset is to copy(```scp```) them directly from whether ```sigyn``` or ```beyla```.

If you are using ```tfrecord``` files, you can run this [code](https://github.com/UCam-CompArch-NN/imagenet-tensorflow/tree/master/TFRecord_check) to check whether these data files are borken after ```scp```.

Notice, it is suggested to put the datasets in ```~/rds/hpc-work```

### Submit the job
To submit a job, simply run:
```Shell
sbatch gpu_cluster_setup/slurm_submit.wilkes2
```
The submitting script contains a number of setups and are relatively well-documented.
Thing you want to pay attentions are probably [here](https://github.com/Aaron-Zhao123/gpu_cluster_setup/blob/0cd1e992319838896dcd75b42e0618fd37b90778/slurm_submit.wilkes2#L88) and [here](https://github.com/Aaron-Zhao123/gpu_cluster_setup/blob/0cd1e992319838896dcd75b42e0618fd37b90778/slurm_submit.wilkes2#L13)

The first is a call to the scripts you want to run, the second link points to the setups (eg: num_gpus, num_cpus, time and etc).
