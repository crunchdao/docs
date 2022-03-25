# Live Score Computation Process

Computing the live score each weeks is a very computationally intensive process.

Targets computer have to be made to make sure everything works as expected.

Here is a schema on how the process is working internally:

![internal schema](../.gitbook/assets/live-computer.drawio.svg)

### Active datasets lookup

To know which dataset is active for a weekly crunch, we need to compute the targets to see which of them are matching.

### Listing of `primary` submissions

Primaries submissions are the the ones that we use for the global live leaderboard. Since you can submit your work many times, it is important to make sure you selected the right submission! Listing them allows us to prepare the parallel computing coming next.

### Initializing the computer

For a dataset's targets to compute, we first need to download data from various provider. Then the intensive computation of the targets can start.

### Test cases

Once the initializer finished his works, the system select 3 random submission. They are used as test cases:

* If none of them are working, the problem is reported to the team. The dataset is skipped until the problem is resolved. The team will then restart the computation.
* If one of them is working, the test is passed! And we can continue to the next step.

### Live Score computation

For each primary submission, the scorer will compute a correlation score.

### Mean by dataset

Once all the submissions has been computed, they are grouped by user then by dataset. The average is what we call the 'dataset mean'.

This process is quite complex since we need to make sure everyone has always 2 scores. In cases where there is only 1, a lookup on previous datasets scores are used.



Many conditions need to be met:

* if the previous dataset is considered as 'updated' (new data has been provided) and you didn't participated: a zero will be given.
* if the previous dataset is not considered as 'updated', the next one will be tested.
* if there is no target corresponding, an equivalent weekly crunch will try to be found:
  * this mean that if the first dataset's target is RED, an equivalent score at target RED will be used. (same for GREEN or BLUE)
  * in case no such equivalent is found: a zero will be given.
* up to 3 rounds will be tested, if you didn't participated to any of them: a zero will be given.

### Mean of mean

Once the mean of all the datasets of a user has participated in has been computed, a global mean is computed. The mean is not fair though. If an user missed a dataset, the mean is still being done as if they have participated to all the datasets. Ensuring that someone that have only participated once will not be on the top instead of someone who participated to all of them.

### Computing global leaderboard

The global leaderboard positions are determined by the global correlation mean.
