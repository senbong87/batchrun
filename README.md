## About

[Batchrun] is a bash script which can be used for running a list of commands in a file.

[Batchrun] automatically ...

* runs the command in the file if the cpu usage is less than certain predefined limit.
* displays the running process, successfully exited process, unsuccessfully finished process.

## Quick Start

1. Download and Set up [Batchrun]

You can download the batchrun script, make it executable and put it in the directory which is in the `$PATH` environment variable. The following example assumes that `~/bin` is in your `$PATH` enviroment variable.

  ```
  $ chmod +x batchrun
  $ cp batchrun ~/bin
  ```

2. Tutorial for [Batchrun]

After finished previous step, you are ready to use `batchrun` to running a list of command in a file. To understand how `batchrun` works, please create following shell scripts and save it as `finite.sh`.

  ```
  #!/bin/bash
  # This script use 'for' loop to create a running process.
  for i in {1..1000000}; do
      ((1+1))
  done
  ```

Run the following command the make `finite.sh` executable.
  
  `$ chmod +x finite.sh`

In the same directory, create following file and save it as `commands.txt`

  ```
  ./finite.sh
  ./finite.sh
  ./finite.sh
  ./finite.sh
  ./finite.sh
  ```

Notice that each line contains a valid command. **_DO NOT SEPARATE ONE COMMAND INTO MULTIPLE LINES_** as this will cause parsing error in terminal. Then, run the following command to use [Batchrun] to execute the command in the `commands.txt`.

  `$ batchrun -i commands.txt`

After running this command, `batchrun` will run the commands in the `commands.txt` based on the current cpu usage. If the current cpu usage exceeds default cpu limit specified in the `batchrun`, `batchrun` will stop issuing subsequent command in the `commands.txt` until the cpu usage falls below the limit. [Batchrun] generates a number of log files in the working directory. These log files provide following information:

  * .process_log: list of commands have been issued in the terminal
  * .success_log: list of commands whose process has successfully exited
  * .failure_log: list of commands whose process has exited with failure
  * .running_log: list of commands whose process is still running
  * .cpu_usage.XXXX: current cpu usage (where XXXX denotes random generated characters)

After running previous `batchrun` command, `batchrun` will generate above log files. Before issuing `batchrun` commands, you have to delete these log files. You can run following command to delete these file:

  `$ batchrun -c`

To specify the cpu limit, `--limit` flag can be used. For example:

  `$ batchrun -l 20 -i commands.txt`

## Warning

[Batchrun] only check whether the first argument of the command is executable. [Batchrun] will not check whether the user issues correct command or not.

## TODO

* Enable user to reset cpu limit when the `batchrun` is running.
* Check whether the command in the input file is complete or not (e.g. missing quotes).

[Batchrun] is a work in progress, so any ideas or patches are appreciated.

[Batchrun]: https://github.com/senbong87/batchrun
