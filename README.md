[简体中文](./README_CN.md)

# RendererTest
A simple CPU-side ray tracer implementation  
Check `files/Ray-Tracing.pdf` for technical details

## Build
The source code included in this repository can be compiled directly on Windows and Linux.  
If compiling on Windows, ensure that the required dependency library (SDL2) files are correctly placed: the `bin` directory contains `.dll` dynamic library files, and the `lib` directory contains static library files.  
If compiling on Linux, please install SDL2 via the system package manager first.  
Recommended to use CLion IDE for one-click compilation.

### Windows
After cloning the repository, simply use CMake to compile.

### Ubuntu/Debian
1. Install SDL2 dependency libraries and GCC using the system package manager
```
sudo apt update && sudo apt install libsdl2-dev libsdl2-image-dev gcc g++
```
2. Install Intel OIDN denoiser using oneAPI  
Install necessary tools
```
sudo apt update && sudo apt install -y gpg-agent wget
```

Download and add the Intel GPG public key, which is used to verify the integrity of packages downloaded from the Intel repository
```
wget -O- https://apt.repos.intel.com/intel-gpg-keys/GPG-PUB-KEY-INTEL-SW-PRODUCTS.PUB \ | gpg --dearmor | sudo tee /usr/share/keyrings/oneapi-archive-keyring.gpg > /dev/null
```

Add the OneAPI repository to the APT source list, which creates a new file telling the system where to get OneAPI packages
```
echo "deb [signed-by=/usr/share/keyrings/oneapi-archive-keyring.gpg] https://apt.repos.intel.com/oneapi all main" | sudo tee /etc/apt/sources.list.d/oneAPI.list
```

Let the package manager fetch new packages and install OIDN (To install all tools: sudo apt install intel-basekit)
```
sudo apt update && sudo apt install intel-renderkit
```

Configure the runtime dynamic library search path (To undo changes: sudo rm /etc/ld.so.conf.d/oneapi.conf && sudo ldconfig)
```
echo "/opt/intel/oneapi/oidn/2.2/lib/" | sudo tee /etc/ld.so.conf.d/oneapi.conf && sudo ldconfig
```

Then use CMake to compile, passing CMake arguments (needs to be replaced with the actual installation path, as it may vary by version)
```
-DOpenImageDenoise_DIR=/opt/intel/oneapi/oidn/2.2/lib/cmake/OpenImageDenoise/
```

3. Set environment variables (required at runtime)  
1. Running with CLion
```
Add environment variables in: Top right project name -> Dropdown menu -> Edit Configuration -> Environment Variables
1. Execute source /opt/intel/oneapi/setvars.sh in a new terminal
2. echo $LD_LIBRARY_PATH
3. Copy the entire output string as the environment variable value
4. Save changes
```

2. Running in the terminal (open a new terminal); the second command does not need to be strictly followed, just run the compiled executable in.
```
1.source /opt/intel/oneapi/setvars.sh
2.cd bin && ./RendererTest
```