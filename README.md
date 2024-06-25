# JFS Kernel Hang Bug Reproduction

We have prepared scripts to reproduce a non-deterministic Kernel Hang bug (likely a race) in the Journaled File System (JFS). We have used Linux Kernel v6.6.1 and v6.9.4 in our experiments, however, we recommend using v6.9.4. A sample dmesg kernel log, captured during this hang bug triggered using our replayer on JFS (mounted on a ramdisk with Linux v6.9.4), is available in this repository as dmesg_jfs_kernel_hang.txt.

* To setup JFS, execute the following command:

> sudo bash ./setup_jfs.sh

Depending upon the function invoked (setup_jfs_on_ramdev or setup_jfs_on_loopdev) in the script, this command sets up up a ramdisk (of size 16 MiB) or a loop device, initializes it using dd, and finally sets up the JFS file system.  Note that we are using all the default options while setting up JFS.

* Then compile the replayer using the following command:

> make replayer

This will produce an executable named 'replay'

* Post this execute the below command:

> sudo bash ./loop_replay.sh

This command replays the sequence of operations capture in the jfs_op_sequence.log file, in a loop for a total of 100 iterations.  A single execution of the replayer takes around 4 minutes, thus completing the complete execution of script in ~5 hrs.  Through our experiments, we have found that this approach helps us in reproducing the non-deterministic kernel hang bug for more than 50% of the executions. During successful executions, when running the replayer on a ramdisk, the bug is typically encountered within 2 hours.
