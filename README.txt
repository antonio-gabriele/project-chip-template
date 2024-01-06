Just to be clear: Not Working !!!

This is a test app to get matter inside cmake project, not your project inside matter !!!

Help needed.

You need to edit CMakeLists.txt file and change:
```
set(CHIP_ROOT /opt/connectedhomeip)
set(CHIP_ROOT_OUT ${CHIP_ROOT}/out/custom)
```
to reflect your project-chip folder.