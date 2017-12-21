# Voting Scheme

---
voter: i,
---
Votes: Vij=(0,1) or (1,0)
---
total number of voters: M
---
max vote chances: N
---
a list of other voters keys: k1,...kj...
---
blacklist: {Pk*}
---
initialize: Xi=N
---

* Voting Stage
1. In time duration T0: each peer i submit Xi votes(transaction), Vij (submitted by peer i, encrypted by peer j's key, i.e., Kj not in the black list) and validates other peers votes encryped by his key. The votes(transaction)is only valid and put on the ledger if peer j says it's valid (i.e., not containing wrong information) and there are less than N votes submitted by peer i on the ledger. If a vote is invalid it does not appear on ledger but peer j will let peer i know about it.
2. Each peer checks how many of his votes are not on ledger (N1), and put those peers who hasn't validated his valid votes in the black list {Pk*}. He continues to step one with N equal to P.

The voting stage finishes if all peer have fulfilled his submission chances, i.e., total votes on ledger = M * N, or times-out. After this stage, each peer i have Ni votes on the ledger Ni<=N. Allowing enough time, if a peer is online and tries his best to submit new valid votes, he will have Ni=N as long as there's at least one peer who is honest and alive to validate his votes. 

* Revealing Stage
1. In time duration T1: each peer reveals the counts encrypted by his key, e.g., peer 0 reveals the result of sum_i(Vi0) (transaction), the transaction is put on the ledger if it is valid, by encrypting the result and compare with ledger records).
2. Each peer checks if all his Ni votes are revealed. If not, put those peers who have his votes but didn't reveal his key on the black list. Say Qi votes are not revealed. Continue to Voting stage but now the starting votes Xi=Qi.

* Statements:
1. If the voter always submit invalid votes or fails to submit anything, the total final votes appeared in legdger would be smaller than N or even could be zero. But this is acceptable because the voter should be punished for his mis-behavior. But we could keep his valid votes if he is only online for some certain time ans then goes offline.

2. For stage one, if the voter always tries his best to submit valid votes, he will have all his N votes appear in ledger as long as there is enough time and there's at least one peer not in the blacklist. But the privacy of his votes would only be preserved with more than one peer who validates his votes.

3. We don't want the valid votes on the ledger fail to be counted in the final result just because the validator fails to reveal result. So after revealing stage, the voter can resubmit Qi votes which are valid but not revealed by validator to some other validators. As long as there are honest validators and enough time, his valid votes would be revealed and sum up to N.

* Example:

Alice, Bob, Carl, Daniel, Eva, Fibby

2 votes per person

---
Alice (blacklist{}): Va,b & V'a,f (v' means invalid content)
---
Bob (blacklist{}): Vb,a & Vb,e 
---
Carl (blacklist{}): Vc,a & Vc,d 
---
Daniel (blacklist{}): Vd,b & V'd,a 
---
Eva (blacklist{}): Ve,f & Ve,c 
---
Fibby (blacklist{}): Vf,b & Vf,c
---

---
Alice validates: Vb,a & Vc,a and invalidates V'd,a
---
Bob validates: Va,b & Vd,b & Vf,b
---
Carl does not validate anything 
---
Daniel validates: Vc,d
---
Eva validates: Vb,e
---
Fibby validates: Ve,f and invalidates V'a,f
---

8 votes in the system, 4 votes short

---
Alice resubmit: Va,d ( this d could be anything not in black list, f is not in black list since he told alice)
---
Bob satisfied
---
Carl satisfied
---
Dainal resubmit something wrong: V'd,a ( a is not blacklisted because a invalidates previously)
---
Eva (blacklist {c}) resubmit: Ve,a
---
Fibby (blacklist {c}) resubmit: Vf,d
---

---
Alice validates: Ve,a and invalidates: V'd,a
---
Bob does nothing
---
Carl does nothing
---
Dainal verifies: Va,d & Vf,d
---
Eva does nothing
---
Fibby does nothing
---
11 votes in the system and 1 vote short

Assume Daniel keeps resubmit invalid votes, so after some iteration, the final votes in the board are still 11:

---
Vb,a | Vc,a | Va,b | Vd,b | Vf,b | Vc,d | Vb,e | Ve,f | Ve,a | Va,d | Vf,d
---

---
Alice reveals result: (Vb,a + Vc,a + Ve,a)
---
Bob reveals result: (Va,b + Vd,b + Vf,b)
---
Carl does nothing
---
Daniel fail to reveal result (Vc,d & Va,d & Vf,d)
---
Eva (blacklist {c}) reveals result (Vb,e)
---
Fibby (blacklist {c}) reveals result (Ve,f)
---

---
Alice (blacklist {d}) finds one vote not revealed Va,d
---
Bob is satisfied
---
Carl (blacklist {d}) finds one vote is not reveald Vc,d 
---
Daniel is satisfied
---
Eva is satisfied
---
Fibby (blacklist {c, d}) finds one vote is not reveald Vf,d
---
Now revealed result in the ledger are: 
---
(Vb,a + Vc,a + Ve,a) | (Va,b + Vd,b + Vf,b) | (Vb,e) |  (Ve,f)
---

---
Alice (blacklist {d}) votes Va,b
---
Carl (blacklist {d}) votes Vc,a
---
Fibby (blacklist {c, d}) votes Vf,b
---

---
Alice validates Vc,a
---
Bob validates Va,b & Vf,b
---
Now ledger have three new votes transaction

---
Vb,a | Vc,a | Va,b | Vd,b | Vf,b | Vc,d | Vb,e | Ve,f | Ve,a | Va,d | Vf,d |Va,b | Vc,a | Vf,b
---

---
Alice reveals Vc,a
---
Bob reveals (Va,b+Vf,b)
---

Now the ledger have three new counts transcation

---
(Vb,a + Vc,a + Ve,a) | (Va,b + Vd,b + Vf,b) | (Vb,e) |  (Ve,f) | (Vc,a) | (Va,b + Vf,b)
---
Note the peers does not to re-reveal previously revealed counts

Every iteration will have fewer votes to be submitted, validated and revealed, leading to convergence finally









