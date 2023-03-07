# hash-miner-tester-trm
The tool implements a fake ethash pool that:  Tracks poolside hash rate, reported hash rate and stale shares. Displays the current bounds for the poolside hash rate at 99% and 95% confidence levels. 
Configurable static epoch.
Uses a configurable static difficulty, presumably much lower than normal pools.
Tracks mining as one huge session to obtain a single large sample set, no rolling windows.

Interpreting Results
The tester will continuously output info about shares, and the current estimate of hash rates every 10 secs. The hash rate information is the interesting part for us. This is an example of a 6h TeamRedMiner test with an aggressive nr shares/sec produced running on three Vegas, which means a dev fee of -1%. The hash rate has converged to the expected value after 1.67 million shares (and, it's mostly luck that it's so close to the target):

Hashrates for 1 active connections:
Global hashrate: 146.63 MH/s vs 148.11 MH/s avg reported [diff -1.01%] (A1670641:S3230:R0 shares, 22041 secs)
Global stats: 99.81% acc, 0.19% stale, 0% rej.
Global approx: [-1.26%, -0.76%] at 99% confidence, [-1.22%, -0.8%] at 95% confidence.
[1] hashrate:  146.63 MH/s vs 148.11 MH/s avg reported [diff -1.01%] (A1670641:S3230:R0 shares, 22041 secs)
In the output above, the "Global" lines are the most important ones. They represent the sum of all connections that have been made to the pool since start.

The first line presents the derived poolside hash rate (146.63 MH/s), the average of the reported hash rates (148.11 MH/s), the difference between the two (-1.01% in this case), and last the nr of A(ccepted), S(tale) and R(ejected) shares.

The second line presents statistics for all received shares.

The third line is the most important line for this test: using proven bounds for a Poisson distribution as per [3], it presents an interval around the current difference between the poolside and avg reported hash rates at 99% and 95% confidence levels. This means that in this specific case, we know that the true poolside hash rate the miner will generate over time is somewhere between -1.26% and -0.76% in 99% of all runs. For a somewhat lower confidence level, we can say that the true poolside hash rate is somewhere between -1.22% and -0.8%. This means that TeamRedMiner is well within the expected bounds after 1.67 million shares.

FOR PASS IN MESSAGES
