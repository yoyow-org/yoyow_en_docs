# 理事会简介
# Introduction to Committee

理事会由一组股东投票通过的理事会成员组成，理事会成员的职责在于设定系统运行参数。

A group of committee members voted in by the shareholders are responsible for setting parameters of the system operation.


## 理事会职责
## The Responsibilities of Committee

- 调节系统运行各项参数，包括手续费费率、出块奖励等
- Adjusting the various system operation parameters, including fees and block production rewards
- 理事会同时拥有部分决策权
- The committee has partial decision-making authority

## 理事会设定
## The Committee Setup

- 理事会成员数初始设定为5人，不可修改。
- The number of committee members is initially fixed at five, and cannot be modified.

- 每届理事会成员任期30天，不可修改
- The tenure of committee members is set at one month, and cannot be modified.

- 初始理事会成员名单由系统指定，后续理事会由持币人投票选举产生
- The committee members on the initial list will be appointed by the system, with subsequent committee members decided by votes taken by token holders.

- 理事会账户没有报酬
- No remuneration for committee accounts

## 候选理事
## Candidates for the Committee

- 任何账户抵押一定数量的YOYO ，即可成为候选理事身份。当前最低抵押为 1000 YOYO
- Any account with collateral over a certain amount of YOYO can become a candidate for the committee. The initial figure is set at 1,000 YOYO.
- 只有候选理事，才有资格被投票成为正式理事。
- Only candidates for the committee are qualified to become formal committee members.

## 理事会选举
## Committee Elections

- 持币人一票一投：即每个账号同时只可支持一位候选人，持票人可随时修改投票
- Each token holder has one vote that can be used for one candidate—that is to say, an account can only support one candidate at a time, and the voter can change their vote at any time.

- 其他规则如投票资格、有效票数、投票有效期、代理等，与见证人投票共用，参考[投票](./vote_pledge/vote.html)。
- Other rules such as voting qualification, total valid votes, validity period for voting, and proxies are used in conjunction with witness voting. Please refer to [Voting](./vote_pledge/vote.html)

- 换届时间点，得票最高的 5 个候选理事，成为新的在任理事。暂不设置最低票数
- The five candidates with the most votes at re-election time will become the new incumbent committee members. Currently, there is no regulation on minimum votes.

## 理事会提案
## Committee Proposals

- 在任理事会成员可以发起提案，由理事会全体成员表决
- Incumbent committee members can initiate proposals to be voted on by all committee members.

- 每个提案包括如下属性：提案内容，表决截止时间，表决通过阈值，提案生效时间。
- Each proposal includes the following attributes: proposal content, voting deadline, threshold for voting approval and the date from which it would take effect.

- 提案通过的阈值根据提案类型预设，不可指定。暂时只设通过阈值，不设否决阈值。
- The threshold for approval will be set in advance according to the type of proposal, and cannot be assigned. Only a threshold for approval has been set for the time being, with no threshold for disapproval.

- 一个提案可包含多项内容，该情况下，必须全部通过或者全部不通过，通过阈值采用多项内容中最高阈值
- A proposal may contain more than one item of content, in which case approval or disapproval of all items is required, and the highest threshold for approval among the various content will be recognized as the threshold for approval.

- 提案生效时间不得晚于发起理事的任期结束时间。生效时间可设为过去的时间，效果为通过后“即时生效”。
- The date from which a proposal would take effect must not be later than the end date of the tenure. The date of effect can be backdated, resulting in it “taking immediate effect.”

- 理事会表决拥有三种状态： 赞成、中立、反对；提案发起人默认赞成；所有理事会成员每人一票，生效前均可改票。
- There are three statuses for committee votes—approving, abstaining and disapproving. It is assumed that the initiator of the proposal has voted to approve it. Each committee member shall have one vote, which can be changed before it comes into effect.

- 赞成人数达到阈值时，如果生效时间已过，则提案即时生效，否则提案将按计划在生效时间生效
- Under the condition that the number of affirmative votes has reached the threshold, if the date of effect is in the past, the proposal will come into effect immediately. Otherwise, the proposal will come into force on the planned date of effect.

- 表决截止时间前，且提案生效前，表决人可以修改意见；表决截止时间后，不可修改意见
- Before the close of voting, and before the proposal’s date of effect, voters are allowed to change their votes. However, after the close of voting, no change will be accepted.

- 理事会提案需在发起理事的任期内表决完成，没表决完成的提案，任期结束时自动作废；表决完成通过的提案，按计划生效，任期结束不影响提案效力
- Committee proposals shall be voted on within the tenure. Proposals whose voting has not finished within that time period will be automatically canceled upon the termination of the tenure. Approved proposals will come into effect as planned and the validity of such proposals will not be impacted by the tenure ending.

- 提案生效并不等于修改的系统参数马上发挥作用，因为不同系统参数预设有不同启用条件/方式。
- Since different preset system parameters have different enabling conditions/methods, a proposals’ entry into force does not mean system parameters will be modified immediately.

## 理事引退
## Resignation of Committee Members

- 候选理事/正式理事 均可终止理事身份，系统会延迟返还押金，目前延迟1天。
- Candidates for committee/formal committee members can terminate their capacity as committee members and the system will delay the return of their deposit, which is currently delayed by one day.

- 正式理事选择引退后，并不影响本任期内身份，但下个任期不再有被投票资格
- After a formal committee member resigns, their capacity within that tenure will not be impacted, but in the next tenure that committee member will lose all voting rights.

- 引退后，所有得票清零（换届时生效）
- After resignation, all votes will undergo zero clearing (coming into effect after the re-elections)

- 若在延迟释放期间重新申请理事身份，尚未释放的币将重新锁定，但已清零的票将无法恢复。
- If an application to become a committee member is filed during the delayed release period, the tokens not released will be locked again. However, cleared votes cannot be recovered.
