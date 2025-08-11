# ModEM-Examples

This repository contains a number of ModEM Examples for use with
[ModEM-Model][ModEM-Model]. The [Magnetetelluric directory][magnet_dir] 
contains basic examples of using the forward and inverse solver for both
2D and 3D.

Most examples contain a run script which you can fun after either copying
or linking a ModEM Executable:

```bash
$ ln -s ~/ModEM-Model/f90/Mod3DMT .
$ # Or:
$ cp ~/ModEM-Model/f90/Mod3DMT .
```

Then you should be able to run the example:

```bash
% bash run.inverse.mpi.sh
```

[magnet_dir]: /Magnetotelluric/

## More Information

For more information, see the [Releated Repositories](#related-repositories).
The [ModEM User's Guide][ModEM-Users-Guide] also contains more information for
running ModEM.

[ModEM-Users-Guide]: https://github.com/magnetotellurics/ModEM/blob/main/doc/userguide/ModEM_UserGuide.pdf

## Related Repositories

* [ModEM-Model][ModEM-Model] - ModEM-Model itself
* [ModEM-Tools][ModEM-Tools] - A collection of MatLab and Python tools
to manipulate ModEM input and output files.

[ModEM-Model]: https://github.com/magnetotellurics/ModEM
[ModEM-Tools]: https://github.com/magnetotellurics/ModEM-Tools
