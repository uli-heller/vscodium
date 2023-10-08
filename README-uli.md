Trying to build the appimage
============================

I use Ubuntu-20.04 as basis for the build.

```
$ sudo apt install -y jq git g++ gcc make python2.7 pkg-config libx11-dev libxkbfile-dev libsecret-1-dev libkrb5-dev
...

$ git clone git@github.com:uli-heller/vscodium.git
Cloning into 'vscodium'...
remote: Enumerating objects: 5172, done.
remote: Counting objects: 100% (5169/5169), done.
remote: Compressing objects: 100% (2088/2088), done.
remote: Total 5172 (delta 2892), reused 5075 (delta 2884), pack-reused 3
Receiving objects: 100% (5172/5172), 29.14 MiB | 4.32 MiB/s, done.
Resolving deltas: 100% (2892/2892), done.

$ ./build/build.sh 
OS_NAME="linux"
SKIP_SOURCE="no"
SKIP_BUILD="no"
SKIP_ASSETS="yes"
VSCODE_ARCH="x64"
VSCODE_LATEST="no"
VSCODE_QUALITY="stable"
get_repo.sh: line 24: jq: command not found

$ # node-v16.20.2-linux-x64.tar.xz manually installed, node18 does not work
$ npm install --global yarn

$ ./build/build.sh
```

## Problems

### gssapi/gssapi.h: No such file or directory

```
...
gyp info spawn args ]
gyp info spawn make
gyp info spawn args [ 'BUILDTYPE=Release', '-C', 'build' ]
make: Entering directory '/home/ubuntu/build/vscodium/vscode/node_modules/kerberos/build'
  CXX(target) Release/obj.target/kerberos/src/kerberos.o
In file included from ../src/kerberos_common.h:5,
                 from ../src/kerberos.h:12,
                 from ../src/kerberos.cc:1:
../src/unix/kerberos_gss.h:21:14: fatal error: gssapi/gssapi.h: No such file or directory
   21 |     #include <gssapi/gssapi.h>
      |              ^~~~~~~~~~~~~~~~~
compilation terminated.
make: *** [kerberos.target.mk:128: Release/obj.target/kerberos/src/kerberos.o] Error 1
make: Leaving directory '/home/ubuntu/build/vscodium/vscode/node_modules/kerberos/build'
gyp ERR! build error 
gyp ERR! stack Error: `make` failed with exit code: 2
gyp ERR! stack     at ChildProcess.onExit (/home/ubuntu/Software/node-v16.20.2-linux-x64/lib/node_modules/npm/node_modules/node-gyp/lib/build.js:201:23)
gyp ERR! stack     at ChildProcess.emit (node:events:513:28)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (node:internal/child_process:293:12)
gyp ERR! System Linux 5.15.0-84-generic
gyp ERR! command "/home/ubuntu/Software/node-v16.20.2-linux-x64/bin/node" "/home/ubuntu/Software/node-v16.20.2-linux-x64/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /home/ubuntu/build/vscodium/vscode/node_modules/kerberos
gyp ERR! node -v v16.20.2
gyp ERR! node-gyp -v v9.1.0
gyp ERR! not ok
info Visit https://yarnpkg.com/en/docs/cli/install for documentation about this command.
```

Fixed by:

```
$ sudo apt install libkrb5-dev
```

### error /home/ubuntu/build/vscodium/vscode/node_modules/native-keymap: Command failed.

