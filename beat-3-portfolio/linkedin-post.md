This semester, I've been contributing to an open-source project called ArduPilot. ArduPilot develops software for drones and other vehicles, and I have been investigating unexpected movement when a copter flying forward switches to an automated mission starting with a takeoff command. I was not originally able to replicate the issue, so I added a simulation test for that situation. 

I started by getting familiar with the codebase and making space on my Mac to run the simulator. Then I got started with setting up a simulator and then making a test and revising it based on recent feedback from the maintainer. In my latest run, the test did pass; however, I still haven't been able to reproduce the larger backward movement originally described in the issue. My pull request is open and under review. 

I have learned how to simulate a flight pattern, which was very cool. This experience helped me understand the importance of testing. Since I was unable to recreate the original issue, I had to rely on testing, which helped me understand how important it is to adapt to the resources available to me when I can't seem to find crucial details. In retrospect, I also think I should have asked the maintainer for assistance sooner. I will continue to investigate the issue and improve the test based on the maintainer's review.

https://github.com/ArduPilot/ardupilot/pull/35129
