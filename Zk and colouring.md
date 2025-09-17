From the ideas i got from this vid: https://youtu.be/Otvcbw6k4eo
The vid involves a method for  [[interactive_zk_proofs]] of [[graph coloring problems]], which they claim can be used to model a lot of problems.
The method has a lot of things that remind me of stuff ive been reading about, a lot of parallels.
The method
- I solve a coloring problem in a particular [[graph]].
- Any permutation of the colors (red becomes blue, green becomes yellow...) Is also a solution
- The verifier can ask the prover to show a particular edge, show that the two colors are different.
- The verifier wants to ask many questions, the prover doesnt want to reveal information.
- The prover picks a particular color permutiation (randomization), commits to them with [[Cryptographic Hash Functions]], lets the verifier pick an edge (challenge) and gives him the real colors, V can verify the colors are the ones P commited to.
- The previous step is repeated with different permutations and different challenges until V is satisfied.
This scheme allows V and only V to know P can solve the problem, V2 cant because since he didnt ask the challenges, he cant be sure that the interaction wasnt planned.
Parallels:
- Commitment schemes, P needs to hide its solution, V needs to be sure that P's solution to his challenges (the edge) matches the graph's solution P hid.
- Randomization, aside from obscuring, P needs to make sure his solution cant be reconstructed through multiple challenges.
- Challenges, V needs to ask multiple questions to be convinced of the proofs.
- 