# [H-i] 

## Summary

## Description

## Impact Explanation

## Likelihood Explanation:

## Poc

## Recommendation

# [H-X] Lender is paying the fee twice when receive a loan in  the giveLoan function.

## Summary
In giveLoan function the lender is paying the fee twice when the pool balance is updated; it is reduced the debt, the interest and the protocol fee. Then, in the same function, there is a fee transfer from the lender to the fee collector.
So the pool balance is reduced by the protocol fee and then the lender has to pay the fee again.
In the repay function the borrower pay back(transfer to the contract) the debt and the interest but the protocol fee is not paid. So at the end there is a difference between the contract balance and the pool balance where the pool balance is lower and that difference is the protocol fee that the lender paid in the giveLoan function.


# [H-X] Lender is paying the fee twice when receive a loan in  the buyLoan function.

## Summary
In buyLoan function the lender is paying the fee twice when the pool balance is updated; it is reduced the debt, the interest and the protocol fee. Then, in the same function, there is a fee transfer from the lender to the fee collector.
So the pool balance is reduced by the protocol fee and then the lender has to pay the fee again.
In the repay function the borrower pay back(transfer to the contract) the debt and the interest but the protocol fee is not paid. So at the end there is a difference between the contract balance and the pool balance where the pool balance is lower and that difference is the protocol fee that the lender paid in the buyLoan function.



# [H-1] New pool balance is reduced by debt twice in refinance function.

## Summary
In refinance function the borrower send the debt amount to refinance, so if total debt is 100 and the borrower wants to refinance 30, he sends 30 as debt parameter. So the borrower pays 100 - 30 = 70 and the loan.deb is updated to 30 and the borrower transfer 70 to the contract.
Then the pool balance is updated by reducing the debt, the interest. Until now everything is fine, the problem is that the pool balance is reduced by the debt amount again in at the end of the function. So if initially the pool balance was 100, after the refinance the pool balance should be 970 as it has only one loan with a debt of 30, but as the pool balance is updated a seconf time the pool balance is reduced by 30 again and becomes 940.

# [H-2] Arbitrary debt increment in giveLoan function.

## Summary
An attacker can create 2 loans with same collateral and debt tokens, same interest rate and same LTV to give a loan each other.
As everytime a loan is given a protocol fee is charged to the new total debt of the destiny. Therefore when a  loan is given a new debt is calculated by previous debt + interest + protocol fee. At the end is the borrower who pays the protocol fee when try to repay the loan. 
This process can be repeated indefinitely and the attacker can get a lot of debt without paying any fee. The only restriction is that the loanRation can't be higher than the maxLoanRatio of the destiny pool, but the maxLoanRatio can be increased by the lender.


# [M-1] Interest rate percentage is not the real interest rate charged.

## Summary
_calculateInterest function returns the interest and the fees, but the interest rate is reduced by the fees before returning. Therefore the interest rate is less that the interest rate set by the lender.

