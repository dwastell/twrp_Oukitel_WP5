ACKNOWLEDGEMENTS
My thanks primarily to rokibhasansagar whose guide to building TWRP for MTK devices inspired me, a novice, to take the plunge:
https://gist.github.com/rokibhasansagar/15c8e728d94a6bd35a687aac73ef79a5

Rokib provided vital assitance along the way, including crucial patches to roomservice.py (see /MISC) which are required so that TWRP can be built with omniROM for unofficial devices. Rokib's droid-builder docker container is also used for the build itself

I also acklowedge chankruze whose WP1 tree provided a useful template: https://github.com/TeamWin/android_device_oukitel_wp1

The youtube videos of AlaskaLinuxUser were also very helpful: https://www.youtube.com/channel/UCnGqG_jyyXmTzdamBpKfeHA

BUILD DETAILS
A ready-built twrp image may be found in /MISC (Note this is for a 4 GB WP5). 
1) Download the TWRP build platform for Android Pie (9.0)
https://github.com/minimal-manifest-twrp/platform_manifest_twrp_omni/tree/twrp-9.0
2) Create the directory /oukitel/wp5 inside /device and clone this repository there
3) The directory MISC is not part of the tree. It contains the patched roomservice.py, which should be copied to /vendor/omni/build/tools replacing the existing version.
4) The recovery image can then be build with the following shell script (docker should, of course, be installed)

make clean
docker run --rm -i -e USER_ID=$(id -u) -e GROUP_ID=$(id -g) -v "$(pwd):/home/builder/twrp/:rw,z" -v "${HOME}/.ccache:/srv/ccache:rw,z"  fr3akyphantom/droid-builder bash << EOF
cd /home/builder/twrp
source build/envsetup.sh
export ALLOW_MISSING_DEPENDENCIES=true
lunch omni_wp5-eng
make -j$(nproc --all) recoveryimage
exit
EOF

5) recovery.img will be found in /out/target/product/wp5. Flash this with fastboot, having ensured you have a copy of the stock recovery.img in case of disaster!

MAGISK INTALLATION
I have experienced various problems flashing some versions of Magisk, leading to boot-loops. The version in /MISC works for me, so I have included it here. Also beware of updates with Magisk Manager - these can sometimes also lead to boot-loops and possible corruption of the recovery partition. Best to back up the your current boot.img with TWRP before flashing.



