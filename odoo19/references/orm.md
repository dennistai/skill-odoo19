# Odoo19 - Orm

**Pages:** 50

---

## Conditional formatting¶

**URL:** https://www.odoo.com/documentation/19.0/applications/productivity/spreadsheet/visualize_data/conditional_formatting.html

**Contents:**
- Conditional formatting¶
- Single color¶
- Color scale¶
- Icon set¶
- Data bar¶

Conditional formatting applies specific formatting, such as a background color or font color, to a range of cells that meet one or more defined criteria, or rules. This allows you to visually highlight important data, patterns, trends, or anomalies.

When a spreadsheet containing dynamic Odoo data is reopened or refreshed, any conditional formatting that has been defined is updated to reflect any changes to the data.

Different types of conditional formatting are possible:

Single color: changes the color of the cell’s background or font, or changes the font formatting if cell values meet the defined conditions, e.g., changes a cell to red if an invoice is overdue.

Color scale: uses color intensity to represent smaller and larger values on a two- or three-color scale, e.g., shows sales performance using increasingly dark tones of the same color.

Icon set: uses sets of arrows, smileys, or dots in green, orange, and red to visually show how cell values in a range compare to certain defined values, e.g., a green smiley to represent sales performance above a certain amount.

Data bar: inserts colored bars inside a cell to show how a value compares to the value of other cells in the defined range, e.g., the customer generating the highest sales has the longest bar, with bars of decreasing length for customers generating lower sales.

To add conditional formatting to a range of cells:

Select the cells the formatting rule should apply to.

Click Format ‣ Conditional formatting from the menu bar.

In the Conditional formatting panel that opens to the right of the spreadsheet, click + Add another rule.

Click Add range to select additional ranges of cells if needed.

Select the type of Format rules, then enter the information required depending on the rule selected.

With the range selected and Single color selected as the rule type:

Define the conditions to be met for the formatting to be applied by choosing the desired value from the Format cells if … dropdown.

Define the Formatting style to be applied by selecting the appropriate font formatting, font color, and/or background color.

In the example, single-color conditional formatting changes the background color of cells containing the value FALSE to red, showing at a glance which eLearning courses are not yet published.

With the range selected and Color scale selected as the rule type:

Define what is used as the Minpoint, i.e., the lowest value, and the MaxPoint, i.e., the highest value, by selecting the appropriate category from the dropdown.

Optionally, to create a three-color scale, add a MidPoint, i.e., the middle value.

Define the color range by choosing a color for the lowest value, i.e., the Minpoint, and the highest value, i.e., the MaxPoint. For a three-color scale, also choose a color for the MidPoint.

In the example, sales performance by salesperson is shown using a two-color scale, with white representing the lowest value, and increasing amounts intensifying in color towards the selected green color.

Adding a Midpoint, with a corresponding color, allows you to highlight performance in relation to a defined target or neutral value, e.g., below or above a target %, or to distinguish between positive or negative values.

With the range selected and Icon set selected as the rule type:

Click on the set of Icons to use.

For the first and second icons:

enter the value that represents the lower threshold for the icon

indicate whether the cell value must be > (greater than) or ≥ (greater than or equal to) the threshold value

define whether the threshold value is a Number, Percentage, Percentile or Formula

The third icon is added to all cells whose values are below the threshold for the second icon.

Click Reverse icons to reverse the order of the icons.

In the example, sales performance by salesperson is shown using a three-scale set of colored dots:

A green dot represents high performance, and is added to total sales amounts greater than or equal to $50,000.

An orange dot represents average performance, and is added to total sales amounts greater than $25,000 (but less than $50,000 as this is the lower threshold for the green dot).

A red dot represents low performance, and is added to all other amounts (i.e., amounts below below $25,000 as this is the lower threshold for the orange dot).

Unlike other conditional formatting rules in Odoo Spreadsheet, this rule allows for the formatting to be applied either on the cells containing the values the rule is based on, or on a corresponding range of cells, e.g., on a column containing text. The Apply to range determines where the formatting is shown, while the Range of values determines which cells the rule is based on.

With Data bar selected as the rule type:

Click below Apply to range, then, in the spreadsheet, select the range of cells where the data bars should appear. Click Confirm.

Click on the Color dot to select the color of the data bars.

Click below Range of values, then, in the spreadsheet, select the range of cells containing the values the rule should be based on. Click Confirm.

In the example, the rule is based on the Total sales amounts in column B (with the Range of values being B2:B14), while the formatting is applied on the Salesperson names in column A (with the Apply to range being A2:A14).

---

## Team performance¶

**URL:** https://www.odoo.com/documentation/19.0/applications/hr/recruitment/team_performance.html

**Contents:**
- Team performance¶
- Open report¶
  - Pivot table view¶
- Use case: recruiter performance over time¶

The Team Performance report in the Recruitment app shows how many applicants each recruiter is managing.

This information is determined by the individuals populating the Recruiter field on each applicant form.

To access the Team Performance report, navigate to Recruitment app ‣ Reporting ‣ Team Performance.

The number of In Progress, Hired, and Refused applicants for each recruiter is displayed in a default (Graph) view.

The information shown is for the Last 365 Days Applicant default filter, as displayed in the search bar.

Hover the cursor over any column to view a popover window, displaying the specific details for that column.

For a more detailed view of the information in the Team Performance report, click the (Pivot) icon in the top-right corner. This displays all the information in a pivot table.

In this view, the job positions populate the rows, while the columns populate the number of applicants. The first column, Applicant, is the total number of applicants across all stages for that job position. The subsequent columns display the total applicants that have been Hired, Refused, and In Progress for each job position.

The displayed information can be modified, if desired.

In this example, there are 20 total applicants. Out of those 20, eight have been hired, five have been refused, and seven are still in the recruitment process.

From the data presented, the Experienced Developer job position is the most successful. This job position has one of the highest number of total applicants (tied with the Marketing and Community Manager position), as well as the most hires.

This pivot table also shows that the Quality Control Inspector position is the hardest to fill, as it has the fewest total applicants.

One way to modify this report is to show how recruiters are performing over time. To show this information, begin with the Team Performance report in the (Pivot) view.

Next, click the (down arrow) in the search bar, revealing a drop-down menu. Click Add Custom Group at the bottom of the Group By column, then click Recruiter. Click away from the drop-down menu to close it. Now, each row on the table represents a recruiter.

To compare the team’s performance over different time periods, click the (down arrow) in the search bar. Click Application Date in the Filters column, revealing various time periods to select.

In this example, the desired data is the comparison between the team’s performance in the third quarter (June - August) and the second quarter (April - July). To do so, click Q3. Once clicked, the current year is also ticked. In this example, it is 2025.

After making this selection, a Comparison column appears. Click Start Date: Previous Period to compare the third quarter with the second quarter, for the various recruiters.

From this report, some things can be extrapolated: the total number of applicants, the number of hired applicants, the number of refused applicants, and the number of applicants still in the recruitment pipeline all increased.

Additionally, Maggie Davidsons had the highest increase in number of hired applicants during the third quarter, while their number of refused applicants went down.

---

## Mailjet API¶

**URL:** https://www.odoo.com/documentation/19.0/applications/general/email_communication/mailjet_api.html

**Contents:**
- Mailjet API¶
- Set up in Mailjet¶
  - Create API credentials¶
  - Add verified sender address(es)¶
  - Add a domain¶
    - Setup in the domain’s DNS¶
    - Return to Mailjet account information¶
- Set up in Odoo¶

Odoo is compatible with Mailjet’s API for mass mailing. Set up a dedicated mass mailing server through Mailjet by configuring settings in the Mailjet account and the Odoo database. In some circumstances, settings need to be configured on the custom domain’s DNS settings as well.

To get started, sign in to the Mailjet Account Information page. Next, navigate to the Senders & Domains section and click on SMTP and SEND API Settings.

Then, copy the SMTP configuration settings onto a notepad. They can be found under the Configuration (SMTP only) section. The SMTP configuration settings include the server address, the security option needed (Use SSL/TLS), and the port number. The settings are needed to configure Mailjet in Odoo, which is covered in the last section.

Mailjet: How can I configure my SMTP parameters?

Odoo blocks port 25 on Odoo Online and Odoo.sh databases.

Next, click on the button labeled Retrieve your API credentials to retrieve the Mailjet API credentials.

Then, click on the eye icon to reveal the API key. Copy this key to a notepad, as this serves as the Username in the Odoo configuration. Next, click on the Generate Secret Key button to generate the Secret Key. Copy this key to a notepad, as this serves as the Password in the Odoo configuration.

The next step is to add a sender address or a domain to the Mailjet account settings so that the email address or domain is approved to send emails using Mailjet’s servers. First, navigate to the Mailjet Account Information page. Next, click on the Add a Sender Domain or Address link under the Senders & Domains section.

Determine if a sender’s email address or the entire domain needs to be added to the Mailjet settings. It may be easier to configure the domain as a whole if DNS access is available. Jump to the Add a domain section for steps on adding the domain.

Either all email addresses of the Odoo database users who are sending emails using Mailjet’s servers need to be configured or the domain(s) of the users’ email addresses can be configured.

By default, the email address originally set up in the Mailjet account is added as a trusted sender. To add another email address, click on the button labeled Add a sender address. Then, add the email address that is configured to send from the custom domain.

At minimum the following email addresses should be set up in the provider and verified in Mailjet:

notifications@yourdomain.com

bounce@yourdomain.com

catchall@yourdomain.com

Replace yourdomain with the custom domain for the Odoo database. If there isn’t one, then use the mail.catchall.domain system parameter.

After that, fill out the Email Information form, making sure to select the appropriate email type: transactional email or mass emails. After completing the form, an activation email is sent to the email address and the trusted sender can be activated.

It is recommended to set up the SPF/DKIM/DMARC settings on the domain of the sender.

Mailjet’s SPF/DKIM documentation

Mailjet’s DMARC documentation

If the database is not using a custom domain, then in order to verify the sender’s address, a temporary alias (of the three email addresses mentioned above) should be set up in Odoo CRM to create a lead. Then, the database is able to receive the verification email and verify the accounts.

By adding an entire domain to the Mailjet account, all the sender addresses related to that domain are automatically validated for sending emails using Mailjet servers. First, navigate to the Mailjet Account Information page. Next, click on Add a Sender Domain or Address link under the Senders & Domains section. Then, click on Add domain to add the custom domain.

The domain needs to be added to the Mailjet account and then validated through the DNS.

After that, fill out the Add a new Domain page on Mailjet and click Continue.

After adding the domain, a validation page will populate. Unless the Odoo database is on-premise (in which case, choose Option 1), choose Option 2: Create a DNS Record. Copy the TXT record information to a notepad and then navigate to the domain’s DNS provider to complete validation.

After getting the TXT record information from the Mailjet account, add a TXT record to the domain’s DNS. This process varies depending on the DNS provider. Consult the provider for specific configuration processes. The TXT record information consists of the Host and Value. Paste these into the corresponding fields in the TXT record.

After adding the TXT record to the domain’s DNS, navigate back to the Mailjet account. Then, navigate to Account Information ‣ Add a Sender Domain or Address, click the gear icon next to Domain, and select Validate.

This action can also be done by going to the Sender domains & addresses page on the Mailjet account information and clicking on Manage.

Next, click Check Now to validate the TXT record that was added on the domain. A success screen will appear if the domain is configured correctly.

After successfully setting up the domain, there is an option to Authenticate this domain (SPF/DKIM). This button populates SPF & DKIM provider.

Mailjet’s SPF/DKIM/DMARC documentation

To complete the setup, navigate to the Odoo database and go to the Settings. With Developer mode (debug mode) turned on, go to the Technical Menu ‣ Email ‣ Outgoing Mail Servers. Then, create a new outgoing server configuration by clicking on the Create button.

Next, input the SMTP server (in-v3.mailjet.com), port number (587 or 465), and Security (SSL/TLS) that was copied earlier from the Mailjet account. They can also be found here. It is recommended to use SSL/TLS even though Mailjet may not require it.

For the Username, input the API KEY. For the Password, input the SECRET KEY that was copied from the Mailjet account to the notepad earlier. These settings can be found on Mailjet ‣ Account Settings ‣ SMTP and SEND API Settings.

Then, if the Mailjet server is used for mass emailing, set the Priority value higher than that of any transactional email server(s). Finally, save the settings and Test the Connection.

---

## Receipts and invoices¶

**URL:** https://www.odoo.com/documentation/19.0/applications/sales/point_of_sale/receipts_invoices.html

**Contents:**
- Receipts and invoices¶
- Receipts¶
  - Reprint a receipt¶
- Invoices¶
  - Configuration¶
  - Invoice a customer¶
  - Retrieve invoices¶
  - QR codes to generate invoices¶

Set up receipts by going to Point of Sale ‣ Configuration ‣ Point of Sale, selecting a POS, and scrolling down to the Bills & Receipts section.

To customize the header and footer, activate Header & Footer and fill in both fields with the information to be printed on the receipts.

To print receipts automatically once the payment is registered, enable the Automatic Receipt Printing setting.

From the POS interface, click Orders, open the dropdown selection menu next to the search bar, and change the default All active orders filter to Paid. Then, select the corresponding order and click Print Receipt.

You can filter the list of orders using the search bar. Type in your reference and click Receipt Number, Date, or Customer.

Point of Sale allows you to issue and print invoices for registered customers upon payment and retrieve all past invoiced orders.

An invoice created in a POS creates an entry into the corresponding accounting journal, previously set up.

To define what journals will be used for a specific POS, go to the POS’ settings and scroll down to the accounting section. Then, you can determine the accounting journals used by default for orders and invoices in the Default Journals section.

Upon processing a payment, click Invoice underneath the customer’s name to issue an invoice for that order.

Select the payment method and click Validate. The invoice is automatically issued and ready to be downloaded and/or printed.

To be able to issue an invoice, a customer must be selected.

To retrieve invoices from the POS dashboard,

access all orders made through your POS by going to Point of Sale ‣ Orders ‣ Orders;

to access an order’s invoice, open the order form by selecting the order, then click Invoice.

Invoiced orders can be identified by the Invoiced status in the Status column.

You can filter the list of orders to invoiced orders by clicking Filters and Invoiced.

Customers can also request an invoice by scanning the QR code printed on their receipt. Upon scanning, they must fill in a form with their billing information and click Get my invoice. On the one hand, doing so generates an invoice available for download. On the other hand, the order status goes from Paid or Posted to Invoiced in the Odoo backend.

To use this feature, you have to enable QR codes on receipts by going to Point of Sale ‣ Configuration ‣ Settings. Then, select the POS in the Point of Sale field, scroll down to the Bills & Receipts section and enable Use QR code on ticket.

---

## Reconciliation models¶

**URL:** https://www.odoo.com/documentation/19.0/applications/finance/accounting/bank/reconciliation_models.html

**Contents:**
- Reconciliation models¶
- Configuration¶
  - Matching conditions¶
  - Counterpart items¶
- Default reconciliation models¶
  - Internal Transfers¶
  - Bank Fees¶
  - Cash Discount¶
- Partner mapping¶

Reconciliation models are custom rules that complement the default set of matching rules and enable more advanced automation of the bank reconciliation process. These models are especially useful when dealing with recurring flows like writing off bank fees or cash discounts.

Odoo Tutorials: Reconciliation models

To access reconciliation models, go to the Accounting Dashboard, click the (dropdown menu) menu on the bank journal, and select Models under the Reconciliation section.

To create a new reconciliation model, click New.

Reconciliation models can be either Manual or Automated. Manual reconciliation models appear as possible action buttons when reconciling. Automatic reconciliation models apply automatically to transactions that meet the reconciliation model’s matching conditions.

Each reconciliation model is configured with matching conditions to identify the relevant bank transactions and Counterpart Items to be generated during reconciliation.

To create an activity on the transaction, select which type of activity to create in the Next Activity field.

If a record matches with several reconciliation models, the first one in the sequence of models is applied. Rearrange the order by dragging and dropping the handle next to the name.

A reconciliation model’s matching conditions determine to which transactions it applies.

The following fields can be used to restrict the reconciliation model’s availability to transactions that meet the conditions:

Amount: Select Is lower than or equal to, Is greater than or equal to, or Is between and enter the amount(s).

Label: Select Contains, Not Contains, or Match Regex and enter the transaction label’s matching condition.

Regular expressions, often abbreviated as regex, can be used in Odoo in various ways to search, validate, and manipulate data. Regex can be powerful but also complex, so it’s essential to use it judiciously.

To use regular expressions in a reconciliation model, set the Label to Match Regex and add an expression. Odoo automatically retrieves the transactions that match the regex expression and the conditions specified in the reconciliation model.

A transaction must meet all conditions for the reconciliation model to be available for it. If no condition is defined (i.e., if all fields are left blank), the reconciliation model will be available for all transactions.

Each line in the Counterpart items tab creates a journal item with the specified details:

Partner: Select the partner, if any, to set on the journal item.

Account: Select the account, if any, to set on the journal item.

Amount Type: Select how the amount of the journal item should be calculated:

Fixed: Use a fixed amount.

Percentage of balance: Use a percentage of the remaining balance of the transaction, regardless of the transaction total.

Percentage of statement line: Use a percentage of the transaction total, regardless of the remaining balance of the transaction.

From label: Use a percentage from the transaction’s label using regex.

Amount: Enter the amount to be used on the journal item. This field will be either a fixed amount, percentage amount, or regex depending on the Account Type.

Taxes: Select a tax, if any, to set on the journal item. This field is hidden behind the (settings adjust) icon by default.

Analytic: Select an analytic distribution, if any, to set on the journal item.

Label: Enter a label, if any, to set on the journal item.

While neither the Partner nor Account fields are mandatory, at least one of the two must be set for the reconciliation model to work correctly.

The reconciliation model can be used for partner mapping if the Counterpart Items include a Partner but no Account.

In Odoo, different models are available by default depending on the company’s fiscal localization. These can be updated if needed. The following reconciliation models exist across most fiscal localizations.

The Internal Transfers reconciliation model is used for making internal transfers from one bank or cash account to another by moving the entire transaction’s balance to a liquidity or internal transfer account. To fully transfer the amount from one account to another, this reconciliation model must be used on both the incoming journal’s transaction and the outgoing journal’s transaction.

The Bank Fees reconciliation model generates a counterpart item that moves the remaining balance of a transaction to a bank fees account (that varies by fiscal localization) and includes “Bank Fees” in the Label of the new item that it creates. This reconciliation model is only applicable to transactions whose label contains “Bank Fees” due to its matching conditions.

An outgoing bank transaction for $103 is partially matched with a vendor bill for $100, leaving $3 of the transaction still unreconciled. Use the Bank Fees reconciliation model to create a new counterpart item for $3 and reconcile it with the remaining $3 of the bank transaction.

The Cash Discount reconciliation model generates a counterpart item that moves the remaining balance of a transaction to a cash discount account (that varies by fiscal localization) and includes “Cash Discount” in the Label of the new item that it creates.

Cash discounts and tax reduction

Partner mapping allows you to establish rules for automatically matching transactions to the correct partner account, saving time and reducing the risk of errors that can occur during manual reconciliation. For example, you can create a partner mapping rule for incoming payments with specific reference numbers or keywords in the transaction description. When an incoming payment meets these criteria, Odoo automatically maps it to the corresponding customer’s account.

To create a partner mapping rule, configure any matching conditions, such as a specific transaction label, and then configure the Partner and any other relevant fields in the Counterpart Items tab. Setting an Account is not mandatory for partner mapping.

---

## Hong Kong¶

**URL:** https://www.odoo.com/documentation/19.0/applications/hr/payroll/payroll_localizations/hong_kong.html

**Contents:**
- Hong Kong¶
- Create employees¶
- Manage contracts¶
- Generate payslips¶
  - Batch payslips¶
  - Individual payslips¶
- Pay employees¶
- Attendances and hourly wage¶
- Time Off with Payroll¶
- Understanding 713 Ordinance¶

Ensure the Hong Kong - Payroll (l10n_hk_hr_payroll) module is installed before proceeding.

Go to the Employees app and click New. Then, configure the following fields:

Under the Work Information tab

Working Hours: HK Standard 40 hours/week option must be selected.

Under the Private Information tab

Surname, Given Name, Name in Chinese: name of the employee.

Private Address: address of the employee.

Bank Account Number: employee’s bank account number.

Current Rental: employee’s rental records (if rental allowance is applicable).

Autopay Type: BBAN, SVID, EMAL, etc.

Autopay Reference: autopay reference number.

Identification No: HKID of the employee.

Gender: gender of the employee.

For the Bank Account Number, this account should be set as Trusted before further processing.

To achieve this, click on the right-arrow button next to Bank Account Number field. Set the Send Money to Trusted by clicking on the toggle.

To populate the Current Rental, click on the History button. Then, click on New. Fill in the relevant details and save the rental record. Upon saving the record, the rental contract state will be visible (at the top-right corner) and can be set to Running.

Under the HR Settings tab:

Volunteer Contribution Option: select either Only Mandatory Contribution, With Fixed %VC, or Cap 5% VC.

MPF Manulife Account: account number, if applicable.

Once the new employee has been created, click the Contracts smart button on the employee record, or navigate to Employees app ‣ Employees ‣ Contracts.

Only one contract can be active simultaneously per employee, but an employee can be assigned consecutive contracts during their employment.

The following are critical for setting up a contract:

Salary Structure Type: set as CAP57: Hong Kong Employee.

Contract Start Date: start date of employment.

Working Schedule: set as HK Standard 40 hours/week (from employee record).

Work Entry Source: select either Working Schedule, Attendances or Planning. This field determines how the work entries are accounted for in the payslip.

Working Schedule: the work entries are generated automatically based on the employee’s working schedule.

Attendances: the work entries are generated based on the check-in/out period logged in the Attendances.

Planning: the work entries are generated from planning shifts only.

Under the Salary Information tab

Wage Type: select Fixed Wage for Full-time or Part-time employees, or Hourly Wage for employees who are paid hourly.

Schedule Pay: the frequency of payslip issuance.

Wage: Monthly or Hourly depending on the Wage Type.

Internet Subscription: this is an optional field to provide additional internet allowance on top of the current salary package.

Timesheets do not impact work entries in Odoo.

Once all information has been setup, set the contract status to Running by clicking the Running button in the top-right of the page.

Once the employees, and their contracts, are configured, payslips can be generated in the Payroll app.

Odoo provides four different salary structures under CAP57 regulation:

CAP57: Employees Monthly Pay: to process the monthly employee salary.

CAP57: Payment in Lieu of Notice: to process final payment upon contract termination using ADW.

CAP57: Long Service Payment: applicable to employees with more than five years of service upon contract termination.

CAP57: Severance Payment: applicable to employees with more than two years of service upon contract termination.

Before running the payslips, the accounts used in the salary rule can be adjusted by navigating to Payroll app ‣ Configuration ‣ Rules.

Odoo can create pay runs in two ways: via batch or individual payslips.

This method of payslip generation is used for recurring payments, since multiple employee payslips can be managed at once. Go to Payroll app ‣ Payslips ‣ Batches.

Enter a Batch Name (e.g. 2024 - Jan) and Period (e.g. 01/01/2024 - 01/31/2024).

Click on Generate Payslips.

Choose which Salary Structure to use for this batch. The department filter allows the batch to only apply to a specific group of employees.

A Payslips smart button is created automatically.

Next, click Create Draft Entry to generate a draft journal entry found in the Other Info tab of each payslip. A Confirmation pop-up window appears asking Are you sure you want to proceed?. Click Ok to create the journal entries.

Go to Payroll app ‣ Payslips ‣ All Payslips.

This method of payslip generation is commonly used to handle non-recurring payments (e.g. CAP57: Payment in Lieu of Notice, CAP57: Long Service Payment or CAP57: Severance Payment).

Select an Employee. When selected, the Contract is filled out automatically.

Select a salary Structure (e.g. CAP57: Employees Monthly Pay).

The Worked Days & Inputs tab automatically compute the worked days/hours and time off leaves that are applicable.

Additional payslip items can be added at this time (e.g. Commissions, Deductions) under the Other Inputs section.

Click on Compute Sheet button to generate the payslip lines. This button updates the Salary Computation tab.

If the work entry for an employee was amended, click the (gear) icon, then click Recompute Whole Sheet to refresh the payslip’s Worked Days & Inputs tab.

The Salary Computation tab shows the detailed breakdown of the computation, based on the salary rules configured for each structure type.

Rent Allowance: amount derived from the employee’s active rental record.

Basic Salary: amount of base salary provided (after rent allowance deduction).

713 Gross: net payable amount considering Commission, Internet Allowance, Reimbursements, Back-pay, Deduction, etc.

MPF Gross: net payable amount from 713 gross after consideration of additional allowances, deductions, and end-of-year payment.

Employee Mandatory Contribution: employee MPF Contribution.

Employer Mandatory Contribution: employer MPF Contribution.

Gross: net payable amount from MPF gross after consideration of MPF deductions.

Net Salary: final payable amount to be paid to the employee.

There are no MPF contributions for the first month. Both employee and employer contribution starts on second month.

Under the Other Inputs section in Worked Days & Inputs tab, there are additional manual input types:

Back Pay: additional salary payout can be included under this category.

Commission: the commission earned during the period can be manually entered here.

Global Deduction: a lump-sum deduction from the entire payslip.

Global Reimbursement: a lump-sum reimbursement to the entire payslip.

Referral Fee: the additional bonus offered for any form of business-related referral.

Moving Daily Wage: to override the ADW value used for leaves computation.

Skip Rent Allowance: if set, the rental allowance is excluded from the current payslip.

Custom Average Monthly Salary: to override the average monthly salary used for end-of-year payment (rule is only applicable to payslips generated in December).

Lieu of Notice Period (Months): only applicable to CAP57: Payment in Lieu of Notice salary structure. By default, the final payout is set as 1-month. Use the Count field under the Other Inputs section to set a different notice period duration.

Once the payslips are ready, click on Compute Sheet, followed by Create Draft entry to generate a draft journal entry found in the Other Info tab of the payslip.

Once the draft journal entries have been posted, the company can now pay the employees. The user can choose between two different payment methods:

From the employee’s payslip (Payroll app ‣ Payslips ‣ All Payslips), once the payslip’s journal entry has been posted, click Register Payment. The process is the same as paying vendor bills. Select the desired bank journal and payment method, then later reconcile the payment with the corresponding bank statement.

For batch payments (Payroll app ‣ Payslips ‣ Batches), once all draft journal entries from the batch are confirmed, click Mark as Paid to post the payment journal entry. Then create a payment in the Accounting app, and reconcile accordingly.

To configure the contract for an employee paid hourly using the Attendances app for hours tracking, navigate to Payroll app ‣ Contracts ‣ Contracts. Create a new contract. It is important to remember to set the Work Entry Source as Attendances, and Wage Type as Hourly Wage.

To record the hours logged by the employee using Attendances app:

Go to Attendances app.

The employee can check-in/out, via the kiosk mode and the time will be logged automatically.

In the Payroll app, review the attendance work entries generated from Payroll app ‣ Work Entries ‣ Work Entries.

Next, generate the payslips and process the payment.

The work entry types and time off types are fully integrated between the Time Off and Payroll apps. There are several default time off types and work entry types specific to Hong Kong which are installed automatically along with the Hong Kong - Payroll module.

Go to Payroll app ‣ Configuration ‣ Work Entry Types and click New.

There are two checkboxes to be considered when setting up the work entry type:

Use 713: Include this leave type as part of 713 computation.

Non-full pay: 80% of the ADW.

Creating and configuring work entry types

The Hong Kong - Payroll module is compliant with 713 Ordinance which relates to the ADW computation to ensure fair compensation for employees.

The ADW computation is as follows:

ADW equals the total wage in a 12-month period, minus the wages of non-full pay, divided by the total days in a 12-month period minus the days of non-full pay.¶

For 418 compliance, there is no automated allocation of the Statutory Holiday entitlement to the employees. As soon as 418 requirements are met, manually allocate the leaves, via the Time Off app.

Before generating payslips, ensure the statuses are Done to validate the outcome.

Mar (One Day Annual Leave)

$730.27 ($65724.33/90)

Apr (One Day 80% Sick Leave)

$584.22 ($730.27*0.8)

Here is an example demonstrating the 713 logic:

Jan: Generate a payslip with a monthly wage of $20200. The ADW is always computed on a cumulative basis of the trailing 12-months.

Feb: Generate a similar payslip, but add an Other Input Type for the Commission.

Mar: Apply for one full-paid annual leave in March. The salary compensation for the leave taken is based on ADW thus far.

Apr: Apply for a 1-day non-full pay leave in April. Since this is a non-full pay leave, the ADW is computed accordingly.

The value of ADW is computed in the backend, and not be visible to the user.

Before generating the below reports, setup the following in Settings app ‣ Payroll.

Configure the following in the Accounting section:

Tick the Payroll HSBC Autopay checkbox.

Autopay Type: Set as H2H Submission.

Select the Bank Account to use.

Configure the following in the HK Localization section:

Employer’s Name shows on reports

Employer’s File Number

There are a total of four IRD reports available:

IR56B: employer’s Return of Remuneration and Pensions.

IR56E: notification of Commencement of Employment.

IR56F: notification of Ceasation of Employment (remaining in HK).

IR56G: notification of Ceasation of Employment (departing from HK permanently).

Go to Payroll app ‣ Reporting, and select one of the IR56B/E/F/G Sheet options:

Fill in the relevant information for the IRD report.

Click on Populate, and the Eligible Employees smart button appears.

The Employee Declarations status is Draft and changed to Generated PDF status once the schedule runs.

Once the PDF is generated, the IRD form may be downloaded.

The scheduled action called Payroll: Generate pdfs can be manually triggered. It is set by default to run the PDF generation monthly.

Go to Payroll app ‣ Reporting ‣ Manulife MPF Sheet.

Select the relevant Year, Month, and Sequence No..

Click on Create XLSX.

The Manulife MPF XLSX file is then generated, and available for download.

Odoo will not be developing further reports for other MPF trustee as there will soon be an eMPF platform setup by the local government.

If HSBC Autopay is selected as the batch payment method, click on Create HSBC Autopay Report, and fill in the mandatory fields:

This creates an .apc file format which can be uploaded to the HSCB portal for processing.

---

## Multi-company Guidelines¶

**URL:** https://www.odoo.com/documentation/19.0/developer/howtos/company.html

**Contents:**
- Multi-company Guidelines¶
- Company-dependent fields¶
- Multi-company consistency¶
- Default company¶
- Views¶
- Security rules¶

This tutorial requires good knowledge of Odoo. Please refer to the Server framework 101 tutorial first if needed.

As of version 13.0, a user can be logged in to multiple companies at once. This allows the user to access information from multiple companies, but also to create/edit records in a multi-company environment.

If not managed correctly, it may be the source of a lot of inconsistent multi-company behaviors. For instance, a user logged in to both companies A and B could create a sales order in company A and add products belonging to company B to it. It is only when the user logs out from company B that access errors will occur for the sales order.

To correctly manage multi-company behaviors, Odoo’s ORM provides multiple features:

Company-dependent fields

Multi-company consistency

When a record is available from multiple companies, we must expect that different values will be assigned to a given field depending on the company from which the value is set.

For the field of the same record to support several values, it must be defined with the attribute company_dependent set to True.

The _compute_display_info method is decorated with depends_context('company') (see depends_context) to ensure that the computed field is recomputed depending on the current company (self.env.company).

When a company-dependent field is read, the current company is used to retrieve its value. In other words, if a user is logged in to companies A and B with A as the main company and creates a record for company B, the value of company-dependent fields will be that of company A.

To read the values of company-dependent fields set by another company than the current one, we need to ensure the company we are using is the correct one. This can be done with with_company(), which updates the current company.

Whenever you are computing/creating/… things that may behave differently in different companies, you should make sure whatever you are doing is done in the right company. It doesn’t cost much to always use with_company to avoid problems later.

When a record is made shareable between several companies by the means of a company_id field, we must take care that it cannot be linked to the record of another company through a relational field. For instance, we do not want to have a sales order and its invoice belonging to different companies.

To ensure this multi-company consistency, you must:

Set the class attribute _check_company_auto to True.

Define relational fields with the attribute check_company set to True if their model has a company_id field.

On each create() and write(), automatic checks will be triggered to ensure the multi-company consistency of the record.

The field company_id must not be defined with check_company=True.

Check the companies of the values of the given field names.

fnames (list) – names of relational fields to check

UserError – if the company_id of the value of any field is not in [False, self.company_id] (or self if res_company).

For res_users relational fields, verifies record company is in company_ids fields.

User with main company A, having access to company A and B, could be assigned or linked to records in company B.

The check_company feature performs a strict check! It means that if a record has no company_id (i.e., the field is not required), it cannot be linked to a record whose company_id is set.

When no domain is defined on the field and check_company is set to True, a default domain is added: ['|', '('company_id', '=', False), ('company_id', '=', company_id)]

When the field company_id is made required on a model, a good practice is to set a default company. It eases the setup flow for the user or even guarantees its validity when the company is hidden from view. Indeed, the company is usually hidden if the user does not have access to multiple companies (i.e., when the user does not have the group base.group_multi_company).

As stated in above, the company is usually hidden from view if the user does not have access to multiple companies. This is assessed with the group base.group_multi_company.

When working with records shared across companies or restricted to a single company, we must take care that a user does not have access to records belonging to other companies.

This is achieved with security rules based on company_ids, which contain the current companies of the user (the companies the user checked in the multi-company widget).

**Examples:**

Example 1 (python):
```python
from odoo import api, fields, models

class Record(models.Model):
    _name = 'record.public'

    info = fields.Text()
    company_info = fields.Text(company_dependent=True)
    display_info = fields.Text(string='Infos', compute='_compute_display_info')

    @api.depends_context('company')
    def _compute_display_info(self):
        for record in self:
            record.display_info = record.info + record.company_info
```

Example 2 (markdown):
```markdown
# Accessed as the main company (self.env.company)
val = record.company_dependent_field

# Accessed as the desired company (company_B)
val = record.with_company(company_B).company_dependent_field
# record.with_company(company_B).env.company == company_B
```

Example 3 (python):
```python
@api.onchange('field_name')
def _onchange_field_name(self):
 self = self.with_company(self.company_id)
 ...

@api.depends('field_2')
def _compute_field_3(self):
 for record in self:
   record = record.with_company(record.company_id)
   ...
```

Example 4 (python):
```python
from odoo import fields, models

class Record(models.Model):
    _name = 'record.shareable'
    _check_company_auto = True

    company_id = fields.Many2one('res.company')
    other_record_id = fields.Many2one('other.record', check_company=True)
```

---

## Pipeline Analysis¶

**URL:** https://www.odoo.com/documentation/19.0/applications/sales/crm/performance/win_loss.html

**Contents:**
- Pipeline Analysis¶
- Navigate the pipeline analysis page¶
  - Search options¶
    - Add custom filters and groups¶
  - Measurement options¶
  - View options¶
- Create reports¶
  - Win/Loss reports¶
    - Customize win/loss reports¶
- Save and share reports¶

The CRM app manages the sales pipeline as leads/opportunities move from stage to stage, origination to sale (Won) or archival (Lost).

After organizing the pipeline, use the search options and reports available on the Pipeline Analysis page to gain insight into the effectiveness of the pipeline and its users.

To access the Pipeline Analysis page, go to CRM app ‣ Reporting ‣ Pipeline.

Upon accessing the Pipeline Analysis page, a bar graph showcasing the leads from the past year automatically populates. The bars represent the number of leads in each stage of the sales pipeline, color-coded to show the month the lead reached that stage.

The interactive elements of the Pipeline Analysis page manipulate the graph to report different metrics in several views. From left-to-right, top-to-bottom, the elements include:

Actions: represented by the ⚙️ (gear) icon, located next to the Pipeline Analysis page title. When clicked, a drop-down menu appears with three options, each with their own sub-menu: Knowledge, Dashboard, Spreadsheet. (See Save and share reports for more information)

The Knowledge option is for linking to or inserting the graph in a Knowledge app article.

The Dashboard option is for adding the graph to a dashboard in the Dashboards app.

The Spreadsheet option is for linking the graph in a spreadsheet in the Documents app.

Search… bar: shows the filters and groupings currently being applied to the graph. To add new filters/groups, type them into the search bar, or click the ⬇️ (down arrow) icon, at the end of the bar, to open a drop-down menu of options. (See Search Options for more information)

In the upper-right corner, there are view options represented by different icons. (See View Options for more information)

Graph view: displays the data in a bar graph. This is the default view.

Pivot view: displays the data in a customizable, categorized metrics table.

Cohort view: displays and organizes the data, based on their Created on and Closed Date week (default), day, month, or year.

List view: displays the data in a list.

Located on the far-left side of the page, beneath the Pipeline Analysis page title, there are more configurable filter and view options.

Measures: opens a drop-down menu of different measurement options that can be seen in the graph, pivot, or cohort view. The Measure drop-down menu is not available in the list view. (See Measurement Options for more information)

Insert in Spreadsheet: opens a pop-up window with options for adding a graph or pivot table to a spreadsheet in the Documents app or a dashboard in the Dashboards app. This option is not available in the cohort or list view.

With the graph view selected, the following options are available:

Bar Chart: switches the graph to a bar chart.

Line Chart: switches the graph to a line chart.

Pie Chart: switches the graph to a pie chart.

Stacked: when selected, the results of each stage of the graph are stacked on top of each other. When not selected, the results in each stage are shown as individual bars.

Descending: re-orders the stages in the graph in descending order from left-to-right. Click the icon a second time to deselect it. Depending on the search criteria, this option may not be available.

Ascending: re-orders the stages in the graph in ascending order from left-to-right. Click the icon a second time to deselect it. Depending on the search criteria, this option may not be available.

With the pivot view selected, the following options are available:

Flip Axis: flips the X and Y axis for the entire table.

Expand All: when additional groupings are selected using the ➕ (plus sign) icons, this button opens those groupings under every row.

Download xlsx: downloads the table as an Excel file.

The Pipeline Analysis page can be customized with various filters and grouping options.

To add new search criteria, type the desired criteria into the search bar, or click the ⬇️ (down arrow) icon, next to the search bar, to open a drop-down menu of all options. See the sections below for more information on what each option does.

The Filters section allows users to add pre-made and custom filters to the search criteria. Multiple filters can be added to a single search.

My Pipeline: show leads assigned to the current user.

Opportunities: show leads that have been qualified as opportunities.

Leads: show leads that have yet to be qualified as opportunities.

Active: show active leads.

Inactive: show inactive leads.

Won: show leads that have been marked Won.

Lost: show leads that have been marked Lost.

Created On: show leads that were created during a specific period of time. By default, this is the past year, but it can be adjusted as needed, or removed entirely.

Expected Closing: show leads that are expected to close (marked Won) during a specific period of time.

Date Closed: show leads that were closed (marked Won) during a specific period of time.

Archived: show leads that have been archived (marked Lost).

Add Custom Filter: allows the user to create a custom filter with numerous options. (See Add Custom Filters and Groups for more information)

The Group By section allows users to add pre-made and custom groupings to the search results. Multiple groupings can be added to split results into more manageable chunks.

The order that groupings are added affects how the final results are displayed. Try selecting the same combinations in a different order to see what works best for each use case.

Salesperson: groups the results by the Salesperson to whom a lead is assigned.

Sales Team: groups the results by the Sales Team to whom a lead is assigned.

City: groups the results by the city from which a lead originated.

Country: groups the results by the country from which a lead originated.

Company: groups the results by the company to which a lead belongs (if multiple companies are activated in the database).

Stage: groups the results by the stages of the sales pipeline.

Campaign: groups the results by the marketing campaign from which a lead originated.

Medium: groups the results by the medium (Email, Google Adwords, Website, etc.) from which a lead originated.

Source: groups the results by the source (Search engine, Lead Recall, Newsletter, etc.) from which a lead originated.

Creation Date: groups the results by the date a lead was added to the database.

Conversion Date: groups the results by the date a lead was converted to an opportunity.

Expected Closing: groups the results by the date a lead is expected to close (marked “Won”).

Closed Date: groups the results by the date a lead was closed(marked “Won”).

Lost Reason: groups the results by the reason selected when a lead was marked “Lost.”

Add Custom Group: allows the user to create a custom group with numerous options. (See Adding Custom Filters and Groups for more information)

The Comparison section allows users to add comparisons to the same search criteria over another period of time.

This option is only available if the search includes time-based filters, such as Created On, Expected Closing, or Date Closed. While multiple time-based filters can be added at once, only one comparison can be selected at a time.

Previous Period: adds a comparison to the same search criteria from the previous period.

Previous Year: adds a comparison to the same search criteria from the previous year.

The Favorites section allows users to save a search for later, so it does not need to be recreated every time.

Multiple searches can be saved, shared with others, or even set as the default for whenever the Pipeline Analysis page is opened.

Save current search: save the current search criteria for later.

Default filter: when saving a search, check this box to make it the default search filter when the Pipeline Analysis page is opened.

Shared: when saving a search, check this box to make it available to other users.

In addition to the pre-made options in the search bar, the Pipeline Analysis page can also utilize custom filters and groups.

Custom filters are complex rules that further customize the search results, while custom groups display the information in a more organized fashion.

To add a custom filter:

On the Pipeline Analysis page, click the down arrow icon next to the Search… bar.

In the drop-down menu, click Add Custom Filter.

The Add Custom Filter pop-up window appears with a default rule (Country is in _____) comprised of three unique fields. These fields can be edited to make a custom rule, and multiple rules can be added to a single custom filter.

To edit a rule, start by clicking the first field (Country), and select an option from the drop-down menu. The first field determines the primary subject of the rule.

Next, click the second field, and select an option from the drop-down menu. The second field determines the relationship of the first and third fields, and is usually an is or is not statement, but can also be greater than or less than statements, and more.

Finally, click the third field, and select an option from the drop-down menu. The third field determines the secondary subject of the rule.

With all three fields selected, the rule is complete.

To add more rules: click New Rule and repeat steps 4-7, as needed.

To delete a rule: click the 🗑️ (trash) icon to the right of the rule.

To duplicate an existing rule: click the ➕ (plus sign) icon to the right of the rule.

To create more complex rules: click the Add branch icon to the right of the rule. This adds another modifier below the rule for adding an “all of” or “any of” statement.

Once all rules have been added, click Add to add the custom filter to the search criteria.

To remove a custom filter: click the ✖️ (x) icon beside the filter in the search bar.

To add a custom group:

On the Pipeline Analysis page, click the down arrow icon next to the search bar.

In the drop-down menu that appears, click Add Custom Group.

Scroll through the options in the drop-down menu, and select one or more groups.

To remove a custom group: click the ✖️ (x) icon beside the custom group in the search bar.

By default, the Pipeline Analysis page measures the total Count of leads that match the search criteria, but can be changed to measure other items of interest.

To change the selected measurement, click the Measures button on the top-left of the page, and select one of the following options from the drop-down menu:

Days to Assign: measures the number of days it took a lead to be assigned after creation.

Days to Close: measures the number of days it took a lead to be closed (marked Won).

Days to Convert: measures the number of days it took a lead to be converted to an opportunity.

Exceeded Closing Days: measures the number of days by which a lead exceeded its Expected Closing date.

Expected MRR: measures the Expected Recurring Revenue of a lead.

Expected Revenue: measures the Expected Revenue of a lead.

Prorated MRR: measures the Prorated Monthly Recurring Revenue of a lead.

Prorated Recurring Revenues: measures the Prorated Recurring Revenues of a lead.

Prorated Revenue: measures the Prorated Revenue of a lead.

Recurring Revenues: measures the Recurring Revenue of a lead.

Count: measures the total amount of leads that match the search criteria.

After configuring filters, groupings, and measurements, the Pipeline Analysis page can display the data in a variety of ways. By default, the page uses the graph view, but can be changed to a pivot view, cohort view, or list view.

To change the pipeline to a different view, click one of the four view icons, located in the top-right of the Pipeline Analysis page.

The graph view is the default selection for the Pipeline Analysis page. It displays the analysis as either a: bar chart, line chart, or pie chart.

This view option is useful for quickly visualizing and comparing simple relationships, like the Count of leads in each stage, or the leads assigned to each Salesperson.

By default, the graph measures the Count of leads in each group, but this can be changed by clicking the Measures button, and selecting another option from the resulting drop-down menu.

When using a bar chart in this view, consider deselecting the Stacked option, in order to make the breakdown of results more legible.

The pivot view displays the results of the analysis as a table. By default, the table groups the results by the stages of the sales pipeline, and measures Expected Revenue.

The pivot view is useful for analyzing more detailed numbers than the graph view can handle, or for adding the data to a spreadsheet, where custom formulas can be set up, like in an Excel file.

The three icons at the top-left of the page perform the following functions:

Flip Axis: flips the X and Y axis for the entire table.

Expand All: when additional groupings are selected using the ➕ (plus sign) icons, this button opens those groupings under every row.

Download xlsx: downloads the table as an Excel file.

The Stage grouping cannot be removed, but the measurement can be changed by clicking the Measures button, and selecting another option.

The cohort view displays the analysis as periods of time (cohorts) that can be set to days, weeks, months, or years. By default, Week is selected.

This view option is useful specifically for comparing how long it has taken to close leads.

From left-to-right, top-to-bottom, the columns in the chart represent the following:

Created On: rows in this column represent the weeks of the year, in which records matching the search criteria exist.

When set to Week, a row with the label W52 2023 means the results occurred in: Week 52 of the Year 2023.

Measures: the second column in the chart is the measurement of the results. By default, it is set to Count, but can be changed by clicking the Measures button, and selecting an option from the drop-down menu.

Closed Date - By Day/Week/Month/Year: this column looks at what percentage of the measured results were closed in subsequent days/weeks/months/years.

Average: this row provides the average of all other rows in the column.

The cohort view can also be downloaded as an Excel file, by clicking the Download icon in the top-left of the page.

The list view displays a single list of all leads matching the search criteria. Clicking a lead opens the record for closer review. Additional details such as Country, Medium, and more can be added to the list, by clicking the Filters icon in the top-right of the list.

This view option is useful for reviewing many records at once.

Clicking the ⚙️ (gear) icon opens the Actions drop-down menu, with options for the following:

Import records: opens a page for uploading a spreadsheet of data, as well as a template spreadsheet to easily format that data.

Export All: downloads the list as an xlsx file for Excel.

Knowledge: inserts a view of, or link to, the list in an article in the Knowledge app.

Dashboard: adds the list to My Dashboard in the Dashboards app.

Spreadsheet: links to, or inserts, the list in a spreadsheet in the Documents app.

On the list view, clicking New closes the list, and opens the New Quotation page. Clicking Generate Leads opens a pop-up window for lead generation. Neither feature is intended to manipulate the list view.

After understanding how to navigate the pipeline analysis page, the Pipeline Analysis page can be used to create and share different reports. Between the pre-made options and custom filter and groupings, almost any combination is possible.

Once created, reports can be saved to favorites, shared with other users, and/or added to dashboards and spreadsheets.

A few common reports that can be created using the Pipeline Analysis page are detailed below.

Win/Loss is a calculation of active or previously active leads in a pipeline that were either marked as Won or Lost over a specific period of time. By calculating opportunities won over opportunities lost, teams can clarify key performance indicators (KPIs) that are converting leads into sales, such as specific teams or team members, certain marketing mediums or campaigns, and so on.

A win/loss report filters the leads from the past year, whether won or lost, and groups the results by their stage in the pipeline. Creating this report requires a custom filter, and grouping the results by Stage.

Follow the steps below to create a win/loss report:

Navigate to CRM app ‣ Reporting ‣ Pipeline.

On the Pipeline Analysis page, click the ⬇️ (down arrow) icon, next to the search bar, to open a drop-down menu of filters and groupings.

In drop-down menu that appears, under the Group By heading, click Stage.

Under the Filters heading, click Add Custom Filter to open another pop-up menu.

In the Add Custom Filter pop-up menu, click on the first field in the Match any of the following rules: section. By default, this field displays Country.

Clicking that first field reveals a sub-menu with numerous options to choose from. From this sub-menu, locate and select the Active option. Doing so automatically populates the remaining fields.

The first field reads: Active. The second field reads: is. And lastly, the third field reads: set.

In total, the rule reads: Active is set.

Click New Rule, change the first field to Active, and the last field to not set. In total, the rule reads Active is not set.

The report now displays the total Count of leads, whether “Won” or “Lost,” grouped by their stage in the CRM pipeline. Hover over a section of the report to see the number of leads in that stage.

After creating a win/loss report, consider using the options below to customize the report for different needs.

A sales manager might group wins and losses by salesperson, or sales team, to see who has the best conversion rate. Or, a marketing team might group by sources, or medium, to determine where their advertising has been most successful.

To add more filters and groups, click the ⬇️ (down arrow) icon, next to the search bar, and select one or more options from the drop-down menu.

Some useful options include:

Created on: adjusting this filter to a different period of time, such as the last 30 days, or the last quarter, can provide more timely results.

Add Custom Filter: clicking this option, and scrolling through the numerous options in the drop-down menu, opens up additional search criteria, like Last Stage Update or Lost Reason.

Add Custom Group > Active: Clicking Add Custom Group ‣ Active separates the results into Won (true) or Lost (false). This shows at what stage leads are being marked Won or Lost.

Multiple Groupings: add multiple Group By selections to split results into more relevant and manageable chunks.

Adding Salesperson or Sales Team breaks up the total count of leads in each Stage.

Adding Medium or Source can reveal what marketing avenues generate more sales.

By default, pivot view groups win/loss reports by Stage and measures Expected Revenue.

To flesh out the table:

Click the ⬇️ (down arrow) next to the search bar.

In the pop-up menu, replace the Stage grouping with something like Salesperson or Medium.

Click the Measures button and click Count to add the number of leads back into the report.

Other useful measures for pivot view include Days to Assign and Days to Close.

In pivot view, the Insert In Spreadsheet button may be greyed out due to the report containing duplicate group bys. To fix this, replace the Stage grouping in the search bar with another option.

In list view, a win/loss report displays all leads on a single page.

To better organize the list, click the ⬇️ (down arrow) next to the search bar, and add more relevant groupings or re-organize the existing ones. To re-order the nesting, remove all Group By options and re-add them in the desired order.

To add more columns to the list:

Click the Filters icon in the top-right of the page.

Select options from the resulting drop-down menu. Some useful filters include:

Campaign: Shows the marketing campaign that originated each lead.

Medium: Shows the marketing medium (Banner, Direct, Email, Google Adwords, Phone, Website, etc.) that originated each lead.

Source: Shows the source of each lead (Newsletter, Lead Recall, Search Engine, etc.).

After creating a report, the search criteria can be saved, so the report does not need to be created again in the future. Saved searches automatically update their results every time the report is opened.

Additionally, reports can be shared with others, or added to spreadsheets/dashboards for greater customization and easier access.

To save a report for later:

On the Pipeline Analysis page, click the ⬇️ (down arrow) icon, next to the search bar.

In the drop-down menu that appears, under the Favorites heading, click Save current search.

In the next drop-down menu that appears, enter a name for the report.

Checking the Default filter box sets this report as the default analysis when the Pipeline Analysis page is accessed.

Checking the Shared box makes this report available to other users.

Finally, click Save. The report is now saved under the Favorites heading.

Inserting a report into a spreadsheet not only saves a copy of the report, it allows users to add charts and formulas like in an Excel file.

To save a report as a spreadsheet:

In Graph or Pivot View:

Click the Insert in spreadsheet button.

In the pop-up menu that appears, click Confirm.

In Cohort or List View:

Click the ⚙️ (gear) icon.

In the drop-down menu that appears, hover over Spreadsheet.

In the next drop-down menu, click either Insert in spreadsheet or Link in spreadsheet.

Saved reports are viewable in the Documents app.

After modifying a spreadsheet and adding additional formulas, consider then adding the entire spreadsheet to a dashboard. Using this method, the spreadsheet can be added to a public dashboard instead of only My Dashboard.

Click File ‣ Add to dashboard.

In the pop-up menu that appears, name the spreadsheet and select a Dashboard Section to house the report.

Adding a report to a dashboard saves it for later and makes it easy to view alongside the rest of My Dashboard.

To add a report to My dashboard:

On the Pipeline Analysis page, click the ⚙️ (gear) icon.

In the drop-down menu that appears, hover over Dashboard.

In the Add to my dashboard drop-down menu, enter a name for the report (by default, it is named Pipeline).

To view a saved report:

Return to the main apps page, and navigate to Dashboards app ‣ My Dashboard.

Convert leads into opportunities

Create and send quotations

---

## Türkiye¶

**URL:** https://www.odoo.com/documentation/19.0/applications/hr/payroll/payroll_localizations/turkey.html

**Contents:**
- Türkiye¶
- Apps & modules¶
- General configurations¶
- Employees¶
  - Work tab¶
  - Personal tab¶
  - Payroll tab¶
    - Contract overview section¶
    - Schedule section¶
- Payroll configuration¶

The Türkiye payroll localization covers salary computations for employees, including both employee and employer payroll taxes. It accounts for federal and state regulations.

Before configuring the Türkiye localization, refer to the general payroll documentation, which includes the basic information for all localizations, as well as all universal settings and fields.

Install the following modules to get all the features of the Türkiye payroll localization:

Includes all salary rules, leave logic, and compensation rules compliant with Türkiye labor laws.

Türkiye - Payroll with Accounting

l10n_tr_hr_payroll_account

l10n_tr_hr_payroll_account

Links payroll and accounting by creating journal entries (per payslip if needed) to record payroll in the company’s books.

First, the company must be configured. Navigate to Settings app ‣ Users & Companies ‣ Companies. From the list, select the desired company, and configure the following fields:

Company Name: Enter the business name in this field.

Address: Complete the full address, including the City, State, ZIP, and Country.

Tax ID: Enter the company’s tax ID.

Company ID : Enter the business’s ID number.

Currency: By default, TRY is selected. If not, select TRY from the drop-down menu.

Phone: Enter the company phone number.

Email: Enter the email used for general contact information.

Every employee being paid must have their employee profiles configured for the Türkiye payroll localization. Additional fields are present after configuring the database for Türkiye.

To update an employee form, open the Employees app and click on the desired employee record. On the employee form, configure the required fields in the related tabs.

Enter the Work Address for the employee in the Location section of the Work tab.

Ensure the employee has a minimum of one trusted bank account listed in the Bank Accounts field in the Private Contact section.

These accounts are used to pay the employee. Payroll cannot be processed for employees without a trusted bank account. If no trusted bank account is set, a warning appears on the Payroll dashboard and an error occurs when attempting to run payroll.

This section holds information that drives salary calculations. Ensure the following fields are configured:

Contract: Ensure a contract start date is entered in the first field.

Wage Type: Select how the employee is paid.

Select Fixed Wage for salaried employees who receive the same amount each pay period.

Select Hourly Wage for employees paid based on hours worked.

Set a default Wage Type in the salary Structure Type to configure employees in bulk. If needed, the default can be overridden on individual employee records if exceptions are needed.

Net to Gross: Leave this box unchecked if the gross salary is based on a fixed gross and a dynamically computed net (Gross to Net). Activate this option if the gross salary is based on a fixed net and a dynamically computed gross (Net to Gross).

Wage: If Net to Gross is unchecked, this field displays Wage. If it is checked, this field displays Net Wage. Enter the monthly amount earned by the employee.

Employee Type: May affect the working hours for the employee.

Contract Type: Determines how the employee is paid and classified, such as Permanent, Temporary, or Student.

Pay Category: Select Türkiye: Employee for this field. This defines when the employee is paid, their default working schedule, and the work entry type it applies to.

Work Entry Source: Defines how work entries are generated for payroll during the specified pay period. The options are:

Working Schedule: Based on the employee’s assigned working schedule (e.g., 40 hours per week).

Attendances: Based on approved checked-in hours in the Attendances app.

Planning: Based on scheduled shifts in the Planning app.

Working Hours: Using the drop-down menu, select the default work schedule. This is particularly important for employees available to receive overtime pay (typically hourly employees, not salaried).

Several sections within the Payroll app install a Salary Structure, Structure Type, Rules, and Parameters specific to Türkiye.

When the l10n_tr_payroll module is installed, a new Salary Structure gets installed, Türkiye: Regular Pay. This structure includes one Structure Type, Türkiye: Employee.

The Salary Structure contains all the individual salary rules that informs the Payroll app how to calculate employee payslips.

To view the salary rules that inform the salary structure what to do, navigate to Payroll app ‣ Configuration ‣ Structures and expand the Türkiye: Employee group to reveal the Türkiye: Monthly Pay structure type. Click Türkiye: Monthly Pay to view the detailed salary rules.

Each rule defines how pay is calculated, taking into account factors such as commissions, bonuses, taxes, and insurance.

Some calculations require specific rates associated with them, or wage caps. Rules Parameters are capable of listing a value, either a percentage or a fixed amount, to reference in the salary rules.

Most rules pull information stored in the parameters module to get the rate of the rule (a percentage) and the cap (a monetary amount).

To view rule parameters, navigate to Payroll app ‣ Configuration ‣ Rule Parameters. Here, all rule parameters are displayed with their linked Salary Rules, which can be accessed. Review the parameters associated with a rule by looking for the Name of the rule, and make any edits as needed.

Ensure the following rules parameters are up to date with current rules and regulations:

Türkiye SSI Base Amount Ceiling

Türkiye Tax Exemption Amount

Türkiye Stamp Tax Percentage

Türkiye Tax Bracket Rates

Türkiye SSI Company Contribution

The minimum wage is updated, and is different from the one added by default in Odoo. To update this, navigate to Payroll app ‣ Configuration ‣ Rule Parameters, then click the rule parameter Türkiye Minimum Wage. Click Add a line and add the new Parameter Value (minimum wage), and reference the date the change goes into effect.

Odoo adds updated rule parameters for the current calendar year. It is not recommended to edit rule parameters unless a parameter has changed, and is different from the rule parameters created by Odoo. Check with all local and national regulations before making any changes to rule parameters.

The following sections explain in detail the various employer and employee taxes specific to Türkiye.

Social insurance rules calculate the contribution amounts that are to be paid by the employer and employee to the SSI. This is only available for Türkiye employees.

Social insurance consists of two main categories:

Social Insurance: Employers contribute 15% of their employee’s salaries to the SSI, while employees contribute 14% of their salaries, which is deducted from their payslips.

Unemployment Insurance: Employers contribute 2% of their employee’s salaries to the SSI, while employees contribute 1% of their salary, which is deducted from their payslips.

Employer and employee contributions for both categories are calculated according to a salary maximum that is revised each year. Therefore, it is necessary to check the corresponding rule parameters to ensure all taxes are current.

Employees in Türkiye are subject to a progressive income tax system, where tax rates increase with higher annual income levels.

To calculate the deductible tax amount for the current month, the following is done:

Compute year-to-date gross amount: This serves as the base amount for calculating the employee’s taxable salary.

Determine year-to-date taxable amount: The taxable amount is compared to progressive tax brackets to compute the year-to-date tax amount due.

Subtract prior month’s tax: The amount taxed in previous months is subtracted from the year-to-date tax to obtain the pre-exemption tax amount due for the current month.

Deduct exemption amount: The exemption amount is deducted from the pre-exemption tax to determine the final tax due amount post-exemptions.

As of 2025, and depending on the annual income of the employee, the following rates apply:

158,000.00 - 330,000.00

330,000.00 - 800,000.00

800,000.00 - 4,300,000.00

Tax brackets are applied progressively. This means each portion of an employee’s income is taxed at its respective rate within each bracket, rather than their entire income being taxed at the highest rate they fall into.

Turkish employees benefit from a monthly personal income tax exemption, which is adjusted each year. For 2025, the exemption amount is TRY 3,315.70 per month, from January through July.

Stamp tax is applied to gross salary payments in Türkiye. The standard rate is 0.759% (i.e. 7.59 per thousand), which is withheld from the employee’s gross wage and remitted to the tax authority GIB by the employer.

The base amount for the stamp tax is the total gross salary amount (including bonuses or extras).

Turkish employees receive a personal stamp tax exemption, the value of which is updated annually. As of 2025, the exemption amount is TRY 197.38 per month, from January through July. This applies to gross earnings up to the legal minimum wage level before calculating stamp tax.

On an employee record, if the option Net to Gross is enabled in the contract section of the payroll tab, the net salary is fixed, and specified in the Net Wage field, while the gross wage is recalculated each month, and increases throughout the year.

The gross wage is what tax calculations are based on, so for these employees, the monthly net wage does not change, but the monthly gross amount is recalculated each month, changing the taxes with it.

In this scenario, the least amount of taxes are paid in the beginning of the year, and taxes increase as the year progresses.

On January 31st, an employee’s year-to-date total income is ₺10,000. The income tax is calculated based on the ₺10,000, which in this example is ₺800. In February, the employee’s year-to-date gross is ₺20,000, and the taxes are calculated based on that figure, for a total of ₺2,000.

Since the employee already paid ₺800 in January, the taxes due for February are ₺1,200 because the employee already paid ₺800 toward that total (₺2,000 - ₺800 = ₺1,200).

On an employee record, if the option Net to Gross is not enabled in the contract section of the payroll tab, the gross salary is calculated based on the Wage field.

The gross wage then has all the salary rules computed, to determine the net wage. The taxes are then computed based on the gross wage. In this scenario, unlike with net to gross, taxes are more consistent and do not change that much, from month to month.

The gross from net calculation depends on the salary rules defined in the Türkiye: Monthly Pay salary structure. Changes made to those rules affect the computed amount.

Before running payroll, the payroll officer must validate employee work entries to confirm pay accuracy and catch errors. This includes checking that all time off is approved and any overtime is appropriate.

Work entries sync based on the employee’s contract configuration. Odoo pulls from the assigned working schedule, attendance records, planning schedule, and approved time off.

Any discrepancies or conflicts must be resolved, then the work entries can be regenerated.

Once everything is correct, draft payslips can be created individually or in groups, referred to in the Payroll app as Pay Runs.

To cut down on the payroll officer’s time, it is typical to process payslips in batches, either by wage type (fixed salary vs hourly), pay schedule (weekly, bi-weekly, monthly, etc.), department (direct cost vs. administration), or any other grouping that best suits the company.

The process of running payroll includes different actions that need to be executed to ensure that the amount withheld from payroll taxes is correct, the amount that the employee receives as their net salary is correct, and the computation of hours worked reflects the employee’s actual hours worked, among others.

When running a payroll batch, check that the period, company, and employees included are correct before starting to analyze or validate the data.

Once the payslips are drafted, review them for accuracy. Check the Worked Days & Inputs tab, and ensure the listed worked time is correct, as well as any other inputs. Add any missing inputs, such as commissions, tips, reimbursements, that are missing.

Next, check the various totals (gross pay, employee taxes, benefits, employer taxes, net salaries), then click Compute Sheet to update the salary calculations, if there were edits. If everything is correct, click Validate.

The accounting process when running payroll has two components: creating journal entries and registering payments.

After payslips are confirmed and validated, journal entries are posted either individually, or in a batch. The journal entry is created first as a draft.

It must be decided if journal entries are done individually or in batches before running payroll.

After the journal entries are validated, Odoo can generate payments.

To generate payments from payslips, employees must have a trusted bank account. If the employee’s bank account is not marked as trusted, payments cannot be generated through Odoo.

Payments can be Grouped by Partner if there is a partner associated with a salary rule.

If there are no errors, payroll is completed for the pay period.

---

## Performance¶

**URL:** https://www.odoo.com/documentation/19.0/developer/reference/backend/performance.html

**Contents:**
- Performance¶
- Profiling¶
  - Enable the profiler¶
  - Analyse the results¶
  - Collectors¶
    - SQL collector¶
    - Periodic collector¶
    - QWeb collector¶
    - Sync collector¶
  - Performance pitfalls¶

Profiling is about analysing the execution of a program and measure aggregated data. These data can be the elapsed time for each function, the executed SQL queries…

While profiling does not improve the performance of a program by itself, it can prove very helpful in finding performance issues and identifying which part of the program is responsible for them.

Odoo provides an integrated profiling tool that allows recording all executed queries and stack traces during execution. It can be used to profile either a set of requests of a user session, or a specific portion of code. Profiling results can be either inspected with the integrated speedscope open source app allowing to visualize a flamegraph view or analyzed with custom tools by first saving them in a JSON file or in the database.

The profiler can either be enabled from the user interface, which is the easiest way to do so but allows profiling only web requests, or from Python code, which allows profiling any piece of code including tests.

Enable the developer mode.

Before starting a profiling session, the profiler must be enabled globally on the database. This can be done in two ways:

Open the developer mode tools, then toggle the Enable profiling button. A wizard suggests a set of expiry times for the profiling. Click on ENABLE PROFILING to enable the profiler globally.

Go to Settings –> General Settings –> Performance and set the desired time to the field Enable profiling until.

After the profiler is enabled on the database, users can enable it on their session. To do so, toggle the Enable profiling button in the developer mode tools again. By default, the recommended options Record sql and Record traces are enabled. To learn more about the different options, head over to Collectors.

When the profiler is enabled, all the requests made to the server are profiled and saved into an ir.profile record. Such records are grouped into the current profiling session which spans from when the profiler was enabled until it is disabled.

Odoo Online databases cannot be profiled.

Starting the profiler manually can be convenient to profile a specific method or a part of the code. This code can be a test, a compute method, the entire loading, etc.

To start the profiler from Python code, call it as a context manager. You may specify what you want to record through the parameters. A shortcut is available for profiling test classes: self.profile(). See Collectors for more information on the collectors parameter.

The profiler is called outside of the assertQueryCount in order to catch queries made when exiting the context manager (e.g., flush).

Context manager to use to start the recording of some execution. Will save sql and async stack trace by default.

db – database name to use to save results. Will try to define database automatically by default. Use value None to not save results in a database.

collectors – list of string and Collector object Ex: [‘sql’, PeriodicCollector(interval=0.2)]. Use None for default collectors

profile_session – session description to use to reproup multiple profile. use make_session(name) for default format.

description – description of the current profiler Suggestion: (route name/test method/loading module, …)

disable_gc – flag to disable gc durring profiling (usefull to avoid gc while profiling, especially during sql execution)

params – parameters usable by collectors (like frame interval)

When the profiler is enabled, all executions of a test method are profiled and saved into an ir.profile record. Such records are grouped into a single profiling session. This is especially useful when using the @warmup and @users decorators.

It can be complicated to analyze profiling results of a method that is called several times because all the calls are grouped together in the stack trace. Add an execution context as a context manager to break down the results into multiple frames.

To browse the profiling results, make sure that the profiler is enabled globally on the database, then open the developer mode tools and click on the button in the top-right corner of the profiling section. A list view of the ir.profile records grouped by profiling session opens.

Each record has a clickable link that opens the speedscope results in a new tab.

Speedscope falls out of the scope of this documentation but there are a lot of tools to try: search, highlight of similar frames, zoom on frame, timeline, left heavy, sandwich view…

Depending on the profiling options that were activated, Odoo generates different view modes that you can access from the top menu.

The Combined view shows all the SQL queries and traces merged togethers.

The Combined no context view shows the same result but ignores the saved execution context <performance/profiling/enable>`.

The sql (no gap) view shows all the SQL queries as if they were executed one after another, without any Python logic. This is useful for optimizing SQL only.

The sql (density) view shows only all the SQL queries, leaving gap between them. This can be useful to spot if eiter SQL or Python code is the problem, and to identify zones in where many small queries could be batched.

The frames view shows the results of only the periodic collector.

Even though the profiler has been designed to be as light as possible, it can still impact performance, especially when using the Sync collector. Keep that in mind when analyzing speedscope results.

Whereas the profiler is about the when of profiling, the collectors take care of the what.

Each collector specializes in collecting profiling data in its own format and manner. They can be individually enabled from the user interface through their dedicated toggle button in the developer mode tools, or from Python code through their key or class.

There are currently four collectors available in Odoo:

By default, the profiler enables the SQL and the Periodic collectors. Both when it is enabled from the user interface or Python code.

The SQL collector saves all the SQL queries made to the database in the current thread (for all cursors), as well as the stack trace. The overhead of the collector is added to the analysed thread for each query, which means that using it on a lot of small queries may impact execution time and other profilers.

It is especially useful to debug query counts, or to add information to the Periodic collector in the combined speedscope view.

Saves all executed queries in the current thread with the call stack.

This collector runs in a separate thread and saves the stack trace of the analysed thread at every interval. The interval (by default 10 ms) can be defined through the Interval option in the user interface, or the interval parameter in Python code.

If the interval is set at a very low value, profiling long requests will generate memory issues. If the interval is set at a very high value, information on short function executions will be lost.

It is one of the best way to analyse performance as it should have a very low impact on the execution time thanks to its separate thread.

This collector saves the Python execution time and queries of all directives. As for the SQL collector, the overhead can be important when executing a lot of small directives. The results are different from other collectors in terms of collected data, and can be analysed from the ir.profile form view using a custom widget.

It is mainly useful for optimizing views.

Record qweb execution with directive trace.

This collector saves the stack for every function’s call and return and runs on the same thread, which greatly impacts performance.

It can be useful to debug and understand complex flows, and follow their execution in the code. It is however not recommended for performance analysis because the overhead is high.

Record complete execution synchronously. Note that –limit-memory-hard may need to be increased when launching Odoo.

Be careful with randomness. Multiple executions may lead to different results. E.g., a garbage collector being triggered during execution.

Be careful with blocking calls. In some cases, external c_call may take some time before releasing the GIL, thus leading to unexpected long frames with the Periodic collector. This should be detected by the profiler and give a warning. It is possible to trigger the profiler manually before such calls if needed.

Pay attention to the cache. Profiling before that the view/assets/… are in cache can lead to different results.

Be aware of the profiler’s overhead. The SQL collector’s overhead can be important when a lot of small queries are executed. Profiling is practical to spot a problem but you may want to disable the profiler in order to measure the real impact of a code change.

Profiling results can be memory intensive. In some cases (e.g., profiling an install or a long request), it is possible that you reach memory limit, especially when rendering the speedscope results, which can lead to an HTTP 500 error. In this case, you may need to start the server with a higher memory limit: --limit-memory-hard $((8*1024**3)).

When working with recordsets, it is almost always better to batch operations.

Don’t call a method that runs SQL queries while looping over a recordset because it will do so for each record of the set.

Instead, replace the search_count with a _read_group to execute one SQL query for the entire batch of records.

This example is not optimal nor correct in all cases. It is only a substitute for a search_count. Another solution could be to prefetch and count the inverse One2many field.

Don’t create records one after another.

Instead, accumulate the create values and call the create method on the batch. Doing so has mostly no impact and helps the framework optimize fields computation.

Fail to prefetch the fields of a recordset while browsing a single record inside a loop.

Instead, browse the entire recordset first.

We can verify that the records are prefetched in batch by reading the field prefetch_ids which includes each of the record ids.browsing all records together is unpractical,

If needed, the with_prefetch method can be used to disable batch prefetching:

Algorithmic complexity is a measure of how long an algorithm would take to complete in regard to the size n of the input. When the complexity is high, the execution time can grow quickly as the input becomes larger. In some cases, the algorithmic complexity can be reduced by preparing the input’s data correctly.

For a given problem, let’s consider a naive algorithm crafted with two nested loops for which the complexity in in O(n²).

Assuming that all results have a different id, we can prepare the data to reduce the complexity.

Choosing the bad data structure to hold the input can lead to quadratic complexity.

If invalid_ids is a list-like data structure, the complexity of the algorithm may be quadratic.

Instead, prefer using set operations like casting invalid_ids to a set.

Depending on the input, recordset operations can also be used.

Database indexes can help fasten search operations, be it from a search in the or through the user interface.

Be careful not to index every field as indexes consume space and impact on performance when executing one of INSERT, UPDATE, and DELETE.

**Examples:**

Example 1 (unknown):
```unknown
with Profiler():
    do_stuff()
```

Example 2 (unknown):
```unknown
with Profiler(collectors=['sql', PeriodicCollector(interval=0.1)]):
    do_stuff()
```

Example 3 (python):
```python
with self.profile():
    with self.assertQueryCount(__system__=1211):
        do_stuff()
```

Example 4 (bash):
```bash
for index in range(max_index):
    with ExecutionContext(current_index=index):  # Identify each call in speedscope results.
        do_stuff()
```

---

## External JSON-2 API¶

**URL:** https://www.odoo.com/documentation/19.0/developer/reference/external_api.html

**Contents:**
- External JSON-2 API¶
- API¶
  - Request¶
  - Response¶
- Configuration¶
  - API Key¶
  - Access Rights¶
  - Database¶
- Transaction¶
- Code Example¶

Odoo is usually extended internally via modules, but many of its features and all of its data are also available externally for analysis or integration with various other softwares. Part of the Models API is easily available over HTTP via the /json/2 endpoint.

The actual models, fields and methods available are specific to every database and can be consulted on their /doc page.

Access to data via the external API is only available on Custom Odoo pricing plans. Access to the external API is not available on One App Free or Standard plans. For more information visit the Odoo pricing page or reach out to your Customer Success Manager.

Post a JSON object at the /json/2/<model>/<method> URL.

Required, the hostname of the server.

Required, bearer followed by an API key.

Required, application/json, a charset is recommended.

Optional, the name of the database to connect to.

Recommended, the name of your software.

Required, the technical model name.

Required, the method to execute.

An array of record ids on which to execute the method. Empty or omitted when calling an @api.model-decorated method.

Optional, an object of additional values. e.g. {"lang": "en_US"}.

As many time as needed, the method’s parameters.

In case of success, a 200 status with the JSON-serialized return value of the called method in the body.

In case of error, a 4xx/5xx status with a JSON-serialized error object in the body.

The fully qualified name of the Python exception that occured.

The exception message, usually the same as arguments[0].

All the exception arguments.

The context used by the request.

The exception traceback, for debugging purpose.

An API key must be set in the Authorization request header, as a bearer token.

Create a new API key for a user via Preferences ‣ Account Security ‣ New API Key.

Both a description and a duration are needed to create a new API key. The description makes it possible to identify the key, and to determine later whether the key is still in use or should be removed. The duration determines the lifetime of the key, after which the key becomes invalid. It is recommended to set a short duration (typically one day) for interactive usage. For security reasons, it is not possible to create keys that last for more than three months. This means that long lasting keys must be rotated at least once every three months.

The Generate Key button creates a strong 160-bits random key. The key value is displayed only once during creation and cannot be retrieved later. Copy the key immediately and store it securely. If the key is compromised or lost, delete it immediately and generate a new one.

Please refer to OWASP’s Secrets Management Cheat Sheet for further guidance on the management of API keys.

The JSON-2 API uses the standard security models of Odoo. All operations are validated against the access rights, record rules and field accesses of the user.

For interactive usage, such as discovering the API or running one-time scripts, it is fine to use a personal account.

For extended automated usage, such as an integration with another software, it is recommended to create and use dedicated bot users. Using dedicated bot users has several benefits:

The minimum required permissions can be granted to the bot, limiting the impact if the API key gets compromised.

The password can be set empty to disable login/password authentication, limiting the likelihood of the account getting compromised.

The Access Log fields use the bot account. No user is impersonalized.

Depending on the deployment, the Host and/or X-Odoo-Database request headers might be required. The Host header is required by HTTP/1.1 and is needed on servers where Odoo is installed next to other web applications, so that a web-server/reverse-proxy is able to route the request to the Odoo server. The X-Odoo-Database header is required when a single Odoo server hosts multiple databases and the dbfilter wasn’t configured to use the Host header.

Most HTTP client libraries automatically set the Host header using the connection URL.

All calls to the JSON-2 endpoint run in their own SQL transaction. The transaction is committed in case of success and is discarded in case of error. Using the JSON-2 API, it is not possible to chain multiple calls inside a single transaction. It means that one must be cautious when making multiple consecutive calls, as the database might be modified by other concurrent transactions. This is especially dangerous when performing operations related to reservations, payments, and such.

The solution is to always call a single method that performs all the related operations in a single transaction. This way, the data is guaranteed to stay consistent: either everything is done (success, commit), or nothing is done (error, rollback).

In the ORM, the search_read method is an example of a single method that performs multiple operations (search then read) in a single transaction. If a concurrent request removes one of the records search retrieves, then there is a risk that subsequent calls to read fail for a missing record error. Such a problem cannot occur in search_read, as the system guarantees proper isolation between transactions.

In business models, those methods are often prefixed by action_, such as sale.order’s action_confirm method, which verifies that a sales order is valid before confirming it.

When no method exists for a set of related operations, a new one can be created in a dedicated module.

Tutorial to create a module

PostgreSQL - Transaction Isolation - Repeatable Read

The following examples showcase how to execute two of the common ORM methods on a dummy database mycompany hosted on the dummy website https://mycompany.example.com. Its dynamic documentation would be available at https://mycompany.example.com/doc.

The above example is equivalent to running:

Then, in a new transaction:

Both the XML-RPC and JSON-RPC APIs at endpoints /xmlrpc, /xmlrpc/2 and /jsonrpc are scheduled for removal in Odoo 20 (fall 2026). Both RPC APIs expose the three same services: common, db (database) and object. All three services are deprecated.

The other controllers @route(type='jsonrpc') (known until Odoo 18 as type='json') are not subject to this deprecation notice.

The common service defines 3 fonctions:

login(db, login, password)

authenticate(db, login, password, user_agent_env)

The version function is replaced by the /web/version endpoint.

The two login and authenticate functions return the user ID corresponding to the user after a successful login. The user ID and password are necessary for subsequent RPC calls to the object service. The JSON-2 API uses a different authentication scheme where neither the user ID nor the password are used. It is still possible to retrieve the user’s own ID by sending a JSON-2 request to res.users/context_get with no ID (the current user is extracted from the API key).

Database Manager Security

The db service defines 13 fonctions:

create_database(master_pwd, db_name, demo, lang, user_password, login, country_code, phone)

duplicate_database(master_pwd, db_original_name, db_name, neutralize_database)

drop(master_pwd, db_name)

dump(master_pwd, db_name, format)

restore(master_pwd, db_name, data, copy)

change_admin_password(master_pwd, new_password)

rename(master_pwd, old_name, new_name)

migrate_databases(master_pwd, databases)

list_countries(master_pwd)

Many of those function are accessible via the /web/database controllers. Those controllers work hand-in-hand with the HTML form at /web/database/manager and are accessible via HTTP.

The following controllers use the verb POST and content-type application/x-www-form-urlencoded.

/web/database/create takes inputs master_pwd, name, login, password, demo, lang, and phone.

/web/database/duplicate takes inputs master_pwd, name, new_name, and neutralize_database (not neutralized by default).

/web/database/drop takes inputs master_pwd and name.

/web/database/backup takes inputs master_pwd, name, and backup_format (zip by default), and returns the backup in the http response.

/web/database/change_password takes inputs master_pwd and master_pwd_new.

The following controller uses the verb POST and content-type multipart/form-data.

/web/database/restore takes inputs master_pwd, name, copy (not copied by default) and neutralize (not neutralized by default), it takes a file input backup_file.

The following controller uses the verb POST and content-type application/json-rpc.

/web/database/list takes an empty JSON object as input, and returns the database list under the JSON response’s result entry.

The remaining function are: server_version, which exists under /web/version, list_lang, and list_countries, which exist via JSON-2 on the res.lang and res.country models, and migrate_databases, which as non-programmable API at the moment.

The object service defines 2 fonctions:

execute(db, uid, passwd, model, method, *args)

execute_kw(db, uid, passwd, model, method, args, kw={})

They both give for access to all public model methods, including the generic ORM ones.

Both functions are stateless. It means that the database, user ID and user password are to be provided for each call. The model, method and arguments must be provided, too. The execute function takes as many extra positional arguments as necessary. The execute_kw function takes an args list of positional arguments and an optional kw dict of keyword arguments.

The records IDs are extracted from the first args. When the called method is decorated with @api.model, no record ID is extracted, and args is left as-is. It is only possible to give a context with execute_kw, as it is extracted from the keyword argument named context.

To run the following:

Using XML-RPC (JSON-RPC would be similar):

The JSON-2 API replaces the object service with a few differences. The database must only be provided (via the X-Odoo-Database HTTP header) on systems where there are multiple databases available for a same domain. The login/password authentication scheme is replaced by an API key (via the Authorization: bearer HTTP header). The model and method are placed in the URL. The request body is a JSON object with all the methods arguments, plus ids and context. All the arguments are named; there is no way in JSON-2 to call a function with positional arguments.

**Examples:**

Example 1 (json):
```json
POST /json/2/res.partner/search_read HTTP/1.1
Host: mycompany.example.com
X-Odoo-Database: mycompany
Authorization: bearer 6578616d706c65206a736f6e20617069206b6579
Content-Type: application/json; charset=utf-8
User-Agent: mysoftware python-requests/2.25.1

{
    "context": {
        "lang": "en_US"
    },
    "domain": [
        ["name", "ilike", "%deco%"],
        ["is_company", "=", true]
    ],
    "fields": ["name"]
}
```

Example 2 (json):
```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

[
   {"id": 25, "name": "Deco Addict"}
]
```

Example 3 (json):
```json
HTTP/1.1 401 Unauthorized
Content-Type: application/json; charset=utf-8

{
  "name": "werkzeug.exceptions.Unauthorized",
  "message": "Invalid apikey",
  "arguments": ["Invalid apikey", 401],
  "context": {},
  "debug": "Traceback (most recent call last):\n  File \"/opt/Odoo/community/odoo/http.py\", line 2212, in _transactioning\n    return service_model.retrying(func, env=self.env)\n  File \"/opt/Odoo/community/odoo/service/model.py\", line 176, in retrying\n    result = func()\n  File \"/opt/Odoo/community/odoo/http.py\", line 2177, in _serve_ir_http\n    self.registry['ir.http']._authenticate(rule.endpoint)\n  File \"/opt/Odoo/community/odoo/addons/base/models/ir_http.py\", line 274, in _authenticate\n    cls._authenticate_explicit(auth)\n  File \"/opt/Odoo/community/odoo/addons/base/models/ir_http.py\", line 283, in _authenticate_explicit\n    getattr(cls, f'_auth_method_{auth}')()\n  File \"/opt/Odoo/community/odoo/addons/base/models/ir_http.py\", line 240, in _auth_method_bearer\n    raise werkzeug.exceptions.Unauthorized(\nwerkzeug.exceptions.Unauthorized: 401 Unauthorized: Invalid apikey\n"
}
```

Example 4 (python):
```python
Traceback (most recent call last):
  File "/opt/Odoo/community/odoo/http.py", line 2212, in _transactioning
    return service_model.retrying(func, env=self.env)
  File "/opt/Odoo/community/odoo/service/model.py", line 176, in retrying
    result = func()
  File "/opt/Odoo/community/odoo/http.py", line 2177, in _serve_ir_http
    self.registry['ir.http']._authenticate(rule.endpoint)
  File "/opt/Odoo/community/odoo/addons/base/models/ir_http.py", line 274, in _authenticate
    cls._authenticate_explicit(auth)
  File "/opt/Odoo/community/odoo/addons/base/models/ir_http.py", line 283, in _authenticate_explicit
    getattr(cls, f'_auth_method_{auth}')()
  File "/opt/Odoo/community/odoo/addons/base/models/ir_http.py", line 240, in _auth_method_bearer
    raise werkzeug.exceptions.Unauthorized(
werkzeug.exceptions.Unauthorized: 401 Unauthorized: Invalid apikey
```

---

## Property fields¶

**URL:** https://www.odoo.com/documentation/19.0/applications/essentials/property_fields.html

**Contents:**
- Property fields¶
- Add property fields¶
- Properties across apps¶

Property fields, or properties, enable the customization of a form view by adding various field types. These fields allow information storage and management by adding values.

Property vs. regular fields

Properties act as pseudo-fields; they behave like regular fields but are not saved as columns in the database. They also rely on a defined parent record.

Adding a property to a task inserts a field in all tasks within the same project while other projects’ tasks remain unaffected.

To add a first property field to a form view, click the (Actions) icon, then select Edit Properties.

In the popover, enter the property’s Label, choose a Field Type, and then configure the field based on the selected type:

Short text on a single line

Enter a Default Value if desired.

Full text on multiple lines

Enter a Default Value if desired.

Enter a Default Value if desired.

Checked or unchecked status

Choose the Default State.

Integer numbers (positive, negative, or zero, without a decimal)

Enter a Default Value if desired.

Decimal numbers (positive, negative, or zero, with a decimal)

Enter a Default Value if desired.

Selection of a (cost) currency

Enter a Default Value if desired.

Selection of a date on a calendar

Select a Default Value if desired.

Selection of a date on a calendar and a time on a clock

Select a Default Value if desired.

Selection of a value from a group of predefined values

Add a selectable option by clicking Add a Value and entering the Option Name.

If desired, set an option as default by clicking the (Select Default) button.

Reorder the options by dragging and dropping them using the (drag handle) button.

Delete an option by clicking the (Remove Property) button.

Selection of multiple values in the form of tags

Enter a Tag name and press Enter to save it.

Change a tag’s color by clicking it and selecting another one.

Selection of a single record from another model

Enter the Model name. Configure its Domain to filter records if needed.

Select a Default Value if desired.

Selection of multiple records from another model

Enter the Model name. Configure its Domain to filter records if needed.

Select a Default Value if desired.

Group several properties under a foldable label

Enter a Suffix to add a contextual description after a field’s value. For example, to indicate the cost per kilometer, use the Monetary field and name it Cost, then enter per km as the Suffix field. The property then displays the following: Cost [added value] per km.

Enable Display in Cards to select whether to display the property in the Kanban, List, or Calendar views’ cards for every field.

Enable AI to add AI fields to the property. Write a Prompt and/or type /field to insert dynamic values.

To add another property, click Add a Property at the bottom of the form.

To edit an existing property, hover the cursor over the property:

Click the (pencil) button to open a popover and modify the property. In the popover, click the (up) or (down) chevron to move a property upwards or downwards.

Click Delete, then Delete to permanently remove it.

Use the (drag handle) icon to drag and drop the property to reorder or regroup.

Click outside the popover to save the added property.

Property fields can be defined in the form view of multiple models. Once set, the property is shared by all records that are linked to the same parent.

Asset/Revenue Recognition

Maintenance Equipment

Project / Field Service

The following models do not depend on any parent and apply to all records:

Contact Form in the Contacts app

Mailing List Contacts in the Email Marketing app

---

## Actions¶

**URL:** https://www.odoo.com/documentation/19.0/developer/reference/backend/actions.html

**Contents:**
- Actions¶
- Bindings¶
- Window Actions (ir.actions.act_window)¶
- URL Actions (ir.actions.act_url)¶
- Server Actions (ir.actions.server)¶
  - State fields¶
  - Evaluation context¶
- Report Actions (ir.actions.report)¶
- Client Actions (ir.actions.client)¶
- Scheduled Actions (ir.cron)¶

Actions define the behavior of the system in response to user actions: login, action button, selection of an invoice, …

Actions can be stored in the database or returned directly as dictionaries in e.g. button methods. All actions share two mandatory attributes:

the category of the current action, determines which fields may be used and how the action is interpreted

short user-readable description of the action, may be displayed in the client’s interface

A client can get actions in 4 forms:

if any action dialog is currently open, close it

if a client action matches, interpret as a client action’s tag, otherwise treat as a number

read the corresponding action record from the database, may be a database identifier or an external id

treat as a client action descriptor and execute

Aside from their two mandatory attributes, all actions also share optional attributes used to present an action in an arbitrary model’s contextual menu:

specifies which model the action is bound to

For Server Actions, use model_id.

specifies the type of binding, which is mostly which contextual menu the action will appear under

Specifies that the action will appear in the Action contextual menu of the bound model.

Specifies that the action will appear in the Print contextual menu of the bound model.

a comma-separated list of view types for which the action appears in the contextual menu, mostly “list” and / or “form”. Defaults to list,form (both list and form )

The most common action type, used to present visualisations of a model through views: a window action defines a set of view types (and possibly specific views) for a model (and possibly specific record of the model).

model to present views for

a list of (view_id, view_type) pairs. The second element of each pair is the category of the view (list, form, graph, …) and the first is an optional database id (or False). If no id is provided, the client should fetch the default view of the specified type for the requested model (this is automatically done by fields_view_get()). The first type of the list is the default view type and will be open by default when the action is executed. Each view type should be present at most once in the list

if the default view is form, specifies the record to load (otherwise a new record should be created)

(id, name) pair, id is the database identifier of a specific search view to load for the action. Defaults to fetching the default search view for the model

whether the views should be open in the main content area (current), in full screen mode (fullscreen) or in a dialog/popup (new). Use main instead of current to clear the breadcrumbs. Defaults to current.

additional context data to pass to the views

filtering domain to implicitly add to all view search queries

number of records to display in lists by default. Defaults to 80 in the web client

For instance, to open customers (partner with the customer flag set) with list and form views:

Or to open the form view of a specific product (obtained separately) in a new dialog:

In-database window actions have a few different fields which should be ignored by clients, mostly to use in composing the views list:

comma-separated list of view types as a string (/!\ No spaces /!\). All of these types will be present in the generated views list (with at least a False view_id)

M2M1 to view objects, defines the initial content of views

Act_window views can also be defined cleanly through ir.actions.act_window.view.

If you plan to allow multiple views for your model, prefer using ir.actions.act_window.view instead of the action view_ids

specific view added to the views list in case its type is part of the view_mode list and not already filled by one of the views in view_ids

These are mostly used when defining actions from Data Files:

will use the “my_specific_view” view even if that’s not the default view for the model.

The server-side composition of the views sequence is the following:

get each (id, type) from view_ids (ordered by sequence)

if view_id is defined and its type isn’t already filled, append its (id, type)

for each unfilled type in view_mode, append (False, type)

technically not an M2M: adds a sequence field and may be composed of just a view type, without a view id.

Allow opening a URL (website/web page) via an Odoo action. Can be customized via two fields:

the address to open when activating the action

the available values are :

new: opens the URL in a new window/page

self: opens the URL in the current window/page (replaces the actual content)

download: redirects to a download URL

This will replace the current content section by the Odoo home page.

Server actions model. Server action work on a base model and offer various type of actions that can be executed automatically, for example using base action rules, of manually, by adding the action in the ‘More’ contextual menu.

Since Odoo 8.0 a button ‘Create Menu Action’ button is available on the action form view. It creates an entry in the More menu of the base model. This allows to create server actions and run them in mass mode easily through the interface.

The available actions are :

‘Execute Python Code’: a block of python code that will be executed

‘Create a new Record’: create a new record with new values

‘Write on a Record’: update the values of a record

‘Execute several actions’: define an action that triggers several other server actions

Allow triggering complex server code from any valid action location. Only two fields are relevant to clients:

the in-database identifier of the server action to run

context data to use when running the server action

In-database records are significantly richer and can perform a number of specific or generic actions based on their state. Some fields (and corresponding behaviors) are shared between states:

Odoo model linked to the action.

code: Executes python code given through the code argument.

object_create: Creates a new record of model crud_model_id following fields_lines specifications.

object_write: Updates the current record(s) following fields_lines specifications

multi: Executes several actions given through the child_ids argument.

Depending on its state, the behavior is defined through different fields. The concerned state is given after each field.

Specify a piece of Python code to execute when the action is called

The code segment can define a variable called action, which will be returned to the client as the next action to execute:

will ask the client to open a form for the record if it fulfills some condition

model in which to create a new record

many2one to ir.model.fields, specifies the current record’s m2o field on which the newly created record should be set (models should match)

fields to override when creating or copying the record. One2many with the fields:

ir.model.fields to set in the concerned model (crud_model_id for creates, model_id for updates)

value for the field, interpreted via type

If value, the value field is interpreted as a literal value (possibly converted), if equation the value field is interpreted as a Python expression and evaluated

Specify the multiple sub-actions (ir.actions.server) to enact in state multi. If sub-actions themselves return actions, the last one will be returned to the client as the multi’s own next action

A number of keys are available in the evaluation context of or surrounding server actions:

model model object linked to the action via model_id

record/records record/recorset on which the action is triggered, can be void.

datetime, dateutil, time, timezone corresponding Python modules

log: log(message, level='info') logging function to record debug information in ir.logging table

Warning constructor for the Warning exception

Triggers the printing of a report.

If you define your report through a <record> instead of a <report> tag and want the action to show up in the Print menu of the model’s views, you will also need to specify binding_model_id from Bindings. It’s not necessary to set binding_type to report, since ir.actions.report will implicitly default to that.

used as the file name if print_report_name is not specified. Otherwise, only useful as a mnemonic/description of the report when looking for one in a list of some sort

the model your report will be about

either qweb-pdf for PDF reports or qweb-html for HTML

the name (external id) of the qweb template used to render the report

python expression defining the name of the report.

Many2many field to the groups allowed to view/use the current report

if set to True, the action will not be displayed on a form view.

Many2one field to the paper format you wish to use for this report (if not specified, the company format will be used)

if set to True, the report is only generated once the first time it is requested, and re-printed from the stored report afterwards instead of being re-generated every time.

Can be used for reports which must only be generated once (e.g. for legal reasons)

python expression that defines the name of the report; the record is accessible as the variable object

Triggers an action implemented entirely in the client.

the client-side identifier of the action, an arbitrary string which the client should know how to react to

a Python dictionary of additional data to send to the client, alongside the client action tag

whether the client action should be open in the main content area (current), in full screen mode (fullscreen) or in a dialog/popup (new). Use main instead of current to clear the breadcrumbs. Defaults to current.

tells the client to start the Point of Sale interface, the server has no idea how the POS interface works.

Tutorial: Client Actions

Actions triggered automatically on a predefined frequency.

Name of the scheduled action (Mainly used in log display)

Number of interval_type uom between two executions of the action

Unit of measure of frequency interval (minutes, hours, days, weeks, months)

Model on which this action will be called

Code content of the action. Can be a simple call to the model’s method :

Next planned execution date of this action (date/time format)

Priority of the action when executing multiple actions at the same time

When running a scheduled action, it’s recommended that you try to batch the progress in order to avoid blocking a worker for a long period of time and possibly run into timeout exceptions. Therefore, you should split the processing so that each call makes progress on some of the work to be done.

When writing such a function, you should focus on processing a single batch. A batch should process one or many records and should generally take no more than a few seconds.

Work is committed by the framework after each batch. The framework will call the function as many times as necessary to process the remaining work. Do not reschedule yourself the job.

Commit and log progress for the batch from a cron function.

The number of items processed is added to the current done count. If you don’t specify a remaining count, the number of items processed is subtracted from the existing remaining count.

If called from outside the cron job, the progress function call will just commit.

processed – number of processed items in this step

remaining – set the remaining count to the given count

deactivate – deactivate the cron after running it

remaining time (seconds) for the cron run

In some cases, you may want to share resources between multiple batches or manage the loop yourself to handle exceptions. In this case, you should inform the scheduler of the progress of your work by calling IrCron._commit_progress() and checking the result. The progress function returns the number of seconds remaining for the call; if it is 0, you must return as soon as possible.

The following is an example of how to commit after each record that is processed, while keeping the connection open.

You should not call cron functions directly. There are two ways to run functions:

Run the CRON job in the current (HTTP) thread.

The job is still ran as it would be by the scheduler: a new cursor is used for the execution of the job.

UserError – when the job is already running

Schedule a cron job to be executed soon independently of its nextcall field value.

By default, the cron is scheduled to be executed the next time the cron worker wakes up, but the optional at argument may be given to delay the execution later, with a precision down to 1 minute.

The method may be called with a datetime or an iterable of datetime. The actual implementation is in _trigger_list(), which is the recommended method for overrides.

at – When to execute the cron, at one or several moments in time instead of as soon as possible.

the created triggers records

Testing of a cron function should be done by calling IrCron.method_direct_trigger() in the registry test mode.

To avoid a fair usage of resources among scheduled actions, some security measures ensure the correct functioning of your scheduled actions.

If a scheduled action encounters an error or a timeout three consecutive times, it will skip its current execution and be considered as failed.

If a scheduled action fails its execution five consecutive times over a period of at least seven days, it will be deactivated and will notify the DB admin.

A hard-limit exists for the cron execution at the database level after which the process executing cron jobs is killed.

**Examples:**

Example 1 (json):
```json
{
    "type": "ir.actions.act_window",
    "res_model": "res.partner",
    "views": [[False, "list"], [False, "form"]],
    "domain": [["customer", "=", true]],
}
```

Example 2 (json):
```json
{
    "type": "ir.actions.act_window",
    "res_model": "product.product",
    "views": [[False, "form"]],
    "res_id": a_product_id,
    "target": "new",
}
```

Example 3 (jsx):
```jsx
<record model="ir.actions.act_window.view" id="test_action_tree">
   <field name="sequence" eval="1"/>
   <field name="view_mode">list</field>
   <field name="view_id" ref="view_test_tree"/>
   <field name="act_window_id" ref="test_action"/>
</record>
```

Example 4 (jsx):
```jsx
<record model="ir.actions.act_window" id="test_action">
    <field name="name">A Test Action</field>
    <field name="res_model">some.model</field>
    <field name="view_mode">graph</field>
    <field name="view_id" ref="my_specific_view"/>
</record>
```

---

## Create leads (from email or manually)¶

**URL:** https://www.odoo.com/documentation/19.0/applications/sales/crm/acquire_leads/email_manual.html

**Contents:**
- Create leads (from email or manually)¶
- Configure email aliases¶
  - Leads created from email¶
- Manually create leads¶
  - Manually create opportunities¶

Leads can be added to the CRM app from custom email aliases, and by manually creating new records. This is in addition to the leads and opportunities created in the app through the website contact form.

First, ensure the Leads feature is enabled in the database by navigating to CRM app ‣ Configuration ‣ Settings. Tick the Leads checkbox, then click Save.

Each sales team has the option to create and utilize their own unique email alias. When messages are sent to this address, a lead (or opportunity), is created with the information from the message.

To create or update a sales teams’ email alias, navigate to CRM app ‣ Configuration ‣ Sales Teams. Click on a team from the list to open the team’s details page.

In the Email Alias field, enter a name for the email alias, or edit the existing name. In the Accept Emails From field, use the drop-down menu to choose who is allowed to send messages to this email alias:

Everyone: messages are accepted from any email address.

Authenticated Partners: only accepts messages from email addresses associated with a a partner (contact or customer) record.

Followers only: only accepts messages from those who are following a record related to the team, such as a lead or opportunity. Messages are also accepted from team members.

Authenticated Employees: only accepts messages from email addresses that are connected to a record in the Employees app.

Leads created from email alias messages can be viewed by navigating to CRM app ‣ Leads. Click a lead from the list to open it, and view the details.

The email received by the alias is added to the chatter thread for the lead. The subject line of the message is added to the title field, and the Email field is updated with the contact’s email address.

If the leads feature is not enabled on the database, messages to the email alias are added to the database as opportunities.

Communication in Odoo by email

Leads can be added directly to the CRM app by manually creating a new record. Navigate to CRM app ‣ Leads to view a list of existing leads.

Leads can also be added via the Generate Leads button.

At the top-left of the list, click New to open a blank Leads form.

In the first field of the new form, enter a title for the new lead. Next, enter a Contact Name, and a Company Name.

If a lead is converted to an opportunity, the Company Name field is used to either link this opportunity to an existing customer, or to create a new customer.

To manually create an opportunity, navigate to CRM app ‣ Sales ‣ My Pipeline. At the top-left of the page, click New to create a new opportunity Kanban card. In the Organization/Contact field, enter the name of the company the opportunity is for.

Choose a name, and enter it in the Opportunity field. This is a required field. When manually creating an opportunity, it is helpful to add a name that relates to the details of the opportunity.

In the example below, the opportunity is named 5 VP Chairs. This identifies the product the customer is interested in, as well as the potential number of products.

Enter the contact information for the opportunity in the Email and Phone fields.

In the Expected Revenue field, enter an estimated value for the opportunity.

The information in the Expected Revenue and priority fields can be used to track performance for individual salespeople, and on a team basis. See Expected revenue report and Assign leads with predictive lead scoring for more information.

Then, use the (star) icons to assign a priority.

Assigning a priority changes the order of leads in Kanban view, with higher priority leads displayed first.

Once all the necessary information has been entered, click Add.

---

## Approval rules¶

**URL:** https://www.odoo.com/documentation/19.0/applications/studio/approval_rules.html

**Contents:**
- Approval rules¶
- Configuration¶
- Use¶

Approval rules are used to automate approval processes for actions. They allow you to define the criteria for when an approval is required before an action can be performed using a button.

To add approval rules with Studio, proceed as follows:

Open Studio and switch to the required view.

Select the button to which the rule should be applied.

Click Add an approval step in the Properties tab.

Specify which users are responsible for approving the action by using one of the following fields or both:

Approvers to specify one or several users;

Approver Group to specify one user group.

An activity is created for all users set as Approvers when their approval is requested.

(optional) Select the Users to Notify via an internal note when the action is approved or rejected.

(optional) Add a Description to be displayed on the button.

You can specify under which condition(s) an approval step should be applied by clicking the (filter) icon next to the Approvers field.

To add another approval step, click Add an approval step. When there are multiple steps, you can:

Enable Exclusive Approval on any step so that a user who approves a step cannot approve another step for the same record.

Change the Approval Order of the steps by selecting a number, 1 being the first step, 2 the second step, and so on. A user responsible for a higher step can approve/reject previous step(s) unless Exclusive Approval is selected.

Click the (trash) icon next to the Approvers field to remove an approval step.

You can create user groups specifically for approvals.

Once an approval rule has been defined for a button, a user avatar icon is displayed next to the button’s label for each approval step. Clicking an icon reveals the step(s).

If an unauthorized user clicks the button, an error message is displayed and an activity is created for the users specified in the Approvers field, if any.

Authorized users can:

Perform the action directly by clicking the button if it is the last/only approval step.

Approve the action and let another user perform it - or move it to the next approval step - by clicking the user avatar icon next to the button’s label, then clicking the (approve).

Reject the action by clicking the user avatar icon next to the button’s label and then the (reject) button.

(only for users selected under the Approvers field) Delegate their approval rights to one or several users for all records by:

Clicking the (kanban view) icon and then Delegate.

Selecting one or several Approvers, Until when they will have approval rights (forever if left empty), and, optionally, the user(s) who should be notified via an internal note using the Notify to field.

A user who approves/rejects an action can revoke their decision by clicking the user avatar icon next to the button’s label and then the (revoke) button. They can also revoke the decision of other users for steps with a lower Approval Order unless Exclusive Approval is enabled.

Approvals are tracked in the record’s chatter. An approval entry is also created every time a Studio approval-related action is performed. To access approval entries, activate the developer mode and go to Settings ‣ Technical ‣ Studio Approval Entries.

---

## Knowledge¶

**URL:** https://www.odoo.com/documentation/19.0/applications/productivity/knowledge.html

**Contents:**
- Knowledge¶
- Article creation¶
  - From scratch¶
  - From a template¶
- Article editing¶
  - Text editor toolbar¶
  - Commands¶
    - Article items¶
  - Cover pictures¶
  - Title emoji¶

Odoo Knowledge is a multipurpose productivity app that allows internal users to enrich their business knowledge base by providing information gathered individually or collaboratively.

The pages on which they gather content are called articles. They are mainly composed of a title and a body. The latter is an HTML field containing text, images, links, records from other models, templates, etc.

Knowledge product page

Knowledge articles can be created from scratch or a pre-configured template. When an article is created under another, the original one is the parent article, while the new one is called a child or nested article, indicating its subordinate position. This structure helps organize content by establishing clear relationships between related articles.

To create a nested article, hover over an article in the sidebar tree and click the (plus) icon.

To create an article from scratch, click New Article in the top-left corner or hover over the Private or Workspace category in the sidebar tree, then click the (plus) icon. Start typing text or select one of the suggested options:

Load a Template: Select a preconfigured template and click Load Template.

Build an Item Kanban: Create items to visualize and manage them in a Kanban view.

Build an Item List: Create a structured list of items to centralize them in a single article.

Build an Item Calendar: Create a calendar view to manage and track items by date.

Generate an Article with AI: Generate content based on a prompt.

After writing the header, click or hover over Untitled in the top bar to automatically name the article after the header. This action does not apply if the article is already titled.

To create an article from a template, follow these steps:

Click Browse Templates at the bottom of the sidebar tree.

Select a preferred template.

To edit an article, select it in the sidebar tree, then edit its content and format it using the text editor toolbar, typing powerbox commands, and adding a cover picture with a title emoji.

To enlarge or reduce the article’s width, click the (More actions) icon, then toggle the Full Width on or off.

To edit a word, sentence, or paragraph, select or double-click it to display the text editor toolbar and apply the desired formatting options.

Click Comment to add a comment to the selected text.

Type / to open the powerbox and use a command. The following commands are exclusive to the Knowledge app:

Show nested articles: Display the child pages of the parent article.

Insert a Kanban view and create article items.

Insert a Card view and create article items.

Insert a List view and create article items.

Insert a Calendar view and create article items.

Add a clipboard section to store content and reuse it in other apps.

Hide the text inside a foldable section.

Insert a shortcut to an article.

Article items are active building blocks within an article, allowing the addition, management, and viewing of various organized content and data.

Article items within a parent article can contain properties, which are shared data fields from the parent, ensuring consistent information across related items and articles.

To add a cover picture, click the (More actions) icon, then Add Cover.

To manage the cover picture, hover the mouse over it and select the preferred option:

Replace Cover: Search from the Unsplash database library, click Add URL and paste the image address, or click Upload an image to upload a file into the image library.

Reposition: Adjust the picture, then click Save Position or Cancel.

After clicking Add Cover, a cover picture is automatically attributed to the article based on the title.

A removed cover picture can be retrieved in the database library. To delete it, hover the mouse over it and click the (trash) icon.

To add a title emoji to the article’s name and header:

Click the (More actions) icon, then Add Icon to generate a random emoji. Click the emoji to select a different one.

Alternatively, click the (page) icon next to the article’s name in the sidebar or the top bar and select the preferred emoji.

To insert a view or a view link into an article, follow these steps:

Go to the desired app and select the preferred view.

Click the (cog) icon, then select Knowledge ‣ Insert view in article or Insert link in article.

Choose the article to insert the view or link to.

Once the view or link is inserted:

Users without access to the view cannot see it in Knowledge, even if they can access the article.

Clicking the inserted link opens a pop-up with the view’s name next to the (Copy Link), (Edit Link), and (Remove Link) icons. Click the name inside the pop-up to open the linked view.

Knowledge allows for managing articles, which consists of structuring, sharing, removing, and retrieving them.

Click the (More actions) icon and select one of the following actions for basic article management:

Move To: Select the article to move under a category or another article, then click Move Article.

Lock Content: Lock the article to stop edits. Click Unlock to edit again.

Create a Copy: Copy the article under the Private section.

Open Version History: Restore a previous version of the article.

Download PDF: Open the browser’s print function.

Add to Templates: Add the article to the list of templates.

Send to Trash: Move the article to the trash.

The following actions only apply to nested articles and article items:

Convert into Article Item: Convert the nested article into an article item.

Convert into Article: Convert the article item into a nested article.

Move an article directly from the sidebar tree by dragging and dropping it under another article or category.

Click the (search) icon in the top-left corner or press CTRL / CMD + K to open the command palette, then type ? to search for visible articles or $ for hidden articles. Alternatively, hover over the Workspace category and click the (eye) icon to find hidden articles.

The sidebar structure follows a hierarchy with parent and nested articles organized within the following categories:

The Favorites category displays all articles marked as favorites.

The Workspace category displays articles accessible to all internal users.

The Shared category displays articles shared with specific users.

The Private category displays personal articles.

To mark an article as a favorite and display the Favorites category, click the (star) icon in the top-right menu.

Nested articles inherit their parent’s access rights, and properties are applied to a group of nested articles under the same parent.

Sharing an article involves configuring access rights, inviting users, providing online access, and determining its visibility in the sidebar tree.

Articles listed under a category in the sidebar tree are visible. Articles that certain users must search for through the command palette due to restricted access rights are hidden.

To copy a specific section of an article, hover over the header (H1, H2, and H3), and click the (link) icon. Clicking the shared link leads to the selected section of the article.

Click Share in the top-right menu to configure access rights.

Allow all internal users to edit the article.

Allow all internal users to read the article only.

Allow only members to access the article from the sidebar tree or by searching for it in the command palette.

The article is visible in the sidebar tree to all internal users.

The article is only visible in the sidebar tree to members, while other users can find it using the hidden article search by pressing CTRL / CMD + K and typing $.

The Default Access Rights apply to all internal users except invited users; specific access rights override default access rights.

Selecting Can edit or Can read in the Default Access Rights moves the article to the Workspace category while selecting No access moves it to the Private category if it is not shared with an invited user.

The Visibility setting only applies to Workspace articles.

To grant specific internal or portal users access to a private nested article or to share a Workspace article with a portal user, follow these steps:

Click Share in the top-right menu.

Select the preferred Permission and add users in the Recipients field.

To generate a URL and share an article, click Share and activate the Share to web toggle. Click Copy Link to copy the article’s URL.

If an article contains inserted views, users with the URL do not see them unless they can access the inserted content.

Having the Website app is necessary to share an article’s URL.

Removing an article involves deleting or archiving it.

Select an article in the sidebar tree and click the (More actions) icon, then Send to Trash. Alternatively, drag and drop the article under Drop here to delete this article at the bottom left corner. The article is moved to the trash for 30 days before being permanently deleted.

To permanently delete an article, click Articles in the top-left menu, select an article, and click Actions ‣ Delete ‣ Delete.

To restore a trashed article, click Open the Trash at the bottom of the sidebar tree, select an article, and click Restore. Alternatively, click Articles in the top-left menu. In the search bar, click Filters ‣ Trashed. Click the article, then Restore.

Click Articles, select an article, and click Actions ‣ Archive ‣ Archive.

To restore an archived article, click Articles. In the search bar, click Filters ‣ Archived. Select the article and go to Actions ‣ Unarchive.

Retrieving Knowledge articles consists of accessing them from Odoo apps or restoring previous versions.

Knowledge articles are accessible from the form view of various apps. Click the (Knowledge) icon in the top right corner to open the command palette, then choose one of the following search methods:

Search for an article: start typing the text to execute a semantic search that identifies relevant article information.

Advanced Search: after typing the text in the search bar, click Advanced Search to perform a parametric search with options to filter, group, or save articles.

To retrieve a previous version of an article, select it in the sidebar tree and click the (More actions) icon, then Open Version History. Select a version and click Restore history.

In the version history, the Content tab shows the selected version, while the Comparison tab displays the differences between the article’s previous and current versions.

Properties are custom fields for storing and managing information that members can add to nested articles or article items.

To add a property, click the (More actions) icon, then Add Properties ‣ Add a Property, enter a Label, and select a Field Type.

Click outside the property field window to save a property.

To remove a property, hover over its name, click the (pencil) icon, then click Delete ‣ Delete. Deleting a property is permanent, and deleting all properties removes the property sidebar panel.

Hover over the property name and click the (pencil) icon to edit it or the (drag handle) icon to move it above or below another property.

Tick Display in Cards to show the properties in an article item’s view that is visible from a parent article.

Click the (cogs) icon to hide the property sidebar panel. Exiting and returning to the article causes the panel to reappear.

---

## Create and send quotations¶

**URL:** https://www.odoo.com/documentation/19.0/applications/sales/crm/acquire_leads/send_quotes.html

**Contents:**
- Create and send quotations¶
- Create a new quotation¶
  - Order lines¶
    - Product catalog¶
- Preview and send quotation¶
- Mark an opportunity won or lost¶

Once a qualified lead has been converted into an opportunity, the next step is to create and deliver a quotation. This process can be easily handled through Odoo’s CRM application.

To create a new quotation, open the CRM app, revealing the Pipeline page on the main CRM dashboard.

From here, click on any opportunity to open it. Review the existing information and update any fields, if necessary.

If a quotation has already been created for this opportunity, it can be found by clicking on the Quotations smart button at the top of the top of the form. The number of existing quotations is listed on the smart button, as well.

At the top-left of the form, click the New Quotation button.

The Sales application must be installed for the New Quotation button to appear.

The Customer field is not required on the opportunity form.

However, customer information must be added or linked before a quotation can be sent. If the Customer field is left blank on the opportunity, clicking the New Quotation button opens a pop-up window with the following options:

Create a new customer: creates a new customer record, using any available information provided on the opportunity form.

Link to an existing customer: opens a drop-down field with existing customer names. Select a name to link this new quotation to an existing customer record.

Do not link to a customer: the quotation will not be linked to a customer, and no changes are made to the customer information.

Once this button is clicked, a new quotation form appears. Confirm the information in the top half of the form, and update any missing or incorrect fields:

Customer: the company or contact for whom this quotation was created.

Referrer: if this customer was referred by another customer or contact, select it from the drop-down menu in this field.

Invoice Address: physical address where the invoice should be sent.

Delivery Address: physical address where any products should be delivered.

Quotation Template: if applicable, select a pre-configured quotation template from this field.

Expiration: date when this quotation is no longer valid.

Quotation Date: creation date of draft/sent orders, confirmation date of confirmed orders. Note that this field is only visible if Developer mode (debug mode) is active.

Recurring Plan: if this quotation is for a recurring product or subscription, select the recurring plan configuration to be used.

Pricelist: select a pricelist to be applied to this order.

Payment Terms: select any applicable payment terms for this quotation.

The Expiration field automatically populates based on the creation date of the quotation, and the default validity time frame.

To update the default validity time frame, navigate to Sales app ‣ Configuration ‣ Settings ‣ Quotations & Orders and update the Default Quotation Validity field. To disable automatic expiration, enter 0 in this field.

When the desired changes are complete, click Save.

When using a quotation template, the expiration date is based off of the Quotation Validity field on the template. To alter the validity date computation on a template, go to Sales app ‣ Configuration ‣ Sales Orders ‣ Quotation Templates.

Then, click on a template to open it, and update the number in the Quotation Validity field.

After updating the customer, payment, and deadline information on the new quotation, the Order Lines tab can be updated with the appropriate product information.

To do that, click Add a product in the Order Lines tab.

Next, type the name of an item into the Product field to search through the product catalog. Then, select a product from the drop-down menu, or create a new one by selecting Create or Create and Edit.

After selecting a product, update the Quantity, if necessary. Confirm the information in the remaining fields.

To remove a line from the quotation, click the (trash can) icon.

To organize products into sections click Add a section and type a name for the section. Then, click the (drag) icon to the left of the name and drag to move the section to the appropriate location. Move each product using the same method to finish organizing the quotation order lines.

To quickly add numerous products to the quotation, click the Catalog button to open the product catalog.

All products in the database are listed as cards and can be sorted in the left panel by Product Category and Attributes.

To add a product, click the Add button on the product card. Set the quantity of the item using the (add) or (subtract) buttons, or type the quantity in the number field between the two buttons. To remove an item, click the Remove button on the product card.

Once all product quantities are set, click the Back to Quotation button to return to the quotation. The items selected in the product catalog now appear in the Order Lines tab.

To see a preview of the quotation as the customer will see it, click the Preview button. Doing so opens a preview in the Customer Portal.

After reviewing the customer preview, click Return to edit mode to return to the quotation form in the backend.

When the quotation is ready to deliver to the customer, click the Send by Email button.

Doing so opens a pop-up window with a pre-configured email message. Information from the quotation, including the contact information, total cost, and quotation title are be imported from the quotation.

A PDF of the quotation is added as an attachment to the email.

A pre-loaded template is used to create the email message. To alter the template, click the internal link to the right of the Load template field, located at the bottom of the email pop-up window.

To select a new template, select an option from the Load template drop-down menu.

Proceed to make any necessary changes to the email, then click Send. A copy of the message is added to the Chatter of the of the record.

After a quotation is sent, the originating opportunity’s Quotations smart button updates with a new count. This quotation, and all other quotations can be accessed through this smart button at the top of the opportunity in the CRM app.

Any quotations attached to the opportunity that are confirmed, and have therefore been converted to sales orders, will be deducted from the number listed on the Quotations smart button. Instead, the value of the sales order will appear in the Orders smart button located in the same control panel.

In order to keep the pipeline up to date and accurate, opportunities need to be identified as won or lost once a customer has responded to a quotation.

To mark an opportunity as won or lost, return to the opportunity using the breadcrumbs at the top-left of the quotation form. Or navigate to CRM app ‣ Sales ‣ My Pipeline and click on the correct opportunity to open it.

At the top-left of the form, click on either the Won or Lost button.

If the opportunity is marked won, a green Won banner is added to the record, and it is moved to the Won stage.

Marking an opportunity as lost, via the Lost button opens a Mark Lost pop-up window, where a Lost Reason can be entered.

From the Lost Reason drop-down field, choose an existing lost reason. If no applicable reason is available, create a new one by entering it into the Lost Reason field, and clicking Create.

It’s best practice to try and use pre-configured Lost Reason values as much as possible or to limit the creation of new values only to sales team leads. Using consistent values for this parameter will make pipeline analysis easier and more accurate when filtering for the Lost Reason parameter.

To set up new values for this field, navigate to CRM ‣ Configuration ‣ Lost Reasons, and click both New and Save for each new entry added to the list.

Additional notes and comments can be added in the Closing Note field.

When all the desired information has been entered in the Mark Lost pop-up window, click Mark as Lost.

Upon clicking Mark as Lost, the pop-up window disappears, and Odoo returns to the opportunity form, where a new red Lost banner is now present in the upper-right corner of the opportunity.

Once an opportunity is marked as lost, it is no longer considered active, and it is removed from the pipeline.

In order to view a lost opportunity from the pipeline, click the down arrow icon to the right of the search bar, and select either Lost or Archived from the drop-down menu that appears.

While opportunities that have been marked as lost are considered Archived, be advised that, in order for an opportunity to be included as lost in reporting, it must be specifically marked as lost, not Archived.

---

## Forms¶

**URL:** https://www.odoo.com/documentation/19.0/developer/howtos/website_themes/forms.html

**Contents:**
- Forms¶
- Default form¶
- Actions¶
- Success¶

Forms in Odoo are very powerful. They are directly integrated with other applications and can be used for many different purposes.

In this chapter, you will discover how to:

Add a form in your custom theme.

Change the action of the form.

Stylize the form thanks to Bootstrap variables.

To add a form to a page, copy and paste the code generated by the Website Builder in the page.

It should look something like the following.

There is a data-model_name in the form tag. It enables you to define different actions for your form.

Send an email (this action is used by default).

Create an opportunity.

The default action is Send an E-mail but following the Apps installed on the database, some other options can be found, such as: Apply for a job, create a customer, create a ticket, create an opportunity, etc.

Please, note that some of these actions may need specific required fields in order to be functional. To not forget some requirements, we highly recommend to preset the form snippet with the Website Builder and copy/paste the generated source code.

Define what happens once the form is submitted thanks to the data-success-mode attribute.

Redirect the user to a page defined in the data-success-page.

Show a message (on the same page).

Add a success message directly under the form tag. Always add the d-none class to make sure that the success message is hidden if the form hasn’t been submitted.

**Examples:**

Example 1 (jsx):
```jsx
<form
   action="/website/form/" method="post"
   enctype="multipart/form-data"
   class="o_mark_required"
   data-mark="*" data-pre-fill="true"
   data-success-mode="redirect"
   data-success-page="/contactus-thank-you"
   data-model_name="mail.mail">
     <div class="s_website_form_rows row s_col_no_bgcolor">
          <div class="form-group s_website_form_field col-12 s_website_form_dnone" data-name="Field">
               <!-- Form fields -->
           </div>
     </div>
</form>
```

Example 2 (jsx):
```jsx
<form data-model_name="mail.mail">
```

Example 3 (jsx):
```jsx
<form data-model_name="hr.applicant">
```

Example 4 (jsx):
```jsx
<form data-model_name="res.partner">
```

---

## Bank reconciliation¶

**URL:** https://www.odoo.com/documentation/19.0/applications/finance/accounting/bank/reconciliation.html

**Contents:**
- Bank reconciliation¶
- Bank reconciliation view¶
  - Transactions¶
    - Possible action buttons¶
- Reconcile transactions¶
  - Existing items¶
  - Set account¶
  - Reconciliation models¶
- Edit lines and unreconcile transactions¶
- Netting¶

Bank reconciliation is the process of validating bank transactions. Many of these transactions are matched with counterpart items related to business records such as customer invoices, vendor bills, and payments, while others that may not have a matching counterpart item (such as bank fees) can be written off manually or with reconciliation models. Not only is bank reconciliation compulsory for most businesses, but it also offers several benefits, such as reduced risk of errors in financial reports, detection of fraudulent activities, and improved cash flow management.

Thanks to the default matching rules and customizable bank reconciliation models, Odoo selects the matching items automatically when possible.

Odoo Tutorials: Bank reconciliation

To access a journal’s Bank Matching view, go to the Accounting Dashboard and either:

click the journal name (e.g., Bank) or its Transactions button to display all transactions, including those previously reconciled, or

click the x to reconcile button to display only unreconciled transactions. To include previously reconciled transactions, remove the Not Matched filter from the search bar.

The Bank Matching view is composed of lines for each transaction of the journal with the newest displayed first. Each transaction has a date, a label, a partner (if set), action buttons, and the transaction amount. Each line can be expanded to show additional information and buttons.

Once a transaction is reconciled, the suggested action button(s) is replaced with the counterpart entry/entries it was matched with or the account(s) it was written off to.

Every transaction is linked to a journal entry that debits/credits the journal’s main account and its suspense account until it is fully reconciled. At that point, the suspense account is replaced with the account of the counterpart item or, in the case of manual matching, the selected account.

Duplicate transactions

Up to two suggested action buttons are available as primary buttons, but all available action buttons are displayed when the transaction is expanded. The following action buttons are available depending on the details of the transaction:

Set Partner: Open a search view to add a partner to the transaction.

Set Account: Open a search view to manually select an account to write off the full amount of the transaction with this account. If necessary, edit the line to change the amount.

Receivable: Write off the transaction to the receivable account of the partner.

Sales: Open a list view of sales orders belonging to the transaction’s Partner (or proceed directly to the form view if only one relevant sales order exists). Select the relevant sales order(s) and click Create Invoices, then return to the Bank Matching view and match the invoice(s) using the Reconcile action button.

Payable: Write off the transaction to the payable account of the partner.

Reconcile: Open a search view of existing items from records such as customer invoices, vendor bills, and payments. Select one or multiple items to add counterpart items with the corresponding accounts of those items.

Batches: Open a short list of batch payments. To view all batch payments, click Search More …. Select a batch payment to add a counterpart item for each payment of the batch with the corresponding account of each payment.

Reconciliation models: Each manual reconciliation model that could apply to the transaction is displayed. Click the reconciliation model’s action button to generate the counterpart items defined on the reconciliation model.

To remove the partner from a transaction, click the (close) icon next to the partner’s name.

Click the (chevron down) button next to the possible action buttons of an expanded line to display any of the above action buttons that are hidden due to space limitations, as well as the following:

Upload bills: Upload one or more bills to be digitized. After digitization, the bills are available for matching via the Reconcile action button.

Manage Models: Open the list view of Reconciliation models.

Open Journal Entry: Open the journal entry of this transaction.

Delete Transaction: Delete this transaction.

Uploading bills from the Bank Matching view does not automatically reconcile them with the active transaction.

In-app purchases (IAP)

When possible, Odoo automatically reconciles transactions based on their fields.

If no partner is set on the transaction, the transaction’s Label is compared with the Number, Customer Reference, Bill Reference, and Payment Reference of existing invoices, bills, and payments.

If a partner is set on the transaction, the transaction is instead matched with invoices, bills, and payments of the partner based on the Amount. The following rules are used in a sequential order to identify and apply a match:

Discounted match: for payment terms with discounts for early payments

Tolerance match: within 3% to account for merchant fees, rounding differences, and user errors

Currency match: when the transaction is in a different currency than the invoice, bill, or payment (with a 3% tolerance for exchange rate differences)

Amount in label: if the invoice Amount is found in the transaction’s Label

In addition to using these fixed matching rules, transactions can be matched automatically with the use of reconciliation models. Otherwise, reconcile transactions manually by following these steps:

Expand the desired line among unmatched bank transactions to display all available action buttons.

Define the counterpart. There are several options for defining a counterpart, including matching existing items, manually setting the account, matching with batch payments, and using reconciliation model buttons.

If the resulting entry is not fully balanced, add another existing counterpart item or write it off by setting the account of the remaining amount.

To reconcile transactions with existing items related to records such as customer invoices, vendor bills, and payments, click the Reconcile action button, select the matching journal item(s) in the list, and click Select.

If the Partner is set, this list is automatically filtered to only include items related to that partner.

Use the search bar within the Search: Journal Items to Match window to search for specific journal items.

If a transaction amount is lower than the invoice or bill it is reconciled with, the transaction is fully reconciled, but the difference remains open on the counterpart item. The remaining amount can be left open to be reconciled later or the invoice or bill can be marked as fully paid. To mark the invoice or bill as fully paid, edit the line, click fully paid, and Save. To reverse this, edit the line again, click partial payment, and Save.

If a transaction amount is greater than the invoice or bill it is reconciled with, the transaction is only partially reconciled. The remaining balance can be reconciled as any other transaction amount.

Existing items of draft entries can be matched. Eventual automatic moves (like currency exchange or cash basis moves) are created in draft simultaneously with the reconciliation. Posting the original entry also posts the automatic move.

If no existing item matches the selected transaction, you can still write off the transaction manually: Click Set Account, then choose the appropriate account. To write off only part of the transaction, edit the line to reflect the correct value and reconcile the remaining amount as desired.

If the partner is set, write the amount off to their receivable or payable account directly by clicking the Receivable or Payable action button.

Use reconciliation models to create custom rules that can be applied automatically or manually via custom buttons for operations that are frequently repeated. These custom buttons allow you to quickly reconcile bank transactions manually and can also be combined with other reconciliation models and with counterpart items when reconciling transactions.

An outgoing bank transaction for $103 is partially matched with a vendor bill for $100, leaving $3 of the transaction still unreconciled. Use the Bank Fees reconciliation model to create a new counterpart item for $3 and reconcile it with the remaining $3 of the bank transaction.

To edit a counterpart item, expand the line, click the (pencil) icon, and edit the necessary fields in Edit Line window.

When the counterpart item is an existing journal item, some fields are read-only.

If a transaction is partially matched with a counterpart item, use the link to mark the invoice as fully paid or to switch back to a partial payment.

To unreconcile a transaction, delete all counterpart items associated with the transaction by clicking on the (trash) icon.

Netting (also known as AP/AR offsetting) is the process of balancing incoming debts from and outgoing debts to the same partner. Reconciling the incoming and outgoing debts creates a new journal entry that balances the debts. Two main scenarios exist:

A bank transaction balances (either fully or partially) the incoming and outgoing debts.

No bank transaction balances the incoming and outgoing debts. This situation can occur either when the debts balance each other completely or when the debts remain unbalanced.

When a bank transaction balances (either fully or partially) the incoming and outgoing debts, reconcile the bank transaction from the Bank Matching view like any other existing items:

Click Reconcile on the transaction.

Select all the relevant counterpart items on both the payable and receivable side.

If a balance remains, depending on the details, the following situations are possible:

An invoice, bill, or other item is not fully reconciled, and the remaining balance can be reconciled with other bank transactions.

The bank transaction itself is not fully reconciled, and the remaining balance can be reconciled as in any other situation.

When no bank transaction balances the incoming and outgoing debts, there is nothing to reconcile from the Bank Matching view. However, the debt amount is visible in both the account receivable and the account payable. To balance these debts so that they no longer appear on the partner ledger, follow these steps:

Go to Accounting ‣ Accounting ‣ Reconcile.

Select the journal items that debit or credit the account receivable and account payable and represent the debts to be netted.

If the debts don’t balance each other perfectly, a Write-Off Entry popup window appears, allowing you to decide how to resolve the remaining balance:

Select Allow partials to only partially reconcile the account receivable and account payable and leave the remaining balance open.

Use a reconciliation model button to write off the balance.

Manually choose an Account, and optionally adjust the Tax, Journal, Label, Date, and To Check fields.

The items are then matched, and their balance is removed from the partner ledger, representing that no payment is due for these debts.

The workflow is the same whether there are only two equal debts in the receivable and payable accounts or multiple debts in each account.

---

## Companies¶

**URL:** https://www.odoo.com/documentation/19.0/applications/general/companies.html

**Contents:**
- Companies¶
- Configuration¶
  - Company¶
  - Users¶
  - Document layout¶
- Branches¶
  - Configuration¶
  - Comprehensive or branch-specific view¶
    - User access¶
    - Shared records¶

In Odoo, a company is an individual business entity that operates independently, with its own legal identity, financial records, and specific operational settings.

To set up a company, follow these steps:

Configure the company details.

Manage users and their access rights.

Customize the document layout.

To create a company, open the Settings app, navigate to the Companies section, and click Manage Companies. In the Companies list view, click New and configure the following fields:

Tax ID: tax identification number.

LEI: legal entity identifier.

Company ID: company’s registry number, if different from Tax ID

Upload the company’s logo and Save.

Alternatively, it is possible to create a company by going to Settings ‣ Users & Companies ‣ Companies.

The company’s General information may vary based on the fiscal localization.

After setting up a company, add users and configure their access and access rights.

Users in multi-company environment

Configure the default layout for all company documents.

Branches represent subdivisions within a company, such as regional offices or departments, that operate under a common parent company. They support hierarchical company structures through configurable settings, enabling comprehensive or branch-specific views with flexible access control, entity-specific or shared record visibility, and customizable reporting.

Independent subsidiaries should be created as additional companies, not branches.

Each branch is linked to its parent company but may contain different or specific information, such as its address or logo. A branch can be a parent company of branches at a lower level to create a multi-level architecture.

Clarify the company’s structure and hierarchy before creating companies and branches in Odoo. A company defined as a parent cannot be converted into a branch later, as doing so may result in access rights issues.

Always create the parent company first.

To create a branch, follow these steps in the Settings app:

Navigate to the Companies section, click Manage Companies, or go to Settings ‣ Users & Companies ‣ Companies.

In the Companies list view, open the desired parent company form.

In the Branches tab, click Add a line and fill in the General Information fields in the Create Branches window.

To create branches from a branch and create a multi-level architecture, click Add a line in the new branch’s Branches tab.

Activate the developer mode to set social media accounts and company-specific email system parameters.

Adding a branch to a company enables multi-company functions.

Selecting the parent company automatically links all its branches, while selecting a branch connects to that branch only. To switch between them, use the company selector.

All configurations, except for accounting settings inherited from the parent company, must be set individually per branch. This allows for branch-specific setups such as loyalty programs, price lists, or inventory locations.

Like in a multi-company environment, parent companies and branches support flexible user access control and access rights. User access can be granted or restricted at the parent company level, the branch level, or both. For example, a user can be limited to a specific branch, while an administrator with access to the parent company can manage all associated branches.

In Odoo, some records are, by default, either specific to a single entity or shared across the parent company and all its branches.

When creating a quotation, invoice, or vendor bill, the active company or branch is automatically selected and displayed in the Company field. If the active company is the parent company or one of its branches, then records specifically linked to that entity are accessible only within that entity and will only be visible when the company or branch is selected using the company selector.

In contrast, some records, such as products or contacts, are not tied to any particular entity and are shared by default across the parent company and all its branches. However, they can be restricted to a single entity by setting the appropriate value in the Company field, if needed.

All reports can be generated for the parent company alone or with its branches, based on user access.

---

## Models & manufacturers¶

**URL:** https://www.odoo.com/documentation/19.0/applications/hr/fleet/models.html

**Contents:**
- Models & manufacturers¶
- Manufacturers¶
  - Add a manufacturer¶
- Models¶
  - Add a model¶
  - Information tab¶
    - Model¶
    - Salary¶
    - Engine¶
  - Vendors tab¶

Odoo’s Fleet app categorizes each vehicle by manufacturer and model (e.g. BMW, X2). Before the vehicle can be added to the Odoo database, its manufacturer and a model records must already exist in the database.

Odoo’s Fleet app comes preconfigured with sixty-six commonly used car and bicycle manufacturers in the database, along with their logos. To view the pre-loaded manufacturers, go to Fleet app ‣ Configuration ‣ Manufacturers.

The default filter, With Models, displays only manufacturers that already have vehicle models. Remove the default filter to view all manufacturers.

Manufacturers re listed alphabetically, and each card shows how many specific models are configured for each particular manufacturer.

To add a new manufacturer to the database, click New in the upper-left corner, to open a blank manufacturers form. Type the name of the manufacturer in the Name field, and select an image to upload for the logo.

When adding a vehicle to the fleet, specify the vehicle model to maintain updated records, which keeps track of specific details, like maintenance schedules and parts compatibility.

Unlike manufacturers, models do not come preconfigured in the Fleet app. When a new vehicle model joins the fleet, the model (and, if necessary, the manufacturer) must be added to the database.

To add a new vehicle model, navigate to Fleet app ‣ Configuration ‣ Models. Click New in the upper-left corner, and enter the following information on the new model form.

Depending on the installed localization, some fields or sections may not appear.

Model name: Enter the model name in the field.

Manufacturer: Using the drop-down menu, select the manufacturer. If it is not configured, add the manufacturer

Vehicle Type: Using the drop-down menu, select one of two preconfigured vehicle types, either Car or Bike.

Additional vehicle types can not be added. Fleet keeps them fixed to preserve its Payroll integration, where vehicles may count as employee benefits.

Category: Using the drop-down menu, select a category for the vehicle or create a new one.

In the Information tab, specify details about the car model, such as the car size, passenger capacity, cost settings (applicable to the Belgium localization only), and engine information.

Seats Number: Enter how many passengers the vehicle can accommodate.

Doors Number: Enter the number of doors the vehicle has.

Model Year: Enter the year the vehicle was manufactured.

Trailer Hitch: Tick this checkbox if the vehicle has a trailer hitch installed.

The Salary section only appears if the company has their localization setting set to Belgium. The cost values are all monthly, with the exception of the Catalog Value (VAT Incl.).

Can be requested: Tick this checkbox if employees can request this model vehicle, if a vehicle is part of their employee contract.

Catalog Value (VAT Incl.): enter the MSRP for the vehicle at the time of purchase or lease.

C02 fee: Represents the carbon dioxide emission fee paid to the Belgian government. This value is automatically calculated, based on Belgian laws and regulations, and cannot be modified. The value is based on the figure entered in the CO2 Emissions field (in the Engine section of the Information tab) on the vehicle form.

Modifying the CO2 Emissions field adjusts the value in the CO2 fee field.

Cost (Depreciated): Enter the monthly vehicle cost, which appears in the salary configurator for future employees. This value impacts the gross and net salary of the employee assigned to the vehicle. This figure is depreciated over time, according to local tax laws. The Cost (Depreciated) does not depreciate automatically on the vehicle model, it only depreciates based on the contract linked to a specific vehicle.

Total Cost (Depreciated): This value is the combination of the Cost (Depreciated) and the C02 fee fields. It also depreciated over time.

Fuel Type: Using the drop-down menu, select the type of fuel the vehicle uses. The default options are Diesel, Gasoline, Full Hybrid Plug-in Hybrid Diesel, Plug-in Hybrid Gasoline, CNG, LPG, Hydrogen, or Electric.

Range: Enter the distance the vehicle can travel on one tank of gas, or one battery charge, in kilometers.

CO2 Emissions: Enter the average carbon dioxide emissions the vehicle produces in grams per kilometer (g/km). This information is provided by the car manufacturer.

CO2 Standard: Enter the standard amount of carbon dioxide in grams per kilometer (g/km) for a similar-sized vehicle.

Transmission: Using the drop-down menu, select the type of transmission, either Manual or Automatic.

Power Unit: Using the drop-down menu, select how the vehicle’s power is measured, either in kilowatts or horsepower.

Power: If the vehicle is electric or hybrid, enter the power the vehicle uses in kilowatts (kW). This field only appears if kW is selected for the Power field.

Horsepower: Enter the vehicle’s horsepower in this field. This field only appears if Horsepower is selected for the Power field.

Horsepower Taxation: Enter the amount that is taxed, based on the size of the vehicle’s engine. This is determined by local taxes and regulations, and varies depending on the location. It is recommended to check with the accounting department to ensure this value is correct. This field only appears if Horsepower is selected for the Power field.

Horsepower Taxation: Enter the amount of taxes incurred according to the engine specifications. The number is dependent on the local tax laws, therefore it is recommended to check with the accounting department to ensure the correct taxation amount is entered. This field only appears if the Power field is set to Horsepower.

Tax Deduction: The percentage that can be deducted from taxes is populated based on the localization, and cannot be modified. This field only appears for certain localizations.

Specify the vendors a vehicle can be purchased from in this tab. With proper setup, requests for quotations for vehicles can be created through Odoo’s Purchase app.

To add a vendor, click Add in the upper-left corner of the Vendors tab. This opens an Add: Vendors pop-up window, with a list of all the vendors currently in the database. Add a vendor by ticking the checkbox next to the vendor name, then click Select. No limitations exist on the number of vendors that can be added to this list.

If a vendor is not already in the database, add a vendor by clicking New in the bottom-left of the Add: Vendors pop-up window. In the Create Vendors form that appears, enter the necessary information, then click Save & Close to add the vendor, or click Save & New to add the current vendor and create another new vendor.

To aid with fleet organization, it is recommended to have vehicle models housed under a specific category. Model categories are set on the vehicle model form.

Odoo does not come with any categories preconfigured; all categories must be added.

To view any categories currently set up in the database, navigate to Fleet app ‣ Configuration ‣ Categories. All categories are displayed in a list view.

To add a new category, click the New button in the top-left corner of the Categories dashboard. A new entry line appears at the bottom of the list. Type in the new category, then either click Save, or click anywhere on the screen, to save the entry.

To reorganize how the categories appear in the list, click on the (draggable) icon to the left of any desired category name, and drag the line to the desired position.

The order of the list does not affect the database in any way. However, it may be preferable to view the vehicle categories in a specific order, for example, by size, or the number of passengers the vehicle can carry.

When used with the Inventory app, the Max Weight and Max Volume fields track a vehicle’s capacity. This helps manage in-house deliveries by showing how much space and weight remain for loading products.

---

## Configure DNS records to send emails in Odoo¶

**URL:** https://www.odoo.com/documentation/19.0/applications/general/email_communication/email_domain.html

**Contents:**
- Configure DNS records to send emails in Odoo¶
- SPF (Sender Policy Framework)¶
- DKIM (DomainKeys Identified Mail)¶
  - Add a CNAME record for domain¶
  - Add a CNAME record for subdomain¶
  - See DNS provider documentation¶
- DMARC (Domain-based Message Authentication, Reporting and Conformance)¶
- SPF, DKIM and DMARC documentation of common providers¶

This documentation presents three complementary authentication protocols (SPF, DKIM, and DMARC) used to prove the legitimacy of an email sender. Not complying with these protocols will greatly reduce chances of your emails to reach their destination.

Odoo Online and Odoo.sh databases using the default Odoo subdomain address (e.g., @company-name.odoo.com) are pre-configured to send authenticated emails compliant with the SPF, DKIM, and DMARC protocols.

If choosing to use a custom domain instead, configuring SPF and DKIM records correctly is essential to prevent emails from being quarantined as spam or not being delivered to recipients.

If using the default Odoo email server to send emails from a custom domain, the SPF and DKIM records must be configured as presented below. If using an outgoing email server, it is required to use the SPF and DKIM records specific to that email service and a custom domain.

Email service providers apply different rules to incoming emails. An email may be classified as spam even if it passes the SPF and DKIM checks.

The Sender Policy Framework (SPF) protocol allows the owner of a domain name to specify which servers are allowed to send emails from that domain. When a server receives an incoming email, it checks whether the IP address of the sending server is on the list of allowed IPs according to the sender’s SPF record.

In Odoo, the SPF test is performed on the bounce address defined under the Alias Domain field found under the database’s General Settings. If using a custom domain as Alias Domain, it is necessary to configure it to be SPF-compliant.

The SPF policy of a domain is set using a TXT record. The way to create or modify this record depends on the provider hosting the DNS zone of the domain name.

If the domain name does not yet have an SPF record, create one using the following input:

If the domain name already has an SPF record, the record must be updated. Do not create a new one, as a domain must have only one SPF record.

If the TXT record is v=spf1 include:_spf.google.com ~all, edit it to add include:_spf.odoo.com: v=spf1 include:_spf.odoo.com include:_spf.google.com ~all

Check the SPF record using a tool like MXToolbox SPF Record Check. The process to create or modify an SPF record depends on the provider hosting the DNS zone of the domain name. The most common providers and their documentation are listed below.

The DomainKeys Identified Mail (DKIM) allows a user to authenticate emails with a digital signature.

When sending an email, the Odoo email server includes a unique DKIM signature in the headers. The recipient’s server decrypts this signature using the DKIM record in the database’s domain name. If the signature and the key contained in the record match, it proves the message is authentic and has not been altered during transport.

Enabling DKIM is required when sending emails from a custom domain using the Odoo email server.

To enable DKIM, add a canonical name (CNAME) record to the domain name system (DNS) zone of the domain name:

If the domain name is company-name.com, make sure to create a CNAME record where the CNAME record (key/name) is odoo._domainkey.company-name.com, and the canonical name (value/content) is odoo._domainkey.odoo.com.. For example, note the differences between each key/value in italics:

odoo._domainkey.odoo.com.

odoo._domainkey.dbname.odoo.com.

… where dbname is the name of the Odoo database.

On most DNS platforms, the DNS provider adds the custom domain (e.g., company-name.com) by default. In this case, the key looks different while the value remains the same:

odoo._domainkey.company-name.com

… where company-name.com is the custom domain.

odoo._domainkey.odoo.com.

odoo._domainkey.dbname.odoo.com.

… where dbname is the name of the Odoo database.

If the DNS provider does not add the custom domain by default, make sure to include it.

If there’s a subdomain (e.g., marketing in marketing.company-name.com), add a CNAME record to include it for compliance as well:

odoo._domainkey.marketing

… where marketing is the subdomain.

odoo._domainkey.marketing.company-name.com

… where company-name.com is the custom domain.

odoo._domainkey.odoo.com.

odoo._domainkey.dbname.odoo.com.

… where dbname is the name of the Odoo database.

The way to create or modify a CNAME record depends on the provider hosting the DNS zone of the domain name. The most common providers and their documentation are listed below.

Check if the DKIM record is valid using a tool like MXToolbox DKIM Record Lookup. Enter example.com:odoo in the DKIM lookup tool, specifying that the selector being tested is odoo for the custom domain example.com.

The DMARC record is a protocol that unifies SPF and DKIM. The instructions contained in the DMARC record of a domain name tell the destination server what to do with an incoming email that fails the SPF and/or DKIM check.

The aim of this documentation is to help understand the impact DMARC has on the deliverability of emails, rather than give precise instructions for creating a DMARC record. Refer to a resource like DMARC.org to set the DMARC record.

There are three DMARC policies:

p=quarantine and p=reject instruct the server that receives an email to quarantine that email or ignore it if the SPF or DKIM check fails.

For the DMARC to pass, the DKIM or SPF check needs to pass and the domains must be in alignment. If the hosting type is Odoo Online, DKIM configuration on the sending domain is required to pass the DMARC.

Passing DMARC generally means that the email will be successfully delivered. However, it’s important to note that other factors like spam filters can still reject or quarantine a message.

p=none is used for the domain owner to receive reports about entities using their domain. It should not impact the deliverability.

_dmarc IN TXT “v=DMARC1; p=none; rua=mailto:postmaster@example.com” means that aggregate DMARC reports will be sent to postmaster@example.com.

GoDaddy SPF, DKIM, or DMARC records

Squarespace DNS records

To fully test the configuration, use the Mail-Tester tool, which gives a full overview of the content and configuration in one sent email. Mail-Tester can also be used to configure records for other, lesser-known providers.

Using Mail-Tester to set SPF Records for specific carriers

Magic Sheet - SPF, DKIM and DMARC configuration [PDF]

**Examples:**

Example 1 (unknown):
```unknown
v=spf1 include:_spf.odoo.com ~all
```

Example 2 (unknown):
```unknown
odoo._domainkey IN CNAME odoo._domainkey.odoo.com.
```

---

## AsiaPay¶

**URL:** https://www.odoo.com/documentation/19.0/applications/finance/payment_providers/asiapay.html

**Contents:**
- AsiaPay¶
- Configuration on AsiaPay Dashboard¶
- Configuration on Odoo¶

AsiaPay is an online payments provider established in Hong Kong and covering several Asian countries and payment methods.

Log into AsiaPay Dashboard according to the account provided by AsiaPay.

PayDollar: For markets in HK, CN, MO, TW, SG, MY, IN, VN, NZ and AU

PesoPay: For market in PH

SiamPay: For market in TH

BimoPay: For market in ID

Go to Profile ‣ Account Information. Copy the values of the Currency and Secure Hash fields and save them for later.

Click on Update to finalize the configuration.

Navigate to the payment provider AsiaPay and change its state to Enabled.

Configure the rest of the options to your liking.

---

## Changelog¶

**URL:** https://www.odoo.com/documentation/19.0/developer/reference/backend/orm/changelog.html

**Contents:**
- Changelog¶
- Odoo version 19.0¶
- Odoo Online version 18.4¶
- Odoo Online version 18.3¶
- Odoo Online version 18.2¶
- Odoo Online version 18.1¶
- Odoo version 18.0¶
- Odoo Online version 17.4¶
- Odoo Online version 17.3¶
- Odoo Online version 17.2¶

Add support for GROUPING SETS for pivot views. See #194413.

Adding support for dynamic dates in domains. See #216665.

Deprecated odoo.osv in #217708.

Deprecated record._cr, record._context, record._uid in #193636.

The reinit option is added to the CLI to reinitialize modules. See #206408.

Possibility to write and combine custom domains for injecting arbitrary SQL. See #205208.

Domain optimization is applied before executing Fields.search methods. All equalities are handled consistently: = is equivalent to in. See #191549.

New cron API for notifying progress with batch commits. See #197781.

Demo data no longer loaded by default. See #194585.

read_group has been deprecated in favor of _read_group for backend usage and of formatted_read_group as formatted public API. See #163300.

@api.private is added to distinguish public Python methods from methods exposed for RPC calls. See #195402.

Native namespaces for odoo module PEP-420. See #195664.

New odoo.domain and odoo.Domain API for domain manipulation. See #170009.

Declare constraints and indexes as model attributes with #175783.

The json controllers have been renamed to jsonrpc. They are called the same, only the type in the python files changed. See #183636.

Searching by name is now implemented as _search_display_name like all other fields. See #174967.

New methods to check access rights and rules now combine both access rights and rules: check_access, has_access and _filtered_access. See #179148.

Translations are made available from the Environment with #174844.

The internal operator inselect is removed. The alternative is to use in with a Query or SQL object. #171371.

We can now group by date parts numbers in read_group, _read_group and domains with #159528.

The group_operator attribute of Field is renamed into aggregator with #127353.

We can now group/aggregate/order by related no-store field with #127353.

Method _flush_search() has been deprecated with #144747. The flushing of fields is now done by execute_query(), and is based on metadata put in the SQL object by _search() and other low-level ORM methods that build such objects. Those methods are also responsible for checking the access rights on the fields that are used in the SQL object.

Introduce an SQL wrapper object to make SQL composition easier and safer with respect to SQL injections. Methods of the ORM now use it internally. Introduced by #134677.

Method name_get() has been deprecated with #122085. Read field display_name instead.

Method _read_group() has a new signature with #110737

Refactor the implementation of searching and reading methods to be able to combine both in a minimal number of SQL queries. We introduce two new methods search_fetch() and fetch() that take advantage of the combination. More details can be found on the pull request #112126.

Translations for translated fields are stored as JSONB values with #97692 and #101115. Code translations are no longer stored into the database. They become static and are extracted from the PO files when needed.

search_count() takes the limit argument into account with #95589. It limits the number of records to count, improving performance when a partial result is acceptable.

New API for flushing to the database and invalidating the cache with #87527. New methods have been added to odoo.models.Model and odoo.api.Environment, and are less confusing about what is actually done in each case. See the section SQL Execution.

The argument args is renamed to domain for search(), search_count() and _search(). #83687

filtered_domain() conserves the order of the current recordset. #83687

browse() does not accept str as ids. #83687

The methods fields_get_keys() and get_xml_id() on Model are deprecated. #83687

The method _mapped_cache() is removed. #83687

Remove the limit attribute of One2many and Many2many. #83687

Specific index types on fields: With #83274 and #83015, developers can now define what type of indexes can be used on fields by PostgreSQL. See the index property of odoo.fields.Field.

The _sequence attribute of Model is removed. Odoo lets PostgreSQL use the default sequence of the primary key. #82727

The method _write() does not raise an error for non-existing records. #82727

The column_format and deprecated attributes of Field are removed. #82727

---

## Forecast report¶

**URL:** https://www.odoo.com/documentation/19.0/applications/sales/crm/performance/forecast_report.html

**Contents:**
- Forecast report¶
- Navigate the forecast report¶
  - Expected closing date¶
  - Prorated revenue¶
- View results¶

The Forecast report in the CRM app allows users to view upcoming opportunities and build a forecast of potential sales. Opportunities are grouped by the month of their expected closing date, and can be dragged-and-dropped to adjust the deadline.

To access the Forecast report, navigate to CRM app ‣ Reporting ‣ Forecast.

The default Forecast report includes opportunities assigned to the current user’s pipeline, and are expected to close within four months. It also shows opportunities without an assigned expected closing date. The opportunities are grouped by month in a (Kanban) view.

Opportunities are grouped by the date assigned in the Expected Closing field on an opportunity form. To change this date directly from the Forecast page, select the Kanban card for the desired opportunity, then click and drag the card to the desired column.

The default time frame for the forecast is month. This can be changed by clicking the (down) icon next to the Search… bar at the top of the report. Under the Group By heading in the resulting drop-down menu, click Expected Closing to expand the list of available options, and select a desired amount of time from the list.

After an opportunity is added to a new month, the Expected Closing field on the opportunity form is updated to the last date of the new month.

The Expected Closing field can also be manually updated on the opportunity card. To do that, click on the Kanban card for an opportunity on the Forecast page to open the opportunity’s detail form. Click in the Expected Closing field, and use the calendar popover to select a new closing date.

The prorated revenue is the Expected Revenue amount that is displayed at the top of the column for each month on the Forecast reporting page. This value is situated to the right of the progress bar. The calculation for Expected Revenue is the total of the prorated revenue specific to that particular time frame.

The prorated revenue is calculated using the formula below:

As opportunities are moved from one column to another, the column’s revenue is automatically updated to reflect the change.

A forecast report for June includes two opportunities:

The first opportunity, Global Solutions, has an expected revenue of $3,800, and a probability of 90%. This results in a prorated revenue of $3,420.

The second opportunity, Quote for 600 Chairs, has an expected revenue of $22,500, and a probability of 20%. This results in a prorated revenue of $4,500.

The combined prorated revenue of the opportunities is $7,920, which is listed at the top of the column for the month.

For more information on how probability is assigned to opportunities, see Assign leads with predictive lead scoring

Click the (area chart) icon to change to graph view. Then, click the corresponding icon at the top of the report to switch to a (bar chart), (line chart), or (pie chart).

Click the (pivot) icon to change to the pivot view, or the (list) icon to change to the list view.

The pivot view can be used to view and analyze data in a more in-depth manner. Multiple measures can be selected, and data can be viewed by month, and by opportunity stage.

To save this report as a favorite, see Favorites.

---

## Create customized reports¶

**URL:** https://www.odoo.com/documentation/19.0/developer/howtos/create_reports.html

**Contents:**
- Create customized reports¶
- Create a model¶
- Populate the model¶
- Use the model¶
- Extra tips¶

SQL views are a technique for creating customized reports to show data that cannot be shown with existing models’ fields and views. In other words, this technique helps avoid unnecessary creation and calculation of additional fields solely for data analysis purposes.

A SQL view is created in a similar manner as a standard model:

Where the attributes:

_auto = False indicates that we do not want to store the model in the database

_rec_name indicates which of the model’s fields represents a record’s name (i.e. the name that will be used in the navigation breadcrumb when opening a record’s form view)

and its fields are defined in the same way as a standard model, except every field is marked as readonly=True.

Don’t forget to add your new model to your security file.

There are 2 ways to populate a SQL view’s table:

override the BaseModel.init() method,

set the _table_query property.

Regardless of which way is used, a SQL query will be executed to populate the model. Therefore, any SQL commands can be used to collect and/or calculate the data needed and you are expected to keep in mind that you are bypassing the ORM (i.e. it is a good idea to read through Security in Odoo if you haven’t already). The columns returned from the SELECT will populate the model’s fields, so ensure that your column names match your field names, or use alias names that match.

In most cases, overriding the BaseModel.init() method is the standard and better option to use. It requires the import of tools and is typically written as follows:

tools.drop_view_if_exists ensures that a conflicting view is not created when the SQL query is executed. It is standard to separate the different parts of the query to allow for easier model extension. Exactly how the query is split up across methods is not standardized, but at minimum, the _select and _from methods are common, and of course, all these methods will return strings.

Example: a SQL view using an override of BaseModel.init()

The _table_query property is used when the view depends on the context. It is typically written as follows:

and follows the same _select and _from methods standards as BaseModel.init().

An example of when the property should be used instead of overriding BaseModel.init() is in a multi-company and multi-currency environment where currency related amounts need to be converted using currency exchange rates when the user switches between companies.

Example: a SQL view using _table_query

Views and menu items for your SQL views are created and used in the same way as any other Odoo model. You are all set to start using your SQL view. Have fun!

A common mistake in SQL views is not considering the duplication of certain data due to table JOINs. This can lead to miscounting when using a field’s aggregator and/or the pivot view. It is best to test your SQL view with sufficient data to ensure the resulting field values are as you expect.

If you have a field that you do not want as a measure (i.e., in your pivot or graph views), add store=False to it, and it will not show.

**Examples:**

Example 1 (python):
```python
from odoo import fields, models


class ModuleReport(models.Model):
    _name = 'module.report'
    _description = "Module Report"
    _rec_name = 'module_field'
    _auto = False
```

Example 2 (sql):
```sql
def init(self):
    tools.drop_view_if_exists(self.env.cr, self._table)
    self.env.cr.execute("""CREATE or REPLACE VIEW %s as (
                           SELECT
                              %s
                           FROM
                              %s
        )""" % (self._table, self._select(), self._from()))
```

Example 3 (python):
```python
@property
def _table_query(self):
    return 'SELECT %s FROM %s' % (self._select(), self._from())
```

---

## Merge similar leads and opportunities¶

**URL:** https://www.odoo.com/documentation/19.0/applications/sales/crm/pipeline/merge_similar.html

**Contents:**
- Merge similar leads and opportunities¶
- Identify similar leads and opportunities¶
  - Comparing similar leads and opportunities¶
- Merging similar leads and opportunities¶
- When leads/opportunities should not be merged¶
  - Lost leads¶
  - Different contact within an organization¶
  - Existing duplicates with more than one salesperson¶
  - Contact information is similar but not exact¶

Odoo automatically detects similar leads and opportunities within the CRM app. Identifying these duplicated records allows them to be merged without losing any information in the process. Not only does this help keep the pipeline organized, but it also prevents customers from being contacted by more than one salesperson.

When merging opportunities, no information is lost. Data from the other opportunity is logged in the chatter, and the information fields, for reference.

Similar leads and opportunities are identified by comparing the email address and phone number of the associated contact. If a similar lead/opportunity is found, a Similar Leads smart button appears at the top of the lead (or opportunity) record.

To compare the details of similar leads/opportunities, navigate to CRM app ‣ Pipeline or CRM app ‣ Leads. Open a lead or opportunity, and click the Similar Leads smart button. Doing so opens a Kanban view that only displays similar leads/opportunities. Click on a card to view the details for the lead/opportunity, and confirm if they should be merged.

When merging, Odoo gives priority to whichever lead/opportunity was created in the system first, merging the information into the first created lead/opportunity. However, if a lead and an opportunity are being merged, the resulting record is referred to as an opportunity, regardless of which record was created first.

After confirming that the leads/opportunities should be merged, return to the Kanban view using the breadcrumb link, or by clicking the Similar Leads smart button. Click the (list) icon to change to list view.

Check the box on the left of the page for the leads/opportunities to be merged. Then, click the Actions icon at the top of the page, to reveal a drop-down menu. From that drop-down menu, select the Merge option to merge the selected opportunities or leads.

When Merge is selected from the Actions drop-down menu, a Merge pop-up modal appears. In that pop-up modal, under the Assign opportunities to heading, select a Salesperson and Sales Team from the appropriate drop-down menus.

Below those fields, the leads/opportunities to merge are listed, along with their related information. To merge those selected leads/opportunities, click Merge.

Merging is an irreversible action. Do not merge leads/opportunities unless absolutely certain they should be combined.

There may be instances where a similar lead or opportunity is identified, but should not be merged. These circumstances vary, based on the processes of the sales team and organization. Some potential scenarios are listed below.

If a lead/opportunity has been marked as lost, it can still be merged with an active lead or opportunity. The resulting lead/opportunity is marked active, and added to the pipeline.

Leads/opportunities from the same organization, but with different points of contact, may not have the same needs. In this case, it is beneficial to not merge these records, though assigning the same salesperson, or sales team, can prevent duplicated work and miscommunication.

If more than one lead/opportunity exists in the database, there may be multiple salespeople assigned to them, who are actively working on them independently. While these leads/opportunities may need to be managed separately, it is recommended that any affected salespeople be tagged in an internal note for visibility.

Similar leads and opportunities are identified by comparing the email addresses and phone numbers of the associated contacts. However, if the email address is similar, but not exact, they may need to remain independent.

Three different leads were added to the pipeline and assigned to different salespeople. They were identified as Similar Leads due to the email addresses of the contacts.

Two of the leads appear to come from the same individual, Robin, and have identical email addresses. These leads should be merged.

The third lead has the same email domain, but the address is different, as is the contact name. While this lead is most likely from the same organization, it is from a different contact, and should not be merged.

---

## Forms spam protection¶

**URL:** https://www.odoo.com/documentation/19.0/applications/websites/website/configuration/spam_protection.html

**Contents:**
- Forms spam protection¶
- Cloudflare Turnstile configuration¶
  - On Cloudflare¶
  - On Odoo¶
- reCAPTCHA v3 configuration¶
  - On Google¶
  - On Odoo¶

Cloudflare Turnstile and Google reCAPTCHA v3 protect website forms, web sign-up pages, and password reset pages against spam and abuse. They attempt to distinguish between human and bot submissions using non-interactive challenges based on telemetry and visitor behavior.

We recommend using Cloudflare Turnstile as reCAPTCHA v3 may not be compliant with local data protection regulations.

All pages using the Form, Newsletter Block, Newsletter Popup snippets, and the eCommerce Extra Step During Checkout form are protected by both tools. Web sign-up pages and password reset pages are also protected.

Cloudflare Turnstile’s documentation

Google’s reCAPTCHA v3 guide

Create a Cloudflare account or use an existing one and log in.

On the dashboard navigation sidebar, click Turnstile.

On the Turnstile Sites page, click Add Site.

Add a Site name to identify it easily.

Enter or select the website’s Domain (e.g., example.com or subdomain.example.com).

Select a Widget Mode:

The Managed mode is recommended, as visitors can be prompted to check a box confirming they are human if deemed necessary by Turnstile.

For the Non-interactive and Invisible modes, visitors are never prompted to interact. In Non-interactive mode, a loading widget can be displayed to warn visitors that Turnstile protects the form; however, the widget is not supported by Odoo.

If the Turnstile check fails, visitors are not able to submit the form, and the following error message is displayed:

The generated keys are then displayed. Leave the page open for convenience, as copying the keys in Odoo is required next.

From the database dashboard, click Settings. Under Integrations, enable Cloudflare Turnstile and click Save.

Open the Cloudflare Turnstile page, copy the Site Key, and paste it into the CF Site Key field in Odoo.

Open the Cloudflare Turnstile page, copy the Secret Key, and paste it into the CF Secret Key field in Odoo.

Navigate to Turnstile on your Cloudflare account to view the solve rates and access more settings.

reCAPTCHA v3 may not be compliant with local data protection regulations.

Open the reCAPTCHA website registration page. Log in or create a Google account if necessary.

On the website registration page:

Give the website a Label.

Leave the reCAPTCHA type on Score based (v3).

Enter one or more Domains (e.g., example.com or subdomain.example.com).

Under Google Cloud Platform, a project is automatically selected if one was already created with the logged-in Google account. If not, one is automatically created. Click Google Cloud Platform to select a project yourself or rename the automatically created project.

Agree to the terms of service.

A new page with the generated keys is then displayed. Leave it open for convenience, as copying the keys to Odoo is required next.

From the database dashboard, click Settings. Under Integrations, enable reCAPTCHA if needed.

Do not disable the reCAPTCHA feature or uninstall the Google reCAPTCHA integration module, as many other modules would also be removed.

Open the Google reCAPTCHA page, copy the Site key, and paste it into the Site Key field in Odoo.

Open the Google reCAPTCHA page, copy the Secret key, and paste it into the Secret Key field in Odoo.

Change the default Minimum score (0.70) if necessary, using a value between 1.00 and 0.00. The higher the threshold is, the more difficult it is to pass the reCAPTCHA, and vice versa. Out of the 11 levels, only the following four score levels are available by default: 0.1, 0.3, 0.7 and 0.9.

Interpret reCAPTCHA scores - Google documentation

You can notify visitors that reCAPTCHA protects a form. To do so, open the website editor and navigate to the form. Then, click somewhere on the form, and on the right sidebar’s Customize tab, toggle Show reCAPTCHA Policy found under the Form section.

If the reCAPTCHA check fails, the following error message is displayed:

Analytics and additional settings are available on Google’s reCAPTCHA administration page. For example, you can receive email alerts if Google detects suspicious traffic on your website or view the percentage of suspicious requests, which could help you determine the right minimum score.

---

## Saudi Arabia¶

**URL:** https://www.odoo.com/documentation/19.0/applications/hr/payroll/payroll_localizations/saudi_arabia.html

**Contents:**
- Saudi Arabia¶
- Apps & modules¶
- General configurations¶
- Employees¶
  - Work tab¶
  - Personal tab¶
  - Payroll tab¶
    - Contract overview section¶
    - Schedule section¶
    - Saudi payroll information section¶

The Saudi Arabia payroll localization covers salary computations for employees, including national and provincial regulations.

Before configuring the Saudi Arabia localization, refer to the general payroll documentation, which includes the basic information for all localizations, as well as all universal settings and fields.

Install the following modules to get all the features of the Saudi Arabia payroll localization:

Saudi Arabia - Payroll

hr_work_entry_holidays

Provides Saudi Arabia payroll basics, including salary structures (Basic/Gross/Net).

Saudi Arabia - Payroll with Accounting

l10n_sa_hr_payroll_account

Links payroll and accounting by creating journal entries (per payslip if needed) to record payroll in the company’s books.

Configure the Saudi Arabia fiscal localization

First, the company must be configured. Navigate to Settings app ‣ Users & Companies ‣ Companies. From the list, select the desired company, and configure the following fields:

Company Name: Enter the business name in this field.

Address: Complete the full address, including the City, State, Zip Code, and Country.

VAT Number: Enter the company’s unique 15-digit VAT number.

Company ID : Enter the business’s MoL number.

Currency: By default, SAR is selected. If not, select SAR from the drop-down menu.

Phone: Enter the company phone number.

Email: Enter the email used for general contact information.

Every employee being paid must have their employee profiles configured for the Saudi Arabia payroll localization. Additional fields are present after configuring the database for Saudi Arabia.

To update an employee form, open the Employees app and click on the desired employee record. On the employee form, configure the required fields in the related tabs.

Enter the Work Address for the employee in the Location section of the Work tab.

Select the correct Nationality (Country) for the employee, using the drop-down menu. The selected nationality determines the GOSI rate.

Ensure the employee has a minimum of one trusted bank account listed in the Bank Accounts field in the Private Contact section.

The employee’s bank account is their IBAN, which is how employees receive their salary according to WPS regulations.

Payroll cannot be processed for employees without a trusted bank account. If no trusted bank account is set, a warning appears on the Payroll dashboard and an error occurs when attempting to run payroll.

This section holds information that drives salary calculations. Ensure the following fields are configured:

Contract: The Validity of the compensation conditions can be updated depending on the needs.

Wage Type: Select how the employee is paid.

Select Fixed Wage for salaried employees who receive the same amount each pay period.

Select Hourly Wage for employees paid based on hours worked.

Set a default Wage Type in the salary Structure Type to configure employees in bulk. If needed, the default can be overridden on individual employee records if exceptions are needed.

Contract Type: Determines how the employee is paid and classified, such as Permanent, Temporary, Seasonal.

Pay Category: Select KSA Employee for this field. This defines when the employee is paid, their default working schedule, and the work entry type it applies to.

Work Entry Source: Defines how work entries are generated for payroll during the specified pay period. The options are:

Working Schedule: Based on the employee’s assigned working schedule (e.g., 40 hours per week).

Attendances: Based on approved checked-in hours in the Attendances app.

Planning: Based on scheduled shifts in the Planning app.

Extra Hours: Tick the checkbox to allow the Attendances app to add any extra work entries logged by the employee.

Working Hours: Using the drop-down menu, select the default work schedule. This is particularly important for employees available to receive overtime pay (typically hourly employees, not salaried).

Enter the employee’s unique 10-digit Saudi National / IQAMA ID number in this field. This number is issued to Saudi citizens by the Ministerial Agency of Civil Affairs, and is used for exporting WPS reports.

The Annual Leave Balance field is not editable, and displays the employee’s available annual vacation days.

This section determines the various benefits the employee receives. Enter the monthly Saudi riyal amount the employee receives from the company for Housing, Transportation, and Other costs.

Enter the annual amount paid by the company for a Saudi Iqama (iqama is the Arabic word for residence permit). An Iqama is an official residency and identification card, similar to a visa, and it is . It is required for all foreign nationals to have a valid Iqama to work, live, and access various services in Saudi Arabia.

Enter the annual amount the employer pays for the employee’s Medical Insurance in the corresponding field.

If the employee needs a Work Permit, enter the annual fees the company pays.

In Saudi Arabia, each employee earns a percentage of their annual salary for each year they have worked with a company. This amount is computed based on their salary. When they leave the company, they receive this compensation in one lump sum.

Enter the Number of Days the employee earns in annual compensation, in the corresponding field. The company sets this amount aside every year, so it is available when the employee leaves.

Several sections within the Payroll app installs a Salary Structure, Structure Type, Rules, and Parameters specific to Saudi Arabia. Additionally, some other configurations are required to run Saudi Arabia payroll.

Navigate to Payroll ‣ Configuration ‣ Settings to access the Payroll app settings required for Saudi Arabia.

First, the company bank account must be configured to pay employees, per WPS regulations. Click into the drop-down space beneath the Establishment Bank Account field, and click Create…. This opens a Create Establishment’s Bank Account pop-up window. Configure the company’s bank account, and ensure it is marked as trusted. Click Save to save the information and close the window.

Next, enter the company’s MoL Establishment ID. This ID number is provided by Saudi Arabia’s Ministry of Labor.

Finally, select the time off type that is used to calculate the Annual Leave Balance set in the employee’s profile.

When the l10n_sa_hr_payroll module is installed, a new Salary Structure gets installed, KSA Employee. This structure includes two Structure Types, Saudi Arabia: Monthly Pay, and SA Salary Advance And Loan Structure.

The Salary Structure contains all the individual salary rules that informs the Payroll app how to calculate employee payslips.

To view the salary rules that inform the salary structure what to do, navigate to Payroll app ‣ Configuration ‣ Structures and expand the KSA Employee group to reveal the two available structure types. Click Saudi Arabia: Monthly Pay to view the detailed salary rules for that structure type.

Each rule defines how pay is calculated, taking into account factors such as allowances, deductions, and company contributions.

Some calculations require specific rates associated with them, or wage caps. Rules Parameters are capable of listing a value, either a percentage or a fixed amount, to reference in the salary rules.

Most rules pull information stored in the parameters module to get the rate of the rule (a percentage) and the cap (a dollar amount).

To view rule parameters, navigate to Payroll app ‣ Configuration ‣ Rule Parameters. Here, all rule parameters are displayed with their linked Salary Rules, which can be accessed. Review the parameters associated with a rule by looking for the Name of the rule, and make any edits as needed.

The Saudi Arabia payroll localization comes with four rule parameters:

Saudi GOSI Company Contribution: This rule parameter determines the calculation of the GOSI contributions the employer must make for Saudi Arabia employees.

Non-Saudi GOSI Company Contribution: This rule parameter determines the calculation of the GOSI contributions the employer must make for each non-Saudi Arabian employee.

Saudi GOSI Employee Contribution: This rule parameter determines the calculation of the GOSI contributions the employee must make, and is deducted from their pay.

Saudi Arabia Overtime Rate: This rule parameter determines the overtime rate for employees.

Odoo adds updated rule parameters for the current calendar year. It is not recommended to edit rule parameters unless a national or provincial parameter has changed, and is different from the rule parameters created by Odoo. Check with all local and national regulations before making any changes to rule parameters.

Before running payroll, the payroll officer must validate employee work entries to confirm pay accuracy and catch errors. This includes checking that all time off is approved and any overtime is appropriate.

Work entries sync based on the employee’s contract configuration. Odoo pulls from the assigned working schedule, attendance records, planning schedule, and approved time off.

Any discrepancies or conflicts must be resolved, then the work entries can be regenerated.

Once everything is correct, draft payslips can be created individually or in groups, referred to in the Payroll app as Pay Runs.

To cut down on the payroll officer’s time, it is typical to process payslips in batches, either by wage type (fixed salary vs hourly), pay schedule (weekly, bi-weekly, monthly, etc.), department (direct cost vs. administration), or any other grouping that best suits the company.

The process of running payroll includes different actions that need to be executed to ensure that the amount that the employee receives as their net salary is correct, any deductions or allocations are correct, and the computation of hours worked reflects the employee’s actual hours worked, among others.

When running a payroll batch, check that the period, company, and employees included are correct before starting to analyze or validate the data.

Once the payslips are drafted, review them for accuracy. Check the Worked Days & Inputs tab, and ensure the listed worked time is correct, as well as any other inputs. Add any missing inputs, such as commissions, tips, reimbursements, that are missing.

Next, check the various totals (gross pay, allowances, contributions, etc.), then click Compute Sheet to update the salary calculations, if there were edits. If everything is correct, click Validate.

The accounting process when running payroll has two components: creating journal entries, and registering payments.

After payslips are confirmed and validated, journal entries are posted either individually, or in a batch. The journal entry is created first as a draft.

It must be decided if journal entries are done individually or in batches before running payroll.

Fifteen accounts from the Saudi Arabia CoA are included with the payroll localization:

400003 Basic Salary: Tracks the basic wages paid to employees.

400004 Housing Allowance: Captures housing allowance payments provided to employees.

400005 Transportation Allowance: Captures transportation allowance payments to employees.

400012 Staff Other Allowances: Covers employee allowances that do not fit standard categories.

106012 Prepaid Employee Expenses: Logged as prepaid assets for employer-paid items that are not wages (for example, Iqama fees and medical insurance) and expensed as consumed.

201006 Leave Days Provision: Accrues the cost of paid leave; the balance is reduced when leave is taken, and any leave not taken may be paid out at end of service.

201022 GOSI Employee Payable: Holds amounts payable for social insurance contributions to the GOSI, including both employee and employer portions.

202001 End of Service Provision: Accumulates the end-of-service benefit monthly so the amount can be paid (partially or fully) when the employee leaves, per service length and reason.

400007 Leave Salary: Records salaries actually paid to employees while on paid leave.

400008 End of Service Indemnity: Captures company expenses set aside to fund end-of-service benefits (expense-side counterpart to the provision).

400009 Medical Insurance: Accounts for employer expenses related to employee medical insurance premiums.

400010 Life Insurance: Tracks employer expenses for life/occupational insurance or the employer’s portion of social insurance.

400014 Visa Expenses: Used to track employer costs for non-Saudi Arabian employees’ visas and related processing/renewal fees.

400074 Salary Deductions: Reflects deductions applied to employee salaries (e.g., advances, fines, statutory deductions).

201002 Payables: Shows amounts payable to employees as salaries (unpaid salary liability at period end).

If everything seems correct on the journal entry draft, post the journal entries.

After the journal entries are validated, Odoo can generate payments.

To generate payments from payslips,employees must have a trusted bank account. If the employee’s bank account is not marked as trusted, WPS files cannot be generated through Odoo.

Payments can be Grouped by Partner if there is a partner associated with a salary rule.

If there are no errors, payroll is completed for the pay period.

Employees are able to request loans or advances against their salaries. Loans are handled by creating salary adjustments, whereas advances are created and managed manually.

Employees can take out loans against their salary and repay the loan with automatic deductions from future paychecks. These loans are handled by creating a salary adjustment to log the total loan amount and set up a repayment plan, and creating a payslip for the total loan amount. The employee then repays the loan in the increments configured in the salary adjustment.

When an employee requests a loan, first a salary adjustment is made, where the total loan amount and repayment plan is configured.

To create the loan and repayment schedule, navigate to Payroll app ‣ Employees ‣ Salary Adjustments. Click New, and a blank Salary Adjustment form loads.

Configure the following fields on the form:

Employees: Using the drop-down menu, select the employee taking out the loan.

Type: Using the drop-down menu, select Loan Deduction.

Payslip Amount: Enter the SAR amount to be repaid each payslip.

Duration: Tick the Limited option, which reveals a until (amount) paid field. First, using the calendar selector, enter the date the repayment begins. Next, enter the total loan amount in the field after until. Once both fields are configured, Odoo calculates when the loan is fully repaid, and displays the date as follows: end in # years: (date).

Note: Enter any details about the loan in this field.

After creating the salary adjustment, a button labeled Create Loan Payslip appears above the form. Click Create Loan Payslip, and Odoo automatically generates a payslip configured for the employee loan, in the amount set up in the salary adjustment.

Review the payslip to ensure all the fields and tabs are correct. The Structure should be SA Salary Advance And Loan Structure. The Salary Inputs tab should list a Loan Deduction, and the Total should be the full amount of the loan.

Click into the Other Info tab, and enter the date the employee receives the payment in the Payment Date field.

Once the payslip form is correct, click Compute Sheet, then click Create Draft Entry, and click OK in the confirmation pop-up window. Next, click Pay to mark the payment as paid, then click Create Payment Report to generate the WPS report.

From the date set on the salary attachment, all future paychecks have the configured repayment amount taken out, to go towards the loan. Once the loan is fully paid, the deductions end.

The deductions should end after the total is repaid, according to the salary adjustment. To ensure payments stop being taken out of the employee’s paychecks after the loan has been repaid, open the salary adjustment record. If the salary adjustment still has a status of Running, click the Mark as Completed button, and the status changes to Closed, and payments stop.

Employees can request an advance of their salary, outside of the regular pay cycle. This process is done by paying the employee with a manually created payslip, then the employee repays the advance either in full in the next payslip, or in multiple payments in subsequent paychecks until the total is repaid.

To issue an advanced payment, navigate to Payroll app ‣ Payslips ‣ Payslips. Click the New Off-Cycle button and a blank Employee Payslips form loads.

Configure the following fields on the payslip:

Employees: Using the drop-down menu, select the employee taking out the advance.

Once the Employee is selected, the Employee Record field populates with the most recent employee version (contract).

Structure: Using the drop-down menu, select SA Salary Advance And Loan Structure.

Next, click Add a line in the Salary Inputs tab. Select Salary Advance for the Type. Next, enter any relevant description in the Notes field, such as Advance for auto repair. Last, enter the amount being advanced in the Amount field.

Once these fields have been configured, click Compute Sheet, then click Create Draft Entry, and click OK in the confirmation pop-up window. Click Pay to mark the payment as paid, then click Create Payment Report to generate the WPS report.

When the employee’s next payslip is generated (using the Saudi Arabia: Monthly Pay structure), a line is added under the Other Inputs tab, titled Advanced Recovery.

The amount is based on the advance previously taken by the employee. If the employee cannot repay the full amount in the paycheck, the repayment can be broken up into multiple payments.

When an employee cannot repay an advance in full in a future payslip, the employee can make partial payments until the advance is repaid. To do this, the amount being deducted from the employee’s paycheck must be manually modified.

Open the employee’s payslip, and under the Salary Inputs tab, modify the amount specified on the Advanced Recovery line.

Odoo automatically calculates the remainder of the advance, and adds an Advanced Recovery line for the balance in the subsequent payslip. If the employee cannot pay the second payment in full, the Advanced Recovery line can be edited in the following payslip.

The WPS report is a mandatory report all Saudi Arabian companies are required to provide the government. The report confirms employee wages, including when and how much they were paid. The WPS report can be generated either per payslip or per pay run.

This report is structured in accordance with the technical guidelines issued by the MHRSD.

To run the WPS report, the Establishment Bank Account and Ministry of Labor Establishment ID fields must be configured for the company.

Ensure the Establishment Bank Account has the Bank Establishment ID, and Bank SARIE Code fields configured, and the bank is marked as Trusted.

Ensure all employees included in the report have a Saudi National / IQAMA ID set on their employee profile.

First, payslips or pay runs are generated, then the journal entries are posted.

Click the Payment Report button on the Pay Runs dashboard, the individual pay run, or an individual payslip, and a payment report pop-up window loads.

Saudi WPS is selected in the Export Format field, by default, and the Payment Date and WPS Value Date fields are both populated according to the selected payslips or pay runs.

The WPS Debit Date is an optional field, and specifies the date the company physically pays the employees (transfers funds from the company account to employee accounts).

Once all fields are configured, click Generate, and the WPS report is created.

---

## Odoo Sign legality in Chile¶

**URL:** https://www.odoo.com/documentation/19.0/applications/productivity/sign/chile.html

**Contents:**
- Odoo Sign legality in Chile¶
- Legal framework for electronic signatures in Chile¶
- How Odoo Sign complies with Chilean regulations¶
- Types of documents you can sign with Odoo Sign¶
- Potential exceptions¶

Odoo Sign is your trusted partner for secure, efficient, and legally compliant electronic signatures in Chile.

In Chile, electronic signatures are regulated under the Law 19.799 on electronic documents, electronic signature and certification services of such signature, Decree 181/2002 and Law No. 21,180, on Digital Transformation of the State. The law establishes the legal basis for acknowledging electronic records, contracts, and signatures. The key points of the law include:

Legal recognition: electronic signatures are legally equivalent to handwritten signatures if they meet certain criteria.

Reliability and security: electronic signatures must be created with a secure method that can reliably identify the signatory and ensure the integrity of the signed document.

Certified electronic signatures: special electronic signature that incorporates certificates issued by a recognized certification authority.

Odoo Sign ensures full compliance with the Electronic Signature Law of Chile by incorporating the following features:

Secure signature creation: Odoo Sign utilizes advanced cryptographic techniques to ensure the authenticity and integrity of electronic signatures.

Third-party signature authenticator: Odoo Online serves as an independent validation mechanism that adds an extra layer of security to the procurement process.

Audit trails: detailed audit logs are maintained to provide evidence of the signing process, including timestamps, IP addresses, and identity verification.

Cryptographic traceability and immutability: Odoo Sign ensures that any operation is logged securely. An audit log provides full transparency to all parties while preserving private data.

Multiple authentication means: authentication by SMS, email, geoIP or handwritten electronic signature.

Odoo Sign is versatile and can be used for a wide range of documents, including but not limited to:

Contracts and agreements: business contracts, employment agreements, and service contracts.

Financial documents: loan agreements, investment documents, and financial reports.

HR documents: employee onboarding forms, non-disclosure agreements (NDAs), and performance reviews.

Commercial transactions: purchase orders, sales agreements, and supplier contracts.

While Odoo Sign is broadly applicable, there are certain exceptions where electronic signatures may not be suitable or legally recognized in Chile:

Wills and trusts: documents related to inheritance, wills, and trusts often require handwritten signatures.

Real estate transactions: some property transactions may still require notarized handwritten signatures.

Government forms: specific government forms and applications may mandate physical signatures.

The information provided on this page is for general informational purposes only and does not constitute legal advice. While Odoo Sign complies with the Electronic Signature Law of Chile, users should consult with legal professionals to ensure specific document types and use cases meet all legal requirements. Compliance with additional industry-specific regulations may also be necessary.

Last updated: June 21, 2024

---

## Insert and link to Odoo data¶

**URL:** https://www.odoo.com/documentation/19.0/applications/productivity/spreadsheet/insert.html

**Contents:**
- Insert and link to Odoo data¶
- Data sources¶
  - Accessing underlying data¶
- Insert a list¶
  - List functions¶
  - List properties¶
  - Manage an inserted list¶
    - Add records/rows to a list¶
    - Add fields/columns to a list¶
    - Duplicate a list¶

Several elements from your Odoo database can be inserted into an Odoo spreadsheet, namely:

lists, i.e., data from a list view

pivot tables, i.e., data from a pivot view

charts, i.e., data from a graph view

Lists, pivot tables, and charts from different apps and models can be inserted into the same spreadsheet.

Each time a list, pivot table, or chart is inserted, a data source is created. This data source connects the spreadsheet to your Odoo database, retrieving up-to-date information every time the spreadsheet is opened, the browser page is reloaded, or data is manually refreshed by clicking Data ‣ Refresh all data from the menu bar.

Inserted lists and inserted pivot tables use formulas with Odoo-specific list functions and pivot table functions to retrieve data from your database and can be further manipulated in the spreadsheet. Certain elements of inserted charts can be modified, but no data manipulation or computation is possible.

If you intend to use global filters to dynamically filter Odoo data in a spreadsheet or dashboard, do not use the same conditions to establish the initial list, pivot table, or chart in your database.

It is also possible to:

add clickable links to Odoo menu items, to other sheets of the same spreadsheet, or to external URLs

insert financial data from your Odoo database using Odoo-specific spreadsheet functions

paste data from another Odoo spreadsheet, Excel spreadsheet, or Google Sheet directly into any Odoo spreadsheet

Data sources, which are created each time a list, pivot table, or chart is inserted into an Odoo spreadsheet, connect the spreadsheet and the relevant model in your database, ensuring the data stays up-to-date and allowing you to access the underlying data.

Each data source is defined by properties that can be accessed via the Data menu. Data sources are identified by their respective (pivot table), (list) or (chart) icon, followed by their ID and name, e.g., (#1) Sales Analysis by Product.

Clicking on a data source opens the related properties in a panel on the right of the spreadsheet.

The properties panel can also be opened by right-clicking any cell of an inserted list or pivot table, then clicking See list properties or See pivot properties, or by clicking the (menu) icon at the top right of an inserted chart, then clicking Edit.

Once the properties of a specific data source are open, they remain open even when navigating between spreadsheet tabs. To close the properties panel, click the (close) icon at the top right of the panel.

Click (pin) at the top of the properties panel to allow another panel, such as the global filters panel, to open beside it.

Deleting an inserted list or pivot table, or deleting the sheet into which it was inserted, does not delete the underlying data source. The data source of an inserted list or pivot table can only be deleted via the data source’s properties.

A warning in the Data menu identifies any data sources for which the corresponding list or pivot table no longer appears in the spreadsheet.

Deleting an inserted chart, on the other hand, also deletes the underlying data source.

The underlying data of an inserted list, pivot table, or chart can be accessed at any time. To view:

an individual record of an inserted list, right-click any cell of the relevant row, then select See record

a list of records referenced by an individual cell of an inserted pivot table, right-click the cell, then select See records

a list of records represented by a data point of an inserted chart, click the data point.

Use the middle mouse button or Ctrl + left-click (Microsoft/Linux), or Command + left-click (Mac OS) to open the results in a new browser tab.

To return to the spreadsheet after viewing the underlying data, click the name of the spreadsheet in the breadcrumbs at the top of the page.

Before inserting a list in a spreadsheet, ensure the list is tailored to your needs. Consider which fields should be visible as well as how the records are filtered and/or sorted. This can impact both the loading time and the user-friendliness of your spreadsheet.

With the relevant list view open in your database, click the (Actions) icon beside the name of the view, then Spreadsheet ‣ Insert list in spreadsheet.

To insert only specific records, select the relevant records, click the Actions button that appears at the top center of the screen, then Insert in spreadsheet.

In the window that opens, edit the Name of the list if needed.

The list name is used in the sheet name and in the list properties.

Edit the number of records, i.e., rows, to be inserted if needed.

By default, the number shown is the number of records visible on the first page of the list. For example, if the list contains 150 records but only 80 are visible, this field will show 80.

While the data in your list is kept up to date thanks to the connection to your database, an inserted list will not automatically expand to accommodate new records, e.g., a new product category or a new salesperson.

If you anticipate new records being added, consider adding extra rows when inserting the list. Records/rows can also be added manually after the spreadsheet has been inserted.

Your company currently has ten product categories and you insert this list in a spreadsheet. If an 11th product category is created and your inserted list only had ten rows, the new category will be inserted in the appropriate position in the spreadsheet, thereby removing an existing category.

One way to avoid this is to add extra rows when inserting the list.

Click Blank spreadsheet to create a new spreadsheet, or select in which existing spreadsheet the list should be inserted.

When inserting a list into a new spreadsheet, the spreadsheet is saved in the Odoo Documents app in the My Drive personal folder.

The list is inserted into a new sheet in the spreadsheet. The sheet tab in the bottom bar shows the name of the list followed by the list ID, e.g., Quotations by Total (List #1). A panel on the right side of the screen shows the list properties.

To sever the link between an inserted list and your database, select the entire list, right-click and select Copy then right-click again and select Paste special ‣ Paste as value.

Do not modify the list ID in the sheet name, as the inserted list retains this ID for the lifetime of the spreadsheet. This list ID is used in the spreadsheet functions that retrieve data from your database.

When a list is inserted into a spreadsheet, the following functions are used to retrieve the header and field values, respectively:

The arguments of the function are as follows:

list_id: the ID assigned when the list is inserted. The first list inserted into a spreadsheet is assigned list ID 1, the second, list ID 2, etc.

index: identifies the line on which the record appeared in the list before insertion. The first line has an index of 1, the second an index of 2, etc.

field_name: the technical name of the field.

Clicking on an individual cell displays the related formula, if relevant, in the formula bar. To display all the formulas of a spreadsheet at the same time, click View ‣ Show ‣ Formulas on the menu bar. The example below shows the functions used to retrieve list headers and values.

The list properties appear on the right side of the screen when a list is inserted. They can be accessed at any time via the Data menu by clicking the relevant list, as prefaced by the (list) icon, or by right-clicking anywhere on the list and clicking See list properties.

The following list properties are shown, some of which can be edited:

List #: the list ID. List IDs are assigned sequentially as additional lists are inserted into the spreadsheet.

List Name: the name of the list. Edit this if needed. Note that editing the list name in the list properties does not modify the list name shown in the sheet name, and vice versa.

Model: the model from which the data has been extracted.

Columns: the fields of the model that were visible when the list was inserted.

Domain: the rules used to determine which records are shown. Click Edit domain to add or edit rules.

When global filters are used, this domain is combined with the selected values of the global filter before the data is loaded into the spreadsheet.

Sorting: how the data is sorted, if applicable. To add a sorting rule, click Add, select the field, then choose whether sorting should be Ascending or Descending. Delete a sorting rule by clicking the (delete) icon.

To duplicate or delete a list’s data source, click the (gear) icon, then click Duplicate or Delete as relevant.

After a list from an Odoo database has been inserted into an Odoo spreadsheet, you can:

add records, i.e., rows

add fields, i.e., columns

duplicate the list to create a new, identical data source

delete the list and its underlying data source

To add records to a list, use one of the following methods:

Select the last row of the table, then hover over the blue square until the plus icon appears. Click and drag down to add the desired number of rows. The cells of the new rows are populated with the appropriate formula to retrieve the list values. If there is corresponding data in your database, the cells are populated.

Position your cursor in the top left cell of the sheet, click Data ‣ Re-insert list from the menu bar, then select the appropriate list. In the pop-up window, indicate the number of records to insert and click Confirm. An updated list is inserted, overwriting the previous list.

The above methods can also be used to add additional blank rows to your spreadsheet table. This may be useful for lists where you expect additional records to be generated in your database, e.g., new product categories or new salespersons.

To add fields/columns to a list:

Select the column to the right or left of where the new column should be inserted.

Click Insert ‣ Insert column then Column left or Column right from the menu bar, or right-click then Insert column left or Insert column right as appropriate.

Copy the header cell of any column, paste it into the header cell of the new column, and press Enter.

Double-click the new header cell then click on the field name that appears in quotation marks at the end of the formula; a list of all the technical names of the fields of the related model appears.

Select the appropriate field name and press Enter. The field’s label appears in the header.

To know a field’s technical name, navigate to the relevant view, activate developer mode, then check the field name by hovering over the question mark beside a field’s label.

With the header cell selected, double-click on the blue square in the bottom-right corner. The cells of the column are populated with the appropriate formula to retrieve the list values. If there is corresponding data in your database, the cells are populated.

Duplicating a list via the list’s properties creates an additional data source. This allows for different manipulations to be performed on the same data within one spreadsheet.

With the list properties open, click the (gear) icon then Duplicate.

The new data source is assigned the next available list ID. For example, if no other lists have been inserted in the meantime, duplicating List #1 results in the creation of List #2.

Unlike when you insert a list, a duplicated list is not automatically inserted into the spreadsheet. To insert it, perform the following steps:

Add a new sheet by clicking the (add sheet) icon at the bottom left of the spreadsheet.

Click Data ‣ Re-insert list from the menu bar, then select the appropriate list.

Define the number of records to insert and click Confirm.

Edit the List Name in the properties panel if needed.

Rename the sheet by right-clicking on the sheet tab, selecting Rename, and entering the new sheet name.

Duplicating an inserted list by copying and pasting it or by duplicating the sheet into which it has been inserted does not create a new data source. Any changes made to the list’s properties would therefore impact any copies of the list.

To fully delete a list and the underlying data source from a spreadsheet, perform the following steps in any order:

Delete the spreadsheet table using your preferred means, e.g., via keyboard commands, spreadsheet menus, or by deleting the sheet. This deletes the visual representation of the data.

From the properties panel of the relevant list, click the (gear) icon then Delete. This deletes the data source of the list from the spreadsheet.

Converting an inserted pivot table to a dynamic pivot table allows you to add, remove, and manipulate dimensions (i.e., columns and rows) and measures. It is therefore possible to insert a basic pivot table with minimal configuration, convert it to a dynamic pivot table, then refine it directly in the spreadsheet.

To insert a pivot table:

With the relevant pivot view open in your database, click Insert in Spreadsheet.

In the window that opens, edit the Name of the pivot if needed.

This name is used in the sheet name and in the pivot table properties.

Click Blank spreadsheet to create a new spreadsheet, or select in which existing spreadsheet the pivot table should be inserted.

When inserting a pivot table into a new spreadsheet, the spreadsheet is saved in the Odoo Documents app in the My Drive personal folder.

The pivot table is inserted into a new sheet in the spreadsheet. The sheet tab in the bottom bar shows the name of the pivot table followed by the pivot table ID, e.g., Sales Analysis by Sales Team (Pivot #1). A panel on the right side of the screen shows the pivot table properties.

To sever the link between an inserted pivot table and your database, select the entire pivot table, right-click and select Copy, then right-click again and select Paste special ‣ Paste as value.

Do not modify the pivot table ID in the sheet name, as the inserted pivot table retains this ID for the lifetime of the spreadsheet. This pivot table ID is used in the spreadsheet functions that retrieve data from your database.

An inserted pivot table that has not been converted to a dynamic pivot table uses the following functions to retrieve the header and field values, respectively:

The arguments of the functions are as follows:

pivot_id: the ID assigned when the pivot table is inserted. The first pivot table inserted in a spreadsheet is assigned pivot ID 1, the second, pivot ID 2, etc.

measure_name: the technical name of what is being measured, followed by the type of aggregation, e.g., product_uom_qty:sum.

domain_field_name: the technical name of the field used as a dimension, e.g., user_id, or, if the dimension is a time period, the technical name of the date field, followed by the time period, e.g., date_order:month.

domain_value: the ID of the record, or, if the dimension is a time period, the date or time period targeted.

Clicking on an individual cell displays the related formula, if relevant, in the formula bar. To display all the formulas of a spreadsheet at the same time, click View ‣ Show ‣ Formulas on the menu bar. The example below shows the functions used to retrieve headers and values of a static pivot table.

The pivot table properties appear on the right side of the screen when a pivot table is inserted. They can be accessed at any time via the Data menu by clicking the relevant pivot table, as prefaced by the (pivot) icon, or by right-clicking anywhere on the pivot table and clicking See pivot properties.

The following pivot table properties are shown, some of which can be edited:

Pivot #: the pivot table ID. Pivot table IDs are assigned sequentially as additional pivot tables are inserted in the spreadsheet.

Name: the name of the pivot table. Edit this if needed. Note that editing the name in the pivot table properties does not modify the name shown in the sheet name, and vice versa.

Model: the model from which the data has been extracted.

Columns and Rows: dimensions you are using to categorize or group data from the model.

Measures: what you are measuring, or analyzing, based on the dimensions you have chosen.

If you attempt to make changes to the columns, rows, or measures of a pivot table that has just been inserted into a spreadsheet, an error appears at the top right of the screen.

To be able to manipulate a pivot table’s properties, convert a static pivot table to a dynamic pivot table.

Domain: the rules used to determine which records are shown. Click Edit domain to add or edit rules.

When global filters are used, this domain is combined with the selected values of the global filter before the data is loaded into the spreadsheet.

To duplicate or delete a pivot table’s data source, click the (gear) icon then Duplicate or Delete.

After a pivot table from an Odoo database has been inserted into an Odoo spreadsheet, you can:

convert it to a dynamic pivot table to be able to manipulate the dimensions and measures

duplicate the pivot table to create a new, identical data source

delete the pivot table and its underlying data source

Duplicating a pivot table via the pivot table’s properties creates an additional data source. This allows for different manipulations to be performed on the same data within one spreadsheet.

For example, you can see the same data aggregated by different dimensions or use global filters to offset the date and create pivot tables that compare the current period’s data with a previous period.

To duplicate a pivot table, perform the following steps:

With the pivot table properties open, click the (gear) icon then Duplicate.

The duplicated pivot table is automatically inserted into a new sheet in the spreadsheet, with the pivot table properties open in the right panel.

Edit the Name in the properties panel and the sheet tab if needed.

The new data source is assigned the next available pivot table ID. For example, if no other pivot tables have been inserted in the meantime, duplicating Pivot #1 results in the creation of Pivot #2.

Duplicating an inserted pivot table by copying and pasting it or by duplicating the sheet does not create a new data source. Any changes made to the pivot table’s properties would therefore impact any copies of the pivot table.

When a pivot table is duplicated, the new pivot table is by default a dynamic pivot table.

To fully delete a pivot table and the underlying data source from a spreadsheet, perform the following steps in any order:

Delete the spreadsheet table using your preferred means, e.g., via keyboard commands, spreadsheet menus, or by deleting the sheet. This deletes the visual representation of the data.

From the properties panel of the relevant pivot table, click the (gear) icon then Delete. This deletes the data source of the pivot table.

To insert a chart from an Odoo database into an Odoo spreadsheet:

With the relevant graph view open in your database, click Insert in Spreadsheet.

In the window that opens, edit the Name of the graph if needed.

Click Blank spreadsheet to create a new spreadsheet, or select in which existing spreadsheet the chart should be inserted.

When inserting a chart into a new spreadsheet, the spreadsheet is saved in the Odoo Documents app in the My Drive personal folder.

Charts are inserted on the first sheet of the spreadsheet. A pane on the right side of the screen shows the chart properties, where various aspects of the chart’s configuration and design can be modified.

Clicking on a data point in a chart opens the relevant list view in the database. In the example, clicking on Jessica Childs opens the list view of all sales by this salesperson that match the domain of the chart.

Adding links to related or supporting information can make your report or dashboard more user-friendly and effective.

You can insert a clickable link from any spreadsheet cell to:

another sheet inside the same spreadsheet

You can insert a clickable link from any chart to an Odoo menu item.

Clicking a link to a menu item provides the same result as navigating via the Odoo menu within an app, e.g., the menu item Sales/Orders/Quotations corresponds to the default view when navigating to Sales ‣ Orders ‣ Quotations.

It is also possible to insert a clickable link to a specific view of a model in a spreadsheet starting from the view itself. However, as this method inserts each new link in a new sheet, it is more efficient to create links to specific views starting from the spreadsheet.

Use the middle mouse button or Ctrl + left-click (Microsoft/Linux), or Command + left-click (Mac OS) to open clickable links in a new browser tab.

To insert a clickable link from a cell:

Click Insert ‣ Link from the menu bar or right-click on the cell, then click Insert link. Next, depending on the desired outcome, perform one of the following actions:

Click the (menu) icon, then Link an Odoo menu. Select the relevant menu item from the list or click Search more to choose from a list of all menu items. Click Confirm.

Click the (menu) icon, then Link sheet, then choose the relevant sheet from the current spreadsheet.

Under Link, type a URL.

Enter or edit the label for the link in the Text field.

To insert a clickable link from a chart to an Odoo menu item:

Hover over the top right of the chart’s box, then click the (menu) icon, then Edit. The chart properties appear at the right of the screen.

At the bottom of the Configuration tab of the chart properties panel, click under Link to Odoo menu, then select a menu.

Hover over the top right of the chart’s box to see that a new (external link) icon has been added.

When building reports and dashboards, it may be useful to include certain accounting-related data, such as account IDs, credits and debits for specific accounts, and dates of the start and end of the tax year.

Odoo-specific spreadsheet functions allow you to retrieve such accounting data from your database and insert it into a spreadsheet.

**Examples:**

Example 1 (unknown):
```unknown
=ODOO.LIST.HEADER(list_id, field_name)
=ODOO.LIST(list_id, index, field_name)
```

Example 2 (unknown):
```unknown
=PIVOT.HEADER(pivot_id, [domain_field_name, …], [domain_value, …])
=PIVOT.VALUE(pivot_id, measure_name, [domain_field_name, …], [domain_value, …])
```

---

## LDAP authentication¶

**URL:** https://www.odoo.com/documentation/19.0/applications/general/users/ldap.html

**Contents:**
- LDAP authentication¶

To configure LDAP authentication in Odoo:

Open the Settings app, scroll down to the Integrations section, and enable LDAP Authentication.

Click Save, then go back to the Integrations section and click LDAP Server.

In the Set up your LDAP Server list, click New, then select the required company in the dropdown list.

In the Server information section, enter the server’s IP address and port in the LDAP server address and LDAP Server port fields, respectively.

Enable Use TLS to request secure TLS/SSL encryption when connecting to the LDAP server, providing the server has StartTLS enabled.

In the Login information section, enter the ID and password of the account used to query the server in the LDAP binddn and LDAP password fields, respectively. If the fields are left empty, the server will perform the query anonymously.

In the Process parameter section, enter:

the LDAP server’s name in the LDAP base field using LDAP format (e.g., dc=example,dc=com);

uid=%s in the LDAP filter field.

In the User information section:

Enable Create user to create a user profile in Odoo the first time someone logs in using LDAP;

Select the User template to be used to create the new user profiles. If no template is selected, the administrator’s profile is used.

When using Microsoft Active Directory (AD) for LDAP authentication, if users experience login issues despite using valid credentials, create a new system parameter to disable referral chasing in the LDAP client:

Activate the developer mode.

Go to Settings ‣ Technical ‣ System Parameters and click New.

Key: auth_ldap.disable_chase_ref

---

## Pro-forma invoices¶

**URL:** https://www.odoo.com/documentation/19.0/applications/sales/sales/invoicing/proforma.html

**Contents:**
- Pro-forma invoices¶
- Configuration¶
- Send pro-forma invoice¶

A pro-forma invoice is an abridged or estimated invoice sent in advance of a delivery of goods. It notes the kind and quantity of goods, their value, and other important information, such as weight and transportation charges.

Pro-forma invoices are commonly used as preliminary invoices with a quotation. They are also used during importation for customs purposes. They differ from a normal invoice, in that they are not a demand (or request) for payment.

In order to utilize pro-forma invoices, the Pro-Forma Invoice feature must be activated.

To enable this feature, navigate to Sales app ‣ Configuration ‣ Settings, and in the Quotations & Orders section, click the checkbox next to Pro-Forma Invoice. Then, click Save to save all changes.

With the Pro-Forma Invoice feature activated, the option to send a pro-forma invoice is now available on any quotation or sales order, via the Send Pro-Forma Invoice button.

Pro-forma invoices can not be sent for a sales order or quotation if an invoice for a down payment has already been sent, or for a recurring subscription.

In either case, the Send Pro-Froma Invoice button does not appear.

However, pro-forma invoices can be sent for services, event registrations, courses, and/or new subscriptions. Pro-forma invoices are not limited to physical, consumable, or storable goods.

When the Send Pro-Forma Invoice button is clicked, a pop-up window appears, from which an email can be sent.

In the pop-up window, the Recipients field is auto-populated with the customer from the sales order or quotation. The Subject field and the body of the email can be modified, if necessary.

The pro-forma invoice is automatically added as an attachment to the email.

When ready, click Send, and Odoo instantly sends the email, with the attached pro-forma invoice, to the customer.

To preview what the pro-forma invoice looks like, click on the PDF at the bottom of the email pop-up window before clicking Send. When clicked, the pro-forma invoice is downloaded instantly. Open that PDF to view (and review) the pro-forma invoice.

---

## Data Cleaning¶

**URL:** https://www.odoo.com/documentation/19.0/applications/productivity/data_cleaning.html

**Contents:**
- Data Cleaning¶
- Install modules¶
- Deduplication¶
  - Merge duplicate records¶
  - Deduplication rules¶
    - Modify a deduplication rule¶
    - Manually run a deduplication rule¶
- Recycle records¶
  - Recycle record rules¶
    - Manually run a recycle rule¶

The Odoo Data Cleaning app maintains data integrity and consistency with the following features:

Deduplicates: merges or removes duplicate entries to ensure data is unique.

Recycles: identifies outdated records to either archive or delete them.

Formats: standardizes text data by finding and replacing it according to specified needs.

Customizable rules ensure text data stays up-to-date, streamlined, consistently formatted, and aligned with company-specific formatting requirements.

The Data Cleaning application consists of several modules. Install the following to access all available features:

Base module to enable the recycle feature, available on Odoo Community edition.

Enables field cleaning feature to format text data across multiple records, available only on Odoo Enterprise edition.

Enables the deduplication feature to find similar (or duplicate) records, and merge them, available only on Odoo Enterprise edition.

Enables the deduplication feature on the CRM app, and uses the CRM default merging feature.

Enables the merge feature for the Helpdesk app.

Enables the merge feature for the Projects app.

Enables the merge feature for the UTM Tracker app.

Creates a warning in cases of products merging that may affect inventory valuation, if the Inventory app is installed.

The Duplicates dashboard groups similar records to be merged by matching conditions within the records set by the deduplication rules.

Navigate to this dashboard by going to Data Cleaning app ‣ Deduplication.

The RULE sidebar lists each of the active deduplication rules, and displays the total number of duplicates detected beside each rule.

By default, the All rule is selected. Records are grouped by their rule, with a Similarity rating (out of 100%), with the following columns:

Created On: the date and time the original record was created.

Name: the name or title of the original record.

Field Values: the original record’s values for the fields used to detect duplicates.

Used In: lists other models referencing the original record.

ID: the original record’s unique ID.

Is Master: the duplicates are merged into the master record. There can only be one master record in a grouping of similar records.

Select a specific rule in the RULE sidebar to filter the duplicate records.

To merge records, first choose a master record within the grouping of similar records. The master record acts as the base, at which any additional information from similar records are merged into.

Optionally, no master record can be set, leaving Odoo to choose a record at random to merge into.

Next, click the Merge button at the top of the similar records grouping. Then, click Ok to confirm the merge.

Once a record is merged, a message is logged in the chatter of the master record, describing the merge. Certain records, like Project tasks, are logged in the chatter with a link to the old record as a convenient reference of the merge.

Discard groupings by clicking the DISCARD button. Upon doing so, the grouping is hidden from the list and archived.

View discarded groupings by selecting the Discarded filter from the search bar.

The Deduplication Rules set the conditions for how records are detected as duplicates.

These rules can be configured for each model in the database, and with varying levels of specificity. To get started, navigate to Data Cleaning app ‣ Configuration ‣ Deduplication.

The deduplication rules run once every day, by default, as part of a scheduled action cron (Data Merge: Find Duplicate Records). However, each rule can be ran manually anytime.

Select a default rule to edit, or create a new rule by clicking on the New button.

First, choose a Model for this rule to target. Selecting a model updates the rule title to the chosen model.

Optionally, configure a Domain to specify the records eligible for this rule. The number of eligible records is shown in the # record(s) link.

Depending on the selected Model, the Duplicate Removal field appears. Choose whether to Archive or Delete merged records.

Next, select a Merge Mode:

Manual: requires each duplicate grouping to be manually merged, also enables the Notify Users field.

Automatic: automatically merges duplicate groupings, without notifying users, based on the records with a similarity percentage above the threshold set in the Similarity Threshold field.

Enable the Active toggle to start capturing duplicates with this rule as soon as it is saved.

Lastly, create at least one deduplication rule in the Deduplication Rules field, by clicking Add a line, under the Unique ID Field column.

Select a field in the model from the Unique ID Field drop-down menu. This field is referenced for similar records.

Select a matching condition in the Match If field to apply the deduplication rule, depending on the text in the Unique ID Field:

Exact Match: the characters in the text match exactly.

Case/Accent Insensitive Match: the characters in the text match, regardless of casing and language-specific accent differences.

At least one Deduplication Rules must be set for the rule to capture duplicates.

A few more fields are available for an advanced configuration.

If on a multi-company database, the Cross-Company field is available. When enabled, duplicates across different companies are suggested.

Activate Developer mode (debug mode) to display the Suggestion Threshold field. Duplicates with a similarity below the threshold set in this field are not suggested.

With the rule’s configuration complete, either close the rule form, or run the rule manually to instantly capture duplicate records.

To manually run a specific deduplication rule at any time, navigate to Data Cleaning app ‣ Configuration ‣ Deduplication, and select the rule to run.

Then, on the rule form, select the Deduplicate button on the top-left. Upon doing so, the Duplicates smart button displays the number of duplicates captured.

Click on the Duplicates smart button to manage these records.

Use the recycle records feature to rid the database of old and outdated records.

The Field Recycle Records dashboard displays records that can be archived or deleted, by matching conditions within the records set by the recycle record’s rules.

Navigate to this dashboard by going to Data Cleaning app ‣ Recycle Records.

The RECYCLE RULES sidebar lists each of the active recycle record rules.

By default, the All option is selected. Records are displayed with the following columns:

Record ID: the ID of the original record.

Record Name: the name or title of the original record.

Select a specific rule in the RECYCLE RULES sidebar to filter the records.

To recycle records, click the Validate button on the row of the record.

Upon doing so, the record is recycled, depending on how the rule is configured, to be either archived or deleted from the database.

Discard groupings by clicking the Discard button. Upon doing so, the record is hidden from the list, and is not detected by the recycle rule again in the future.

View discarded records by selecting the Discarded filter from the search bar drop-down menu.

The Recycle Records Rules set the conditions for how records are recycled.

These rules can be configured for each model in the database, and with varying levels of specificity. To get started, navigate to Data Cleaning app ‣ Configuration ‣ Recycle Records.

Recycle rules run once a day, by default, as part of a scheduled action cron (Data Recycle: Clean Records). However, each rule can be run manually anytime.

By default, no recycle record rules exist. Click the New button to create a new rule.

On the recycle record rule form, first choose a Model for this rule to target. Selecting a model updates the rule title to the chosen model.

Optionally, configure a Filter to specify the records eligible for this rule. The number of eligible records is shown in the # record(s) link.

Next, configure the field and time range for how the rule detects the records to recycle:

Time Field: select a field from the model to base the time (Delta).

Delta: type the length of time, which must be a whole number (e.g. 7).

Delta Unit: select the unit of time (Days, Weeks, Months, or Years).

Then, select a Recycle Mode:

Manual: requires each detected record to be manually recycled, and enables the Notify Users field.

Automatic: automatically merges recycled groupings, without notifying users.

Lastly, select a Recycle Action to either Archive or Delete records. If Delete is selected, choose whether or not to Include Archived records in the rule.

With the rule’s configuration complete, either close the rule form, or run the rule manually to instantly capture records to recycle.

A recycle rule can be configured to delete archived leads and opportunities that were last updated a year ago, and with a specific lost reason, by using the following configuration:

Model: Lead/Opportunity

Lost Reason is in Too expensive

Time Field: Last Updated on (Lead/Opportunity)

Recycle Mode: Automatic

Recycle Action: Delete

To manually run a specific recycle rule at any time, navigate to Data Cleaning app ‣ Configuration ‣ Recycle Records, and select the rule to run.

Then, on the rule form, click the Run Now button on the top-left. Upon doing so, the Records smart button displays the number of records captured.

Click the Records smart button to manage these records.

Use the field cleaning feature to maintain consistent formatting of names, phone numbers, IDs and other fields throughout a database.

The Field Cleaning Records dashboard displays formatting changes to data in fields of a record, to follow a convention set by the field cleaning rules.

Navigate to this dashboard by going to Data Cleaning app ‣ Field Cleaning.

The CLEANING RULES sidebar lists each of the active cleaning rules.

By default, the All rule is selected. Records are listed with the following columns:

Record ID: the ID of the original record.

Record Name: the name or title of the original record.

Field: the original record’s field that contains the value to format.

Current: the current value in the field of the original record.

Suggested: the suggested formatted value in the field of the original record.

To clean and format records, click the Validate button on the row of the record.

Upon doing so, the record is formatted and/or cleaned.

Discard records by clicking the Discard button. Upon doing so, the record is hidden from the list and will not be detected by the field cleaning rule again in the future.

View discarded records by selecting the Discarded filter from the search bar.

The Field Cleaning Rules set the conditions for fields to be cleaned and/or formatted.

These rules can be configured for each model in the database, and with varying levels of specificity. To get started, navigate to Data Cleaning app ‣ Configuration ‣ Field Cleaning.

The field cleaning rules run once every day, by default, as part of a scheduled action cron (Data Cleaning: Clean Records). However, each rule can be ran manually anytime.

By default, a Contact rule exists to format and clean up the Contacts app records. Select the Contact record to make edits, or select the New button to create a new rule.

On the field cleaning rule form, first choose a Model for this rule to target. Selecting a model updates the rule title to the chosen model.

Next, configure at least one rule by clicking Add a line in the Rules section.

Upon doing so, a Create Rules popover window appears with the following fields to configure:

Select a Field To Clean from the model to assign to an action.

Choose one of the following Action options:

Trim Spaces reveals the Trim field to select the All Spaces or Superfluous Spaces option. Leading, trailing, and successive spaces are considered superfluous.

The contact name Dr. John Doe can be formatted with the following Trim options:

All Spaces: DR.JohnDoe

Superfluous Spaces: DR. John Doe

Set Type Case reveals the Case field to select either First Letters to Uppercase, All Uppercase, or All Lowercase.

The lead/opportunity title lumber inc, Lorraine douglas can be formatted with the following Case options:

First Letters to Uppercase: Lumber Inc, Lorraine Douglas

All Uppercase: LUMBER INC, LORRAINE DOUGLAS

All Lowercase: lumber inc, lorraine douglas

Format Phone converts the phone number to an international country format.

Belgium: 061928374 +32 61 92 83 74

United States: 800 555-0101 +1 800-555-0101

Scrap HTML converts HTML to plain text.

Once a field and action are selected, click Save to close the Create Rules popover window.

Then, select a Cleaning Mode:

Manual: requires each detected field to be manually cleaned and enables the Notify Users field.

Automatic: automatically cleans fields without notifying users.

With the rule’s configuration complete, either close the rule form, or run the rule manually to instantly capture fields to clean.

To manually run a specific field cleaning rule at any time, navigate to Data Cleaning app ‣ Configuration ‣ Field Cleaning, and select the rule to run.

Then, on the rule form, select the Clean button on the top-left. Upon doing so, the Records smart button displays the number of records captured.

Click on the Records smart button to manage these records.

The Merge Action Manager enables or disables the Merge action available in the Actions menu for models in the database.

Enable Developer mode (debug mode) and navigate to Data Cleaning app ‣ Configuration ‣ Merge Action Manager.

Models are listed with the following columns:

Model: technical name of the model.

Model Description: display name of the model.

Type: whether the model is of the Base Object or Custom Object type.

Transient Model: the model handles temporary data that does not need to be stored long-term in the database.

Can Be Merged: enables the Merge action for the model.

To view which models are enabled by default, use the search bar to filter models that Can Be Merged.

**Examples:**

Example 1 (jsx):
```jsx
<h1>John Doe</h1>
<p>Lorem ipsum dolor sit <a href="https://example.com">amet</a>.</p>
```

Example 2 (unknown):
```unknown
**John Doe** Lorem ipsum dolor sit amet [1] .[1] https://example.com
```

---

## ORM API¶

**URL:** https://www.odoo.com/documentation/19.0/developer/reference/backend/orm.html

**Contents:**
- ORM API¶
- Models¶
  - AbstractModel¶
  - Model¶
  - TransientModel¶
- Fields¶
  - Basic Fields¶
  - Advanced Fields¶
    - Date(time) Fields¶
    - Relational Fields¶

Model fields are defined as attributes on the model itself:

this means you cannot define a field and a method with the same name, the last one will silently overwrite the former ones.

By default, the field’s label (user-visible name) is a capitalized version of the field name, this can be overridden with the string parameter.

For the list of field types and parameters, see the fields reference.

Default values are defined as parameters on fields, either as a value:

or as a function called to compute the default value, which should return that value:

Base class for Odoo models.

Odoo models are created by inheriting one of the following:

Model for regular database-persisted models

TransientModel for temporary data, stored in the database but automatically vacuumed every so often

AbstractModel for abstract super classes meant to be shared by multiple inheriting models

The system automatically instantiates every model once per database. Those instances represent the available models on each database, and depend on which modules are installed on that database. The actual class of each instance is built from the Python classes that create and inherit from the corresponding model.

Every model instance is a “recordset”, i.e., an ordered collection of records of the model. Recordsets are returned by methods like browse(), search(), or field accesses. Records have no explicit representation: a record is represented as a recordset of one record.

To create a class that should not be instantiated, the _register attribute may be set to False.

Whether a database table should be created. If set to False, override init() to create the database table.

Automatically defaults to True for abstract models.

To create a model without any table, inherit from AbstractModel.

Whether the ORM should automatically generate and update the Access Log fields.

Defaults to whatever value was set for _auto.

SQL table name used by model if _auto

Whether the model is abstract.

Whether the model is transient.

the model name (in dot-notation, module namespace)

the model’s informal name

Python-inherited models:

str or list(str) or tuple(str)

If _name is set, name(s) of parent models to inherit from

If _name is unset, name of a single model to extend in-place

dictionary {‘parent_model’: ‘m2o_field’} mapping the _name of the parent business objects to the names of the corresponding foreign key fields to use:

implements composition-based inheritance: the new model exposes all the fields of the inherited models but stores none of them: the values themselves remain stored on the linked record.

if multiple fields with the same name are defined in the _inherits-ed models, the inherited field will correspond to the last one (in the inherits list order).

field to use for labeling records, default: name

default order field for searching results

On write and create, call _check_company to ensure companies consistency on the relational fields having check_company=True as attribute.

the many2one field used as parent field

set to True to compute parent_path field.

Alongside a parent_path field, sets up an indexed storage of the tree structure of records, to enable faster hierarchical queries on the records of the current model using the child_of and parent_of domain operators.

field to determine folded groups in kanban views

alias of odoo.orm.models.BaseModel

Main super-class for regular database-persisted Odoo models.

Odoo models are created by inheriting from this class:

The system will later instantiate the class once per database (on which the class’ module is installed).

Whether a database table should be created. If set to False, override init() to create the database table.

Automatically defaults to True for abstract models.

To create a model without any table, inherit from AbstractModel.

Whether the model is abstract.

Model super-class for transient records, meant to be temporarily persistent, and regularly vacuum-cleaned.

A TransientModel has a simplified access rights management, all users can create new records, and may only access the records they created. The superuser has unrestricted access to all TransientModel records.

maximum number of transient records, unlimited if 0

maximum idle lifetime (in hours), unlimited if 0

Clean the transient records.

This unlinks old records from the transient model tables whenever the _transient_max_count or _transient_max_hours conditions (if any) are reached.

Actual cleaning will happen only once every 5 minutes. This means this method can be called frequently (e.g. whenever a new record is created).

Example with both max_hours and max_count active:

Suppose max_hours = 0.2 (aka 12 minutes), max_count = 20, there are 55 rows in the table, 10 created/changed in the last 5 minutes, an additional 12 created/changed between 5 and 10 minutes ago, the rest created/changed more than 12 minutes ago.

age based vacuum will leave the 22 rows created/changed in the last 12 minutes

count based vacuum will wipe out another 12 rows. Not just 2, otherwise each addition would immediately cause the maximum to be reached again.

the 10 rows that have been created/changed the last 5 minutes will NOT be deleted

The field descriptor contains the field definition, and manages accesses and assignments of the corresponding field on records. The following attributes may be provided when instantiating a field:

string (str) – the label of the field seen by users; if not set, the ORM takes the field name in the class (capitalized).

help (str) – the tooltip of the field seen by users

readonly (bool) – whether the field is readonly (default: False) This only has an impact on the UI. Any field assignation in code will work (if the field is a stored field or an inversable one).

whether the field is readonly (default: False)

This only has an impact on the UI. Any field assignation in code will work (if the field is a stored field or an inversable one).

required (bool) – whether the value of the field is required (default: False)

index (str) – whether the field is indexed in database, and the kind of index. Note: this has no effect on non-stored and virtual fields. The possible values are: "btree" or True: standard index, good for many2one "btree_not_null": BTREE index without NULL values (useful when mostvalues are NULL, or when NULL is never searched for) "trigram": Generalized Inverted Index (GIN) with trigrams (good for full-text search) None or False: no index (default)

whether the field is indexed in database, and the kind of index. Note: this has no effect on non-stored and virtual fields. The possible values are:

"btree" or True: standard index, good for many2one

values are NULL, or when NULL is never searched for)

"trigram": Generalized Inverted Index (GIN) with trigrams (good for full-text search)

None or False: no index (default)

default (value or callable) – the default value for the field; this is either a static value, or a function taking a recordset and returning a value; use default=None to discard default values for the field

groups (str) – comma-separated list of group xml ids (string); this restricts the field access to the users of the given groups only

company_dependent (bool) – whether the field value is dependent of the current company; The value is stored on the model table as jsonb dict with the company id as the key. The field’s default values stored in model ir.default are used as fallbacks for unspecified values in the jsonb dict.

whether the field value is dependent of the current company;

The value is stored on the model table as jsonb dict with the company id as the key.

The field’s default values stored in model ir.default are used as fallbacks for unspecified values in the jsonb dict.

copy (bool) – whether the field value should be copied when the record is duplicated (default: True for normal fields, False for one2many and computed fields, including property fields and related fields)

store (bool) – whether the field is stored in database (default:True, False for computed fields)

default_export_compatible (bool) – whether the field must be exported by default in an import-compatible export

search (str) – name of a method that implements search on the field. The method takes an operator and value. Basic domain optimizations are ran before calling this function. For instance, all '=' are transformed to 'in', and boolean fields conditions are made such that operator is 'in'/'not in' and value is [True]. The method should return NotImplemented if it does not support the operator. In that case, the ORM can try to call it with other, semantically equivalent, operators. For instance, try with the positive operator if its corresponding negative operator is not implemented. The method must return a Search domains that replaces (field, operator, value) in its domain. Note that a stored field can actually have a search method. The search method will be invoked to rewrite the condition. This may be useful for sanitizing the values used in the condition, for instance. def _search_partner_ref(self, operator, value): if operator not in ('in', 'like'): return NotImplemented ... # add your logic here, example return Domain('partner_id.ref', operator, value)

name of a method that implements search on the field. The method takes an operator and value. Basic domain optimizations are ran before calling this function. For instance, all '=' are transformed to 'in', and boolean fields conditions are made such that operator is 'in'/'not in' and value is [True].

The method should return NotImplemented if it does not support the operator. In that case, the ORM can try to call it with other, semantically equivalent, operators. For instance, try with the positive operator if its corresponding negative operator is not implemented. The method must return a Search domains that replaces (field, operator, value) in its domain.

Note that a stored field can actually have a search method. The search method will be invoked to rewrite the condition. This may be useful for sanitizing the values used in the condition, for instance.

aggregator (str) – default aggregate function used by the webclient on this field when using “Group By” feature. Supported aggregators are: count : number of rows count_distinct : number of distinct rows bool_and : true if all values are true, otherwise false bool_or : true if at least one value is true, otherwise false max : maximum value of all values min : minimum value of all values avg : the average (arithmetic mean) of all values sum : sum of all values

default aggregate function used by the webclient on this field when using “Group By” feature.

Supported aggregators are:

count : number of rows

count_distinct : number of distinct rows

bool_and : true if all values are true, otherwise false

bool_or : true if at least one value is true, otherwise false

max : maximum value of all values

min : minimum value of all values

avg : the average (arithmetic mean) of all values

sum : sum of all values

group_expand (str) – function used to expand results when grouping on the current field for kanban/list/gantt views. For selection fields, group_expand=True automatically expands groups for all selection keys. @api.model def _read_group_selection_field(self, values, domain): return ['choice1', 'choice2', ...] # available selection choices. @api.model def _read_group_many2one_field(self, records, domain): return records + self.search([custom_domain])

function used to expand results when grouping on the current field for kanban/list/gantt views. For selection fields, group_expand=True automatically expands groups for all selection keys.

compute (str) – name of a method that computes the field See alsoAdvanced Fields/Compute fields

name of a method that computes the field

Advanced Fields/Compute fields

precompute (bool) – whether the field should be computed before record insertion in database. Should be used to specify manually some fields as precompute=True when the field can be computed before record insertion. (e.g. avoid statistics fields based on search/_read_group), many2one linking to the previous record, … (default: False) WarningPrecomputation only happens when no explicit value and no default value is provided to create(). This means that a default value disables the precomputation, even if the field is specified as precompute=True. Precomputing a field can be counterproductive if the records of the given model are not created in batch. Consider the situation were many records are created one by one. If the field is not precomputed, it will normally be computed in batch at the flush(), and the prefetching mechanism will help making the computation efficient. On the other hand, if the field is precomputed, the computation will be made one by one, and will therefore not be able to take advantage of the prefetching mechanism. Following the remark above, precomputed fields can be interesting on the lines of a one2many, which are usually created in batch by the ORM itself, provided that they are created by writing on the record that contains them.

whether the field should be computed before record insertion in database. Should be used to specify manually some fields as precompute=True when the field can be computed before record insertion. (e.g. avoid statistics fields based on search/_read_group), many2one linking to the previous record, … (default: False)

Precomputation only happens when no explicit value and no default value is provided to create(). This means that a default value disables the precomputation, even if the field is specified as precompute=True.

Precomputing a field can be counterproductive if the records of the given model are not created in batch. Consider the situation were many records are created one by one. If the field is not precomputed, it will normally be computed in batch at the flush(), and the prefetching mechanism will help making the computation efficient. On the other hand, if the field is precomputed, the computation will be made one by one, and will therefore not be able to take advantage of the prefetching mechanism.

Following the remark above, precomputed fields can be interesting on the lines of a one2many, which are usually created in batch by the ORM itself, provided that they are created by writing on the record that contains them.

compute_sudo (bool) – whether the field should be recomputed as superuser to bypass access rights (by default True for stored fields, False for non stored fields)

recursive (bool) – whether the field has recursive dependencies (the field X has a dependency like parent_id.X); declaring a field recursive must be explicit to guarantee that recomputation is correct

inverse (str) – name of a method that inverses the field (optional)

related (str) – sequence of field names See alsoAdvanced fields/Related fields

sequence of field names

Advanced fields/Related fields

Basic string field, can be length-limited, usually displayed as a single-line string in clients.

size (int) – the maximum size of values stored for that field

trim (bool) – states whether the value is trimmed or not (by default, True). Note that the trim operation is applied by both the server code and the web client This ensures consistent behavior between imported data and UI-entered data. The web client trims user input during in write/create flows in UI. The server trims values during import (in base_import) to avoid discrepancies between trimmed form inputs and stored DB values.

states whether the value is trimmed or not (by default, True). Note that the trim operation is applied by both the server code and the web client This ensures consistent behavior between imported data and UI-entered data.

The web client trims user input during in write/create flows in UI.

The server trims values during import (in base_import) to avoid discrepancies between trimmed form inputs and stored DB values.

translate (bool or callable) – enable the translation of the field’s values; use translate=True to translate field values as a whole; translate may also be a callable such that translate(callback, value) translates value by using callback(term) to retrieve the translation of terms.

Encapsulates a float.

The precision digits are given by the (optional) digits attribute.

digits (tuple(int,int) or str) – a pair (total, decimal) or a string referencing a DecimalPrecision record name.

When a float is a quantity associated with an unit of measure, it is important to use the right tool to compare or round values with the correct precision.

The Float class provides some static methods for this purpose:

round() to round a float with the given precision. is_zero() to check if a float equals zero at the given precision. compare() to compare two floats at the given precision.

To round a quantity with the precision of the unit of measure:

To check if the quantity is zero with the precision of the unit of measure:

To compare two quantities:

The compare helper uses the __cmp__ semantics for historic purposes, therefore the proper, idiomatic way to use this helper is like so:

if result == 0, the first and second floats are equal if result < 0, the first float is lower than the second if result > 0, the first float is greater than the second

Encapsulates a binary content (e.g. a file).

attachment (bool) – whether the field should be stored as ir_attachment or in a column of the model’s table (default: True).

Encapsulates an html code content.

sanitize (bool) – whether value must be sanitized (default: True)

sanitize_overridable (bool) – whether the sanitation can be bypassed by the users part of the base.group_sanitize_override group (default: False)

sanitize_tags (bool) – whether to sanitize tags (only a white list of attributes is accepted, default: True)

sanitize_attributes (bool) – whether to sanitize attributes (only a white list of attributes is accepted, default: True)

sanitize_style (bool) – whether to sanitize style attributes (default: False)

sanitize_conditional_comments (bool) – whether to kill conditional comments. (default: True)

sanitize_output_method (bool) – whether to sanitize using html or xhtml (default: html)

strip_style (bool) – whether to strip style attributes (removed and therefore not sanitized, default: False)

strip_classes (bool) – whether to strip classes attributes (default: False)

Encapsulates an image, extending Binary.

If image size is greater than the max_width/max_height limit of pixels, the image will be resized to the limit by keeping aspect ratio.

max_width (int) – the maximum width of the image (default: 0, no limit)

max_height (int) – the maximum height of the image (default: 0, no limit)

verify_resolution (bool) – whether the image resolution should be verified to ensure it doesn’t go over the maximum image resolution (default: True). See odoo.tools.image.ImageProcess for maximum image resolution (default: 50e6).

If no max_width/max_height is specified (or is set to 0) and verify_resolution is False, the field content won’t be verified at all and a Binary field should be used.

Encapsulates a float expressed in a given res_currency.

The decimal precision and currency symbol are taken from the currency_field attribute.

currency_field (str) – name of the Many2one field holding the res_currency this monetary field is expressed in (default: 'currency_id')

Encapsulates an exclusive choice between different values.

selection (list(tuple(str,str)) or callable or str) – specifies the possible values for this field. It is given as either a list of pairs (value, label), or a model method, or a method name.

selection_add (list(tuple(str,str))) – provides an extension of the selection in the case of an overridden field. It is a list of pairs (value, label) or singletons (value,), where singleton values must appear in the overridden selection. The new values are inserted in an order that is consistent with the overridden selection and this list: selection = [('a', 'A'), ('b', 'B')] selection_add = [('c', 'C'), ('b',)] > result = [('a', 'A'), ('c', 'C'), ('b', 'B')]

provides an extension of the selection in the case of an overridden field. It is a list of pairs (value, label) or singletons (value,), where singleton values must appear in the overridden selection. The new values are inserted in an order that is consistent with the overridden selection and this list:

ondelete – provides a fallback mechanism for any overridden field with a selection_add. It is a dict that maps every option from the selection_add to a fallback action. This fallback action will be applied to all records whose selection_add option maps to it. The actions can be any of the following: ’set null’ – the default, all records with this option will have their selection value set to False. ’cascade’ – all records with this option will be deleted along with the option itself. ’set default’ – all records with this option will be set to the default of the field definition ’set VALUE’ – all records with this option will be set to the given value <callable> – a callable whose first and only argument will be the set of records containing the specified Selection option, for custom processing

provides a fallback mechanism for any overridden field with a selection_add. It is a dict that maps every option from the selection_add to a fallback action.

This fallback action will be applied to all records whose selection_add option maps to it.

’set null’ – the default, all records with this option will have their selection value set to False.

’cascade’ – all records with this option will be deleted along with the option itself.

’set default’ – all records with this option will be set to the default of the field definition

’set VALUE’ – all records with this option will be set to the given value

<callable> – a callable whose first and only argument will be the set of records containing the specified Selection option, for custom processing

The attribute selection is mandatory except in the case of related or extended fields.

Very similar to Char but used for longer contents, does not have a size and usually displayed as a multiline text box.

translate (bool or callable) – enable the translation of the field’s values; use translate=True to translate field values as a whole; translate may also be a callable such that translate(callback, value) translates value by using callback(term) to retrieve the translation of terms.

Dates and Datetimes are very important fields in any kind of business application. Their misuse can create invisible yet painful bugs, this section aims to provide Odoo developers with the knowledge required to avoid misusing these fields.

When assigning a value to a Date/Datetime field, the following options are valid:

A date or datetime object.

A string in the proper server format:

YYYY-MM-DD for Date fields,

YYYY-MM-DD HH:MM:SS for Datetime fields.

The Date and Datetime fields class have helper methods to attempt conversion into a compatible type:

to_date() will convert to a datetime.date

to_datetime() will convert to a datetime.datetime.

To parse date/datetimes coming from external sources:

Date / Datetime comparison best practices:

Date fields can only be compared to date objects.

Datetime fields can only be compared to datetime objects.

Strings representing dates and datetimes can be compared between each other, however the result may not be the expected result, as a datetime string will always be greater than a date string, therefore this practice is heavily discouraged.

Common operations with dates and datetimes such as addition, subtraction or fetching the start/end of a period are exposed through both Date and Datetime. These helpers are also available by importing odoo.tools.date_utils.

Datetime fields are stored as timestamp without timezone columns in the database and are stored in the UTC timezone. This is by design, as it makes the Odoo database independent from the timezone of the hosting server system. Timezone conversion is managed entirely by the client side.

Encapsulates a python date object.

Return the current day in the format expected by the ORM.

This function may be used to compute default values.

Return the current date as seen in the client’s timezone in a format fit for date fields.

This method may be used to compute default values.

record – recordset from which the timezone will be obtained.

timestamp – optional datetime value to use instead of the current date and time (must be a datetime, regular dates can’t be converted between timezones).

Attempt to convert value to a date object.

If a datetime object is given as value, it will be converted to a date object and all datetime-specific information will be lost (HMS, TZ, …).

value (str or date or datetime) – value to convert.

an object representing value.

Convert a date or datetime object to a string.

value – value to convert.

a string representing value in the server’s date format, if value is of type datetime, the hours, minute, seconds, tzinfo will be truncated.

Get start of a time period from a date or a datetime.

value – initial date or datetime.

granularity – type of period in string, can be year, quarter, month, week, day or hour.

a date/datetime object corresponding to the start of the specified period.

Get end of a time period from a date or a datetime.

value – initial date or datetime.

granularity – Type of period in string, can be year, quarter, month, week, day or hour.

A date/datetime object corresponding to the start of the specified period.

Return the sum of value and a relativedelta.

value – initial date or datetime.

args – positional args to pass directly to relativedelta.

kwargs – keyword args to pass directly to relativedelta.

the resulting date/datetime.

Return the difference between value and a relativedelta.

value – initial date or datetime.

args – positional args to pass directly to relativedelta.

kwargs – keyword args to pass directly to relativedelta.

the resulting date/datetime.

Encapsulates a python datetime object.

Return the current day and time in the format expected by the ORM.

This function may be used to compute default values.

Return the current day, at midnight (00:00:00).

Return the given timestamp converted to the client’s timezone.

This method is not meant for use as a default initializer, because datetime fields are automatically converted upon display on client side. For default values, now() should be used instead.

record – recordset from which the timezone will be obtained.

timestamp (datetime) – naive datetime value (expressed in UTC) to be converted to the client timezone.

timestamp converted to timezone-aware datetime in context timezone.

Convert an ORM value into a datetime value.

value (str or date or datetime) – value to convert.

an object representing value.

Convert a datetime or date object to a string.

value (datetime or date) – value to convert.

a string representing value in the server’s datetime format, if value is of type date, the time portion will be midnight (00:00:00).

Get start of a time period from a date or a datetime.

value – initial date or datetime.

granularity – type of period in string, can be year, quarter, month, week, day or hour.

a date/datetime object corresponding to the start of the specified period.

Get end of a time period from a date or a datetime.

value – initial date or datetime.

granularity – Type of period in string, can be year, quarter, month, week, day or hour.

A date/datetime object corresponding to the start of the specified period.

Return the sum of value and a relativedelta.

value – initial date or datetime.

args – positional args to pass directly to relativedelta.

kwargs – keyword args to pass directly to relativedelta.

the resulting date/datetime.

Return the difference between value and a relativedelta.

value – initial date or datetime.

args – positional args to pass directly to relativedelta.

kwargs – keyword args to pass directly to relativedelta.

the resulting date/datetime.

The value of such a field is a recordset of size 0 (no record) or 1 (a single record).

comodel_name (str) – name of the target model Mandatory except for related or extended fields.

domain – an optional domain to set on candidate values on the client side (domain or a python expression that will be evaluated to provide domain)

context (dict) – an optional context to use on the client side when handling that field

ondelete (str) – what to do when the referred record is deleted; possible values are: 'set null', 'restrict', 'cascade'

bypass_search_access (bool) – whether access rights are bypassed on the comodel (default: False)

delegate (bool) – set it to True to make fields of the target model accessible from the current model (corresponds to _inherits)

check_company (bool) – Mark the field to be verified in _check_company(). Has a different behaviour depending on whether the field is company_dependent or not. Constrains non-company-dependent fields to target records whose company_id(s) are compatible with the record’s company_id(s). Constrains company_dependent fields to target records whose company_id(s) are compatible with the currently active company.

One2many field; the value of such a field is the recordset of all the records in comodel_name such that the field inverse_name is equal to the current record.

comodel_name (str) – name of the target model

inverse_name (str) – name of the inverse Many2one field in comodel_name

domain – an optional domain to set on candidate values on the client side (domain or a python expression that will be evaluated to provide domain)

context (dict) – an optional context to use on the client side when handling that field

bypass_search_access (bool) – whether access rights are bypassed on the comodel (default: False)

The attributes comodel_name and inverse_name are mandatory except in the case of related fields or field extensions.

Many2many field; the value of such a field is the recordset.

comodel_name (str) – name of the target model (string) mandatory except in the case of related or extended fields

relation (str) – optional name of the table that stores the relation in the database

column1 (str) – optional name of the column referring to “these” records in the table relation

column2 (str) – optional name of the column referring to “those” records in the table relation

The attributes relation, column1 and column2 are optional. If not given, names are automatically generated from model names, provided model_name and comodel_name are different!

Note that having several fields with implicit relation parameters on a given model with the same comodel is not accepted by the ORM, since those field would use the same table. The ORM prevents two many2many fields to use the same relation parameters, except if

both fields use the same model, comodel, and relation parameters are explicit; or

at least one field belongs to a model with _auto = False.

domain – an optional domain to set on candidate values on the client side (domain or a python expression that will be evaluated to provide domain)

context (dict) – an optional context to use on the client side when handling that field

check_company (bool) – Mark the field to be verified in _check_company(). Add a default company domain depending on the field attributes.

One2many and Many2many fields expect a special command to manipulate the relation they implement.

Internally, each command is a 3-elements tuple where the first element is a mandatory integer that identifies the command, the second element is either the related record id to apply the command on (commands update, delete, unlink and link) either 0 (commands create, clear and set), the third element is either the values to write on the record (commands create and update) either the new ids list of related records (command set), either 0 (commands delete, unlink, link, and clear). This triplet is aliased as CommandValue.

Via Python, we encourage developers craft new commands via the various functions of this namespace. We also encourage developers to use the command identifier constant names when comparing the 1st element of existing commands.

Via RPC, it is impossible nor to use the functions nor the command constant names. It is required to instead write the literal 3-elements tuple where the first element is the integer identifier of the command.

Create new records in the comodel using values, link the created records to self.

In case of a Many2many relation, one unique new record is created in the comodel such that all records in self are linked to the new record.

In case of a One2many relation, one new record is created in the comodel for every record in self such that every record in self is linked to exactly one of the new records.

Return the command triple (CREATE, 0, {values})

Write values on the related record.

Return the command triple (UPDATE, {id}, {values})

Remove the related record from the database and remove its relation with self.

In case of a Many2many relation, removing the record from the database may be prevented if it is still linked to other records.

Return the command triple (DELETE, {id}, 0)

Remove the relation between self and the related record.

In case of a One2many relation, the given record is deleted from the database if the inverse field is set as ondelete='cascade'. Otherwise, the value of the inverse field is set to False and the record is kept.

Return the command triple (UNLINK, {id}, 0)

Add a relation between self and the related record.

Return the command triple (LINK, {id}, 0)

Remove all records from the relation with self. It behaves like executing the unlink command on every record.

Return the command triple (CLEAR, 0, 0)

Replace the current relations of self by the given ones. It behaves like executing the unlink command on every removed relation then executing the link command on every new relation.

Return the command triple (SET, 0, {ids})

Pseudo-relational field (no FK in database).

The field value is stored as a string following the pattern "res_model,res_id" in database.

Pseudo-relational field (no FK in database).

The field value is stored as an integer id in database.

Contrary to Reference fields, the model has to be specified in a Char field, whose name has to be specified in the model_field attribute for the current Many2oneReference field.

model_field (str) – name of the Char where the model name is stored.

Fields can be computed (instead of read straight from the database) using the compute parameter. It must assign the computed value to the field. If it uses the values of other fields, it should specify those fields using depends().

dependencies can be dotted paths when using sub-fields:

computed fields are not stored by default, they are computed and returned when requested. Setting store=True will store them in the database and automatically enable searching and grouping. Note that by default, compute_sudo=True is set on the field.

searching on a computed field can also be enabled by setting the search parameter. The value is a method name returning a Search domains.

computed fields are readonly by default. To allow setting values on a computed field, use the inverse parameter. It is the name of a function reversing the computation and setting the relevant fields:

multiple fields can be computed at the same time by the same method, just use the same method on all fields and set all of them:

While it is possible to use the same compute method for multiple fields, it is not recommended to do the same for the inverse method.

During the computation of the inverse, all fields that use said inverse are protected, meaning that they can’t be computed, even if their value is not in the cache.

If any of those fields is accessed and its value is not in cache, the ORM will simply return a default value of False for these fields. This means that the value of the inverse fields (other than the one triggering the inverse method) may not give their correct value and this will probably break the expected behavior of the inverse method.

A special case of computed fields are related (proxy) fields, which provide the value of a sub-field on the current record. They are defined by setting the related parameter and like regular computed fields they can be stored:

The value of a related field is given by following a sequence of relational fields and reading a field on the reached model. The complete sequence of fields to traverse is specified by the related attribute.

Some field attributes are automatically copied from the source field if they are not redefined: string, help, required (only if all fields in the sequence are required), groups, digits, size, translate, sanitize, selection, comodel_name, domain, context. All semantic-free attributes are copied from the source field.

By default, related fields are:

computed in superuser mode

Add the attribute store=True to make it stored, just like computed fields. Related fields are automatically recomputed when their dependencies are modified.

You can specify precise field dependencies if you don’t want the related field to be recomputed on any dependency change:

You cannot chain Many2many or One2many fields in related fields dependencies.

related can be used to refer to a One2many or Many2many field on another model on the condition that it’s done through a Many2one relation on the current model. One2many and Many2many are not supported and the results will not be aggregated correctly:

If length of current recordset is 1, return id of unique record in it.

Raise an Error otherwise.

Name field displayed by default in the web client

By default, it equals to _rec_name value field but the behavior can be customized by overriding _compute_display_name

These fields are automatically set and updated if _log_access is enabled. It can be disabled to avoid creating or updating those fields on tables for which they are not useful.

By default, _log_access is set to the same value as _auto

Stores when the record was created, Datetime

Stores who created the record, Many2one to a res.users.

Stores when the record was last updated, Datetime

Stores who last updated the record, Many2one to a res.users.

_log_access must be enabled on TransientModel.

A few field names are reserved for pre-defined behaviors beyond that of automated fields. They should be defined on a model when the related behavior is desired:

default value for _rec_name, used to display records in context where a representative “naming” is necessary.

toggles the global visibility of the record, if active is set to False the record is invisible in most searches and listing.

Set active to False on a recordset for active records.

Note, you probably want to override write() method if you want to take action once the active field changes.

Set active to True on a recordset for inactive records.

Note, you probably want to override write() method if you want to take action once the active field changes.

lifecycle stages of the object, used by the states attribute on fields.

default_value of _parent_name, used to organize records in a tree structure and enables the child_of and parent_of operators in domains.

When _parent_store is set to True, used to store a value reflecting the tree structure of _parent_name, and to optimize the operators child_of and parent_of in Search domains. It must be declared with index=True for proper operation.

Main field name used for Odoo multi-company behavior.

Used by :meth:~odoo.models._check_company to check multi company consistency. Defines whether a record is shared between companies (no value) or only accessible by the users of a given company.

Many2one :type: res_company

Similarly to fields, you can declare Constraint, Index and UniqueIndex. The name of the attribute must begin with _ to avoid name clashes with field names.

You can customize error messages. They can either be strings and their translation will be provided in the internal reflected constraint table. Otherwise, they can be functions that take (env, diag) as parameters which respectively denote the environment and psycopg diagnostics.

Interactions with models and records are performed through recordsets, an ordered collection of records of the same model.

Contrary to what the name implies, it is currently possible for recordsets to contain duplicates. This may change in the future.

Methods defined on a model are executed on a recordset, and their self is a recordset:

Iterating on a recordset will yield new sets of a single record (“singletons”), much like iterating on a Python string yields strings of a single characters:

Recordsets provide an “Active Record” interface: model fields can be read and written directly from the record as attributes.

When accessing non-relational fields on a recordset of potentially multiple records, use mapped():

Field values can also be accessed like dict items, which is more elegant and safer than getattr() for dynamic field names. Setting a field’s value triggers an update to the database:

Trying to read a field on multiple records will raise an error for non relational fields.

Accessing a relational field (Many2one, One2many, Many2many) always returns a recordset, empty if the field is not set.

Odoo maintains a cache for the fields of the records, so that not every field access issues a database request, which would be terrible for performance. The following example queries the database only for the first statement:

To avoid reading one field on one record at a time, Odoo prefetches records and fields following some heuristics to get good performance. Once a field must be read on a given record, the ORM actually reads that field on a larger recordset, and stores the returned values in cache for later use. The prefetched recordset is usually the recordset from which the record comes by iteration. Moreover, all simple stored fields (boolean, integer, float, char, text, date, datetime, selection, many2one) are fetched altogether; they correspond to the columns of the model’s table, and are fetched efficiently in the same query.

Consider the following example, where partners is a recordset of 1000 records. Without prefetching, the loop would make 2000 queries to the database. With prefetching, only one query is made:

The prefetching also works on secondary records: when relational fields are read, their values (which are records) are subscribed for future prefetching. Accessing one of those secondary records prefetches all secondary records from the same model. This makes the following example generate only two queries, one for partners and one for countries:

The methods search_fetch() and fetch() can be used to populate the cache of records, typically in cases where the prefetching mechanism does not work well.

Decorate a method so that it is called by the daily vacuum cron job (model ir.autovacuum). This is typically used for garbage-collection-like tasks that do not deserve a specific cron job.

A return value can be a tuple (done, remaining) which have simular meaning as in _commit_progress().

Decorate a constraint checker.

Each argument must be a field name used in the check:

Invoked on the records on which one of the named fields has been modified.

Should raise ValidationError if the validation failed.

@constrains only supports simple field names, dotted names (fields of relational fields e.g. partner_id.customer) are not supported and will be ignored.

@constrains will be triggered only if the declared fields in the decorated method are included in the create or write call. It implies that fields not present in a view will not trigger a call during a record creation. A override of create is necessary to make sure a constraint will always be triggered (e.g. to test the absence of value).

One may also pass a single function as argument. In that case, the field names are given by calling the function with a model instance.

Return a decorator that specifies the field dependencies of a “compute” method (for new-style function fields). Each argument must be a string that consists in a dot-separated sequence of field names:

One may also pass a single function as argument. In that case, the dependencies are given by calling the function with the field’s model.

Return a decorator that specifies the context dependencies of a non-stored “compute” method. Each argument is a key in the context’s dictionary:

All dependencies must be hashable. The following keys have special support:

company (value in context or current company id),

uid (current user id and superuser flag),

active_test (value in env.context or value in field.context).

Decorate a record-style method where self is a recordset, but its contents is not relevant, only the model is. Such a method:

Decorate a method that takes a list of dictionaries and creates multiple records. The method may be called with either a single dict or a list of dicts:

Return a decorator to decorate an onchange method for given fields.

In the form views where the field appears, the method will be called when one of the given fields is modified. The method is invoked on a pseudo-record that contains the values present in the form. Field assignments on that record are automatically sent back to the client.

Each argument must be a field name:

If the type is set to notification, the warning will be displayed in a notification. Otherwise it will be displayed in a dialog as default.

@onchange only supports simple field names, dotted names (fields of relational fields e.g. partner_id.tz) are not supported and will be ignored

Since @onchange returns a recordset of pseudo-records, calling any one of the CRUD methods (create(), read(), write(), unlink()) on the aforementioned recordset is undefined behaviour, as they potentially do not exist in the database yet.

Instead, simply set the record’s field like shown in the example above or call the update() method.

It is not possible for a one2many or many2many field to modify itself via onchange. This is a webclient limitation - see #2693.

Mark a method to be executed during unlink().

The goal of this decorator is to allow client-side errors when unlinking records if, from a business point of view, it does not make sense to delete such records. For instance, a user should not be able to delete a validated sales order.

While this could be implemented by simply overriding the method unlink on the model, it has the drawback of not being compatible with module uninstallation. When uninstalling the module, the override could raise user errors, but we shouldn’t care because the module is being uninstalled, and thus all records related to the module should be removed anyway.

This means that by overriding unlink, there is a big chance that some tables/records may remain as leftover data from the uninstalled module. This leaves the database in an inconsistent state. Moreover, there is a risk of conflicts if the module is ever reinstalled on that database.

Methods decorated with @ondelete should raise an error following some conditions, and by convention, the method should be named either _unlink_if_<condition> or _unlink_except_<not_condition>.

at_uninstall (bool) – Whether the decorated method should be called if the module that implements said method is being uninstalled. Should almost always be False, so that module uninstallation does not trigger those errors.

The parameter at_uninstall should only be set to True if the check you are implementing also applies when uninstalling the module.

For instance, it doesn’t matter if when uninstalling sale, validated sales orders are being deleted because all data pertaining to sale should be deleted anyway, in that case at_uninstall should be set to False.

However, it makes sense to prevent the removal of the default language if no other languages are installed, since deleting the default language will break a lot of basic behavior. In this case, at_uninstall should be set to True.

Decorate a record-style method to indicate that the method cannot be called using RPC. Example:

If you have business methods that should not be called over RPC, you should prefix them with “_”. This decorator may be used in case of existing public methods that become non-RPC callable or for ORM methods.

The environment stores various contextual data used by the ORM:

cr: the current database cursor (for database queries);

uid: the current user id (for access rights checks);

context: the current context dictionary (arbitrary metadata);

su: whether in superuser mode.

It provides access to the registry by implementing a mapping from model names to models. It also holds a cache for records, and a data structure to manage recomputations.

When creating a recordset from an other recordset, the environment is inherited. The environment can be used to get an empty recordset in an other model, and query that model:

Some lazy properties are available to access the environment (contextual) data:

Return the current language code.

Return the current user (as an instance).

current user - sudoed

Return the current company (as an instance).

If not specified in the context (allowed_company_ids), fallback on current user main company.

AccessError – invalid or unauthorized allowed_company_ids context key content.

current company (default=`self.user.company_id`), with the current environment

No sanity checks applied in sudo mode! When in sudo mode, a user can access any company, even if not in his allowed companies.

This allows to trigger inter-company modifications, even if the current user doesn’t have access to the targeted company.

Return a recordset of the enabled companies by the user.

If not specified in the context(allowed_company_ids), fallback on current user companies.

AccessError – invalid or unauthorized allowed_company_ids context key content.

current companies (default=`self.user.company_ids`), with the current environment

res.company recordset

No sanity checks applied in sudo mode ! When in sudo mode, a user can access any company, even if not in his allowed companies.

This allows to trigger inter-company modifications, even if the current user doesn’t have access to the targeted company.

Return the record corresponding to the given xml_id.

xml_id (str) – record xml_id, under the format <module.id>

raise_if_not_found (bool) – whether the method should raise if record is not found

ValueError – if record wasn’t found and raise_if_not_found is True

Return whether the environment is in superuser mode.

Return whether the current user has group “Access Rights”, or is in superuser mode.

Return whether the current user has group “Settings”, or is in superuser mode.

Execute the given query, fetch its result and it as a list of tuples (or an empty list if no result to fetch). The method automatically flushes all the fields in the metadata of the query.

Return a new version of this recordset attached to an extended context.

The extended context is either the provided context in which overrides are merged or the current context in which overrides are merged e.g.:

Return a new version of this recordset attached to the given user, in non-superuser mode, unless user is the superuser (by convention, the superuser is always in superuser mode.)

Return a new version of this recordset with a modified context, such that:

When using an unauthorized company for current user, accessing the company(ies) on the environment may trigger an AccessError if not done in a sudoed environment.

Return a new version of this recordset attached to the provided environment.

The returned recordset has the same prefetch object as self.

Return a new version of this recordset with superuser mode enabled or disabled, depending on flag. The superuser mode does not change the current user, and simply bypasses access rights checks.

Using sudo could cause data access to cross the boundaries of record rules, possibly mixing records that are meant to be isolated (e.g. records from different companies in multi-company environments).

It may lead to un-intuitive results in methods which select one record among many - for example getting the default company, or selecting a Bill of Materials.

The returned recordset has the same prefetch object as self.

The cr attribute on environments is the cursor for the current database transaction and allows executing SQL directly, either for queries which are difficult to express using the ORM (e.g. complex joins) or for performance reasons:

Executing raw SQL bypasses the ORM and, by consequent, Odoo security rules. Please make sure your queries are sanitized when using user input and prefer using ORM utilities if you don’t really need to use SQL queries.

The recommended way to build SQL queries is to use the wrapper object

An object that wraps SQL code with its parameters, like:

The code is given as a %-format string, and supports either positional arguments (with %s) or named arguments (with %(name)s). The arguments are meant to be merged into the code using the % formatting operator. Note that the character % must always be escaped (as %%), even if the code does not have parameters, like in SQL("foo LIKE 'a%%'").

The SQL wrapper is designed to be composable: the arguments can be either actual parameters, or SQL objects themselves:

The combined SQL code is given by sql.code, while the corresponding combined parameters are given by the list sql.params. This allows to combine any number of SQL terms without having to separately combine their parameters, which can be tedious, bug-prone, and is the main downside of psycopg2.sql <https://www.psycopg.org/docs/sql.html>.

The second purpose of the wrapper is to discourage SQL injections. Indeed, if code is a string literal (not a dynamic string), then the SQL object made with code is guaranteed to be safe, provided the SQL objects within its parameters are themselves safe.

The wrapper may also contain some metadata to_flush. If not None, its value is a field which the SQL code depends on. The metadata of a wrapper and its parts can be accessed by the iterator sql.to_flush.

Join SQL objects or parameters with self as a separator.

Return an SQL object that represents an identifier.

One important thing to know about models is that they don’t necessarily perform database updates right away. Indeed, for performance reasons, the framework delays the recomputation of fields after modifying records. And some database updates are delayed, too. Therefore, before querying the database, one has to make sure that it contains the relevant data for the query. This operation is called flushing and performs the expected database updates.

Before every SQL query, one has to flush the data needed for that query. There are three levels for flushing, each with its own API. One can flush either everything, all the records of a model, or some specific records. Because delaying updates improves performance in general, we recommend to be specific when flushing.

Flush all pending computations and updates to the database.

Process the pending computations and database updates on self’s model. When the parameter is given, the method guarantees that at least the given fields are flushed to the database. More fields can be flushed, though.

fnames – optional iterable of field names to flush

Process the pending computations and database updates on the records self. When the parameter is given, the method guarantees that at least the given fields on records self are flushed to the database. More fields and records can be flushed, though.

fnames – optional iterable of field names to flush

Because models use the same cursor and the Environment holds various caches, these caches must be invalidated when altering the database in raw SQL, or further uses of models may become incoherent. It is necessary to clear caches when using CREATE, UPDATE or DELETE in SQL, but not SELECT (which simply reads the database).

Just like flushing, one can invalidate either the whole cache, the cache of all the records of a model, or the cache of specific records. One can even invalidate specific fields on some records or all records of a model. As the cache improves performance in general, we recommend to be specific when invalidating.

Invalidate the cache of all records.

flush – whether pending updates should be flushed before invalidation. It is True by default, which ensures cache consistency. Do not use this parameter unless you know what you are doing.

Invalidate the cache of all records of self’s model, when the cached values no longer correspond to the database values. If the parameter is given, only the given fields are invalidated from cache.

fnames – optional iterable of field names to invalidate

flush – whether pending updates should be flushed before invalidation. It is True by default, which ensures cache consistency. Do not use this parameter unless you know what you are doing.

Invalidate the cache of the records in self, when the cached values no longer correspond to the database values. If the parameter is given, only the given fields on self are invalidated from cache.

fnames – optional iterable of field names to invalidate

flush – whether pending updates should be flushed before invalidation. It is True by default, which ensures cache consistency. Do not use this parameter unless you know what you are doing.

The methods above keep the caches and the database consistent with each other. However, if computed field dependencies have been modified in the database, one has to inform the models for the computed fields to be recomputed. The only thing the framework needs to know is what fields have changed on which records.

One has to figure out which records have been modified. There are many ways to do this, possibly involving extra SQL queries. In the example above, we take advantage of the RETURNING clause of PostgreSQL to retrieve the information without an extra query. After making the cache consistent by invalidation, invoke the method modified on the modified records with the fields that have been updated.

Notify that fields will be or have been modified on self. This invalidates the cache where necessary, and prepares the recomputation of dependent stored fields.

fnames – iterable of field names modified on records self

create – whether called in the context of record creation

before – whether called before modifying records self

Create new records for the model.

The new records are initialized using the values from the list of dicts vals_list, and if necessary those from default_get().

vals_list – values for the model’s fields, as a list of dictionaries: [{'field_name': field_value, ...}, ...] For backward compatibility, vals_list may be a dictionary. It is treated as a singleton list [vals], and a single record is returned. see write() for details

values for the model’s fields, as a list of dictionaries:

For backward compatibility, vals_list may be a dictionary. It is treated as a singleton list [vals], and a single record is returned.

see write() for details

AccessError – if the current user is not allowed to create records of the specified model

ValidationError – if user tries to enter invalid value for a selection field

ValueError – if a field name specified in the create values does not exist.

UserError – if a loop would be created in a hierarchy of objects a result of the operation (such as setting an object as its own parent)

Duplicate record self updating it with default values.

default – dictionary of field values to override in the original values of the copied record, e.g: {'field_name': overridden_value, ...}

Return default values for the fields in fields_list. Default values are determined by the context, user defaults, user fallbacks and the model itself.

fields – names of field whose default is requested

a dictionary mapping field names to their corresponding default values, if they have a default value.

Unrequested defaults won’t be considered, there is no need to return a value for fields whose names are not in fields_list.

Create a new record by calling create() with only one value provided: the display name of the new record.

The new record will be initialized with any default values applicable to this model, or provided through the context. The usual behavior of create() applies.

name – display name of the record to create

the (id, display_name) pair value of the created record

Update all records in self with the provided values.

vals – fields to update and the value to set on them

AccessError – if user is not allowed to modify the specified records/fields

ValidationError – if invalid values are specified for selection fields

UserError – if a loop would be created in a hierarchy of objects a result of the operation (such as setting an object as its own parent)

For numeric fields (Integer, Float) the value should be of the corresponding type

For Boolean, the value should be a bool

For Selection, the value should match the selection values (generally str, sometimes int)

For Many2one, the value should be the database identifier of the record to set

The expected value of a One2many or Many2many relational field is a list of Command that manipulate the relation the implement. There are a total of 7 commands: create(), update(), delete(), unlink(), link(), clear(), and set().

For Date and ~odoo.fields.Datetime, the value should be either a date(time), or a string.

If a string is provided for Date(time) fields, it must be UTC-only and formatted according to odoo.tools.misc.DEFAULT_SERVER_DATE_FORMAT and odoo.tools.misc.DEFAULT_SERVER_DATETIME_FORMAT

Other non-relational fields use a string for value

Return a recordset for the ids provided as parameter in the current environment.

Search for the records that satisfy the given domain search domain.

domain – A search domain. Use an empty list to match all records.

offset – number of results to ignore (default: none)

limit – maximum number of records to return (default: all)

at most limit records matching the search criteria

AccessError – if user is not allowed to access requested information

This is a high-level method, which should not be overridden. Its actual implementation is done by method _search().

Return the number of records in the current model matching the provided domain.

domain – A search domain. Use an empty list to match all records.

limit – maximum number of record to count (upperbound) (default: all)

This is a high-level method, which should not be overridden. Its actual implementation is done by method _search().

Search for the records that satisfy the given domain search domain, and fetch the given fields to the cache. This method is like a combination of methods search() and fetch(), but it performs both tasks with a minimal number of SQL queries.

domain – A search domain. Use an empty list to match all records.

field_names – a collection of field names to fetch, or None for all accessible fields marked with prefetch=True

offset – number of results to ignore (default: none)

limit – maximum number of records to return (default: all)

at most limit records matching the search criteria

AccessError – if user is not allowed to access requested information

Search for records that have a display name matching the given name pattern when compared with the given operator, while also matching the optional search domain (domain).

This is used for example to provide suggestions based on a partial value for a relational field. Should usually behave as the reverse of display_name, but that is not guaranteed.

This method is equivalent to calling search() with a search domain based on display_name and mapping id and display_name on the resulting search.

name – the name pattern to match

domain – search domain (see search() for syntax), specifying further restrictions

operator – domain operator for matching name, such as 'like' or '='.

limit – max number of records to return

list of pairs (id, display_name) for all matching records.

Make sure the given fields are in memory for the records in self, by fetching what is necessary from the database. Non-stored fields are mostly ignored, except for their stored dependencies. This method should be called to optimize code.

field_names – a collection of field names to fetch, or None for all accessible fields marked with prefetch=True

AccessError – if user is not allowed to access requested information

This method is implemented thanks to methods _search() and _fetch_query(), and should not be overridden.

Read the requested fields for the records in self, and return their values as a list of dicts.

fields – field names to return (default is all fields)

load – loading mode, currently the only option is to set to None to avoid loading the display_name of m2o fields

a list of dictionaries mapping field names to their values, with one dictionary per record

AccessError – if user is not allowed to access requested information

ValueError – if a requested field does not exist

This is a high-level method that is not supposed to be overridden. In order to modify how fields are read from database, see methods _fetch_query() and _read_format().

Get fields aggregations specified by aggregates grouped by the given groupby fields where record are filtered by the domain.

domain – A search domain. Use an empty list to match all records.

groupby – list of groupby descriptions by which the records will be grouped. A groupby description is either a field (then it will be grouped by that field) or a string 'field:granularity'. Right now, the only supported granularities are 'day', 'week', 'month', 'quarter' or 'year', and they only make sense for date/datetime fields. Additionally integer date parts are also supported: 'year_number', 'quarter_number', 'month_number', 'iso_week_number', 'day_of_year', 'day_of_month', ‘day_of_week’, ‘hour_number’, ‘minute_number’ and ‘second_number’.

aggregates – list of aggregates specification. Each element is 'field:agg' (aggregate field with aggregation function 'agg'). The possible aggregation functions are the ones provided by PostgreSQL, 'count_distinct' with the expected meaning and 'recordset' to act like 'array_agg' converted into a recordset.

having – A domain where the valid “fields” are the aggregates.

offset – optional number of groups to skip

limit – optional max number of groups to return

order – optional order by specification, for overriding the natural sort ordering of the groups, see also search().

list of tuples containing in the order the groups values and aggregates values (flatten): [(groupby_1_value, ... , aggregate_1_value_aggregate, ...), ...]. If group is related field, the value of it will be a recordset (with a correct prefetch set).

AccessError – if user is not allowed to access requested information

Return the definition of each field.

The returned value is a dictionary (indexed by field name) of dictionaries. The _inherits’d fields are included. The string, help, and selection (if present) attributes are translated.

allfields – fields to document, all if empty or not provided

attributes – attributes to return for each field, all if empty or not provided

dictionary mapping field names to a dictionary mapping attributes to values.

A search domain is a first-order logical predicate used for filtering and searching recordsets. You combine simple conditions on a field expression with logical operators.

Domain can be used as a builder for domains.

A domain can be a simple condition (field_expr, operator, value) where:

a field name of the current model, or a relationship traversal through a Many2one using dot-notation e.g. 'street' or 'partner_id.country'. If the field is a date(time) field, you can also specify a part of the date using 'field_name.granularity'. The supported granularities are 'year_number', 'quarter_number', 'month_number', 'iso_week_number', 'day_of_week', 'day_of_month', 'day_of_year', 'hour_number', 'minute_number', 'second_number'. They all use an integer as value.

an operator used to compare the field_expr with the value. Valid operators are:

greater than or equal to

less than or equal to

unset or equals to (returns true if value is either None or False, otherwise behaves like =)

matches field_expr against the value pattern. An underscore _ in the pattern stands for (matches) any single character; a percent sign % matches any string of zero or more characters.

matches field_expr against the %value% pattern. Similar to =like but wraps value with ‘%’ before matching

case insensitive like

case insensitive =like

is equal to any of the items from value, value should be a collection of items

is a child (descendant) of a value record (value can be either one item or a list of items).

Takes the semantics of the model into account (i.e following the relationship field named by _parent_name).

is a parent (ascendant) of a value record (value can be either one item or a list of items).

Takes the semantics of the model into account (i.e following the relationship field named by _parent_name).

matches if any record in the relationship traversal through field_expr (Many2one, One2many, or Many2many) satisfies the provided domain value. The field_expr should be a field name.

like any, but bypasses access checks.

variable type, must be comparable (through operator) to the named field.

To search for partners named ABC, with a phone or mobile number containing 7620:

To search sales orders to invoice that have at least one line with a product that is out of stock:

To search for all partners born in the month of February:

Domain can be used to serialize the domain as a list of simple conditions represented by 3-item tuple (or a list). Such a serialized form may be sometimes faster to read or write. Domain conditions can be combined using logical operators in a prefix notation. You can combine 2 domains using '&' (AND), '|' (OR) and you can negate 1 using '!' (NOT).

Yield simple conditions of the domain

Map a function to each condition and return the combined result

Perform optimizations of the node given a model.

It is a pre-processing step to rewrite the domain into a logically equivalent domain that is a more canonical representation of the predicate. Multiple conditions can be merged together.

It applies basic optimizations only. Those are transaction-independent; they only depend on the model’s fields definitions. No model-specific override is used, and the resulting domain may be reused in another transaction without semantic impact. The model’s fields are used to validate conditions and apply type-dependent optimizations. This optimization level may be useful to simplify a domain that is sent to the client-side, thereby reducing its payload/complexity.

Validates that the current domain is correct or raises an exception

In the context of search domains, for date and datetime fields, the value can be a moment relative to now in the timezone of the user. A simple language is provided to specify these dates. It is a space-separated string of terms. The first term is optional and is “today” (at midnight) or “now”. Then, each term starts with “+” (add), “-” (subtract) or “=” (set), followed by an integer and date unit or a lower-case weekday.

The date units are: “d” (days), “w” (weeks), “m” (months), “y” (years), “H” (hours), “M” (minutes), “S” (seconds). For weekdays, “+” and “-” mean next and previous weekday (unless we are already in that weekday) and “=” means in current week starting on Monday. When setting a date, the lower-units (hours, minutes and seconds) are set to 0.

Delete the records in self.

AccessError – if the user is not allowed to delete all the given records

UserError – if the record is default property for other records

Return the list of actual record ids corresponding to self.

Returns the environment of the given recordset.

The subset of records in self that exist. It can be used as a test on records:

By convention, new records are returned as existing.

Verify that the current recordset holds a single record.

odoo.exceptions.ValueError – len(self) != 1

Return some metadata about the given records.

list of ownership dictionaries for each requested record with the following keys: id: object id create_uid: user who created the record create_date: date when the record was created write_uid: last user who changed the record write_date: date of the last change to the record xmlid: XML ID to use to refer to this record (if there is one), in format module.name xmlids: list of dict with xmlid in format module.name, and noupdate as boolean noupdate: A boolean telling if the record will be updated or not

list of ownership dictionaries for each requested record with the following keys:

create_uid: user who created the record

create_date: date when the record was created

write_uid: last user who changed the record

write_date: date of the last change to the record

xmlid: XML ID to use to refer to this record (if there is one), in format module.name

xmlids: list of dict with xmlid in format module.name, and noupdate as boolean

noupdate: A boolean telling if the record will be updated or not

Recordsets are immutable, but sets of the same model can be combined using various set operations, returning new recordsets.

record in set returns whether record (which must be a 1-element recordset) is present in set. record not in set is the inverse operation

set1 <= set2 and set1 < set2 return whether set1 is a subset of set2 (resp. strict)

set1 >= set2 and set1 > set2 return whether set1 is a superset of set2 (resp. strict)

set1 | set2 returns the union of the two recordsets, a new recordset containing all records present in either source

set1 & set2 returns the intersection of two recordsets, a new recordset containing only records present in both sources

set1 - set2 returns a new recordset containing only records of set1 which are not in set2

Recordsets are iterable so the usual Python tools are available for transformation (map(), sorted(), ifilter(), …) however these return either a list or an iterator, removing the ability to call methods on their result, or to use set operations.

Recordsets therefore provide the following operations returning recordsets themselves (when possible):

Return the records in self satisfying func.

func – a function, Domain or a dot-separated sequence of field names

recordset of records satisfying func, may be empty.

Return the records in self satisfying the domain and keeping the same order.

domain – A search domain.

Apply func on all records in self, and return the result as a list or a recordset (if func return recordsets). In the latter case, the order of the returned recordset is arbitrary.

func – a function or a dot-separated sequence of field names

self if func is falsy, result of func applied to all self records.

The provided function can be a string to get field values:

Since V13, multi-relational field access is supported and works like a mapped call:

Return the recordset self ordered by key.

key – It can be either of: a function of one argument that returns a comparison key for each record a string representing a comma-separated list of field names with optional NULLS (FIRST|LAST), and (ASC|DESC) directions None, in which case records are ordered according the default model’s order

a function of one argument that returns a comparison key for each record

a string representing a comma-separated list of field names with optional NULLS (FIRST|LAST), and (ASC|DESC) directions

None, in which case records are ordered according the default model’s order

reverse – if True, return the result in reverse order

Eagerly groups the records of self by the key, returning a dict from the key’s result to recordsets. All the resulting recordsets are guaranteed to be part of the same prefetch-set.

Provides a convenience method to partition existing recordsets without the overhead of a _read_group(), but performs no aggregation.

unlike itertools.groupby(), does not care about input ordering, however the tradeoff is that it can not be lazy

key – either a callable from a Model to a (hashable) value, or a field name. In the latter case, it is equivalent to itemgetter(key) (aka the named field’s value)

Odoo provides three different mechanisms to extend models in a modular way:

creating a new model from an existing one, adding new information to the copy but leaving the original module as-is

extending models defined in other modules in-place, replacing the previous version

delegating some of the model’s fields to records it contains

When using the _inherit and _name attributes together, Odoo creates a new model using the existing one (provided via _inherit) as a base. The new model gets all the fields, methods and meta-information (defaults & al) from its base.

“This is model 0 record A” “This is model 1 record B”

the second model has inherited from the first model’s check method and its name field, but overridden the call method, as when using standard Python inheritance.

When using _inherit but leaving out _name, the new model replaces the existing one, essentially extending it in-place. This is useful to add new fields or methods to existing models (created in other modules), or to customize or reconfigure them (e.g. to change their default sort order)

When _inherit is set to a string, then _name is set to the same value, unless _name is explicitly set.

It will also yield the various automatic fields unless they’ve been disabled

The third inheritance mechanism provides more flexibility (it can be altered at runtime) but less power: using the _inherits a model delegates the lookup of any field not found on the current model to “children” models. The delegation is performed via Reference fields automatically set up on the parent model.

The main difference is in the meaning. When using Delegation, the model has one instead of is one, turning the relationship in a composition instead of inheritance

and it’s possible to write directly on the delegated field:

when using delegation inheritance, methods are not inherited, only fields

_inherits is more or less implemented, avoid it if you can;

chained _inherits is essentially not implemented, we cannot guarantee anything on the final behavior.

A field is defined as class attribute on a model class. If the model is extended, one can also extend the field definition by redefining a field with the same name and same type on the subclass. In that case, the attributes of the field are taken from the parent class and overridden by the ones given in subclasses.

For instance, the second class below only adds a tooltip on the field state

The Odoo Exceptions module defines a few core exception types.

Those types are understood by the RPC layer. Any other exception type bubbling until the RPC layer will be treated as a ‘Server error’.

Generic error managed by the client.

Typically when the user tries to do something that has no sense given the current state of a record.

Warning with a possibility to redirect the user instead of simply displaying the warning message.

message (str) – exception message and frontend modal content

action_id (int) – id of the action where to perform the redirection

button_text (str) – text to put on the button that will trigger the redirection.

additional_context (dict) – parameter passed to action_id. Can be used to limit a view to active_ids for example.

Login/password error.

Traceback only visible in the logs.

When you try to log with a wrong password.

Remove the traceback, cause and context of the exception, hiding where the exception occured but keeping the exception message.

This method must be called in all situations where we are about to print this exception to the users.

It is OK to leave the traceback (thus to not call this method) if the exception is only logged in the logs, as they are only accessible by the system administrators.

When you try to read a record that you are not allowed to.

Missing value(s) in cache.

When you try to read a value in a flushed cache.

When you try to write on a deleted record.

Violation of python constraints.

When you try to create a new user with a login which already exist in the db.

**Examples:**

Example 1 (python):
```python
from odoo import models, fields
class AModel(models.Model):
    _name = 'a.model.name'

    field1 = fields.Char()
```

Example 2 (unknown):
```unknown
field2 = fields.Integer(string="Field Label")
```

Example 3 (unknown):
```unknown
name = fields.Char(default="a value")
```

Example 4 (python):
```python
def _default_name(self):
    return self.get_value()

name = fields.Char(default=lambda self: self._default_name())
```

---

## Expected revenue report¶

**URL:** https://www.odoo.com/documentation/19.0/applications/sales/crm/performance/expected_revenue_report.html

**Contents:**
- Expected revenue report¶
- Create an expected revenue report¶
  - Add custom filters¶
    - Add filter for expected closing date¶
    - Exclude unassigned leads¶
    - Add a filter for sales teams¶
- View results¶
  - View options¶

Expected revenue is the total cash value of leads that are expected to close by a certain date, usually the end of the current month.

An expected revenue report compiles all active leads in a sales pipeline that have a set expected closing date, and compares how sales teams are performing in a given time frame.

By pulling a monthly expected revenue report, sales managers can see which team members are reaching their goals, and who may need additional assistance to close valuable deals.

To create an expected revenue report, first navigate to CRM app ‣ Reporting ‣ Pipeline. This opens the Pipeline Analysis dashboard.

The Pipeline Analysis dashboard includes several filters in the search bar by default. Remove these before adding any additional custom filters.

On the top-left of the report, click Measures, then select Expected Revenue from the drop-down menu.

At the top of the page, click the 🔻(triangle pointed down) icon to the right of the Search… bar to open the drop-down menu that contains Filters, Group By, and Favorites columns. Under the Filters column, click Add Custom Filter, which opens an Add Custom Filter pop-up window.

In order to generate an expected revenue report, filters need to be created for the following conditions:

Expected closing date: limits results to only include leads expected to close within a specific time frame.

Exclude unassigned leads: excludes leads without an assigned salesperson.

Specific sales teams: limits results to only include leads assigned to one or more sales teams. This filter is optional and should not be included if the report is intended for the entire company.

On the Add Custom Filter pop-up window, click into the first field of the new rule. Type Expected Closing into the Search… bar, or scroll to select it from the list. Click in the second field and select is set. This limits the results to only include leads where an estimated closing date is listed.

Next, click the ➕ (plus) icon to the right of the rule to duplicate it.

Using the ➕ (plus) icon makes it easy to add multiple rules based on the same filter.

In the second field of the new rule, select is between from the drop-down menu. This creates a set time frame during which the expected closing date must occur for leads to be included in the results.

Click in each date field, one at a time, and use the calendar popover window to add both a start and end date to the rule. This is usually the beginning and ending of the current month, or fiscal quarter.

After filtering for the expected closing date, add a New Rule. Then, click into the new rule’s first field, and type Salesperson in the Search… bar, or scroll through the list to select it. Click in the rule’s second field and select is set from the drop-down menu. This excludes any results without an assigned salesperson.

This filter is optional. To view results for the entire company, do not add this filter, and continue to View results.

To limit the results of the report to one or more sales teams, click New Rule. Next, click the first field for the new rule, and type Sales Team in the Search… bar, or scroll to search through the list to locate it.

In the rule’s second field, select is in from the drop-down menu. Selecting this operator limits results to the sales teams indicated in the next field.

Lastly, click into the third field, and either: make a selection from the complete list revealed in the popover menu, or type the first few characters of the specific sales team’s title to quickly find and select it as a parameter.

Multiple teams can be added to the Sales Team rule, where each parameter is treated with an “or” (e.g. “any”) operator in the search logic.

At the top of the Add Custom Filter form, there is an option to match any or all of the rules. In order to properly run the report, only records that match all of the following filters should be included. Before adding the filters, make sure all is selected in this field.

At the bottom of the Add Custom Filter form, click Add.

The expected revenue report benefits from utilizing multiple views. The default graph view can be used to identify which salespeople are expected to bring in the most revenue, while the list view and pivot view provide more detail on specific deals.

The graph view is used to visualize data, and is beneficial in identifying patterns and trends.

Bar charts are used to show the distribution of data across several categories or among several salespeople.

Line charts are useful to show changing trends over a period of time.

Pie charts are useful to show the distribution, or comparison, of data among a small number of categories or salespeople, specifically how they form the meaningful part of a whole picture.

The default view for the expected revenue report is the bar chart, stacked. To change to a different graph view, click one of the icons at the top-left of the report. While both the line chart and bar chart are available in stacked view, the pie chart is not.

Graph view icons in order: bar chart, line chart, pie chart, stacked.¶

The list view provides a list of all leads that are expected to close by the designated date. Clicking on a lead in list view opens the record for detailed analysis, but many insights can be gleaned from the basic view.

To switch to the list view, click the ≣ (list) icon at the top-right of the report.

To add additional metrics to the report, click the additional options menu indicated by the toggle icon at the top-right of the list.

Clicking the toggle icon in list view opens the additional options menu.¶

Select any additional metrics from the drop-down menu to add them to the list view. Some options that may be useful are Expected Closing and Probability.

The pivot view arranges all leads that are expected to close by the designated date into a dynamic table.

To switch to the pivot view, click the Pivot icon at the top-right of the report.

When the pivot view is selected for this report, the X-axis lists the stages in the pipeline, while the Y-axis defaults to group the results by their creation date. To switch these groupings, click the flip access icon (⇄) at the top of the report.

To add additional measures to the report, click the Measures button at the top-left of the report. Select any additional metrics from the drop-down menu.

To add a group to a row or column to the pivot view, click the ➕ (plus sign) next to Total, and then select one of the groups. To remove one, click the ➖ (minus sign) and de-select the appropriate option.

Click Insert in Spreadsheet to add the pivot view into an editable spreadsheet format within the Dashboards app. If the Odoo Documents app is installed, the report can be inserted into a blank or existing spreadsheet, and exported.

---

## Audience targeting¶

**URL:** https://www.odoo.com/documentation/19.0/applications/marketing/marketing_automation/target_audience.html

**Contents:**
- Audience targeting¶
- Defining filters¶

The Target and Filter fields on the campaign form, also referred to as the domain, contain the parameters used to define the target audience for the campaign’s reach (i.e., the unique contact records in the database, and imported list, etc.).

Target: specifies the type of records available for use in the campaign, such as Lead/Opportunity, Event Registration, Contact, The assigned records model determines the fields that are available throughout the campaign, including the fields available in the Filter section, and in dynamic placeholders.

Save as Favorite Filter: saves the current Filter for future use with the current Target model, and can be managed from the Marketing Automation app ‣ Configuration ‣ Favorite Filters menu.

Unicity based on: specifies the Target model field where duplicates should be avoided. Traditionally, the Email field is used, but any available field can be used.

Filter: contains an interactive form with configurable logic to further refine the targeting parameters under the chosen Target model. See more details in the Defining filters section.

Include archived: allows or disallows the inclusion of archived records in the target audience.

A Responsible user can be assigned to the campaign by activating Developer mode (debug mode).

Each activity in a campaign’s workflow can target a subset of the target audience; see the Campaign workflow activities documentation for more information.

The default campaign Filter configuration is set to Match all records, indicating that the campaign is targeting all records of the Target model.

To refine the Filter rules of a campaign, click the ➕ Add condition button to reveal a new row with configurable rule parameters. See the Search, filter, and group records documentation for more information on how to create filter rules.

At the bottom of the filter rules is a # record(s) button, which indicates the total number of records targeted by this domain. Select the # record(s) button to open a Selected records pop-up window, in which the targeted records can be viewed.

Activate Developer mode (debug mode) to reveal each field’s technical name and data type, as well as the # Code editor text area below the filter rules, to view and edit the domain manually.

To target all leads and opportunities from the CRM app that are in the New stage, and have an expected revenue greater than $1,000, the following should be entered:

Target: Lead/Opportunity

Unicity based on: Email (Lead/Opportunity)

Filter: Match all 🔽 (down arrow) of the following rules:

Expected Revenue > 1,000

any 🔽 (down arrow) of:

With the above configuration, the campaign targets 157 record(s).

Domain developer documentation

Campaign workflow activities

Testing/running campaigns

---

## United Arab Emirates¶

**URL:** https://www.odoo.com/documentation/19.0/applications/hr/payroll/payroll_localizations/united_arab_emirates.html

**Contents:**
- United Arab Emirates¶
- Configuration¶
- Employees¶
- Contracts¶
- Salary structures and salary rules¶
  - Other input rules¶
  - End of service (EOS)¶
  - End of service provision (EOS Provision)¶
  - Annual leave provisions¶
  - Social insurance contributions¶

Install the following modules to get all the features of the United Arab Emirates Payroll localization:

United Arab Emirates - Payroll

Includes all rules, calculations, and salary structures.

United Arab Emirates - Payroll with Accounting

l10n_ae_hr_payroll_account

Includes all accounts related to the payroll module.

United Arab Emirates fiscal localization documentation

First, configure the employee general information and then configure the following fields under the Private Information tab:

Nationality (Country): The nationality affects an employee’s payslips, whether they are nationals or expats.

Identification Number: Used to extract the WPS report.

Bank Account: Used to extract the WPS report and generate payments for those employees.

The Nationality (Country) field needs to be set even if the employee is a UAE national since there is a different type of handling if they are citizens of a GCC country.

Once the employee form has been created, ensure the contract is enabled by clicking on the Contracts smart button, or going to Employees ‣ Contracts.

The following contractual information related to employees working in the United Arab Emirates are found under the Salary Information tab:

Wage Type: select Fixed Wage for Full-time or Part-time employees, or Hourly Wage for employees who are paid hourly.

Scheduled Pay: the frequency of payslip issuance.

Wage: Monthly or Hourly depending on the Wage Type.

Transportation Allowance

The allowance values set on the contract are used on the payslip lines as allowances.

Number of Leave Days: Used to specify the number of annual leave days that an employee deserves in a particular year. Regardless of the actual number of leaves that the employee gets (extra leave days for some internal company reasons), the final calculation of the end of service and unpaid leaves is dependent on the number set on this field.

The Number of Leave Days affects the calculation for unpaid leave provisions.

Is DEWS Applied: DIFC Employee Workplace Savings (DEWS), if the employee is a UAE national and has DEWS applied, tick this checkbox.

Computed Based On Daily Salary: Defines the way that the end of service is calculated:

Do not tick this checkbox if the standard calculation is to be used. This computes the compensation amount by dividing the monthly salary by 30 and then multiplying it by 21.

Tick this checkbox and directly set the actual Daily Salary so that it is used in the end of service calculation.

Tick this checkbox and directly set the actual Daily Salary so that it is used in the end of service calculation.

The following are the different allowances that can be defined directly on the payslip form to allow for the values that are set against these inputs to affect the WPS calculations as monthly variable salaries for the specific employee that they are linked to.

Rules that are related to the WPS setup, are linked to other input types, and whenever they are used, their values are reflected on the WPS as monthly variable salary for that specific employee.

Annual Passage Allowance

End of service (EOS) provides the calculation for the allowance that the employee gets at the end of their service. It is triggered when the employee’s departure reason is set by archiving the employee’s record.

There are several different calculations depending on the scenario:

The Employee spent less than a year in the company: The employee does not get any EOS allowance since they are not eligible for it (they are eligible once they complete their first year in the company).

The Employee spent more than a year and less than 5 years in the company: The employee is eligible for an equivalent of 21 days of salary for each year they spent on the company.

There are two ways for calculating the daily wage that gets paid for the employee against the 21 days of the EOS: Either by the default way of dividing the monthly basic wage by 30. Or, it can be manually input on the contract of the employee under the Daily Salary field.

The Employee spent more than 5 years in the company: The employee is eligible for an equivalent of 30 days of salary for each year they spent on the company. In this case, if the default method is used, then the employee gets paid an equivalent of 1 month of salary, and the set Daily Salary field, they will get the amount for the day multiplied by 30.

There are two payslips printout formats, one for normal salaries and one for end of service payslips, it is based on the employee being archived and having a departure reason or not.

The EOS provision provides the calculation for the end-of-service provision amount that the company puts aside every month to count for the EOS that will be paid to them as an EOS allowance.

Unlike the EOS, the provision is part of the employee’s payslip from the start of their contract.

Just like the EOS, the provision has two calculations depending on the period spent by the employee in the company:

Less than 5 years: \(\frac{\text{Monthly Wage}}{30}\times{\frac{21}{12}}\)

More than 5 years: \(\frac{\text{Monthly Wage}}{30}\times{\frac{30}{12}}\)

This rule is not shown to the employee on the payslip printout and it does not affect their net payable, it is only for internal use by the company.

Annual leave provisions are used for calculating the annual leave provision accumulated each month, just like the EOS provision, it does not affect the total amount paid to the employee, it is for internal use by the company.

It is calculated by dividing the employee’s total salary (Total Salary = Wage + Allowances) by 30 to get the daily salary. The daily salary is then multiplied by the eligible leave days and divided by 12 to determine the monthly provision amount.

Social insurance contributions calculate the social insurance, which is only available to UAE nationals.

The company contributes 15% of the total monthly salary for the employee if the company is in Abu Dhabi and 12.5% if the company is in another Emirate.

The total monthly salary for the employee = [basic + all allowances set on the contract].

On the other hand, the employee contributes 5% of their total monthly salary and that amount gets deducted from the payslip amount.

Annual remaining leave balance rules are used for calculating the amount to be paid to or taken from the employee based on the number of leave days deserved by the employee during the current year.

The annual leave time off type is specified using the Is Annual Leave checkbox.

If enabled, the rule calculates the amount of leave days deserved by the employee up to the current date and subtracts the number of annual leave days taken, and if the result is positive, this means that the employee should be compensated for remaining amount and if negative this means that the employee is liable to the company for the difference.

Sick leave rules provide the calculation for cases where the employee is on sick leave and decides how the payslip should be affected.

There are 3 cases for the employee to have:

Fully paid sick leave: The employee can upload a sick leave certificate (SLI). Employees are eligible for 15 days of this type of leave per calendar year.

The SLI is not mandatory in Odoo but can be done from the setup of the time off types.

50% paid sick leave: Same as the fully paid one, but the employees are eligible for 30 days from this leave type. These 30 days are counted after the first 15 fully paid days.

0% paid sick leave: Same as the fully paid one, but the employees are eligible for 45 days from this leave type. These 45 days are counted after the first 15/30 fully/half-paid days.

As per the labor law of the United Arab Emirates, the 15, 30, 45 days are not specified as working days or calendar days so this point will rely on the company policy.

The amount paid for the employee per sick leave day is counted as follows:

Where the gross per month is the basic + all other allowances set on the employee’s contract.

DEWS allows for calculating the DEWS amounts for the employees who are eligible for it and would like to be registered on it under their current contract with the company.

It is calculated based on the number of years that employees have spent in the company:

Less than 5 years: 5.83% is deducted from the employee’s BASIC salary towards the DEWS.

More than 5 years: 8.33% of The employee’s BASIC is deducted from the total payable for that employee.

Unpaid leaves allows for calculating the amount to be deducted when an employee takes an unpaid leave. It is calculated by the following equation:

Where the gross per month is the basic + all other allowances set on the employee’s contract.

The out of contract days rule provides a calculation for the days before/after the contract period that overlaps with the contract of days on the employee’s payslips.

Payslips are generated for the period of 1st-30th of September but the contract expires on the 21st, in this case, there are 7 days flagged as out of contract.

It is calculated by the following equation:

Manual deductions allows the user to add manual deductions to be applied to employees per payslip.

The amount to be deducted and the description of the deduction is to be set directly on the payslip manually as other inputs.

Net salary showcases the net amount that the employee will get based on the payslip.

It is calculated by adding basic to all allowances and deducting all deductions from it.

The approach taken for the rules above is to first get the full amounts for all static amounts that are set on the contract and then deduct the amounts that should be deducted such as unpaid leaves, sick leaves, manual deductions, commission, etc.

The accounts are linked to each payroll rule as a debit or credit so that when a draft entry is generated from a payslip, the amounts are reflected on the accounts accordingly.

The accounts need to be set in a way that would make the end-result entry balanced, otherwise a warning is raised if it is not balanced and it will not generate the entry.

After reviewing the payslips and making sure that all the amounts are correct, generate a draft entry, either one entry for all employees or an entry per employee depending on the setup done on the settings.

Debit and credit accounts set up for the basic and allowance rules.

After a batch or a payslip’s journal entry has been posted, the company can proceed to pay their employees.

In the batch itself or on the payslip, by clicking on the pay button, a payment is created and linked to the posted entry for the payslip. The same can be done for batch payslips if one payment is done from a single/multiple payment bank/cash journal.

Once the payslip is generated, the employee will be able to access the slips from their portal users. They will automatically receive an email mentioning that the payslips are now available to be viewed on their portal view.

Two printout formats can be extracted from the payslip, it depends on the type of the payslip either a Monthly payslip or an End of Service Payslip. It is triggered if the employee for the payslip is generated is archived during that month.

This structure is used when off-cycle payslips are required to make payments to employees for special situations, such as one-time or advance salaries. Examples of one-time payments include:

Employees may request a portion of their salary before the end of the pay cycle. In such cases, the payroll officer can issue a salary advance using the following steps:

Create an off-cycle payslip using the United Arab Emirates: Instant Pay structure.

Add another input of type Salary Advance and specify the amount to be paid to the employee.

Confirm the payslip and process the payment.

In the next cycle, when a payslip is generated for the employee using the United Arab Emirates: Regular Pay structure, another input type, Advance Recovery is automatically added for the same amount that was previously paid.

If the employee and payroll officer agree to recover the advance over two consecutive cycles, the payroll officer can adjust the Advance Recovery amount on the first payslip. The remaining balance is automatically added to the following cycle.

The WPS is a report that needs to be submitted by the company to prove that they paid their employees the right amounts on the right dates. It can either be generated per payslip or batch.

The following steps need to be followed before generating the report:

Go to Payroll ‣ Configuration ‣ Settings and under the UAE Payroll WPS Settings section, configure the following:

Employer Unique ID: Set a unique identifier for the company to be used in the WPS report.

Salaries Bank Account: Select a bank account or start typing to Create and edit a new bank account.

When setting the Salaries Bank Account make sure to complete the following:

Account Holder: set as the company.

Account Number: has to be a valid IBAN.

Bank: has to have the UAE Routing Code Agent ID set.

Send Money: should be enabled and set to Trusted.

Set the unique identifier on all of the employees who are a part of the target of the batch/payslip.

The Identification No field can be found on the employee’s page under the Private Information tab.

Once the initial setup is done, the WPS can be generated either for one payslip or for a batch as follows:

Generate the payslip one by one or as a batch.

Post the draft entity related to the payslips.

Create the payment report and set the Export Format to UAE WPS.

The report comes in a .sif format as per the governmental requirements, so either use software that can open .sif files or convert it to another format (.xslx) to be able to review it.

The resulting file consists of the following:

Employee Detail Record (EDR): includes details of the employees on the batch. There should be one EDR record per employee.

Employee Variable Pay (EVP): includes the details of the variable salary the employee got on that payslip. If the employee has any. The variable amounts are calculated from when other inputs are used that are linked to the salary rules (Payroll ‣ Configuration ‣ Rules).

Salary Control Record (SCR): There should only be one SCR per WPS file as it indicates the employer details and the totals for the payslips.

---

## Models, modules, and apps¶

**URL:** https://www.odoo.com/documentation/19.0/applications/studio/models_modules_apps.html

**Contents:**
- Models, modules, and apps¶
- Suggested features¶
  - Contact details¶
  - User assignment¶
  - Date & Calendar¶
  - Date range & Gantt¶
  - Pipeline stages¶
  - Tags¶
  - Picture¶
  - Lines¶

Models determine the logical structure of a database and how data is stored, organized, and manipulated. In other words, a model is a table of information that can be linked with other tables. A model usually represents a business concept, such as a sales order, contact, or product.

Modules and apps contain various elements, such as models, views, data files, web controllers, and static web data.

All apps are modules. Larger, standalone modules are typically referred to as apps, whereas other modules usually serve as add-ons to said apps.

When you create a new model or app with Studio, you can choose to add up to 14 features to speed up the creation process. These features bundle fields, default settings, and views that are usually used together to provide some standard functionality. Most of these features can be added later on, but adding them from the start makes the model creation process much easier. Furthermore, these features interact together in some cases to increase their usefulness.

Creating a model with the Picture and Pipeline stages features enabled adds the image in the card layout of the Kanban view.

Selecting Contact details adds to the Form view a Many2One field linked to the Contact model and two of its Related Fields: Phone and Email. The Contact field is also added to the List view, and the Map view is activated.

Selecting User assignment adds to the Form view a Many2One field linked to the Contact model, with the following Domain: Share User is not set to only allow the selection of Internal Users. In addition, the many2one_avatar_user widget is used to display the user’s avatar. The Responsible field is also added to the List view.

Selecting Date & Calendar adds to the Form view a Date field and activates the Calendar view.

Selecting Date range & Gantt adds to the Form view two Date fields next to each other: one to set a start date, the other to set an end date, using the daterange widget, and activates the Gantt view.

Selecting Pipeline stages activates the Kanban view, adds several fields such as Priority and Kanban State, and three stages: New, In Progress, and Done. The Pipeline status bar and the Kanban State field are added to the Form view. The Color field is added to the List view.

The Pipeline stages feature can be added at a later stage.

Selecting Tags adds to the Form and List views a Tags field, creating a Tag model with preconfigured access rights in the process.

Selecting Picture adds to the top-right of the Form view an Image field.

The Picture feature can be added at a later stage.

Selecting Lines: adds to the Form view a Lines field inside a Tab component.

Selecting Notes adds to the Form view an Html field using the full width of the form.

Selecting Monetary value adds to the Form and List views a Monetary field. The Graph and Pivot views are also activated.

A Currency field is added and hidden from the view.

Selecting Company adds to the Form and List views a Many2One field linked to the Company model.

This is only useful if you work in a multi-company environment.

Selecting Custom Sorting adds to the List view a drag handle icon to manually reorder records.

Selecting Chatter adds to the Form view Chatter functionalities (sending messages, logging notes, and scheduling activities).

The Chatter feature can be added at a later stage.

Selecting Archiving adds to the Form and List views the Archive action and hides archived records from searches and views by default.

When you do any customization with Studio, a new module named studio_customization is added to your database. You can export this module as a ZIP file using the Studio Export function. The module can then be imported into another Odoo database. This may be useful, for example, when setting up a new module or for training purposes.

Exporting and importing customizations in this way, rather than using the standard Odoo export and import functions, means data is imported in a logical way. For example, if the module contains customers and sales orders, the customers are created first, since these are required for the sales orders to be created.

To export customizations, click the (Toggle Studio) button on the main Odoo dashboard, then Export, then either:

download all Studio customizations by clicking the Export button; or

choose what data to export by clicking Configure data and demo data to export.

To select specific models to export, click New on the Studio Export screen, then start typing the name of the relevant model or select it from the list.

Click Preset to see a list of all models in your database with records that have been modified using Studio and all custom models created using Studio. To configure one of these models for export, click on the model to open it and make the required changes.

Tick the following options as relevant:

Demo: if the exported records should be considered as demo data when imported.

Attachments: if attachments related to exported records should be included in the export.

Updatable: if the exported records should be able to be updated during a module update.

If necessary, edit the Domain to determine which of the model’s records should be exported. To do so, click the Edit Domain button or (Modify filter) then Edit Domain, as appropriate. Proceed to make any required changes.

After configuring a model for export, click Studio Export to return to the main screen. To download a ZIP file with the customizations for all the listed models, click Export.

It is not necessary to select one or more models as all listed models will be included in the export. To remove a model from the export, select it and click the Actions button then Delete.

In the Studio Export window:

leave the checkboxes unticked to export only the customizations done with Studio.

tick Include Data to include data from the selected models in the export.

tick Include Demo Data to include data from the selected models that is flagged as demo data. Ticking this option also ticks Include Data.

Click the Export button to download the ZIP file.

Before importing, make sure the destination database is on the same Odoo version and contains the same apps and modules as the source database. Studio does not add the underlying modules as dependencies of the exported module.

To import and install Studio customizations in another Odoo database:

Connect to the destination database.

Click the (Toggle Studio) button on the main Odoo dashboard, then Import.

Upload the exported ZIP file. If demo data should be imported, tick Load demo data.

---

## View records¶

**URL:** https://www.odoo.com/documentation/19.0/developer/reference/user_interface/view_records.html

**Contents:**
- View records¶
- Generic structure¶
- View types¶
- Fields¶
- Inheritance¶
  - View resolution¶
  - Inheritance specs¶
  - Inheritance position¶
- Model commons¶

Views are what define how records should be displayed to end-users. They are specified in XML and stored as records themselves, meaning they can be edited independently from the models that they represent. They are flexible and allow a high level of customization of the screens that they control. There exist various types of views. Each represents a visualization mode: form, list, kanban, etc.

Basic views generally share the common minimal structure defined below. Placeholders are denoted in all caps.

Display and edit the data from a single record.

View and edit multiple records.

Apply filters and perform searches. The results are displayed in the current list, kanban… view.

Display records as “cards”, configurable as a small template.

Templating of reporting, website…

Visualize aggregations over a number of records or record groups.

Display aggregations as a pivot table.

Display records as events in a daily, weekly, monthly, or yearly calendar.

Display and understand the way some data changes over a period of time.

Display records as a Gantt chart.

Display computed information in numerical cells; are hardly configurable.

Display records on a map, and the routes between them.

View records expose a number of fields.

Only useful as a mnemonic/description of the view when looking for one in a list of some sort. Most Odoo view names start with the name of the addon and end with the type of view being discussed.

The model linked to the view, if applicable.

The description of the view layout depending on the view type.

The groups allowed to use/access the current view.

If the view extends an existing view, the extension will be applied only for a given user, if that user has access to the provided groups_id.

When requesting a view by specifying the model and type, the matching view with the lowest priority is returned (it is the default view).

It also defines the order of views application during view resolution. When a view is requested by id and its mode is not primary, its closest parent with mode = primary is matched.

Reference to the parent view on which the inheritance will be applied. Its value is used by default. Specify the parent using the ref attribute with ref="ADDON.MODEL_parent_view_TYPE".

The addon name (before the dot) is not necessary if the inheritance is done on a record of the same module.

See Inheritance for more information.

Only applies if this view inherits from an other one (inherit_id is set).

If the view is requested, the closest primary view is looked up (via inherit_id). Then, all views inheriting from it with this view’s model are applied.

The closest primary view is fully resolved (even if it uses a different model than the current one). Then, the view’s inheritance specs are applied, and the result is used as if it were this view’s actual arch.

A case in which one would want to override mode while using inherit_id is delegation inheritance. In that case, your derived model is separated from its parent, and views matching with one won’t match with the other. Assuming one inherits from a view associated with the parent model and wants to customize the derived view to show data from the derived model, the mode of the derived view needs to be set to primary because it is the base (and maybe only) view for that derived model. Otherwise, the view matching rules won’t apply.

See Inheritance for more information.

Selection: extension / primary

The current context and user access rights may also impact the view abilities.

Inheritance allows for customizing delivered views. It makes it possible, for example, to add content as modules are installed, or to deliver different displays according to the action.

Inherit views generally share the common structure defined below. Placeholders are denoted in all caps. This synthetic view will update a node targeted by an XPath, and another targeted by its name and attributes.

The inherit_id and mode fields determine the view resolution. The xpath or NODE elements indicate the inheritance specs. The expr and position attributes specify the inheritance position.

Resolution generates the final arch for a requested/matched primary view as follow:

if the view has a parent, the parent is fully resolved, then the current view’s inheritance specs are applied;

if the view has no parent, its arch is used as-is;

the current view’s children with mode extension are looked up, and their inheritance specs are applied depth-first (a child view is applied, then its children, then its siblings).

The inheritance is applied according to the inherit_id field. If several view records inherit the same view, the order is determined by the priority.

The result of applying children views yields the final arch.

Inheritance specs are applied sequentially and are comprised of:

an element locator to match the inherited element in the parent view;

children element to modify the inherited element.

There are three types of element locators:

An xpath element with an expr attribute. expr is an XPath expression1 applied to the current arch, matching the first node it finds;

A field element with a name attribute, matching the first field with the same name.

All other attributes are ignored.

Any other element, matching the first element with the same name and identical attributes.

The attributes position and version are ignored.

An extension function is added for simpler matching in QWeb views: hasclass(*classes) matches if the context node has all the specified classes.

The inheritance specs accept an optional position attribute, defaulting to inside, that specifies how the matched node should be modified.

The content of the inheritance spec is appended to the matched node.

The content of the inheritance spec is appended to the matched node’s parent after the matched node.

The content of the inheritance spec is appended to the matched node’s parent before the matched node.

The content of the inheritance spec replaces the matched node. Any text node containing only $0 within the contents of the spec is replaced by a copy of the matched node, effectively wrapping the matched node.

The content of the inheritance spec should be made of only attribute elements, each with a name attribute and an optional body.

If the attribute element has a body, a new attributed named after its name is added to the matched node with the attribute element’s text as value.

If the attribute element has no body, the attribute named after its name is removed from the matched node.

If the attribute element has an add attribute, a remove attribute, or both, the value of the matched node’s attribute named after name is recomputed to account for the value(s) of add, remove, and an optional separator attribute defaulting to ,. add includes its value(s), separated by separator. remove removes its value(s), separated by separator.

The attribute position="move" is set on the content of the inheritance spec to specify how nodes are moved relatively to the inheritance spec’s element locator, on which the attribute position must also be set, with values inside, replace, after, or before.

Returns the fields_views of given views, along with the fields of the current model, and optionally its filters for the given action.

The return of the method can only depend on the requested view types, access rights (views or other records), view access rules, options, context lang and TYPE_view_ref (other context values cannot be used).

Python expressions contained in views or representing domains (on python fields) will be evaluated by the client with all the context values as well as the record values it has.

views – list of [view_id, view_type]

options (dict) – a dict optional boolean flags, set to enable: toolbarincludes contextual actions when loading fields_views load_filtersreturns the model’s filters action_idid of the action to get the filters, otherwise loads the global filters or the model

a dict optional boolean flags, set to enable:

includes contextual actions when loading fields_views

returns the model’s filters

id of the action to get the filters, otherwise loads the global filters or the model

dictionary with fields_views, fields and optionally filters

Get the detailed composition of the requested view like model, view architecture.

The return of the method can only depend on the requested view types, access rights (views or other records), view access rules, options, context lang and TYPE_view_ref (other context values cannot be used).

view_id (int or None) – id of the view or None

view_type (str) – type of the view to return if view_id is None, one of 'form', 'list', …

options – options to return additional features param bool mobile true if the web client is currently using the responsive mobile view (to use kanban views instead of list views for x2many fields)

options to return additional features

true if the web client is currently using the responsive mobile view (to use kanban views instead of list views for x2many fields)

composition of the requested view (including inherited views and extensions)

AttributeError – if the inherited view has unknown position to work with other than ‘before’, ‘after’, ‘inside’, ‘replace’ if some tag other than ‘position’ is found in parent view

if the inherited view has unknown position to work with other than ‘before’, ‘after’, ‘inside’, ‘replace’

if some tag other than ‘position’ is found in parent view

**Examples:**

Example 1 (jsx):
```jsx
<record id="ADDON.MODEL_view_TYPE" model="ir.ui.view">
  <field name="name">NAME</field>
  <field name="model">MODEL</field>
  <field name="arch" type="xml">
    <VIEW_TYPE>
      <views/>
    </VIEW_TYPE>
  </field>
</record>
```

Example 2 (jsx):
```jsx
<record id="ADDON.MODEL_view_TYPE" model="ir.ui.view">
    <field name="model">MODEL</field>
    <field name="inherit_id" ref="VIEW_REFERENCE"/>
    <field name="mode">MODE</field>
    <field name="arch" type="xml">
        <xpath expr="XPATH" position="POSITION">
            <CONTENT/>
        </xpath>
        <NODE ATTRIBUTES="VALUES" position="POSITION">
            <CONTENT/>
        </NODE>
    </field>
</record>
```

Example 3 (jsx):
```jsx
<xpath expr="page[@name='pg']/group[@name='gp']/field" position="inside">
    <field name="description"/>
</xpath>

<div name="name" position="replace">
    <field name="name2"/>
</div>
```

Example 4 (jsx):
```jsx
<notebook position="inside">
    <page string="New feature">
        ...
    </page>
</notebook>
```

---

## Odoo Sign legality in Kenya¶

**URL:** https://www.odoo.com/documentation/19.0/applications/productivity/sign/kenya.html

**Contents:**
- Odoo Sign legality in Kenya¶
- Legal framework for electronic signatures in Kenya¶
- How Odoo Sign complies with Kenyan regulations¶
- Types of documents you can sign with Odoo Sign¶
- Potential exceptions¶

Odoo Sign is your trusted partner for secure, efficient, and legally compliant electronic signatures in Kenya.

In Kenya, electronic signatures are regulated under the Information and Communications Act. The law establishes the legal basis for acknowledging electronic records, contracts, and digital signatures. The key points of the law include:

Legal recognition: electronic signatures are legally equivalent to handwritten signatures if they meet certain criteria.

Reliability and security: electronic signatures must be created with a secure method that can reliably identify the signatory and ensure the integrity of the signed document.

Certified electronic signature: digital signature generated using a digital certificate supplied by a qualified provider.

Odoo Sign ensures full compliance with the Electronic Signature Law of Kenya by incorporating the following features:

Secure signature creation: Odoo Sign utilizes advanced cryptographic techniques to ensure the authenticity and integrity of electronic signatures.

Third-party signature authenticator: Odoo Online serves as an independent validation mechanism that adds an extra layer of security to the procurement process.

Audit trails: detailed audit logs are maintained to provide evidence of the signing process, including timestamps, IP addresses, and identity verification.

Cryptographic traceability and immutability: Odoo Sign ensures that any operation is logged securely. An audit log provides full transparency to all parties while preserving private data.

Multiple authentication means: authentication by SMS, email, geoIP or handwritten electronic signature.

Odoo Sign is versatile and can be used for a wide range of documents, including but not limited to:

Contracts and agreements: business contracts, employment agreements, and service contracts.

Financial documents: loan agreements, investment documents, and financial reports.

HR documents: employee onboarding forms, non-disclosure agreements (NDAs), and performance reviews.

Commercial transactions: purchase orders, sales agreements, and supplier contracts.

While Odoo Sign is broadly applicable, there are certain exceptions where electronic signatures may not be suitable or legally recognized in Kenya:

Wills and trusts: documents related to inheritance, wills, and trusts often require handwritten signatures.

Real estate transactions: some property transactions may still require notarized handwritten signatures.

Government forms: specific government forms and applications may mandate physical signatures.

The information provided on this page is for general informational purposes only and does not constitute legal advice. While Odoo Sign complies with the Electronic Signature Law of Kenya, users should consult with legal professionals to ensure specific document types and use cases meet all legal requirements. Compliance with additional industry-specific regulations may also be necessary.

Last updated: June 21, 2024

---

## Worksheets¶

**URL:** https://www.odoo.com/documentation/19.0/applications/services/field_service/worksheets.html

**Contents:**
- Worksheets¶
- Configuration¶
  - Create a worksheet template¶
- Add a worksheet template to a field service task¶
- Use worksheets on site¶

Worksheets help your field service workers perform and report their on-site tasks. They can feature various information, such as instructions, to-do lists, etc. You can also format your worksheet using checkboxes, bullet points, blank fields to fill in, HTML, and add files, images, links, and more.

It is common for businesses to have their workers perform the same type of field service repeatedly. Making custom worksheet templates eliminates the need to recreate the same worksheet each time you plan a similar field service task.

To use worksheets in Field Service, go to Field Service ‣ Configuration ‣ Settings, enable the Worksheets feature, and click Save.

Worksheet templates are designed using Studio. Enabling the Worksheets feature automatically installs the Studio app, which may impact your price plan.

To create your worksheet templates, go to Field Service ‣ Configuration ‣ Worksheet Templates. Click New and give your worksheet template a name. Manually save, then click Design Template to open Studio and customize your worksheet template.

In Studio, drag and drop the desired fields from the left column into your worksheet on the right. To rearrange the fields on the worksheet, drag and drop them in the desired order. Click a field to customize its properties.

When your worksheet template is complete, click Close in the top right corner of the page to leave Studio.

Fields and widgets in Studio

Go to your field service task, select a Worksheet Template, and click Save.

By default, the Default Worksheet template is selected. To define another default worksheet template, click the ➔ (Internal link) icon that appears when you hover your mouse over the Project field on the task form.

Then, in the Settings tab, scroll down to the Field service section and select the Worksheet Template you want to set up as default.

To complete the worksheet on site, access the task and click the Worksheet smart button.

As soon as you save a worksheet, the label of the Worksheet smart button on the task changes to Worksheet Complete instead, even if some fields are left blank.

Any field defined as Required has to be filled for a worksheet to be saved.

---

## Information panel¶

**URL:** https://www.odoo.com/documentation/19.0/applications/websites/livechat/information.html

**Contents:**
- Information panel¶
- Accessing the info panel¶
- Contact information¶
- Status¶
  - In progress¶
  - Waiting for customer¶
  - Looking for help¶
- Notes¶
- Tags¶
- Chatbot answers¶

The Live Chat information panel provides agents with the context they need to handle live chat conversations efficiently. It gathers key information about the visitor and the conversation history, allowing agents to respond faster and avoid repetitive messaging.

To view the info panel, open a live chat conversation either in the Discuss app or the Live Chat app. Live chat discussions in the Discuss app are listed on the left side panel, under the title of the live chat channel where the conversation began. In the Live Chat app, conversations can be accessed by navigating to Live Chat ‣ Sessions ‣ All Conversations.

The information panel appears on the right side of the conversation. Click the (information) icon to toggle the info panel open or closed.

If the live chat conversation involves a known contact, a View Contact button appears at the top of the panel. Clicking this button opens the contact record for the customer, without closing the conversation. The contact record links to the customers’ sales, invoices, meetings, and previous live chat sessions.

In an active livechat conversation, the Status can be set to allow agents to identify conversations that require immediate action, and inform other agents which conversations require their expertise at any given time.

The In progress status is the default status for a conversation. This status indicates that the customer is waiting for a response from an agent.

If a conversation is set to Waiting for customer, it will revert to In progress as soon as the customer sends a new message.

The Waiting for customer status indicates that an agent has sent a message to the customer and is waiting for a response. When this status is applied, the conversation is highlighted in yellow with a (hourglass) icon.

The Waiting for customer status must be manually applied.

If a conversation is marked with the status Looking for help, it moves from its original channel name heading to the Looking for help heading, and the (exclamation circle) icon is added. Any user with Live Chat permissions can view or join these conversations, even if they are not currently active in a live chat channel.

If a conversation with the Looking for help status has an expertise listed that matches the user’s, the conversation is marked with a (star) icon.

To join a conversation in progress, click the (sign in) icon at the top of the information panel. Doing so moves the conversation back to the channel heading and reverts the status to In progress. The customer is also informed that a new user has joined the chat.

If the icon does not appear, manually set the status to In progress to inform other agents the conversation is being handled.

All conversations that have been tagged with the status Looking for help can be found by navigating to Live Chat app ‣ Sessions ‣ Looking for Help. Use the filters to find the appropriate session date.

The Notes field allows agents to leave comments about the conversation, recap the situation when transferring it to another agent, or to add context to the conversation that can be viewed later in reporting.

Tags can be added to a conversation to assist with categorizing, tracking issues, and enhancing reporting. Click Tags , then select a tag from the list, or enter a new one in the field. Multiple tags can be added to a single conversation.

If the conversation was forwarded from a chatbot, the answers selected from the chatbot are included on the information panel under Chatbot answers.

The Expertise field allows agents to designate the topic of conversation to a specific skillset or knowledge scope. This helps to categorize the conversation for assignment purposes, as well as issue tacking and enchanced reporting. Click into the field and select one or more options from the drop-down list.

The Country & Language section identifies where the customer is located, and their language. A visitor’s language is determined via their browser’s language settings.

Conversations are assigned to operators based on a number of criteria, including availability and the number of ongoing conversations. While the operator’s main language and additional languages are taken into consideration, they do not supersede all other criteria.

Any recent live chat conversations with this customer also appear in the information panel. Click on the (external link) icon on the ticket title to open the conversation in a new tab.

Any open Helpdesk tickets created by the customer also appear in the information panel. Click on the (external link) icon on the ticket title to open the ticket record in a new tab.

For closed conversations, the Outcome field defines how the conversation concluded:

No Answer: assigned when the customer does not respond to the agent. This usually occurs when the session is initiated, but the customer does not engage or send additional messages.

No One Available: assigned when no agents are available to respond to the customer. This occurs when the session is initiated, but no operator is online or available to be assigned to the chat.

Success: assigned when the live chat session is completed successfully. This outcome does not depend the customer providing a positive rating, it is dependent on the session being resolved without escalation or failure.

Escalated: assigned when the session is forwarded to another operator. This indicates the initial operator could not resolve the issue, and required assistance.

When the conversation is complete, scroll to the bottom of the info panel. The email address in the field can be edited to send to a different address.click the (paper plane) icon to send a copy of the conversation transcript.

---

## Odoo rich-text editor¶

**URL:** https://www.odoo.com/documentation/19.0/applications/essentials/html_editor.html

**Contents:**
- Odoo rich-text editor¶
- Text editor toolbar¶
- Powerbox commands¶
  - Insert media¶
    - Media editor toolbar¶
    - Icon editor toolbar¶

The Odoo rich-text editor allows for creating and editing rich-text content in HTML fields, such as the Internal Notes and Description fields, as well as in the Knowledge articles and the Studio report editor, among others. Start typing or use the toolbar or powerbox for formatting and structuring text.

Hover over any element in the text (header, table, clipboard, etc.) to reveal the (drag) icon. Click and hold the icon to drag and drop the element elsewhere in the text.

To edit a word, sentence, or paragraph, select or double-click it to display the text editor toolbar and apply any of the following standard formatting options:

Font style: Define the font style using various options, such as Header 1 to 6, Normal, Paragraph, Code, and Quote.

Font size: Select the preferred font size.

(Toggle bold): Put the text in bold.

(Toggle italic): Put the text in italics.

(Toggle underline): Underline the text.

(Apply Font Color): Customize the font colors:

Solid: Select the preferred color from the predefined palette.

Custom: Customize the color palette using the wheel or by configuring the hex code and RGBA values.

Gradient: Select a predefined gradient or customize it by choosing between Linear or Radial and adjusting the wheel.

(Reset): Restore the original font/background color.

(Add a link): Insert or edit a URL link to a selected text, and optionally upload an image using its file URL.

Odoo AI: Write a prompt and get AI-generated content. Optionally, click the AI suggestions instead of writing a prompt.

Click the (Expand toolbar) icon to display additional formatting options:

Font family: Use the Default system font or select a preferred font family for the text.

(Toggle strikethrough): Strike through the text.

(Apply Background Color): Customize the background colors.

(Remove Format): Remove all formatting applied to a selected text.

(Toggle List): Select the following list options:

(Bulleted list): Turn the text into a bulleted list.

(Numbered list): Turn the text into a numbered list.

(Checklist): Turn the text into a checklist.

(Align text): Select the following text alignments:

(align left): Align the text on the left.

(align center): Align the text in the center.

(align right): Align the text on the right.

(justify): Apply straight edges to both text margins.

Translate with AI: Translate the content in the installed languages with AI.

Use the following keyboard shortcuts to apply formatting:

Emphasis: Press CTRL/CMD + B, CTRL/CMD + I, or CTRL/CMD + U to apply the bold, italics, or underlined effect.

Numbered list: Type 1., 1), A., or A) to start a numbered list.

Bulleted list: Type * or - to start a bulleted list.

Click a hyperlinked text and perform one of the following actions: (Copy Link), (Edit Link), or (Remove Link).

Commands enable editing and managing various types of features within the text editor, such as tables, banners, headers, and more.

To use a command, type / to open the powerbox, then enter the command’s name or select from multiple features to insert tables, images, banners, etc.

Starting a new paragraph displays a tooltip with command shortcut icons. Click an icon to add the command, or click the (ellipsis) icon to open the powerbox for all commands.

Commands specific to particular apps are excluded from this description.

Insert a horizontal rule separator.

Convert into 2 columns.

Convert into 3 columns.

Convert into 4 columns.

Create a bulleted list.

Create a numbered list.

Hide a group of text under a foldable toggle.

To organize a table, hover over a column or row to reveal the table menu. Click the (ellipsis) icon to move, insert, or delete a column or row.

Insert an info banner.

Insert a success banner.

Insert a warning banner.

Insert a danger banner.

Medium section heading.

Small section heading.

Paragraph block: Insert a paragraph.

Switch the text’s direction.

Add a blockquote section.

Insert an image or icon: Search the Unsplash database or upload images, documents, or icons.

Add a download box: share images, recordings, or documents that internal users can download.

Add a link: Type the label and enter a URL or upload a file, then click Apply.

Add a button: Type the label, enter a URL or upload a file, select the button style, type, and size, then click Apply.

Highlight the structure (headings): Create a table of content based on the headings.

Add an emoji: search for the desired emoji.

Insert a rating of up to 3 stars.

Insert a rating of up to 5 stars.

To insert media, type /Media or click the (media) icon in the tooltip, then choose from the following tabs:

Search the Unsplash database to find a suitable image.

Add URL: Copy-paste the image address.

Upload an image: Upload an image into the library.

Search for a document in the database.

Add URL: Copy-paste a valid URL.

Upload a document: Upload a document from a local drive.

Icons: Search for and select one of the available icons.

Videos: Paste a video URL of the following sources: YouTube, Vimeo, Dailymotion, and Youku. Alternatively, type code to embed a video.

When adding a video, use the toggles to enable autoplay or looping, hide player controls or the fullscreen button, or set a start time.

After inserting an image, click it to display the media editor toolbar, and apply any of the following formatting options:

(Preview image): Preview the image, zoom in or out, rotate it, print it, or download it. Exit the preview by clicking the (close) icon in the top right corner.

Description: Edit the image description and tooltip, then click Apply.

Caption: Write a caption under 100 characters below the image.

(Rounded): Apply a rounded shape to the corners of the image.

(Circle): Apply a circular shape to the image.

(Shadow): Apply a shadow effect to the image.

(Thumbnail): Apply a border to the image.

(Padding): Add an image padding and choose from Small, Medium, Large, or XL sizes.

Resize: Restore the image to its default size or set its size to 25%, 50%, or 100%.

(object): Resize and rotate the image. Click the (object) icon a second time to reset the transformation.

(Crop image): Crop the image manually or apply the following options:

Choose from the Flexible, 16:9, 4:3, 1:1, or 2:3 aspect ratios.

Rotate left or right.

Flip horizontally or vertically.

(Add a link): Add a link to the image, type the URL, then click Apply. To remove the link, click (Remove Link).

(Replace): Replace the image by searching in the Unsplash database, adding a URL, or uploading a different one.

(Delete): Delete the image.

After inserting an icon, click it to display the icon editor toolbar, and apply any of the following formatting options:

(Select Font Color): Customize the font color.

(Select Background Color): Customize the background color.

Resize icon: From 1x to 5x.

(Toggle icon spin): Activate the spin animation.

Replace: Select a different icon.

---

## Fields and widgets¶

**URL:** https://www.odoo.com/documentation/19.0/applications/studio/fields.html

**Contents:**
- Fields and widgets¶
- Simple fields¶
  - Text (char)¶
  - Multiline Text (text)¶
  - Integer (integer)¶
  - Decimal (float)¶
  - Monetary (monetary)¶
  - Html (html)¶
  - Date (date)¶
  - Date & Time (datetime)¶

Fields structure the models of a database. If you picture a model as a table or spreadsheet, fields are the columns where data is stored in the records (i.e., the rows). Fields also define the type of data that is stored within them. How the data is presented and formatted on the UI is defined by their widget.

From a technical point of view, there are 15 field types in Odoo. However, you can choose from 20 fields in Studio, as some field types are available more than once with a different default widget.

New Fields can only be added to the Form and List views. On other views, you can only add Existing Fields (fields already on the model).

Simple fields contain basic values, such as text, numbers, files, etc.

Non-default widgets, when available, are presented as bullet points or sub-headings below.

The Text field is used for short text containing any character. One text line is displayed when filling out the field.

Badge: displays the value inside a rounded shape, similar to a tag. The value cannot be edited on the UI, but a default value can be set.

Copy to Clipboard: users can copy the value by clicking a button.

E-mail: the value becomes a clickable mailto link.

Image: displays an image using a URL. The value cannot be edited manually, but a default value can be set.

This works differently than selecting the Image field directly, as the image is not stored in Odoo when using a Text field with the Image widget. For example, it can be useful if you want to save disk space.

Phone: the value becomes a clickable tel link.

Tick Enable SMS to add an option to send an SMS directly from Odoo next to the field.

URL: the value becomes a clickable URL.

The Multiline Text field is used for longer text containing any type of character. Two text lines are displayed on the UI when filling out the field.

Copy to Clipboard: users can copy the value by clicking a button.

The Integer field is used for all integer numbers (positive, negative, or zero, without a decimal).

Percentage Pie: displays the value inside a percentage circle, usually for a computed value. The value cannot be edited on the UI, but a default value can be set.

Progress Bar: displays the value next to a percentage bar, usually for a computed value. The field cannot be edited manually, but a default value can be set.

Handle: displays a drag handle icon to order records manually in List view.

The Decimal field is used for all decimal numbers (positive, negative, or zero, with a decimal).

Decimal numbers are displayed with two decimals after the decimal point on the UI, but they are stored in the database with more precision.

Monetary: it is similar to using the Monetary field. It is recommended to use the latter as it offers more functionalities.

Percentage: displays a percent character % after the value.

Percentage Pie: displays the value inside a percentage circle, usually for a computed value. The field cannot be edited manually, but a default value can be set.

Progress Bar: displays the value next to a percentage bar, usually for a computed value. The field cannot be edited manually, but a default value can be set.

Time: the value must follow the hh:mm format, with a maximum of 59 minutes.

The Monetary field is used for all monetary values.

When you first add a Monetary field, you are prompted to add a Currency field if none exists already on the model. Odoo offers to add the Currency field for you. Once it is added, add the Monetary field again.

The Html field is used to add text that can be edited using the Odoo HTML editor.

Multiline Text: disables the Odoo HTML editor to allow editing raw HTML.

The Date field is used to select a date on a calendar.

Remaining Days: the remaining number of days before the selected date is displayed (e.g., In 5 days), based on the current date. This field should be set to Read only.

The Date & Time field is used to select a date on a calendar and a time on a clock. The user’s current time is automatically used if no time is set.

As well as general properties, some specific properties are available for Date & Time fields that have the Date & Time or Date Range widget set.

The Date Range widget is used to display a period of time defined by a start date and an end date in a single line. A date range can have a mandatory start and end date, e.g., for a multi-day event, or allow an optional start or end date, e.g., for a field service intervention or a project task.

Adding a date range requires two fields: a Date & Time field with the Date Range widget set and another field that is selected as the start date or end date. This underlying field can be an existing Date or Date & Time field, or one created specifically for this purpose.

Identify an existing Date or Date & Time field that can be used as the underlying start/end date field, or add a new one. If the date range:

has a mandatory start date and end date, this field can be either the start date or end date; the outcome is the same.

allows an optional start or end date, this field is the start date or end date, respectively.

To avoid displaying the same information twice, the underlying start/end date field can be made invisible by enabling Invisible or removed from the view by clicking Remove from view.

Add a Date & Time field and set the Widget field to Date Range.

Enter an appropriate Label.

Select the underlying start/end date field from the Start date field or End date field dropdown, as relevant.

If the date range should have a mandatory start and end date, enable Always range.

Update any other general properties or specific properties for Date & Time fields as needed, then click Close in the upper right corner of the screen.

The Remaining Days widget displays the remaining number of days before the selected date (e.g., In 5 days), based on the current date and time. This field should be set to Read only.

The Checkbox field is used when a value should only be true or false, indicated by checking or unchecking a checkbox.

Button: displays a radio button. The widget works without switching to the edit mode.

Toggle: displays a toggle button. The widget works without switching to the edit mode.

The Selection field is used when users should select a single value from a group of predefined values.

Badge: displays the value inside a rounded shape, similar to a tag. The value cannot be edited on the UI, but a default value can be set.

Badges: displays all selectable values simultaneously inside rectangular shapes, organized horizontally.

Priority: displays star symbols instead of values, which can be used to indicate an importance or satisfaction level, for example. This has the same effect as selecting the Priority field, although, for the latter, four priority values are already predefined.

Radio: displays all selectable values at the same time as radio buttons.

By default, radio buttons are organized vertically. Enable Display horizontally to switch the way they are displayed.

Status Bar: displays all selectable values at the same time as an arrow progress bar.

By default, values on the status bar are selectable. Disable Clickable to prevent the value being edited on the UI.

The Priority field is used to display a three-star rating system, which can be used to indicate importance or satisfaction level. This field type is a Selection field with the Priority widget selected by default and four priority values predefined. Consequently, the Badge, Badges, Radio, and Selection widgets have the same effects as described under Selection.

To change the number of available stars by adding or removing values, click Edit Values. Note that the first value is equal to 0 stars (i.e., when no selection is made), so having four values results in a three-star rating system, for example.

The File field is used to upload any type of file, or sign a form (Sign widget).

Image: users can upload an image file, which is then displayed in Form view. This has the same effect as using the Image field.

PDF Viewer: users can upload a PDF file, which can be then browsed from the Form view.

Sign: users can electronically sign the form. This has the same effect as selecting the Sign field.

The Image field is used to upload an image and display it in Form view. This field type is a File field with the Image widget selected by default. Consequently, the File, PDF Viewer, and Sign widgets have the same effects as described under File.

To change the display size of uploaded images, select Small, Medium, or Large under the Size option.

The Sign field is used to sign the form electronically. This field type is a File field with the Sign widget selected by default. Consequently, the File, Image, and PDF Viewer widgets have the same effects as described under File.

To give users the Auto option when having to draw their signature, select one of the available Auto-complete with fields (Text, Many2One, and Related Field on the model only). The signature is automatically generated using the data from the selected field.

Relational fields are used to link and display the data from records on another model.

Non-default widgets, when available, are presented as bullet points below.

The Many2One field is used to link another record (from another model) to the record being edited. The record’s name from the other model is then displayed on the record being edited.

On the Sales Order model, the Customer field is a Many2One field pointing at the Contact model. This allows many sales orders to be linked to one contact (customer).

To prevent users from creating a new record in the linked model, tick Disable creation.

To prevent users from opening records in a pop-up window, tick Disable opening.

To help users only select the right record, click on Domain to create a filter.

To only trigger the search for a linked record after a minimum number of characters has been entered, enter the desired number in the Typeahead search field. In situations where the data set is large, this can enhance both search relevancy and performance.

Badge: displays the value inside a rounded shape, similar to a tag. The value cannot be edited on the UI.

Radio: displays all selectable values at the same time as radio buttons.

The One2Many field is used to display the existing relations between a record on the current model and multiple records from another model.

You could add a One2Many field on the Contact model to look at one customer’s many sales orders.

To use a One2Many field, the two models must have been linked already using a Many2One field. One2Many relations do not exist independently: a reverse-search of existing Many2One relations is performed.

The Lines field is used to create a table with rows and columns (e.g., the lines of products on a sales order).

To modify the columns, click on the Lines field and then Edit List View. To edit the form that pops up when a user clicks on Add a line, click on Edit Form View instead.

The Many2Many field is used to link multiple records from another model to multiple records on the current model. Many2Many fields can use Disable creation, Disable opening, Domain, just like Many2One fields.

On the Task model, the Assignees field is a Many2Many field pointing at the Contact model. This allows a single user to be assigned to many tasks and many users to be assigned to a single task.

To only trigger the search for the linked record after a minimum number of characters has been entered, enter the desired number in the Typeahead search field. In situations where the data set is large, this can enhance both search relevancy and performance.

Checkboxes: users can select several values using checkboxes.

Tags: users can select several values appearing in rounded shapes, also known as tags. This has the same effect as selecting the Tags field.

The Tags field is used to display several values from another model appearing in rounded shapes, also known as tags. This field type is a Many2Many field with the Tags widget selected by default. Consequently, the Checkboxes and Many2Many widgets have the same effects as described under Many2Many.

To display tags with different background colors, tick Use colors.

A Related Field is not a relational field per se; no relationship is created between models. It uses an existing relationship to fetch and display information from another record.

To display the email address of a customer on the Sales Order model, use the Related Field partner_id.email by selecting Customer and then Email.

Invisible: Enable this property when it is not necessary for users to view a field on the UI. This helps declutter the UI by only showing the essential fields depending on a specific situation.

The Invisible attribute also applies inside Studio. To view hidden fields in Studio, click on a view’s View tab and enable Show Invisible Elements.

Required: Enable this property if a field should always be completed by the user before being able to proceed.

Readonly: Enable this property if users should not be able to modify a field.

You can choose to enable Invisible, Required and Readonly for specific records only by clicking on Conditional and creating a filter.

On the Form view of the Contact model, the Title field only appears when Individual is selected, as that field would not be helpful for a Company contact.

Label: the field’s name on the UI. This is not the name used in the PostgreSQL database. To view and change the latter, activate the developer mode and edit the Technical Name.

Help Tooltip: To explain the purpose of a field, add a description. The text is displayed inside a tooltip box when hovering with your mouse over the question mark beside the field’s label.

Widget: To change the default appearance or functionality of a field, select one of the available widgets.

Placeholder: To provide an example of how a field should be completed, add placeholder text. The text appears in light gray until a value is entered.

Default value: To display a default value in a field when a record is created, add a value.

Allow visibility to groups: To limit which users can view the field, select one or more user access groups.

Forbid visibility to groups: To prevent certain users from seeing the field, select one or more user access groups.

For Date & Time fields that have the Date & Time or Date Range widget set, some specific properties are available:

Minimal precision: Determine the smallest date unit that must be selected in the date selector. The possible values are Day, Month, Year or Decade. If no value is selected, the user must select a day in the date selector.

Maximal precision: Determine the largest date unit that can be used to navigate the date selector. The possible values are Day, Month, Year or Decade. If no value is selected, the user can navigate the date selector by decade.

Warning for future dates: Enable this property to display a warning icon if a future date is selected.

Date format: By default the date will be shown as Apr 2, 2025, 08:05 AM. Enable this property to show the date in the format 4/2/2025 08:05:00. The numeric mode is the format set on the current language. In this mode the seconds are always shown.

Show date: This property is enabled by default for Date & Time fields. Disable this property to show only the time.

Show time: This property is enabled by default for Date & Time fields. On a read-only field, disable the property to show only the date. This can keep a list view less cluttered, for example.

Show seconds: This property is disabled by default for Date & Time fields. Enable the property to show the seconds.

Time interval: Enter a value to determine the minute intervals shown in the time selector. For example, enter 15 to allow quarter-hour intervals. The default value is set to 5 minutes.

Earliest accepted date: Enter the earliest date that can be selected in the date selector in ISO-format, i.e., YYYY-MM-DD. If the current date is always the earliest accepted date, enter today. On the date selector, dates prior to the earliest accepted date are grayed out.

Latest accepted date: Enter the latest date that can be selected in the date selector in ISO-format, i.e., YYYY-MM-DD. If the current date is always the latest accepted date, enter today. On the date selector, dates later than the latest accepted date are grayed out.

---

## Create opportunities from web contact forms¶

**URL:** https://www.odoo.com/documentation/19.0/applications/sales/crm/acquire_leads/opportunities_form.html

**Contents:**
- Create opportunities from web contact forms¶
- Customize contact forms¶
  - Customize contact form fields¶
- View opportunities¶

Adding a contact form to a website makes it easy to convert visitors into leads and opportunities. After a visitor submits their information, an opportunity can be created automatically, and assigned to a designated sales team and salesperson.

By default, the Contact Us page on an Odoo website displays a preconfigured contact form. This form can be customized, as needed, to suit the needs of a specific sales team.

Navigate to Website app ‣ Contact Us, then click Edit in the top-right of the screen to open the web editor. Click on the form building block in the body of the webpage to open the form configuration settings on the right sidebar. The following options are available to customize the contact form from the From section of the right sidebar:

Action: the default action for a contact form is Send an Email. Select Create an Opportunity from the drop-down list to capture the information in the CRM app.

Sales Team: choose a sales team from the drop-down menu that the opportunities from this form should be assigned to. This field only appears if the Action field is set to Create an Opportunity.

Salesperson: if the opportunities should be assigned to a specific salesperson, select them from the drop-down menu. If no selection is made in this field, the opportunities are assigned based on the team’s existing rules.

Marked Fields: use this field to alter how the form handles marked fields. The default option is to treat marked fields as Required, which is the recommended setting.

Mark Text: choose how Marked Fields should be identified. The default character is an asterisk (*).

Labels Width: use this field to alter the pixel width of the labels, if desired.

On Success: select how the webpage reacts after a customer successfully submits a form. Nothing keeps the customer on the same screen, with the addition of a confirmation message that the form was submitted successfully. Redirect sends the customer to a new webpage, based on the address provided in the URL field below. Show Message replaces the form with a preconfigured message that informs the customer someone should respond to them as soon as possible.

URL: if Redirect is selected in the On Success field, enter the URL for the webpage, where customers should be directed after successfully submitting a form.

Visibility: use the drop-down menu to add any visibility conditions for this field, if desired.

If leads are activated in CRM settings, selecting Create an Opportunity generates a lead instead. To activate leads, navigate to CRM app ‣ Configuration ‣ Settings, and tick the Leads checkbox. Then, click Save.

In addition to the settings for the form, the settings for each field can be customized, as well. With the web editor menu still open, click into a field to open the Field configuration settings section on the sidebar. The following options are available to customize a field:

Type: choose a custom field option or an existing field type.

Input Type: determine the type of information customers should input. Available options are Text, Email, Telephone, or Url. The selection made in this field limits the format that customers can use when entering information.

Label: enter the name for the field.

Position: choose the way the label is aligned with the rest of the form. The label can be hidden, above the field, to the left of the field, or right adjusted and closer to the field.

Description: slide the toggle to add a description for the field, which can provide additional instructions to customers. Click under the field on the form to add the description.

Placeholder: enter an example to help users know how to input information where formatting is important, such as a phone number or email address.

Default Value: enter a value to include in the form, by default, if the customer does not provide information in the field. It is not recommended to include a default value for required fields.

Required: slide the toggle to mark this field as required if it must be filled in for every submission.

Visibility: select when this field should be visible. Use the button on the left to choose whether to show or hide this field on a desktop users. Use the button on the right to choose whether to show or hide this field to mobile users.

Animation: select if this field should have any animation.

After a customer submits a contact form, and an opportunity is created, it is assigned based on the form settings. To view opportunities, navigate to CRM app ‣ Sales ‣ My Pipeline.

If leads are activated on the database, contact form submissions are generated as leads, not opportunities. To activate leads, navigate to CRM app ‣ Configuration ‣ Settings, and tick the Leads checkbox. Then, click Save.

Navigate to CRM app ‣ Leads to view the newly-created leads.

On the My Pipeline dashboard, click on an opportunity card in the Kanban view to open the opportunity record. The information submitted by the customer is visible on the opportunity record.

As the contact form fields are customizable, the fields on the opportunity record, where the form information is stored, varies accordingly.

If the preconfigured contact form is used, the Subject field is added to the Title field, and the content in the Notes field, which is labeled as Your Question, is added to the Internal Notes tab.

Convert leads into opportunities

Assign leads with predictive lead scoring

---

## Performance management¶

**URL:** https://www.odoo.com/documentation/19.0/applications/websites/ecommerce/performance.html

**Contents:**
- Performance management¶
- Data monitoring¶
- Analytics¶
- Email queue optimization¶

Odoo integrates a variety of tools to analyze and improve the performance of your e-commerce website.

Website allows monitoring and analysis of the sales performance of your e-commerce. To access the reporting view, go to Website ‣ Reporting ‣ eCommerce. This dashboard helps you monitor everything related to sales, such as sales performance per product, category, day, etc.

By clicking Measures, you can select the type of measurement used, such as:

Other options include multiple views (Pivot, etc.), comparison by periods or years, and directly insert in spreadsheet, etc.

It is possible to link your Odoo website with Plausible.io and Google Analytics.

For websites handling flash sales (e.g., event ticket sales) or experiencing high traffic spikes, order confirmation emails can become a performance bottleneck, potentially slowing down the checkout process for other customers.

To improve performance, these emails can be queued and processed separately from the order confirmation flow. This is managed by the Sales: Send pending emails scheduled action, which sends queued emails as soon as possible.

To enable asynchronous email sending:

Enable the developer mode.

Go to Settings ‣ Technical ‣ System Parameters and set the sale.async_emails system parameter to True.

Go to Settings ‣ Technical ‣ Scheduled Actions and ensure that the Sales: Send pending emails scheduled action is enabled.

Enabling this feature may delay order confirmation and invoice emails by a few minutes. It is recommended only for high-traffic websites, as it can introduce unnecessary delays for e-commerce websites with moderate traffic.

---

## Analyze performance¶

**URL:** https://www.odoo.com/documentation/19.0/applications/sales/crm/performance.html

**Contents:**
- Analyze performance¶

---

## AI fields¶

**URL:** https://www.odoo.com/documentation/19.0/applications/productivity/ai/fields.html

**Contents:**
- AI fields¶
- Adding a new AI field¶
  - Add via edit properties¶
- Computing AI fields¶

AI fields allow users to utilize Odoo’s built-in AI capabilities directly in forms and records. When an AI field is configured, the system can generate or suggest values automatically based on the record’s context, existing data, or external information.

This feature is especially useful for creating product descriptions, summarizing notes, or generating structured content from unstructured data.

AI fields can be added to a record through the Studio app or property field.

Installing Studio may impact the current pricing plan for a database. For more information, refer to Odoo’s pricing page or contact your account manager.

First, navigate to the page in the database where a new field is needed. Then, click on the studio icon to open the app. On the left sidebar, click and drag the AI Field option and place it in the desired location on the record.

After the field is placed, an Add an AI Field pop-up opens. Use the Field Type drop-down menu to select a field type:

Next is the Prompt field. In Odoo, prompts define the instructions that guide the AI when generating or improving the content of the AI field. The prompt tells the system what kind of information to produce, how to format it, and what tone or style to use.

When creating a prompt, use the /field command to reference specific fields in the database. For example, to reference the Company field on a record, enter /field, and click the Field Selector option. Type Company in the search bar, and select Company from the list.

Once the prompt is complete, click Add Field. Before closing Studio, click on the left sidebar and update the Label field with a title for the new AI field. Then click Close to exit Studio.

After the field is added, click the AI icon to refresh the field value.

If the AI is unable to complete the request, a warning message is generated. This could mean that the prompt is asking for information that is not available, in the given context, or that the prompt is providing unclear instructions. Use this as an indication to revisit the prompt and add additional context or instructions.

AI fields can also be added as property fields without opening the Studio app. Click on the Actions icon and select Edit Properties. Tick the AI checkbox, confirm the field should be AI-enabled, then follow the steps above to define the field type and enter the prompt.

Property fields can only be added to form views.

To compute, or refresh, an AI field, click on the AI icon next to the field. Clicking the button updates the field based on the prompt.

Additionally, a scheduled action runs once a day to fill all AI fields that are currently empty. This action is active by default.

To run the scheduled action manually, navigate to Settings app ‣ Technical ‣ Automation ‣ Scheduled Actions. Click on AI Fields:Compute AI fields to open it, then click Run Manually.

---

## Accidents¶

**URL:** https://www.odoo.com/documentation/19.0/applications/hr/fleet/accidents.html

**Contents:**
- Accidents¶
- Structure¶
- Log accidents and repairs¶
- Service stages¶
  - Service record¶
  - Kanban view¶
- Accident reporting¶
  - Services dashboard¶
  - Reporting dashboard¶
- Manage accident repairs¶

When managing a fleet, accidents are inevitable. Tracking accidents is crucial for understanding vehicle maintenance costs and identifying safe drivers.

Odoo’s Fleet app offers multiple ways of tracking accidents. Below are step-by-step instructions for one possible method to monitor accidents and repair costs.

For this use case, go to Fleet app ‣ Fleet ‣ Services. On the service form, two service types are created: Accident - Driver's Fault and Accident - No Fault.

This setup records repairs associated with accidents and organizes them by fault.

When an accident occurs, a service record is created. The specific repairs needed for the accident are logged in the Description of the service record, and the details about the accident are logged in the Notes section.

With this organizational structure, it is possible to view all accidents organized by fault, car, driver, or cost.

To manage accidents, the creation of service records is required.

Refer to the Services documentation for detailed instructions on creating service records in Odoo’s Fleet app.

To log an accident and initiate the repair process, the first step is to create a service record detailing the specific repairs needed.

Some accidents require multiple repairs with several different vendors. For these scenarios, a separate service record is needed for each vendor performing repairs. To keep records organized, it is recommended to keep the Notes field identical, and attach the same important documentation, such as a police report.

Navigate to Fleet app ‣ Fleet ‣ Services to view the main Services dashboard. Click New in the top-left corner, and a blank service form loads.

Enter the following information on the form:

Description: enter the description of repairs needed to fully repair the vehicle, such as Bodywork, Windshield Replacement, or Replacement Bumper, Tires, and Windows.

Service Type: for this example, select either Accident - Driver's Fault or Accident - No Fault, depending on the situation.

When entering either of these two Service Types for the first time, type in the new service type, then click Create (new service type). A Create Service Type pop-up window appears, with the new service type populating the Name field. In the Category field, select Service from the drop-down menu, then click the Save & Close button.

Once an accident service type has been added to the database, it is available to select from the drop-down menu in the Service Type field.

Date: using the calendar popover window, select the date the accident occurred. Navigate to the desired month using the (arrow) icons, then click the date to select it.

Cost: leave this field blank, as the repair cost is not yet known.

Vendor: select the vendor performing the repairs using the drop-down menu. If the vendor has not already been entered in the system, type in the vendor name, and click either Create to add them, or Create and edit… to add and configure the vendor.

Vehicle: select the vehicle that was in the accident from the drop-down menu. When the vehicle is selected, the Driver field is populated, and the unit of measure for the Odometer Value field appears.

Driver: the current driver listed for the selected vehicle populates this field when the Vehicle is selected. If a different driver was operating the vehicle when the accident occurred, select the correct driver from the drop-down menu.

Odometer Value: enter the odometer reading when the accident occurred. The units of measure are either in kilometers (km) or miles (mi), depending on how the selected vehicle was configured.

NOTES: enter the specific details of the accident at the bottom of the service form, such as Hit a deer or Rear-ended at an intersection while stopped.

Odoo provides the ability to attach any important paperwork, such as repair estimates and police reports, to the service record. To do so, click the (paperclip) icon, located in the chatter of the form, and a file explorer pop-up window appears. Navigate to the desired record, and click Open to upload the file.

Once a file is added to a service record, a Files section appears in the chatter. To attach more records, click Attach files to add more documents.

In Odoo’s Fleet app, there are four default service stages:

The default stage when a service record is created. The service has been requested, but repairs have not begun. The Cost field for this stage remains zero.

The repair is in-process, but not yet complete. The estimate for repairs is listed in the Cost field.

All repairs listed on the service form have been completed. The Cost field is updated to reflect the final total cost charged for the repairs.

The service request has been cancelled.

During the repair process, change the service status to reflect the vehicle’s current state in one of two ways: on the individual service record, or in the Kanban service view.

Open the main Services dashboard, by navigating to Fleet app ‣ Fleet ‣ Services. Next, click on the individual service record to open the detailed service form. Click the desired stage in the top-right corner, above the service form, to change the status.

Open the main Services dashboard, by navigating to Fleet app ‣ Fleet ‣ Services. First, click the Kanban icon in the top-right of the screen, which organizes all repairs by vehicle.

Next, remove the default Service Type filter in the search bar. Upon doing so, all services appear in a Kanban view, organized by their respective Status.

Drag-and-drop the service record to the desired stage.

One of the main reasons to track accidents using the method outlined in this document is the ability to view the total accident cost, determine the safest drivers, and calculate the actual total cost for specific vehicles.

The main Services dashboard displays all the various accident information, while the Reporting dashboard displays the total cost for specific vehicles.

Navigate to Fleet app ‣ Fleet ‣ Services to view the Services dashboard. All service records are displayed in a (List) view, grouped alphabetically, by Service Type.

The two service types created for accident tracking appear in the list: Accident - Driver Fault and Accident - No Fault.

Each grouping displays the number of records within each type, and lists the individual records beneath each grouping title.

In this example, there are three accidents where the driver was at fault, and one accident that was not the driver’s fault. This dashboard also displays the estimated total Cost for all the accidents in each group.

An estimated $3,284.00 dollars are costs from driver-caused accident repairs. The no-fault accident has no cost associated with it, since the repair has not been completed, and no estimate exists yet.

The total Cost calculates all costs on the repair form, including estimated costs, as well as final repair costs. This number may not be accurate, if there are any repairs in the Running stage, and the final bill has not yet been calculated.

Navigate to Fleet app ‣ Reporting ‣ Costs to view the Cost Analysis report. This report displays a (Bar Chart) of all Contract and Service costs for the current year, organized by month (Date : (year)), by default. The Sum, represented by a gray dotted line, is the combined total of both the Contract and Service costs.

To view the total cost by vehicle, click the (Toggle Search Bar) icon at the right of the search bar, revealing a drop-down menu. Click Vehicle in the Group By column, and the data is organized by vehicle.

This displays the true cost for each vehicle, including both the contract cost (such as the monthly vehicle lease cost) and all service costs, including all accidents. Hover over a column to reveal a data popover window, which displays the vehicle name and the total cost. This allows for a more complete view of the vehicle cost.

To view the individual cost details for both contract costs and repairs, click the (Pivot) icon in the top-right corner of the Cost Analysis dashboard. This displays each vehicle on a separate line, and displays the Contract cost and Service cost, as well as the Total cost.

The (Pivot) view organizes the data by vehicle, by default, therefore grouping the data by Vehicle is not required. If this filer is already activated, it does not affect the presented data.

For companies with multiple employees, who manage a large fleet of vehicles, displaying only service records in the New and Running stages can be time-saving, if there are a large number of records in the Services dashboard.

Navigate to Fleet app ‣ Fleet ‣ Services, where all service requests are organized by Service Type. Next, click the (Toggle Search Panel) icon at the right of the search bar, revealing a drop-down menu. Click Add Custom Filter in the Filters column, and a Add Custom Filter pop-up window appears.

Three drop-down fields need to be configured on the pop-up window.

In the first field, scroll down, and select Stage.

Leave the second field set to =.

Select Running from the drop-down menu in the last field.

Next, click the (plus) icon to the right of the last field, and an identical rule appears beneath the current rule.

Then, change Running to New in the third field of the second rule, leaving the other fields as-is.

Click the Add button at the bottom to add the new custom filter.

This slight modification only presents services in the New and Running stages. This is a helpful report for a company managing a high number of repairs at any given time.

To have this report appear as the default report when opening the Services dashboard, click the (Toggle Search Panel) icon at the far-right of the search bar. Next, click Save current search, beneath the Favorites column, which reveals another drop-down column beneath it. Tick the checkbox beside Default Filter, then click Save. Then, this customized Services dashboard appears, by default, any time the Services dashboard is accessed.

---
