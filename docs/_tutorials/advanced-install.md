---
title: "Installation Details"
date: 2020-10-28
---

The quickest way to get started with DeepSpeed is via pip, this will install
the latest release of DeepSpeed which is not tied to specific PyTorch or CUDA
versions. DeepSpeed includes several C++/CUDA extensions that we commonly refer
to as our 'ops'.  By default, all of these extensions/ops will be built
just-in-time (JIT) using [torch's JIT C++ extension loader that relies on
ninja](https://pytorch.org/docs/stable/cpp_extension.html) to build and
dynamically link them at runtime.

**Note:** [PyTorch](https://pytorch.org/) must be installed _before_ installing
DeepSpeed.
{: .notice--info}

```bash
pip install deepspeed
```

After installation, you can validate your install and see which ops your machine
is compatible with via the DeepSpeed environment report with `ds_report` or
`python -m deepspeed.env_report`. We've found this report useful when debugging
DeepSpeed install or compatibility issues.

```bash
ds_report
```

## Pre-install DeepSpeed Ops

Sometimes we have found it useful to pre-install either some or all DeepSpeed
C++/CUDA ops instead of using the JIT compiled path. In order to support
pre-installation we introduce build environment flags to turn on/off building
specific ops.

You can indicate to our installer (either install.sh or pip install) that you
want to attempt to install all of our ops by setting the `DS_BUILD_OPS`
environment variable to 1, for example:

```bash
DS_BUILD_OPS=1 pip install deepspeed
```

DeepSpeed will only install any ops that are compatible with your machine.
For more details on which ops are compatible with your system please try our
`ds_report` tool described above.

If you want to install only a specific op (e.g., FusedLamb), you can toggle
with `DS_BUILD` environment variables at installation time. For example, to
install DeepSpeed with only the FusedLamb op use:

```bash
DS_BUILD_FUSED_LAMB=1 pip install deepspeed
```

Available `DS_BUILD` options include:
* `DS_BUILD_OPS` toggles all ops
* `DS_BUILD_CPU_ADAM` builds the CPUAdam op
* `DS_BUILD_FUSED_ADAM` builds the FusedAdam op (from [apex](https://github.com/NVIDIA/apex))
* `DS_BUILD_FUSED_LAMB` builds the FusedLamb op
* `DS_BUILD_SPARSE_ATTN` builds the sparse attention op
* `DS_BUILD_TRANSFORMER` builds the transformer op
* `DS_BUILD_STOCHASTIC_TRANSFORMER` builds the stochastic transformer op
* `DS_BUILD_UTILS` builds various optimized utilities


## Install DeepSpeed from source

After cloning the DeepSpeed repo from GitHub, you can install DeepSpeed in
JIT mode via pip (see below). This install should complete
quickly since it is not compiling any C++/CUDA source files.

```bash
pip install .
```

For installs spanning multiple nodes we find it useful to install DeepSpeed
using the
[install.sh](https://github.com/microsoft/DeepSpeed/blob/master/install.sh)
script in the repo. This will build a python wheel locally and copy it to all
the nodes listed in your hostfile (either given via --hostfile, or defaults to
/job/hostfile).


## Feature specific dependencies

Some DeepSpeed features require specific dependencies outside of the general
dependencies of DeepSpeed.

* Python package dependencies per feature/op please
see our [requirements
directory](https://github.com/microsoft/DeepSpeed/tree/master/requirements).

* We attempt to keep the system level dependencies to a minimum, however some features do require special system-level packages. Please see our `ds_report` tool output to see if you are missing any system-level packages for a given feature.

## Pre-compiled DeepSpeed builds from PyPI

Coming soon
