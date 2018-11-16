# 投票
# Voting

YOYOW采用DPoS共识算法，根据这种算法，全网持有代币的人可以通过投票系统来选择见证人、理事会，从而实现对YOYOW系统的决策。

YOYOW uses the DPoS consensus algorithm. All token holders are able to choose witnesses and the committee through the voting system, allowing them to make decisions about the YOYOW system.

## 投票资格
## Voting Qualification

- 持币数量到达一定数量后，即可以获得投票资格，当前为10000 YOYO，参见[参数表](https://yoyow.bts.ai/fees/index#tab-parameters)中`min_governance_voting_balance`字段。
- Voting qualification is assigned to those holders owning a certain amount of tokens. The current value is 10,000 YOYO. Please refer to the field `min_governance_voting_balance` in [Parameter Table](https://yoyow.bts.ai/fees/index#tab-parameters).
- 被理事会设置黑名单的账户，丧失投票资格。
- Accounts included in the committee's blacklist are disqualified from voting.

## 投票有效期
## Validity Period of Voting

- 拥有投票资格的用户，初次进行投票操作时激活投票意向。有效期为T ,初始默认90天。
- Users qualified for voting will activate their intention to vote when they vote for the first time. The validity period is T, which is initially set at 90 days.
- 在有效期内，如有新的投票操作，则有效期递延时间T。即过期时间变更为：更新投票操作时间 + T
- If there is any new voting action within the validity period, it will be extended by T, which means, the expiry date will be changed to: the date of the updated voting action + T.
- 在有效期内，如无新的投票操作，则认为不再有投票意向
- If there is no new voting action within the validity period, it is assumed that there is no more voting intention.
- 用户失去投票资格， 或不再有投票意向，则选票失效
- If users are disqualified from voting, or have no more voting intention, their votes will become void.

## 投票方式
## Voting Method

- 理事会选举：一票一投，每个账号同时只可支持一位候选人，持票人可随时修改投票。
- Committee elections: one vote for one candidate—each account can only support one candidate at a time, and the voter can change the vote at any time

- 见证人选举：一票多投，为了强化共识，每个账户可以给多个见证人投票。
- Witness elections: one vote for numerous candidates—to strengthen consensus, each account can vote for numerous witnesses.

## 有效得票计算
## Valid Vote Counting

为增加投票公平性，计票综合考虑了投票者当前持币量、平均持币量、持币时间等因素

To enhance the fairness of voting, vote counting also takes into consideration the voters' number of tokens held, average token volume, time held and other factors.

- 账户在激活投票意向时，开始积累币龄。
- When the account activates the voting intention, it begins to accumulate the age of the tokens.
- 投票人的有效票数为前60天内的平均余额和账号当前余额，这两个值中的较小值，即：有效票数 = min(60天平均余额，当前余额)。也就是说，在币转入时，币龄慢慢积累增加相应部分；币转出时币龄立即减少相应部分。
- The number of valid votes is equal to average balance over 60 days, or the current balance of the account (whichever is smaller), which means: number of valid votes = min (average balance within 60 days, current balance). That is to say, when the tokens are transferred in, the age of tokens gradually accumulates and increases the corresponding portion; when the tokens are transferred out, the age of tokens immediately decreases the corresponding portion.

- 被投票人的得票数，等于所有支持者有效票数之和
- The number of votes for a candidate is equal to the total number of valid votes from supporters.

- 因为实时更新所有账户币龄计算量太大，故计票采用延迟更新模式，可能与实时数据有少量计票误差。
- As it takes a lot of effort to count based on the age of the tokens in all accounts, which are updated in real-time, a delayed updating mode is used for vote counting, which means there might be a few deviations from counting based on real-time statistics.

## 投票代理
## Voting Proxies

- 投票可设置代理人，如：账户A设置账户B为投票代理，则B的投票对象得到的票数为A的有效票数 + B的有效票数。 A 称之为委托人，B 称之为代理人
- A proxy can be designated for voting activities. For example: when Account A designates Account B as voting proxy, the voter of Account B will receive the valid vote(s) from both A and B. In this case, A is the client and B is the proxy.
- 可设置多层代理。如：账户A 设置账户B为投票代理，账户B设置了账户C为投票代理。但代理总层数有上限。
- Multi-layered proxies are allowed. For example: Account A designates Account B as its voting proxy, and Account B designates Account C as its voting proxy. However, there are restrictions on the total number of proxy layers.

- 不可代理自己。
- Any account cannot designate itself as a voting proxy.
- 代理人进行投票操作时，会刷新委托人的投票意向有效期。
- Whenever there is an action of proxy voting, the validity period of the client's intention to vote will be updated.

- 如果代理人失去投票意向或者投票资格，代理关系自动解除，委托人投票状态变为“不投票给任何人”，委托人投票意向有效期不变
- When the proxy has no intention to vote or no qualification to vote, the proxy relationship will be automatically terminated, and the client's voting status will be changed to "with no vote for anyone" while the validity period of the client's intention to vote remains the same.
- 代理人重新激活投票意向时，代理关系不恢复
- When a proxy reactivates their intention to vote, the proxy relationship is not recovered.
