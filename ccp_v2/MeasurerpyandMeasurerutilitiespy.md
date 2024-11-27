Measurer.py and Measurer_utilities.py
Hi, 

is there a recommendation on where to store these files and which kernels to use for correct PyTorch usage and module loading?
I downlaoded these scripts to my UseCase Folder and I can successfully load the measurer module by using sys.path.append() beforehand but it cannot allocate measurer_utilities which is sitting in the same directory. 
I am having trouble with the measurere.end() function to properly run torch.cuda.is_available() depending on which kernel I start the Notebook with, and everythin associated with measurer_utilitites.bytes_to() independet from kernel.

Any tipps?


Hi 

Can you give us more details? What do you have as message error? Are using the up to date files?

Best,

-Bachir.
After restarting the kernel, it seems to work smoothly now, thank you anyways. :)