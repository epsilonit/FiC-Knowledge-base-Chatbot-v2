# How to add a logger to your processes
The Python library `loguru` offers an extremly simple way to create logfiles to register what happens in your scripts and notebooks, and when.
## Steps to take
1. Install `loguru` (`pip install loguru`)
2. Import the library `from loguru import logger`
3. Add a logfile `logger.add("path/to/logfile.log")`
4. Add `logger.info("Your comment")` to log when the execution reaches that point in the script
5. (optional) Add `@logger.catch` decorator to function definitions to log exceptions

### Example
```
# Set up logger
from loguru import logger
...
@logger.catch
def data_processing(data, year):
    <function definition>
...
@logger.catch
def data_saving(data, year):
    <function definition>
...
# add log file
logger.add("logfile.log")
...
logger.info(f"Processing data for year {year}")
data_processing(data, year)
logger.info("Finished processing data, now saving")
data_saving(data, year)
logger.info("Finished saving data")
...
```

## Use it with the `measurer`
To log when the measurer has started and stopped, and where the results are saved, pass the logger to the functions `Measurer.start` and `Measurer.end`.
### Example
```
tracker = measurer.start(data_path=data_path, logger=logger)
...
measurer.end(tracker=tracker,
             shape=[],
             libraries=[k for k, v in globals().items() if type(v) is ModuleType and not k.startswith('__')],
             data_path=data_path,
             csv_file"path/tp/csv.csv",
            logger=logger)
```