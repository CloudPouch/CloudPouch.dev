# CloudPouch User Guide

## Configuration
The `config.json` file location depends on the OS you're using:

* MacOs - `/Users/<YOUR_USER_NAME>/Library/Application Support/CloudPouch/config.json`
* Windows - `c:\Users\<YOUR_USER_NAME>\AppData\Roaming\CloudPouch\config.json`
* Linux - `~/.config/CloudPouch/config.json`
## Logs
The `main.log` file location depends on the OS you're using:

* MacOs - `/Users/<YOUR_USER_NAME>/Library/Logs/CloudPouch`
* Windows - `c:\Users\<YOUR_USER_NAME>\AppData\Roaming\CloudPouch\logs`
* Linux - `~/.config/CloudPouch/`

## Cached data
CloudPouch stores cached data as JSON files on your local hard drive:
* MacOs - `/Users/<YOUR_USER_NAME>/Library/Application Support/CloudPouch/data`
* Windows - `c:\Users\<YOUR_USER_NAME>\AppData\Roaming\CloudPouch\data`
* Linux - ``

# Troubleshooting

## Ubuntu Linux problem

When application doesn't want to start on Linux, check logs for `GPU process isn't usable. Goodbye.` message. Something similar to:

```
12:48:08.895 › Checking for update
12:48:08.931 › Generated new staging user ID: 08751b29-ea12-5a1c-b8b6-4256748d7cd6
(node:708150) ExtensionLoadWarning: Warnings loading extension at /home/<USER_NAME>/.config/CloudPouch/extensions/lmhkpmbekcpmknklioeibfkpmmfibljd:
  Unrecognized manifest key 'commands'.
  Unrecognized manifest key 'homepage_url'.
  Unrecognized manifest key 'page_action'.
  Unrecognized manifest key 'short_name'.
  Unrecognized manifest key 'update_url'.
  Permission 'notifications' is unknown or URL pattern is malformed.
  Permission 'contextMenus' is unknown or URL pattern is malformed.
  Cannot load extension with file or directory name _metadata. Filenames starting with "_" are reserved for use by the system.

(Use `cloudpouch --trace-warnings ...` to show where the warning was created)
Added Extension:  Redux DevTools
12:48:10.316 › Update for version 1.19.0 is not available (latest version: 1.19.0, downgrade is disallowed).
12:48:10.317 › checkForUpdatesAndNotify called, downloadPromise is null
[708150:0727/124810.448196:FATAL:gpu_data_manager_impl_private.cc(415)] GPU process isn't usable. Goodbye.
Trace/breakpoint trap (core dumped)
```
### solution
Run application with `--in-process-gpu` parameter. For example:
```
./Downloads/CloudPouch-1.19.0.AppImage --in-process-gpu
```
