## EJB数据查询

### 1.前端数据

| 接口                           | 统计项         | 备注                                       |
| ---------------------------- | ----------- | :--------------------------------------- |
| api/v2/loan/amount/statistic | 累计成交金额      | investAmount：tb_invest中FROZEN、SETTLED、CLEARED、OVERDUE、BREACH状态金额总和 |
|                              | 投资人赚取收益     | repayTotalAmount：tb_loan_repayment中状态为REPAYED的利息总和。 unRepayTotalInterestAmount：tb_loan_repayment中状态为UNDUE、OVERDUE、BREACH的利息总和 |
|                              | 待收总额        | unRepayTotalAmount:tb_loan_repayment中状态为UNDUE、OVERDUE、BREACH的本金+利息总和 |
| dashboard                    | 当前总可用余额     | totalAvailable：查询tb_user_fund中的总可用余额     |
| analysis/comingRepayments    | 未来七天到期还款金额  | 7天内，tb_loan_repayment中所有状态的本金+利息合        |
|                              | 提现申请        |                                          |
|                              | 申请总额        |                                          |
|                              | 在投/已结清金额    |                                          |
|                              | 今日/累计借款总额   |                                          |
|                              | 借款申请(今日/累计) |                                          |
|                              |             |                                          |
|                              |             |                                          |
|                              |             |                                          |
|                              |             |                                          |



### 2.个人资金明细

| 接口                       | 统计项                     | 备注   |
| ------------------------ | ----------------------- | ---- |
| {id}/statistics/loan     |                         |      |
|                          |                         |      |
| totalLoanAmount          | 累计贷款总额                  |      |
| currentLoanAmount        | 现有贷款总额：已安排、开放投标、已满标、已结算 |      |
| totalLoanNum             | 累计贷款总数                  |      |
| clearedLoanNum           | 累计贷款总数                  |      |
| settledLoanNum           | 还款中贷款数                  |      |
| overdueLoanNum           | 已逾期贷款数                  |      |
| pendingLoanRequestAmount | 正在审批的借款总金额              |      |
| pendingLoanNum           | 待审批贷款数                  |      |
|                          |                         |      |

