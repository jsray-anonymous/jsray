# JSRay
This is the artifact to be reviewed for WWW24. Patches are anonymized and not released publicly.

We will provide more instructions and documentation in the final release.

## Environment
- Debian 9 (may also work on newer version, but not tested)
- At least 200GB of free disk space

## Build chromium
Check [official document](https://chromium.googlesource.com/chromium/src/+/main/docs/linux/build_instructions.md) if you meet any problem.

Install depot_tools
```
git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
# change to your own path
export PATH="$PATH:/path/to/depot_tools"
```

Get code
```
mkdir chromium && cd chromium
fetch --nohooks chromium

# have a cup of coffee

cd src
git checkout -b dev 96.0.4664.45
# If you miss build dependencies, try this
# ./build/install-build-deps.sh
gclient runhooks
```

Patch
```
cd third_party/blink
# change to your path
git apply /path/to/0001-jsray-blink.patch

cd ../../v8
# change to your path
git apply /path/to/0001-jsray-v8.patch
cd ../
```

Build
```
gn gen out/Default '--args=cc_wrapper="ccache" is_debug=false enable_nacl=false'
time autoninja -C out/Default/ chrome
```

# Test
```
/path/to/chrome --headless --no-sandbox --user-data-dir=profile file:///path/to/example/simple.html
```
Output will be logged in `out.log` with script activities. If there is any warning in the output, delete operation is performed. By tracing IDs in the log, we can further identify self-deleting scripts (scripts/generate).

Please note that using `file://` is for demonstration only!


