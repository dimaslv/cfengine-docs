---
layout: default
title: packagesmatching
published: true
tags: [reference, utility functions, functions, packages, inventory, packagesmatching]
---

[%CFEngine_function_prototype(package_regex, version_regex, arch_regex, method_regex)%]

**Description:** Return a data container with the list of installed packages
matching the parameters.

This function searches for the [anchored][anchored] regular expressions in the
list of currently installed packages.

The return is a data container with a list of package descriptions, looking like
this:

```
[
   {
      "arch":"default",
      "method":"dpkg",
      "name":"zsh-common",
      "version":"5.0.7-5ubuntu1"
   }
]
```

[%CFEngine_function_attributes(package_regex, version_regex, arch_regex, method_regex)%]

**Argument Descriptions:**

* `package_regex` - Regular expression matching packge name
* `version_regex` - Regular expression matching package version
* `arch_regex` - Regular expression matching package architecutre
* `method_regex` - Regular expression matching package method (apt-get, rpm, etc ...)

The following code extracts just the package names, then looks for
some desired packages, and finally reports if they are installed.

**IMPORTANT:** The data source used when querying depends on policy configuration.
When `package_inventory` in `body common control` is configured, CFEngine will record the packages installed and the package updates available for the configured package modules.
In the [Masterfiles Policy Framework][Masterfiles Policy Framework] `package_inventory` will be [configured](https://github.com/cfengine/masterfiles/blob/3dc1f629544b24261975ecf86e02554d4daf346e/promises.cf.in#L92) to the default for the hosts platform.
Since only one `body common control` can be present in a policy set any bundles which use these functions will typically need to execute in the context of a full policy run.
If there is no `package_inventory` attribute such as on package module unsupported platforms or when a policy entry file other than promises.cf is selected with the `--file -f` argument then the legacy package methods data will be used.
At no time will both standard and legacy data be available to these functions.

[%CFEngine_include_example(packagesmatching.cf)%]

**Example:**

```cf3
"all_packages" data => packagesmatching(".*", ".*", ".*", ".*");
```

**Refresh rules:**
* installed packages cache used by packagesmatching() is refreshed at the end of each agent run in accordance with constraints defined in the relevant package module body.
* installed packages cache is refreshed after installing or removing a package.
* installed packages cache is refreshed if no local cache exists.
        This means a reliable way to force a refresh of CFEngine's internal package cache is to simply delete the local cache:

```cf3
$(sys.statedir)/packages_installed_<package_module>.lmdb*
```

Or in the case of legacy package methods:

```cf3
$(sys.statedir)/software_packages.csv
```


**History:** Introduced in CFEngine 3.6

**See also:** `packageupdatesmatching()`, [Package information cache tunables in the MPF][Masterfiles Policy Framework#Configure periodic package inventory refresh interval]
