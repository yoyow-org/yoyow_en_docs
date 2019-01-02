# Introduction to Committee

A group of committee members voted in by the token holders are responsible for setting parameters of the system operation.

## The Responsibilities of Committee

- Adjusting the various system operation parameters, including fees and block production rewards

- The committee has partial decision-making authority

## The Committee Setup

- The number of committee members is initially fixed at five, and cannot be modified.

- The tenure of committee members is set at 30 days, and cannot be modified.

- The committee members on the initial list will be appointed by the system, with subsequent committee members decided by votes taken by token holders.

- No remuneration for committee accounts

## Candidates for the Committee

- Any account with collateral over a certain amount of YOYO can become a candidate for the committee. The initial figure is set at 1,000 YOYO.

- Only candidates for the committee are qualified to become formal committee members.

## Committee Elections

- Each token holder has one vote that can be used for one candidate—that is to say, an account can only support one candidate at a time, and the voter can change their vote at any time.

- Other rules such as voting qualification, total valid votes, validity period for voting, and proxies are used in conjunction with witness voting. Please refer to [Voting](../vote_pledge/vote.html)

- The five candidates with the most votes at re-election time will become the new incumbent committee members. Currently, there is no regulation on minimum votes.

## Committee Proposals

- Incumbent committee members can initiate proposals to be voted on by all committee members.

- Each proposal includes the following attributes: proposal content, voting deadline, threshold for voting approval and the date from which it would take effect.

- The threshold for approval will be set in advance according to the type of proposal, and cannot be assigned. Only a threshold for approval has been set for the time being, with no threshold for disapproval.

- A proposal may contain more than one item of content, in which case approval or disapproval of all items is required, and the highest threshold for approval among the various content will be recognized as the threshold for approval.

- The date from which a proposal would take effect must not be later than the end date of the tenure. The date of effect can be backdated, resulting in it “taking immediate effect.”

- There are three statuses for committee votes—approving, abstaining and disapproving. It is assumed that the initiator of the proposal has voted to approve it. Each committee member shall have one vote, which can be changed before it comes into effect.

- Under the condition that the number of affirmative votes has reached the threshold, if the date of effect is in the past, the proposal will come into effect immediately. Otherwise, the proposal will come into force on the planned date of effect.

- Before the close of voting, and before the proposal’s date of effect, voters are allowed to change their votes. However, after the close of voting, no change will be accepted.

- Committee proposals shall be voted on within the tenure. Proposals whose voting has not finished within that time period will be automatically canceled upon the termination of the tenure. Approved proposals will come into effect as planned and the validity of such proposals will not be impacted by the tenure ending.

- Since different preset system parameters have different enabling conditions/methods, a proposals’ entry into force does not mean system parameters will be modified immediately.

## Resignation of Committee Members

- Candidates for committee/formal committee members can terminate their capacity as committee members and the system will delay the return of their deposit, which is currently delayed by one day.

- After a formal committee member resigns, their capacity within that tenure will not be impacted, but in the next tenure that committee member will lose all voting rights.

- After resignation, all votes will undergo zero clearing (coming into effect after the re-elections)

- If an application to become a committee member is filed during the delayed release period, the tokens not released will be locked again. However, cleared votes cannot be recovered.
