VScode构建CMake工程需要进行以下几点：
* 配置CMake参数
* 编写cpp_properties.json
* 编写launch.json(gdb信息)

1. 配置CMake

首先创建工程文件夹，进入文件夹后，按ctrl+shift+p调出命令栏，找到CMake：Configure，选择后即可。
CMake工程会将所有的生成文件都生成到${工作文件夹}/build中，可以通过编写cmakelist.txt进行修改。

2. 编写c_cpp_properties.json

从命令栏中选中c/cpp:edit confifurations(json),c/c++插件会自动生成一个cpp_properties.json，在此json文件中加入"configurationProvider": ""configurationProvider": "ms-vscode.cmake-tools"。该代码意味着从CMake-tool插件中获得代码检查信息。

3. 编写launch.json

从侧边栏中选中debug并创建一个launch.json,并将program修改为${command:cmake.launchTargetPath}，将miDebuggerPath修改为gdb.exe。