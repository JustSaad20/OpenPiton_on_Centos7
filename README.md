This document provides a step-by-step guide for installing OpenPiton on a CentOS 7 system.

### 1. System Preparation

1. **Install dependencies:**
    - Run the following commands to install necessary dependencies:
        ```
        yum makecache
        yum update
        yum install python
        yum install -y perl ntp python3 perl perl-CPAN glibc.i686 elfutils-libelf-devel tcsh dtc
        ```

### 2. OpenPiton Setup

1. **Clone repository:**
    - Clone the OpenPiton repository using the following command:
        ```
        git clone https://github.com/PrincetonUniversity/openpiton.git
        ```

- **Set environment variables:**
    - Set the `PITON_ROOT` environment variable to the location of your cloned OpenPiton repository:
    - Remember to replace `/home/saad/Desktop/openpiton` with the actual location of your OpenPiton repository.
        ```
        export PITON_ROOT=/home/saad/Desktop/openpiton
        ```

- Navigate to the OpenPiton directory:

    ```
    cd $PITON_ROOT
    ```

- Source the setup scripts:

    ```
    source $PITON_ROOT/piton/piton_settings.sh
    source $PITON_ROOT/piton/ariane_setup.sh
    ```


- **Configure Git:**
    - As root user (using `su root`), modify your Git configuration file:
        ```
        su root
        nano ~/.gitconfig
        ```
- - Add the following lines to the file:
        
        ```
        [url "https://github.com/riscv"]
            insteadOf = git://github.com/riscv.git
        [url "https://github.com/qemu/"]
            insteadOf = git://git.qemu-project.org/
        [url "https://gitlab.com/qemu-project"]
            insteadOf = git://git.qemu-org
        [http]
            sslVerify = false
        [url "https://github.com/rth7680"]
            insteadOf = git://github.com/rth7680
        ```
![gitconfig_SS](/gitconfig_SS.png?raw=true)

    - Save and close the file.

- **Build tools:**
    - Run the following command to build the necessary tools:
        ```
        piton/ariane_build_tools.sh
        ```

### 3. Testing

1. **Navigate to build directory:**
    ```
    cd $PITON_ROOT/build
    ```

- **Run simulations:**
    - Run the following commands to first build the core for simulations:
        - **Manycore simulation (2x2 tiles):**
            ```
            sims -sys=manycore -x_tiles=2 -y_tiles=2 -vlt_build -ariane
            ```


- **Run the Dhrystone benchmark  as an example (2x2 tiles, precompiled, timeout 10 seconds):**
    ```
    sims -sys=manycore -vlt_run -x_tiles=2 -y_tiles=2 dhrystone.riscv -ariane -precompiled -rtl_timeout 10000000
    ```
![dhrystone](/dhrystone_run.png?raw=true)


### 4. Adding Environment Variables to Shell (Optional)

To automatically set the environment variables on every startup, you can add the following lines to your `.rc` file located in your home directory:

```
export PITON_ROOT=/home/saad/Desktop/openpiton
source $PITON_ROOT/piton/piton_settings.sh
source $PITON_ROOT/piton/ariane_setup.sh
```



This guide should assist you in setting up OpenPiton on your CentOS 7 system. Refer to the official OpenPiton documentation for further details and advanced functionalities.
