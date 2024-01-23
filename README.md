# fox-vgl-desktop
XFCE Desktop apptainer definition for use with Open Ondemand including VirtualGL support

This is just provided as an example - not a turn-key solution.

IMPORTANT:

- You must configure Apptainer to enable Nvidia support (see apptainer.org docs)
- You must ensure that the user(s) have permission to access the render interface (/dev/dri/<card>)
- You must pass the VGL_DISPLAY env var to the apptainer so it knows which one to use
- You must start the app with `vglrun` from VirtualGL
- You must edit the definition file to add a correct URL for the VirtualGL rpm.

An example snippet from submit.yml.erb on the Fox OOD implemenation of Paraview:
```
    export APPTAINERENV_CUDA_VISIBLE_DEVICES=$CUDA_VISIBLE_DEVICES
    pci=$(nvidia-smi --query-gpu=gpu_bus_id --format=csv,noheader | head -1 | cut -b 5- | tr '[:upper:]' '[:lower:]')
    card=$(ls -d /sys/bus/pci/devices/"$pci"/drm/card? | awk -F/ '{print $NF}')
    export VGL_DISPLAY=/dev/dri/$card
    apptainer run --env dridev=$VGL_DISPLAY --nv /cluster/opt/OOD/fox-paraview-0.0.1.sif /bin/bash container.sh
```

