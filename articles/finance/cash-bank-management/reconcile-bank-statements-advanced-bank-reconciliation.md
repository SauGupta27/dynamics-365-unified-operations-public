---
# required metadata

title: Reconcile bank statements by using advanced bank reconciliation
description: The Advanced bank reconciliation feature lets you import electronic bank statements and automatically reconcile them with bank transactions in Microsoft Dynamics 365 Finance. This article explains the reconciliation process.
author: EricWangChen
ms.date: 04/20/2024
ms.topic: article


# optional metadata

ms.search.form: BankReconciliationWorksheet
# ROBOTS: 
audience: Application User
# ms.devlang: 
ms.reviewer: twheeloc
# ms.tgt_pltfrm: 
ms.assetid: 9df13adf-aa9d-4f6b-bde6-25a214611692
ms.search.region: global
# ms.search.industry: 
ms.author: wangchen
ms.search.validFrom: 2016-02-28
ms.dyn365.ops.version: AX 7.0.0

---

# Reconcile bank statements by using advanced bank reconciliation

[!include [banner](../includes/banner.md)]

The Advanced bank reconciliation feature lets you import electronic bank statements and automatically reconcile them with bank transactions in Dynamics 365 Finance. This article explains the reconciliation process.

## Import an electronic bank statement by using Electronic reporting

You import your bank statements by using the **Import statement** action on the **Bank statements** page. On the bank statement, the bank account is identified through a combination of values that are set on the bank account details. These values include the bank name, bank account number, routing number, Society for Worldwide Interbank Financial Telecommunication (SWIFT) code, and International Bank Account Number (IBAN). 

You can upload a bank statement that contains information for either a single account or multiple accounts. If there are multiple accounts, the accounts can be in different legal entities.

- To import a single bank statement file for a single account, set the **Import statement for multiple bank accounts in all legal entities** option to **No**, and select the bank account that is associated with the statement. Select **Browse** to select the associated bank statement file, and then select **Upload**.
- To import a single bank statement file for multiple accounts, set the **Import statement for multiple bank accounts in all legal entities** option to **Yes**. Select **Browse** to select the associated bank statement file, and then select **Upload**.

If any statements in the electronic file can't be associated with a bank account or if it is associated with multiple bank accounts by using the identifying fields, they won't be imported. However, other statements in the file can still be imported. The user then receives a message that states that the import of bank statements was unsuccessful for specific bank accounts. 

> [!NOTE] 
> The user who is importing the bank statement file must have access to a legal entity to import statements for that legal entity's bank accounts. 

You can use a zip file to upload multiple statement files to Finance in a single process. To import multiple bank statement files for multiple accounts, combine all the bank statement files into one zip file. In the **Import bank statements** dialog box, set the **Import statement for multiple bank accounts in all legal entities** option to **Yes**. Select **Browse** to select the zip file that contains the bank statement files, and then select **Upload**. The import process will recognize the zip file and upload each statement that is included in it, regardless of the legal entity of the bank account.

A **Reconcile after import** option is available. When you set this option to **Yes**, the system automatically validates the bank statement, creates a new bank reconciliation and worksheet, and runs the Default matching rule set when the bank statement is uploaded. This functionality automates the process up to the point where transactions must be manually matched.

You can also use Electronic reporting (ER) to periodically import your bank statements from a SharePoint folder. To import bank statements, follow these steps.

1. Configure SharePoint for a bank statement format. For more information, see [Configure data import from SharePoint](../../fin-ops-core/dev-itpro/analytics/er-configure-data-import-sharepoint.md?context=%2Fdynamics365%2Fcontext%2Ffinance).
1. In Feature management, enable the **Automatic importing bank statement from SharePoint folder** feature.
1. Enable batch processing during the bank statement import, and configure recurrence settings. Bank statements can then be imported from the configured SharePoint folder in the statement format.

## Import an electronic bank statement by using data entities

You can import your bank statements by using the data entity framework. Two entities are available:

- Bank statement header
- Bank statement lines

Here's the template for importing a bank statement header:

- STATEMENTID
- BANKACCOUNT
- CURRENCY
- OPENINGBALANCE
- ENDINGBALANCE
- FROMDATE	
- TODATE

