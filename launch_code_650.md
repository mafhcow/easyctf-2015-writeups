# Launch Code (650)

## Problem

Recently the NSA has intercepted a program which generates the launch codes for Chinese nuclear weapons.

The launch code generator program takes in a password which is at most 63 characters, and the NSA has also intercepted a password belonging to an agent identified as &quot;Agent 23102&quot;.&nbsp;Unfortunately, each launch code generator program is locked to a specific agent id, and each agent has a unique password.&nbsp;The program generates the correct launch code based on a combination of the agent id watermarked to the program and the correct password.

Our intel suggests that the program we have intercepted belongs to an agent with the id 5068, and the password we have intercepted belongs to an agent with the id of 23102.

**Your task:**
Reverse engineer the launch code generator program, and modify it so that it will work with agent 23102&#39;s password instead of agent 5068.&nbsp;Then, using the intercepted password, generate a valid launch code and report it back to us!

**Intercepted Program:** [LaunchCode.exe](files/LaunchCode.exe)

<small>MD5: bd1f54a4fa1f19168303275d6d02ca0f</small>

**Agent 23102&#39;s Passcode:** diguo zhuyi hai cunzai

## Hint

Hint: it's not Feynman.

## Write-Up

Open up the executable file using a program which can disassemble the exe such as IDA Pro Free. Using the "names" tab, jump to the portion of the code which stores aTheLaunchCodeI. Using this we can jump to the core of the exe by utilizing the data cross reference. 

The code which computes the launch code is extremely complex, but one particular instruction stands out. Specifically, where 0x1357 is moved into eax. We determine that hex(5068) = 0x13cc, which is relatively close to 0x1357. We hazard a guess that in general, to generate launch codes for agent N we instead need to move N - 117 into eax. So for agent 23102, we would need to put hex(23102 - 117) = 0x59c9 into eax. 

At this point one can either directly edit the file and then run the exe inputting password diguo zhuyi hai cunzai, or one can plant a breakpoint using IDA Pro and force modify eax to 0x59c9 after 0x1357 is moved in. As the exe quickly closes out after outputting the launch code the second method is preferrable due to the fact you can place another breakpoint after the launchcode is outputted. In either case the output is 6672-50f3-c62b-7231, which is the correct answer.
