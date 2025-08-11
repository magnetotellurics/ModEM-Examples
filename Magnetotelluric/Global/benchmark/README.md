Global Example
==============

This Global example can be ran with the [Global branch][global-branch] of the
ModEM-Model repository. After you compiled the `Global3D` you can run it simply
by running:

```
$ cd benchmark
$ ln -s ~/ModEM-Model/Global3D .
$ mpiexec -n 4 ./Global3D
$ # Or:
$ ./Global3D
```

The Global3D executable will read from the `fwd_startup` file.

[global-branch]: https://github.com/magnetotellurics/ModEM/tree/global
