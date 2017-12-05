# Pull image
docker pull phelipemizerani/seleniumchromeheadless

# What is included

* Node 6
* Google Chrome latest version
* ChromeDriver 2.33

# Usage

Enter the root directory of your project and execute the command below changing ${runTest} by your command that execute your tests.
 
```sh
$ docker run -i --rm --net=host -v /dev/shm:/dev/shm --privileged -v `pwd`:`pwd` -w `pwd` phelipemizerani/seleniumchromeheadless ${runTest}
```


#### Example
```sh
$ docker run -i --rm --net=host -v /dev/shm:/dev/shm --privileged -v `pwd`:`pwd` -w `pwd` phelipemizerani/seleniumchromeheadless npm run test
```

### Why mapping /dev/shm ?

Docker has hardcoded value of 64MB for /dev/shm. Because of that you can encounter an error session deleted becasue of page crash on memory intensive pages.

Up until Docker 1.10 the easiest way to mitigate that problem was to share /dev/shm with the host. Starting with Docker 1.10, we can use the option --shm-size to set the size of the shared memory to any arbitrary value. I recommend 2g, however you may want to experiment with this value.

### Why --privileged?

Chrome uses sandboxing, therefore if you try and run Chrome within a non-privileged container you will receive the following message:

"Failed to move to new namespace: PID namespaces supported, Network namespace supported, but failed: errno = Operation not permitted".

The --privileged flag gives the container almost the same privileges to the host machine resources as other processes running outside the container, which is required for the sandboxing to run smoothly.

### Why --net=host?

This options is required only if the dockerised Protractor is run against localhost on the host. Imagine this sscenario: you run an http test server on your local machine, let's say on port 8000. You type in your browser http://localhost:8000 and everything goes smoothly. Then you want to run the dockerised Protractor against the same localhost:8000. If you don't use --net=host the container will receive the bridged interface and its own loopback and so the localhost within the container will refer to the container itself. Using --net=host you allow the container to share host's network stack and properly refer to the host when Protractor is run against localhost.