Here's the template for importing bank statement lines:

- LINENUMBER	
- BANKACCOUNT	
- STATEMENTID	
- BOOKINGDATE	
- AMOUNT	
- BANKSTATEMENTTRANSACTIONCODE	
- COUNTERAMOUNT	
- COUNTERCURRENCY	
- COUNTEREXCHANGERATE	
- CREDITORREFERENCEINFORMATION	
- DOCUMENTNUMBER	
- ENTRYREFERENCE	
- INSTRUCTEDAMOUNT	
- INSTRUCTEDCURRENCY	
- INSTRUCTEDEXCHANGERATE	
- LINESTATUS	
- REFERENCENUMBER	
- RELATEDBANK	
- RELATEDBANKACCOUNT	
- REVERSAL	
- TRADINGPARTY

For more information, see [Data entities overview](../../fin-ops-core/dev-itpro/data-entities/data-entities.md).

## Validate the bank statement
To validate a statement, on the **Bank statements** page, select **Validate**. Bank statements must be validated before they can be reconciled. This step is automatically completed if you set the **Reconcile after import** option to **Yes** at the time of import. 

Bank statement validation verifies the following details:

- The bank statement matches the selected bank account.
- The bank statement currency matches the bank account currency.
- The opening balance of the statement equals the closing balance of the previous statement for the bank account.
- The date doesn't overlap the date for another bank statement for the same bank account.
- Dates on the statement lines are between the from-date and to-date of the bank statement.
- The opening balance and summarized line amounts equal the ending balance.

When the validation is completed, the status of the bank statement is updated to **Validated**. A bank statement must be validated before it can be reconciled.

## Reconcile the bank statement
After you've imported an electronic bank statement and validated the statement on the **Bank statements** page, you can reconcile the bank statement by using the **Bank reconciliation** and **Bank reconciliation worksheet** pages. 

On the **Bank reconciliation** page, select **New** to create a new reconciliation, and then select the bank account of the statement that was imported. A bank account can have only one open bank reconciliation. The cut-off date determines the bank statement transactions and Operations bank transactions that are included on the reconciliation worksheet. By default, the current system date is used as the cut-off date, but you can change the date for the reconciliation. The remaining header information is automatically taken from the statement. This step is automatically completed if you set the **Reconcile after import** option to **Yes** at the time of import. 

On the **Bank reconciliation** page, select **Worksheet** to open the **Bank reconciliation worksheet** page. If you set the **Reconcile after import** option to **Yes**, the Default matching rule set is automatically run for the reconciliation. To manually run matching rules, select **Run matching rules** to select the matching rule sets or matching rules to run against the bank transactions. If you have many transactions to process, you can complete this step as a batch process. 

The **Bank reconciliation worksheet** page has four grids that contain transactions. The two upper grids show transactions from the bank statement and Operations that haven't yet been matched. The two lower grids show matched transactions. The **Bank statement transaction details** tab shows details for the unmatched bank statement transaction that is selected in the upper grid. 

You can enable additional filtering capabilities and a new grid for new transactions by turning on the **Advanced bank reconciliation improvement: enable filtering and provide separate grid for new transactions** feature.

There are three ways to match or reconcile bank statement transactions:

- Match the transactions with Operations bank transactions.
- Match the transactions with a reversal bank statement transaction.
- Mark the transactions as **New**, so that they can be posted later as a bank transaction in Finance.

To manually match transactions, select the transactions in the **Bank statement transactions** grid, select the corresponding transactions in the **Operations bank transactions** grid, and then select **Match**. The selected transactions are moved from the upper grids for unmatched transactions to the lower grids for matched transactions. Additionally, the matched and unmatched total amounts are updated. You can have one-to-one, many-to-one, and many-to-many transaction matches. Matches must follow the rules for allowed date differences and transaction type mapping. These rules are set on the **Cash and bank management parameters** page.

