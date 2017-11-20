# HPC cluster setup guide
This setup guide explains what I've done to make tensorflow work under the Cambridge gpu cluster.

First, source ```run.sh``` in this directory (**comment out the source virtualenv part**), this loads a number of required modules such as cudnn and python.

Type ```module list``` to make sure modules are loaded correctly:
```
```


I installed a tensorflow-gpu in virtualenv.
The installation follows the official documentation of tensorflow, nothing tricky.
I installed my virtualenv at ```~/tensorflow-env-py36```

### Some useful commands:
1. ```sacct```: shows what jobs are running under your account
2. ```mybalances```: as the name suggests, shows your balance
3. ```module list```: shows all the loaded modules

### Interactive session:
```sintr -A MULLINS-SL2-GPU -p pascal --gres=gpu:2 --time=01:00:00 --nodes=1 --ntasks=4```

You can check this [manual](https://slurm.schedmd.com/sbatch.html) for the parameters.
You need to load some modules now to make the virtualenv environment work.
