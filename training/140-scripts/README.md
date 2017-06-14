To run your R code from the command line


1. SSH into `lightfoot.vbi.vt.edu`
2. SSH into the RStudio container: `ssh 172.16.238.10`
3. `cd` into wherever you need to run your script
4. run it: `Rscript my_script.R`


If you get some scary warning about a middleman attacker or something,
it's because you've connected to the rstudio server before,
and it saved that *exact* version.
Since our servers are constantly being rebuilt, chances are it will not be the same from the last time you did this

To fix:

1. open up the known_hosts file: `nano ~/.ssh/known_hosts`
2. delete the line in the file that has the rstudio IP address (e.g., `172.16.238.10`)
3. save the known hosts file, and exit
4. try to `ssh` again
