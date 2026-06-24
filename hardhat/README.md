Privacy-Preserving AI Bounty Judge

Lifecycle

1. Bounty owner creates a bounty with a submission deadline and reveal deadline.
2. Participants generate a commitment using:
   keccak256(answer, salt, msg.sender, bountyId).
3. During the submission phase, participants submit only the commitment hash through submitCommitment().
4. After the submission deadline, participants reveal their answer and salt using revealAnswer().
5. The contract verifies that the revealed answer matches the original commitment.
6. Only successfully revealed answers are stored as valid submissions.
7. After the reveal phase ends, the bounty owner calls judgeAll() to obtain AI evaluation.
8. The bounty owner finalizes the winner using finalizeWinner().

Test Plan

Test 1: Valid Commitment and Reveal

- Submit commitment.
- Reveal correct answer and salt.
- Expect success.

Test 2: Invalid Reveal

- Submit commitment.
- Reveal with incorrect salt.
- Expect transaction revert with "Invalid reveal".

Test 3: Double Reveal

- Reveal once successfully.
- Attempt second reveal.
- Expect revert.

Test 4: Reveal Before Submission Deadline

- Attempt reveal before submission phase ends.
- Expect revert.

Test 5: Judge Before Reveal Deadline

- Attempt AI judging before reveal phase ends.
- Expect revert.

Architecture Note

On-chain:

- Commitment hashes
- Bounty metadata
- Revealed answers
- AI review results
- Winner information

Hidden during submission:

- Original answers
- Salts

The commit-reveal mechanism prevents participants from copying answers before the reveal phase. Only verified reveals become eligible for AI judging. AI evaluation is performed after all reveals are complete.

Reflection

A bounty system should keep participant answers hidden during the submission phase while keeping bounty metadata and rewards public. Commitment hashes should be public because they prove participation without revealing the answer itself. Answers should remain private until the reveal phase is complete. AI is well suited for evaluating large numbers of submissions consistently and efficiently. Humans should define judging criteria and review edge cases or disputes. Final decisions that affect rewards may benefit from human oversight. Combining transparent smart contracts with AI evaluation can improve fairness while reducing bias and plagiarism.
