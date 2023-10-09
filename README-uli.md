Trying to build the appimage
============================

I use Ubuntu-20.04 as basis for the build.

```
$ sudo apt install -y jq git g++ gcc make python2.7 pkg-config libx11-dev libxkbfile-dev libsecret-1-dev libkrb5-dev imagemagick

$ git clone git@github.com:uli-heller/vscodium.git
Cloning into 'vscodium'...
remote: Enumerating objects: 5172, done.
remote: Counting objects: 100% (5169/5169), done.
remote: Compressing objects: 100% (2088/2088), done.
remote: Total 5172 (delta 2892), reused 5075 (delta 2884), pack-reused 3
Receiving objects: 100% (5172/5172), 29.14 MiB | 4.32 MiB/s, done.
Resolving deltas: 100% (2892/2892), done.

$ # node-v16.20.2-linux-x64.tar.xz manually installed, node18 does not work
$ npm install --global yarn

$ ./build/build.sh -p
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

### 'vscode-linux-x64-build-rpm' errored after 47 ms

```
...
[17:21:36] Finished 'vscode-linux-x64-build-deb' after 1.18 min
Done in 76.25s.
++ [[ '' != \n\o ]]
++ yarn gulp vscode-linux-x64-build-rpm
yarn run v1.22.19
$ node --max_old_space_size=8192 ./node_modules/gulp/bin/gulp.js vscode-linux-x64-build-rpm
[17:21:42] Using gulpfile ~/build/vscodium/vscode/gulpfile.js
[17:21:42] Starting 'vscode-linux-x64-build-rpm'...
[17:21:42] Starting clean-x86_64 ...
[17:21:42] Finished clean-x86_64 after 2 ms
[17:21:42] Starting vscode-linux-x64-prepare-rpm ...
[17:21:42] 'vscode-linux-x64-build-rpm' errored after 47 ms
[17:21:42] Error: find-requires failed with exit code null.
stderr: null
    at calculatePackageDeps (/home/ubuntu/build/vscodium/vscode/build/linux/rpm/calculate-deps.js:31:15)
    at /home/ubuntu/build/vscodium/vscode/build/linux/rpm/calculate-deps.js:12:44
    at Array.map (<anonymous>)
    at generatePackageDeps (/home/ubuntu/build/vscodium/vscode/build/linux/rpm/calculate-deps.js:12:32)
    at Object.getDependencies (/home/ubuntu/build/vscodium/vscode/build/linux/dependencies-generator.js:63:50)
    at /home/ubuntu/build/vscodium/vscode/build/gulpfile.vscode.linux.js:187:46
    at /home/ubuntu/build/vscodium/vscode/build/lib/task.js:45:28
    at new Promise (<anonymous>)
    at _doExecute (/home/ubuntu/build/vscodium/vscode/build/lib/task.js:34:12)
    at _execute (/home/ubuntu/build/vscodium/vscode/build/lib/task.js:25:11)
error Command failed with exit code 1.
info Visit https://yarnpkg.com/en/docs/cli/run for documentation about this command.
```

Fixed by:

```diff
diff --git a/prepare_assets.sh b/prepare_assets.sh
index fdee918..42f101d 100755
--- a/prepare_assets.sh
+++ b/prepare_assets.sh
@@ -4,6 +4,7 @@
 set -e
 
 APP_NAME_LC="$( echo "${APP_NAME}" | awk '{print tolower($0)}' )"
+SHOULD_BUILD_RPM=no
 
 npm install -g checksum
```

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

New error:

```
...
+ sed -i -e 's|^_script+=("||g' /tmp/recipe_script
+ sed -i -e 's|")$||g' /tmp/recipe_script
+ bash -ex /tmp/recipe_script
+ sed -i -e 's|/usr/share/pixmaps/||g' usr/share/applications/codium.desktop
+ cp usr/share/applications/codium.desktop .
+ cp usr/share/pixmaps/vscodium.png .
+ /usr/bin/convert vscodium.png -resize 512x512 usr/share/icons/hicolor/512x512/apps/vscodium.png
/tmp/recipe_script: line 4: /usr/bin/convert: No such file or directory
```

Fixed by:

```
sudo apt install imagemagick
```

Still no appimage!

```diff
diff --git a/build/linux/appimage/recipe.yml b/build/linux/appimage/recipe.yml
index 59c2737..b4a8ee0 100644
--- a/build/linux/appimage/recipe.yml
+++ b/build/linux/appimage/recipe.yml
@@ -17,7 +17,7 @@ ingredients:
   script:
     - pwd
     - cp ../../../../vscode/.build/linux/deb/amd64/deb/*.deb .
-    - ls @@APPNAME@@_*.deb | cut -d _ -f 2 > VERSION
+    - ls -t @@APPNAME@@_*.deb | head -1 | cut -d _ -f 2 > VERSION
 
 script:
   - sed -i -e 's|/usr/share/pixmaps/||g' usr/share/applications/@@APPNAME@@.desktop
```

Now I see an appimage!
