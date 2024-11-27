Measurer not getting the right library name
https://github.com/FAIRiCUBE/common-code/blob/e94501bebebf2474b52a0342cf2aabc8e32a87b2/record-computational-demands-automatically/example.py#L20

- it would be nicer if the measurer got the full name of the library instead of the alias. Example `geopandas` instead of `gpd`
- some libraries are not recorded

Example libraries in the script:
````
import sqlite3
import pandas as pd
import xarray as xr
from src import utils
from src.measurer import Measurer
import time
from dask.distributed import Client
from dask.distributed import LocalCluster
import geopandas as gpd
```

What the measurer gets:
````
sqlite3
pd
xr
utils
time
gpd
```
Here `dask` library is missing.
Hi 

- I just updated the code (example.py) and now the measurer returns the full name instead of the alias.

- Regarding undetected libraries, the problem is given by the type different than ModuleType when using the 'from' statement and I am trying to solve it.

Thanks

Maria,did the last version resolved your issue, can this issue be closed maybe?

yes, it looks like that the libraries are correctly listed