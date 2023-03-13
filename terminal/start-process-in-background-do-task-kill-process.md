To run E2E tests that depend on our production build, we need to start a webserver, run our tests and finally kill our webserver. 
To do this, you need to start the webserver in the background and kill it once the E2E tests have run to completion. 
Here's a simple bash script that does that:

```sh
#!/bin/bash

# start our webserver, hide its output and move it to the background 
npm run serve > /dev/null 2>&1 &

# run our test harness
npm run test-harness

# kill the last started background process
kill $!
```

Alternatively, these would work as well:

* `kill %1` - kill the first job
* `kill %-` - kill the last job

References:
* https://stackoverflow.com/questions/30171050/start-a-process-in-background-do-a-task-then-kill-the-process-in-the-backgroun
* https://stackoverflow.com/questions/1624691/linux-kill-background-task