```
...
[4/4] Building fresh packages...
error /home/ubuntu/build/vscodium/vscode/node_modules/native-keymap: Command failed.
Exit code: 1
Command: node-gyp rebuild
Arguments: 
Directory: /home/ubuntu/build/vscodium/vscode/node_modules/native-keymap
Output:
gyp info it worked if it ends with ok
gyp info using node-gyp@9.1.0
gyp info using node@16.20.2 | linux | x64
gyp info find Python using Python version 3.8.10 found at "/usr/bin/python3"
gyp info spawn /usr/bin/python3
gyp info spawn args [
gyp info spawn args   '/home/ubuntu/Software/node-v16.20.2-linux-x64/lib/node_modules/npm/node_modules/node-gyp/gyp/gyp_main.py',
gyp info spawn args   'binding.gyp',
gyp info spawn args   '-f',
gyp info spawn args   'make',
gyp info spawn args   '-I',
gyp info spawn args   '/home/ubuntu/build/vscodium/vscode/node_modules/native-keymap/build/config.gypi',
gyp info spawn args   '-I',
gyp info spawn args   '/home/ubuntu/Software/node-v16.20.2-linux-x64/lib/node_modules/npm/node_modules/node-gyp/addon.gypi',
gyp info spawn args   '-I',
gyp info spawn args   '/home/ubuntu/.cache/node-gyp/25.8.4/include/node/common.gypi',
gyp info spawn args   '-Dlibrary=shared_library',
gyp info spawn args   '-Dvisibility=default',
gyp info spawn args   '-Dnode_root_dir=/home/ubuntu/.cache/node-gyp/25.8.4',
gyp info spawn args   '-Dnode_gyp_dir=/home/ubuntu/Software/node-v16.20.2-linux-x64/lib/node_modules/npm/node_modules/node-gyp',
gyp info spawn args   '-Dnode_lib_file=/home/ubuntu/.cache/node-gyp/25.8.4/<(target_arch)/node.lib',
gyp info spawn args   '-Dmodule_root_dir=/home/ubuntu/build/vscodium/vscode/node_modules/native-keymap',
gyp info spawn args   '-Dnode_engine=v8',
gyp info spawn args   '--depth=.',
gyp info spawn args   '--no-parallel',
gyp info spawn args   '--generator-output',
gyp info spawn args   'build',
gyp info spawn args   '-Goutput_dir=.'
gyp info spawn args ]
Package xkbfile was not found in the pkg-config search path.
Perhaps you should add the directory containing `xkbfile.pc'
to the PKG_CONFIG_PATH environment variable
No package 'xkbfile' found
gyp: Call to '${PKG_CONFIG:-pkg-config} x11 xkbfile --cflags | sed s/-I//g' returned exit status 0 while in binding.gyp. while trying to load binding.gyp
gyp ERR! configure error 
gyp ERR! stack Error: `gyp` failed with exit code: 1
gyp ERR! stack     at ChildProcess.onCpExit (/home/ubuntu/Software/node-v16.20.2-linux-x64/lib/node_modules/npm/node_modules/node-gyp/lib/configure.js:284:16)
gyp ERR! stack     at ChildProcess.emit (node:events:513:28)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (node:internal/child_process:293:12)
gyp ERR! System Linux 5.15.0-84-generic
gyp ERR! command "/home/ubuntu/Software/node-v16.20.2-linux-x64/bin/node" "/home/ubuntu/Software/node-v16.20.2-linux-x64/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /home/ubuntu/build/vscodium/vscode/node_modules/native-keymap
gyp ERR! node -v v16.20.2
gyp ERR! node-gyp -v v9.1.0
gyp ERR! not ok
info Visit https://yarnpkg.com/en/docs/cli/install for documentation about this command.
```

Fixed by:

```
sudo apt-get install -y g++ gcc make python2.7 pkg-config libx11-dev libxkbfile-dev libsecret-1-dev
```

According to [StackOverflow](https://stackoverflow.com/questions/55878536/no-package-xkbfile-found-when-build-vscode-on-ubuntu).

### No AppImage

```diff
diff --git a/prepare_assets.sh b/prepare_assets.sh
index 44cc343..fdee918 100755
--- a/prepare_assets.sh
+++ b/prepare_assets.sh
@@ -133,9 +133,9 @@ else
     yarn gulp "vscode-linux-${VSCODE_ARCH}-build-rpm"
   fi
 
-  # if [[ "${SHOULD_BUILD_APPIMAGE}" != "no" ]]; then
-  #   . ../build/linux/appimage/build.sh
-  # fi
+  if [[ "${SHOULD_BUILD_APPIMAGE}" != "no" ]]; then
+    . ../build/linux/appimage/build.sh
+  fi
 
   cd ..
 
@@ -156,12 +156,12 @@ else
     mv vscode/.build/linux/rpm/*/*.rpm assets/
   fi
 
-  # if [[ "${SHOULD_BUILD_APPIMAGE}" != "no" ]]; then
-  #   echo "Moving AppImage"
-  #   mv build/linux/appimage/out/*.AppImage* assets/
+  if [[ "${SHOULD_BUILD_APPIMAGE}" != "no" ]]; then
+    echo "Moving AppImage"
+    mv build/linux/appimage/out/*.AppImage* assets/
 
-  #   find assets -name '*.AppImage*' -exec bash -c 'mv $0 ${0/_-_/-}' {} \;
-  # fi
+    find assets -name '*.AppImage*' -exec bash -c 'mv $0 ${0/_-_/-}' {} \;
+  fi
 
   VSCODE_PLATFORM="linux"
 fi
```