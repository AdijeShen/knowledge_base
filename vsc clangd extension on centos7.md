In centos7, when using vscode to develop cpp code with extension clangd. It's unable to run clangd because the libc version problem. 

The problem solver should be compile clangd from source: ref : https://jdhao.github.io/2021/07/03/install_clangd_on_linux/
```
git clone --depth=1 https://github.com/llvm/llvm-project.git

cd llvm-project
mkdir build && cd build
cmake -G "Unix Makefiles" -DLLVM_ENABLE_PROJECTS="clang;clang-tools-extra" -DCMAKE_INSTALL_PREFIX=~/tools/llvm -DCMAKE_BUILD_TYPE=Release ../llvm
make -j 16
make install
```

then in the vscode setting.json, set the clangd.path
```
"clangd.path": "/root/tools/llvm/bin/clangd",
```

then in the cpp project, a `compile_commands.json` should be added for clangd to recognize the header,
i'm using a cmake to compile to project, so add a compile flag with cmake:
```
cmake -DCMAKE_EXPORT_COMPILE_COMMANDS=1
```

e.g.


```
cmake -S . -B build -DCMAKE_EXPORT_COMPILE_COMMANDS=1
```

then the `compile_commands.json` would be in build directory, put it into the source dir to make clangd available.
