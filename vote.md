# Voting Scheme

voter: i,
Votes: Vij=(0,1) or (1,0)
total number of voters: M
max vote chances: N
a list of other voters keys: k1,...kj...
blacklist: {Pk*}

*Voting Stage
initialize: X=N
1. In time duration T0: each peer i submit Xi votes(transaction), Vij (submitted by peer i, encrypted by peer j's key, i.e., Kj not in the black list) and validates other peers votes encryped by his key. The votes(transaction)is only valid and put on the ledger if peer j says it's valid (i.e., not containing wrong information) and there are less than N votes submitted by peer i on the ledger.
2. Each peer checks how many of his votes are not on ledger (N1), and put those peers who hasn't validated his votes in the black list {Pk*}. He continues to step one with N equal to P.

The voting stage finishes if all peer have fulfilled his submission chances, i.e., total votes on ledger = M * N, or times-out. After this stage, each peer i have Ni votes on the ledger Ni<=N. Allowing enough time, if a peer is online and tries his best to submit new valid votes, he will have Ni=N as long as there's at least one peer who is honest and alive to validate his votes. 

*Revealing Stage
1. In time duration T1: each peer reveals the counts encrypted by his key, e.g., peer 0 reveals the result of sum_i(Vi0) (transaction), the transaction is put on the ledger if it is valid, by encrypting the result and compare with ledger records).
2. Each peer checks if all his Ni votes are revealed. If not, put those peers who have his votes but didn't reveal his key on the black list. Say Qi votes are not revealed. Continue to Voting stage but now the starting votes Xi=Qi.

*Statements:
1. If the voter always submit invalid votes or fails to submit anything, the total final votes appeared in legdger would be smaller than N or even could be zero. But this is acceptable because the voter should be punished for his mis-behavior. But we could keep his valid votes if he is only online for some certain time ans then goes offline.

2. For stage one, if the voter always tries his best to submit valid votes, he will have all his N votes appear in ledger as long as there is enough time and there's at least one peer not in the blacklist. But the privacy of his votes would only be preserved with more than one peer who validates his votes.

3. We don't want the valid votes on the ledger fail to be counted in the final result just because the validator fails to reveal result. So after revealing stage, the voter can resubmit Qi votes which are valid but not revealed by validator to some other validators. As long as there are honest validators and enough time, his valid votes would be revealed and sum up to N.


