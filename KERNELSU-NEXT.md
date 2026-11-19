# KernelSU Next integration

Official upstream: https://github.com/KernelSU-Next/KernelSU-Next
Branch: `legacy`. Pinned commit: `e5f5ec998de88b9fc43c62cff8f7ae12d3f4800a`.
Reported version code: 33327 (upstream commit-count formula with offset 30289).

Source is vendored at KernelSU/, replacing the old backslashxx gitlink. No
recursive submodule checkout is needed. Upstream licenses, manager signature
verification and policy controls are retained. No manager APK is installed.

All three 8895 defconfigs enable built-in KSU and manual hooks. Native/compat
execve and execveat share one hook with real fd/flags pointers. The read syscall
uses the current one-argument Next API. Existing access/stat/reboot calls remain;
old-fork fstat-return and SELinux callbacks are removed.

KSU can be disabled using scripts/config --file <output>/.config --disable KSU
followed by olddefconfig. CI checks all three device configs, then compiles the
same device with KSU disabled and enabled. This does not replace phone testing.

The official Next Kbuild includes compatibility backports that may modify kernel
source while building. CI preserves that diff in its diagnostics. Build success,
Android 17 boot, manager authorization and root operation are not yet verified.

## Verified build result

Actions run 44379364933 passed: all three device configurations, S8 Image with
Next enabled (linked kernelsu_init symbol), and S8 Image with KSU disabled.
The exact compatibility source changes recorded by that successful run are now
committed, avoiding reliance on late source mutation and an incremental retry.
S8+/Note8 full Image builds, Android 17 ROM boot and manager/root runtime tests
remain unverified.

https://github.com/BasicOS2025/android_kernel_samsung_universal8895/actions/runs/44379364933
