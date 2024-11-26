# Issue Summary

The model crashed with fatal error due to the number of allocated CPU processes not matching up with the parameters I specified in SIZE.h. My SIZE.h file is for a no mpi run since it only uses a single processor. 

# MITgcm Message

This error message is taken from the file STDERR.0000

(PID.TID 0000.0001) *** ERROR *** EEBOOT_MINIMAL: No. of procs=    16 not equal to nPx*nPy=     1
(PID.TID 0000.0001) *** ERROR *** EEDIE: earlier error in multi-proc/thread setting
(PID.TID 0000.0001) *** ERROR *** PROGRAM MAIN: ends with fatal Error

# Issue Remedy

Modified my SIZE.h for a MPI run for the correct number of CPU processes.
