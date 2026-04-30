# vm370r6

An experiment with VM/370R6 and how we can bridge a virtual mainframe and a git project.

The instructions and files in this project are based on the instructions in the R6-README.txt file.

# Base VM/370 R6 install

To automatically set up a base VM/370 environment, you'll start with clear DASD volumes. Remove the files provided in the repo and create new ones with:

```shell
dasdinit CPR6L0.3330-1 3330 CPR6L0 404 && \
dasdinit VMREL6.3330-1 3330 VMREL6 404
```

Then you start Hercules with the bootstrap configuration. This configuration doesn't use shadow files for the disks and will change them directly. This way, if you delete the shadow files, you restore the machine to its freshly installed state.

```shell
hercules -f vm370r6-bootstrap.cnf -r vm370r6-bootstrap.rc
```

A couple seconds later (on modern machines this process is really fast), you'll be able to reboot your hercules with the runtime config:

```shell
hercules -f vm370r6.cnf -r vm370r6.rc
```

Keep in mind this is a very spartan OS if compared with VM/380 CE. It expects terminals to have 24 lines and 80 columns and will not recognize anything different.

## What now?

The plan is to incrementally add the differences between this and the community edition in a way that each step can be examined. That will not be possible for every step, but we'll try to be close to it.