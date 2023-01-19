"test software projects are as susceptible to failure as any other software project. In fact, in my experience, they fail more often, mainly because most organizations don't apply the same care and professionalism to their testware as they do to their shipping products."

"As a community, perhaps we're still in the phase of admiring the cool idea of test automation, and not yet to the point of recognizing its pitfalls and gotchas."

## Debunking the Classic Argument for Automation

"Automated tests execute a sequence of actions
without human intervention. This approach helps
eliminate human error, and provides faster
results. Since most products require tests to be
run many times, automated testing generally
leads to significant labor cost savings over time.
Typically a company will pass the break-even
point for labor costs after just two or three runs
of an automated test."

### Reckless assumption #1: Testing is a sequence of actions:

A more useful way to think about testing is as a sequence of interactions interspersed with evaluations. Some interactions are predictable some are not. 

If we try to reduce testing to a rote series of actions the result will be a narrow and shallow set of tests.

If you set out to automate all the necessary test execution, you'll probably spend a lot of money and time creating relatively weak tests that ignore many interesting bugs, and find many "problems" that turn out to be merely unanticipated correct behavior.

### Reckless Assumption #2 Testing means repeating the same actions over and over
Once a specific test case is executed a single time, and no bug is found, there is little chance that the test case will ever find a bug, unless a new bug is introduced into the system. 

If there is variation in the test cases, though, there is a greater likelihood of revealing problems both new and old. Variability is one of the great advantages of hand testing over script and playback testing.

Highly repeatable testing can actually minimize the chance of discovering all the important problems, for the same reason stepping in someone else's footprints minimizes the chance of being blown up by land mine.

### Reckless Assumption #3 We can automate testing actions.

Some tasks that are easy for people are hard for computers. Probably the hardest part of automation is interpreting test results. For GUI software, it is very hard to automatically notice all categories of significant problems while ignoring the insignificant problems

### Reckless Assumption #4: An automated test is faster, because it needs no human intervention.

All automated test suites require human intervention, if only to diagnose the results and fix broken tests. 

### Reckless Assumption #5: Automation reduces human error

Yes, some errors are reduced. Namely, the ones that humans make when they are asked carry out a long list of mundane mental and tactile activities.

### Reckless Assumption #6 We can quantify the costs and benefits of manual vs. automated testing

The truth is, hand testing and automated testing are really two different processes, rather than two different ways to execute the same process.

Therefore, direct comparison of them in terms of dollar cost or number of bugs found is meaningless.

### Reckless Assumption #7 Automation will lead to "significant labor cost savings."

"Typically a company will pass the break-even point for labor costs after just two or three runs of an automated test." This loosey goosey estimate may have come from field data or from the fertile mind of a marketing wonk. In any case, it's a crock. The cost of automated testing is comprised of several parts: ♦ The cost of developing the automation. ♦ The cost of operating the automated tests. ♦ The cost of maintaining the automation as the product changes. ♦ The cost of any other new tasks necessitated by the automation.

Writing a single test script is not necessarily a lot of effort, but constructing a suitable test harness can take weeks or months. As can the process of deciding which tool to buy, which tests to automate, how to trace the automation to the rest of the test process, and of course, learning how to use the tool and then actually writing the test programs

### Reckless Assumption #8 Automation will not harm the test project.

I've left for last the most thorny of all the problems that we face in pursuing an automation strategy: it's dangerous to automate something that we don't understand.