# Android 17 cpuset mount compatibility

Android 17 libprocessgroup mounts v1 cpuset with noprefix,cpuset_v2_mode.
The old kernel rejected cpuset_v2_mode with EINVAL. Backport the root flag,
mount-option parser/show handler, and all ten cpuset behavior checks from
upstream b8d1b8ee93df8ffbabbeadd65d39853cfad6d698 (Waiman Long, 2017).
The helper takes one root snapshot and checks NULL during initialization.

https://github.com/torvalds/linux/commit/b8d1b8ee93df8ffbabbeadd65d39853cfad6d698

This preserves allowed CPU/node masks over hotplug instead of merely accepting
and ignoring the option. No controller or permission checks are disabled.
Kernel compilation does not establish correct behavior on a physical phone.
