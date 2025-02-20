# How to enable GPU on Hyper-V GPU vm

This guide provides instructions for enabling GPU passthrough on a Hyper-V virtual machine. It is a translation of the original document found at [FreeDidi](https://www.freedidi.com/9857.html). in combine with this [Vidio tutorial](https://www.bilibili.com/video/BV1Fu41177Xj/?spm_id_from=333.1007.top_right_bar_window_default_collection.content.click&vd_source=b2973771919b8bc7d90d211c07483768 )


# Step 1: modify the VM settings
Run the following PowerShell commands, replacing `[VM_NAME]` with the name of your virtual machine:

```powershell
$vm = "[VM_NAME]"
Add-VMGpuPartitionAdapter -VMName $vm
Set-VMGpuPartitionAdapter -VMName $vm -MinPartitionVRAM 80000000 -MaxPartitionVRAM 100000000 -OptimalPartitionVRAM 100000000 -MinPartitionEncode 80000000 -MaxPartitionEncode 100000000 -OptimalPartitionEncode 100000000 -MinPartitionDecode 80000000 -MaxPartitionDecode 100000000 -OptimalPartitionDecode 100000000 -MinPartitionCompute 80000000 -MaxPartitionCompute 100000000 -OptimalPartitionCompute 100000000
Set-VM -GuestControlledCacheTypes $true -VMName $vm
Set-VM -LowMemoryMappedIoSpace 1Gb -VMName $vm
Set-VM -HighMemoryMappedIoSpace 32GB -VMName $vm
```

# Step 2: Copy Driver Files from Host to VM
1. Find the current host GPU driver file from `Deveice Manager`.  Eg: nvlt.inf_amd64_XXX
1. Navigate to the driver files on the host machine:  
Path: `C:\Windows\System32\DriverStore\FileRepository\`
2. Copy the files to the VM:  
Destination Path: `C:\Windows\System32\HostDriverStore\FileRepository\`


 

# Step 3: Copy the Display Driver from Host to VM
## For Nvidia GPUs:
1. Locate the driver on the host:  
Path: `C:\Windows\System32\nvapi64.dll`  
2. Copy it to the VM:  
Destination Path: `C:\Windows\System32\nvapi64.dll`


## For AMD GPUs:
Copy all files related to the GPU driver from the host machine.  
These files can be found in the GPU driver's directory within the driver manager.  
Ensure the copied files are placed in the same paths within the VM as on the host machine.  


# Notes:
This guide assumes you have administrative access to both the host and the VM.
Compatibility and success depend on your hardware and driver configuration.