Penny differences might occur in your reconciliation. You can match a single bank statement transaction and a single Operations bank transaction that have penny differences if the penny differences are within the tolerance amount that is defined by the **Allowed penny difference** field on the bank account. The amount is shown in the **Correction amount** field on the matched Operations bank transaction. When the bank reconciliation is marked as reconciled, corrections are automatically posted by using the main account that is defined on the associated bank transaction type. Corrections aren't supported for the **Check** and **Deposit** document types. 

Bank statement transaction reversals are matched by using the reconciliation worksheet. Two statement lines can be matched if the amounts are opposite, and if one of the transactions is marked as a reversal. You can also set up a matching rule for the **Clear reversal statement lines** action.

Reversed Operations bank transactions must be reconciled by using the **Operations bank transactions** page. You can reconcile two Operations bank transactions together if the documents have the same bank account, document type, and payment reference, and if they have opposite amounts. You can also reconcile a single canceled check to prevent those transactions from appearing on the reconciliation worksheet. 

If there are new bank-initiated transactions, such as interest, fees, and charges, that aren't yet in Finance, you can add them to a journal that's associated with the selected bank statement reconciliation. Select a bank statement transaction in the **Bank statement transactions** grid for unmatched transactions, and then select **Mark as new**. The status of the transaction is set to **New**, and the transaction is moved to the **Bank statement transactions** grid for matched transactions. Later, from the **Bank statement** page, post the transactions that are marked as **New**. If you turn on the **Enable posting of new transactions in bank reconciliation** feature, transactions that are marked as **New** can also be posted directly from the Bank reconciliation worksheet. For more feature enhancements, turn on the **New voucher and date for new transactions in the advanced bank reconciliation bank statement** feature.

You can unmatch transactions that were incorrectly matched. Select the matched bank statement transaction, and then select **Unmatch**. All associated transactions are moved back to the upper grids for unmatched transactions, and the matched and unmatched total amounts are updated. 

After you complete the reconciliation process, mark the Bank reconciliation worksheet as reconciled. This process automatically posts correction amounts using the accounts set on the **Bank transaction type** page. The bank reconciliation for a statement can be marked as reconciled at any time, even if there are bank statement lines that haven't been matched. The unmatched transactions automatically move to the next reconciliation worksheet as unmatched bank statement transactions to be reconciled. After a bank statement reconciliation has been marked as reconciled, it can't be undone. The reconciliation isn't editable, and you don't have the ability to make updates to that reconciliation. You can turn on the **Enable bank reconciliation reversal even new transactions exist in posted bank statement** feature to reverse reconciliation. To improve process performance, turn on the **Enable batch mode for "Mark as reconciled" in advance bank reconciliation** feature.

## Post new transactions that are associated with the reconciliation

Bank statement transactions that you marked as **New** on the reconciliation worksheet are posted on the **Bank statement** page. On the **Bank statement** page, select the statement ID to view the statement details. On the **Accounting** menu, you can use the **View distributions** and **View accounting** options to view details behind the new transactions and the associated ledger entries. Select the **Post** option to post the bank statement lines that are marked as **New** to the general ledger. Posting can be completed only one time per bank statement. To reverse a posted bank statement through a new transaction, turn on the **Reverse posted bank statement with new transactions** feature.

To view the voucher of a new transaction on a **Bank statement** page, turn on the **Display vouchers in bank statement** feature.

## Post customer and vendor payment journals from bank statements and reconciliation worksheets

In Dynamics 365 Finance version 10.0.36, the **Generate customer and vendor payments from bank statement and reconciliation** feature is available. This feature enhances the bank statement and reconciliation worksheet. You can post customer and vendor payment journals directly from selected bank statement lines. The posted customer and vendor payment journals are automatically matched with the original bank statement lines in the bank reconciliation worksheet.

To use the feature, follow these steps.

1. On the **Bank accounts** setup page, on the **Reconciliation** FastTab, enter the **Default customer payment journal** and **Default vendor payment journal** names.
2. On the **Bank statement** page or the **Bank reconciliation worksheet** page, select the required bank statement lines.
3. Select **Generate payment journal** to generate and post the payment journal.

> [!NOTE]
> The **Generate customer and vendor payments from bank statement and reconciliation** feature is in preview in version 10.0.36 and is available only in sandbox environments.

[!INCLUDE[footer-include](../../includes/footer-banner.md)]
