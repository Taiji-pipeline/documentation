Advanced Usage
==============

Visualizing the workflow
------------------------

You can generate a HTML visualization of the Taiji pipeline using the following command:

:: 

    taiji view Taiji.html

Open the "Taiji.html" using your web browser to see what the pipeline can do.

Cache manipulation
------------------

The workflow cache (default to ``sciflow.db``) stores data that are neccessary for
resuming the program at any checkpoints.
Taiji provides several commands that allow you to inspect and clear the cache.

* Inspection: ``taiji show STEP_NAME``.
* Deletion: ``taiji delete STEP_NAME`` or ``taiji delete STEP_NAME --delete-all``.

Parallelism and Distributed computing
-------------------------------------

Use ``-n num_jobs`` to tell Taiji how many concurrent jobs you want to run.

If you run the program without distributed computing, you need to use ``+RTS -N10`` to
specify the number of cores (here we use 10 cores).

Taiji currently supports distributed computing systems like slurm or PBS.
To use this feature, first add configuration to your ``config.yml`` file.
Then this feature can be turned on by ``taiji run --config config.yml --cloud``.