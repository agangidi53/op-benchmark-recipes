# Running SPEC jbb2015 on Power ppc64le systems
## Installation  
  
Download the SPECjbb2015 suite from the SPEC website and install it:

[https://www.spec.org/order.html](https://www.spec.org/order.html)

Copy the included tar file to the _SPECjbb2015_ installation directory. From there, untar it to obtain additional scripts and configuration files needed to run the benchmark:
```bash
cp ibm_java8_sr3fp22.power8.kit.tar.xz $specjbb2015_install_dir
cd $specjbb2015_install_dir
tar -xvf ibm_java8_sr3fp22.power8.kit.tar.xz
```

Please choose to overwrite all files.

>Note: If needed, the kit can be run with OpenJDK. For ease of use, IBM SDK 8.0.3.22 has been included in the tar file. Newer versions of the IBM SDK can be obtained at https://developer.ibm.com/javasdk/downloads/sdk8/
## Running the benchmark (as root user)
Before running, make sure the _numactl_ package is installed and the system smt setting is _on_. In Ubuntu:
```bash
apt install numactl && ppc64_cpu --smt=on
```

Inside the _SPECjbb2015_ installation dir and depending of the Power machine configuration (number of sockets and cores), run the appropriate script below :

    Single socket, 10 cores (Habanero) run : ./run_multi.1socket.10core.sh 
    
    Two sockets, 20 cores (10 cores/socket, Firestone) run: ./run_multi.2socket.20core.sh
    
    Two sockets, 24 cores - DCM (2 chips and 12 cores/socket, Tuleta) run: ./run_multi.2socket.dcm.24core.sh

> Note: The scripts apply the appropriate tuning to the machine before running the benchmarks by calling `tune.sh` automatically, so this does not have to be done as a separate step.

If your configuration is different from above, the scripts can be modified for your hardware. The number of `groups` in the benchmark depend on the number of `NUMA nodes` in the machine, and the `processor binding` depends on the `number of cores` in each NUMA node. The differences in the scripts above illustrate the changes required.

### Monitoring the progress of the run and getting results

A benchmark run takes about 2 hours to complete. Each run creates a results directory with the current time stamp as the name.

While the benchmark is in progress, to check its status:
```bash
tail -f controller.out
```

When the run ends, the tail of `controller.out` contains the metrics of interest *max jOPs* and *critical jOPs*

For more information please check [https://www.spec.org/jbb2015](https://www.spec.org/jbb2015/), where a `Users Guide` and other details on the benchmark can be obtained.
