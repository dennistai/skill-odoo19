# Odoo19 - Views

**Pages:** 29

---

## QWeb Reports¶

**URL:** https://www.odoo.com/documentation/19.0/developer/reference/backend/reports.html

**Contents:**
- QWeb Reports¶
- Report template¶
  - Minimal viable template¶
  - Translatable Templates¶
  - Barcodes¶
  - Useful Remarks¶
- Paper Format¶
- Custom Reports¶
- Custom fonts¶
- Reports are web pages¶

Reports are written in HTML/QWeb, like website views in Odoo. You can use the usual QWeb control flow tools. The PDF rendering itself is performed by wkhtmltopdf.

Reports are declared using a report action, and a Report template for the action to use.

If useful or necessary, it is possible to specify a Paper Format for the report report.

Report templates will always provide the following variables:

a reference to time from the Python standard library

res.user record for the user printing the report

record for the current user’s company

the current website object, if any (this item can be present but None)

the base url for the webserver

a function taking datetime.datetime in UTC1 and converting it to the timezone of the user printing the report

A minimal template would look like:

Calling external_layout will add the default header and footer on your report. The PDF body will be the content inside the <div class="page">. The template’s id must be the name specified in the report declaration; for example account.report_invoice for the above report. Since this is a QWeb template, you can access all the fields of the docs objects received by the template.

By default, the rendering context will also expose the following items:

records for the current report

list of ids for the docs records

model for the docs records

If you wish to access other records/models in the template, you will need a custom report, however in that case you will have to provide the items above if you need them.

If you wish to translate reports (to the language of a partner, for example), you need to define two templates:

The main report template

The translatable document

You can then call the translatable document from your main template with the attribute t-lang set to a language code (for example fr or en_US) or to a record field. You will also need to re-browse the related records with the proper context if you use fields that are translatable (like country names, sales conditions, etc.)

If your report template does not use translatable record fields, re-browsing the record in another language is not necessary and will impact performances.

For example, let’s look at the Sale Order report from the Sale module:

The main template calls the translatable template with doc.partner_id.lang as a t-lang parameter, so it will be rendered in the language of the partner. This way, each Sale Order will be printed in the language of the corresponding customer. If you wish to translate only the body of the document, but keep the header and footer in a default language, you could call the report’s external layout this way:

Please take note that this works only when calling external templates, you will not be able to translate part of a document by setting a t-lang attribute on an xml node other than t-call. If you wish to translate part of a template, you can create an external template with this partial template and call it from the main one with the t-lang attribute.

Barcodes are images returned by a controller and can easily be embedded in reports thanks to the QWeb syntax (e.g. see attributes):

More parameters can be passed as a query string

Twitter Bootstrap and FontAwesome classes can be used in your report template

Local CSS can be put directly in the template

Global CSS can be inserted in the main report layout by inheriting its template and inserting your CSS:

If it appears that your PDF report is missing the styles, please check these instructions.

Paper formats are records of report.paperformat and can contain the following attributes:

only useful as a mnemonic/description of the report when looking for one in a list of some sort

a small description of your format

either a predefined format (A0 to A9, B0 to B10, Legal, Letter, Tabloid,…) or custom; A4 by default. You cannot use a non-custom format if you define the page dimensions.

output DPI; 90 by default

page dimensions in mm

Landscape or Portrait

boolean to display a header line

By default, the reporting system builds rendering values based on the target model specified through the model field.

However, it will first look for a model named report.{module.report_name} and call that model’s _get_report_values(doc_ids, data) in order to prepare the rendering data for the template.

This can be used to include arbitrary items to use or display while rendering the template, such as data from additional models:

When using a custom report, the “default” document-related items (doc_ids, doc_model and docs) will not be included. If you want them, you will need to include them yourself.

In the example above, the rendering context will contain the “global” values as well as the lines we put in there but nothing else.

If you want to use custom fonts you will need to add your custom font and the related less/CSS to the web.reports_assets_common assets bundle. Adding your custom font(s) to web.assets_common or web.assets_backend will not make your font available in QWeb reports.

You will need to define your @font-face within this less file, even if you’ve used in another assets bundle (other than web.reports_assets_common).

After you’ve added the less into your assets bundle you can use the classes - in this example h1-title-big - in your custom QWeb report.

Reports are dynamically generated by the report module and can be accessed directly via URL:

For example, you can access a Sale Order report in html mode by going to http://<server-address>/report/html/sale.report_saleorder/38

Or you can access the pdf version at http://<server-address>/report/pdf/sale.report_saleorder/38

it does not matter what timezone the python:datetime object is actually in (including no timezone), its timezone will unconditionally be set to UTC before being adjusted to the user’s

**Examples:**

Example 1 (jsx):
```jsx
<template id="report_invoice">
    <t t-call="web.html_container">
        <t t-foreach="docs" t-as="o">
            <t t-call="web.external_layout">
                <div class="page">
                    <h2>Report title</h2>
                    <p>This object's name is <span t-field="o.name"/></p>
                </div>
            </t>
        </t>
    </t>
</template>
```

Example 2 (jsx):
```jsx
<!-- Main template -->
<template id="report_saleorder">
    <t t-call="web.html_container">
        <t t-foreach="docs" t-as="doc">
            <t t-call="sale.report_saleorder_document" t-lang="doc.partner_id.lang"/>
        </t>
    </t>
</template>

<!-- Translatable template -->
<template id="report_saleorder_document">
    <!-- Re-browse of the record with the partner lang -->
    <t t-set="doc" t-value="doc.with_context(lang=doc.partner_id.lang)" />
    <t t-call="web.external_layout">
        <div class="page">
            <div class="oe_structure"/>
            <div class="row">
                <div class="col-6">
                    <strong t-if="doc.partner_shipping_id == doc.partner_invoice_id">Invoice and shipping address:</strong>
                    <strong t-if="doc.partner_shipping_id != doc.partner_invoice_id">Invoice address:</strong>
                    <div t-field="doc.partner_invoice_id" t-options="{&quot;no_marker&quot;: True}"/>
                <...>
            <div class="oe_structure"/>
        </div>
    </t>
</template>
```

Example 3 (jsx):
```jsx
<t t-call="web.external_layout" t-lang="en_US">
```

Example 4 (jsx):
```jsx
<img t-att-src="'/report/barcode/QR/%s' % 'My text in qr code'"/>
```

---

## Glossary¶

**URL:** https://www.odoo.com/documentation/19.0/developer/glossary.html

**Contents:**
- Glossary¶

string identifier stored in ir.model.data, can be used to refer to a record regardless of its database identifier during data imports or export/import roundtrips.

External identifiers are in the form module.id (e.g. account.invoice_graph). From within a module, the module. prefix can be left out.

Sometimes referred to as “xml id” or xml_id as XML-based Data Files make extensive use of them.

inspired by jinja variables, format strings allow more easily mixing literal content and computed content (expressions): content between {{ and }} is interpreted as an expression and evaluated, other content is interpreted as literal strings and displayed as-is

any computer system or subsystem to capture, store, manipulate, analyze, manage or present spatial and geographical data.

process of removing extraneous/non-necessary sections of files (comments, whitespace) and possibly recompiling them using equivalent but shorter structures (ternary operator instead of if/else) in order to reduce network traffic

---

## Goals¶

**URL:** https://www.odoo.com/documentation/19.0/applications/hr/appraisals/goals.html

**Contents:**
- Goals¶
- View goals¶
- Create goals¶
  - Tags¶
- Update goals¶
- Complete goals¶

The Odoo Appraisals application allows managers to set (and track) clear goals for their employees. Continuous progress towards goals give employees a concrete target between reviews, and give managers reliable insights when evaluating performance.

To view all goals, navigate to Appraisals app ‣ Goals. This presents all the goals for every employee, in a default list view, grouped by Employee.

Click on an employee to expand the listed goals. Each goal displays the following information:

Name: The name of the goal.

Created on: The date the goal was made.

Progress: The percentage of progress the employee has achieved.

Employee: The employee assigned to the goal.

Only employees with goals assigned to them appear in the list.

To create new goals, navigate to Appraisals app ‣ Goals, and click New in the top-left corner to open a blank goal form. Add the following information on the form:

Goal: Type in a brief name for the goal in this field.

Employee: Using the drop-down menu, select the employee being assigned the goal. Once this field is populated, the employee’s manager populates the Manager field.

Progress: Click the current percentage of competency for the goal. The options are 0%, 25%, 50%, 75%, or 100%.

Manager: Using the drop-down menu, select the employee’s manager (if not already selected).

Deadline: Using the calendar selector, enter the due date for the goal.

Tags: Using the drop-down menu, add any relevant tags to the goal.

Description tab: Enter any details regarding the goal in this tab.

Some goals can be broken down into steps, which may be input as a checklist. A checklist is a tool the employee may use to mark their progress.

Adding tags to goals can help when viewing the goals report, to see how many goals with specific tags are assigned to employees.

To view all the current tags, and add new ones, navigate to Appraisals app ‣ Configuration ‣ Tags. All tags appear in a list view. The default tags are: External, Hard Skills, Internal, Programming, Soft Skills, and Training.

To add a new tag, click the New button in the upper-left corner, and a new line appears at the bottom of the list. Enter the tag, then press return or click away from the field.

During employee appraisals, goals are reviewed to see how much progress the employee has made. When an employee has achieved the next level of progress, the goal must be updated accordingly.

To update a goal’s progress percentage, navigate to Appraisals app ‣ Goals. Expand the employee whose goals are being evaluated, and click on an individual goal to open the goal record.

Click the new Progress box to set the new progress level. It is recommended to add notes in the Description tab, as the employee progresses with the goal. The notes should include dates the progress changed, and any supporting information regarding the change.

Goal progress can be updated at any time by the employee’s manager, not only during an appraisal.

When a goal has been met, it is important to update the record. Navigate to Appraisals app ‣ Goals. Expand the employee whose goals are being evaluated, and click on an individual goal to open the goal record.

Click the Mark as Done button in the upper-left corner. A green Done banner appears in the top-right corner of the goal card, and the Progress changes to 100%.

On the Goals dashboard, completed goals are indicated with a green 100% in the Progress column.

---

## Service rental products¶

**URL:** https://www.odoo.com/documentation/19.0/applications/sales/rental/service_products.html

**Contents:**
- Service rental products¶
- Settings¶
- App integration configuration¶
- Rental services¶
- Create a new service product¶
  - Configure rental price¶
- Create a rental order with a service product¶
- Customer signature¶
  - Signing a document from an email link¶
- Entering time for the rental order¶

The Rental app is a comprehensive tool that enables users to manage the scheduling, pricing, and inventory for both physical goods (products) and non-physical goods (services) within a single platform.

This flexibility allows for combining products and services like bike rentals with guided tours, or booking a studio with a photographer.

This document covers how to configure a rental service to automatically sync with staff shifts, track time sheet hours, and set up and link project tasks based on a rental order.

To configure default settings on rental products, navigate to Rental app ‣ Configuration ‣ Settings.

In the Rental section, under the Default Delay Costs subsection, fill in the Apply after field.

For finer control, configure the costs of late returns for the Per Hour and Per Day fields at the product level. If the defaults apply to all products, leave the Product field blank.

In the Default Padding Time section, fill in the Padding field.

Next, enable Rental Transfers. In the Rent Online section, fill in the Minimal Rental Duration field and designate Unavailability days. Click Save to apply the changes.

The following apps are essential for workflow efficiency and automation when creating a service product and rental order:

Sales app: Enables the use of online payments and utilizes quotation templates within the Rental app.

Sign app: Allows for the upload and customization of different rental and service agreements. These documents are used to facilitate the Request Signature feature.

Project and Planning apps: Integrate with the Rental app to automatically match purchased products and services with employees based on availability.

Online payment order confirmation

To view all products that can be rented in the database, navigate to Rentals app ‣ Products. By default, the Rental filter appears in the search bar, and the view is Kanban. Remove the filter, then click the search bar. From the preset filters, select Services. All the configured services appear.

Each Kanban card displays the name and rental price of the service.

The Project and the Sales apps must be installed for following options to be available:

Ticking the Sales checkbox displays the Create on Order and Invoicing Policy.

To set up a new rental service, go to the Rental app ‣ Products and then click New. In the new product window, the Rental checkbox is already ticked by default.

Tick the Sales checkbox. Select the Product Type as a Service. In the Create on Order drop-down menu, select Project & Task. In the Invoicing Policy drop-down menu, select Based on Timesheets.

Tick the Plan Services checkbox and either create a new role or select a pre-existing one. To create a new role, type in the name of the role in the blank field and click Create and edit that appears.

In the Create Planning Role pop-up window, enter the role’s name. Select an option for the Services and Resources, and click Save.

Click the Rental prices tab and in the Pricing section, click Add a price to enter a new rental rate. Choose a pricing period (the unit of duration of the rental) in the Period column, or create a new pricing period by typing in the name and clicking Create and edit.

Customize rental rate time periods by navigating to Rental app ‣ Configuration ‣ Rental periods.

Next, enter the Price for that specific Period. To apply the configured rental rate to an existing pricelist, click in the Pricelist column and select the desired list from the drop-down menu.

In the Reservations section, fill in the Hourly Fine, Daily Fine, and the Reserve product time. These values are automatically populated from the Default Delay Costs section, provided they have been configured in the Rental app ‣ Configuration ‣ Settings.

Click the (Save manually) icon near the top to save.

A photography studio rents out its photographers on an hourly and daily basis. They want to add a new four hour package for $750.

All reservations require 24-hour notice to reserve a photographer, but they do not charge a fine if the reservations go over the agreed time. Instead, they default to their hourly fee.

Create a new pricing period by navigating to Rental app ‣ Configuration ‣ Rental periods. Click New and configure the period for four hours.

Navigate to the Photographer service product and in the Rental prices tab, add the four hour period set at $750. Manually save to apply changes.

In Odoo, a rental order is the same as a sales order. When creating orders in the Rental app they are referred to as rental orders.

Navigate to the Rental app which opens the Rental Orders dashboard. Click New to open a new rental order form.

Enter the Customer field, and select a Quotation Template, if applicable.

Next, set the desired rental duration in the Rental period field. To adjust the rental duration, click the first date in the Rental period field, and select the range of dates and times to represent the rental duration from the pop-up calendar form that appears.

Once complete, click Apply in the calendar pop-up form. Following that, the pop-up form disappears, and the designated rental time period is represented in the Duration field.

Next, add a rental service in the Order Lines tab, by clicking Add a product and selecting the desired rental service to add to the form. Enter the desired amount in the Quantity column.

If a rental product is added before the Rental period field has been properly configured, the user can still adjust the Rental period field accordingly. Select the desired range of dates to represent the duration of the rental, then click Update Rental Prices in the Duration field.

Doing so reveals a Confirmation pop-up window. If everything is correct, click Ok, and Odoo recalculates the rental price accordingly.

Once all information has been entered correctly on the rental order form, click Send to email the quotation to the customer. When the customer approves the quotation, click Confirm. A banner displays on the rental order stating its current status.

Odoo enables electronic signature requests for customer service agreements and any other documents that require signatures. Service agreements detail the business relationship and mutual duties. These agreements protect both the provider and the customer by creating clear, enforceable guidelines.

If signatures are required, go to the Rental app and from the default Rental Orders dashboard, select the desired rental order. Click the (Actions) icon, and click Request Signature.

A New Signature Request pop-up window displays. Select the desired document from the Template drop-down menu.

Doing so reveals a New Signature Request pop-up window. Upon confirming the information in the New Signature Request pop-up form, click Send to initiate the signing process.

A link to the signature request will appear in the chatter of the rental order. The document is accessible to the customer via the customer portal or email.

Requesting an electronic signature can be done at any stage of the order. This feature requires the Sign app. Typically, rental or service agreements are signed after the rental order is confirmed to establish the responsibilities and terms for the parties involved.

The Request Signature feature only allows the customer to sign the document through their email or customer portal. The customer cannot sign the document through the user’s Sign app.

When a customer clicks Sign document, a separate page is then revealed, showcasing the document to be signed. The customer begins the process by clicking Click to start.

The app guides the signer to the required signature locations and allows them to create electronic signatures to complete the form.

The Adopt your signature pop-up window that appears in the Odoo Rental application. Once the document has been signed and completed, click Validate & Send Completed Document at the bottom of the document.

Odoo presents the option to download the signed document for record-keeping purposes, if necessary.

Odoo Tutorials: Sign.

For the appropriate smart buttons to display, the Project, Timesheet, and Planning apps are needed. The selected service product on the rental order must be properly configured to integrate with the recommended apps.

Navigate to the rental orders by Rental app ‣ Orders ‣ Orders and select the desired rental order. At the top of the rental order form, the following smart buttons appear:

or tasks related to the rental order.

are related to the rental order.

related to the rental order.

Click the Tasks smart button to view a Kanban view of all the associated tasks that were automatically created when confirming the rental order. Click the desired task, then select the Timesheets tab.

Click Add a line to enter the number of hours worked on the task manually. Click on the Sales Order smart button to return to the rental order.

Once time is added to the Timesheets tab of a task, the status of the rental order is automatically changed to Picked-up and the Return button appears.

Go to the desired invoice by navigating to the Rental app and, on the Rental Orders dashboard, click To Invoice in the INVOICE STATUS section to view all rental orders that require invoices to be sent.

Click on the desired rental order and click Create Invoice. Select Regular invoice from the Create invoice(s) window and click Create Draft.

If all the details are correct, click Confirm. Click Send to email the invoice to the customer or Print and then click Pay if the customer is in person.

In the Pay pop-up window, select a Journal and click Create Payment.

Click on the Payments smart button that appears on the top of the rental order. Click Validate on the Payment page.

Use the breadcrumbs to return to the rental order.

When time is entered on the Timesheets tab of an associated task, the rental order status automatically changes to Picked-up. This happens even if time is entered before the physical product ordered with the service is picked up.

If a product is rented alongside a service, it is advised to pick it up before entering time on the associated task. The Pickup button is still available on the rental order if time is entered before picking up the product.

When a customer picks up the product, navigate to the appropriate rental order, click Pickup, and then click Validate in the Validate a pickup pop-up form that appears.

Doing so places a Picked-up status banner on the rental order.

Regardless if there is a product rented along with a service, the service or product must be returned on the rental order.

When a customer returns the products or when the service has been completed, navigate to the appropriate rental order and click Return. Validate the return by clicking Validate in the Validate a return pop-up form that appears.

Doing so places a Returned status banner on the rental order.

The photography studio had a customer who wanted to rent one of their photographers and banner decorations for a home photo shoot. The booking was for two hours.

On the Validate a return form the rental order, the banner line item matches number of banners picked up and the photographer line item matches the number of hours submitted on the Timesheets tab on the related task.

---

## Geolocation¶

**URL:** https://www.odoo.com/documentation/19.0/applications/general/integrations/geolocation.html

**Contents:**
- Geolocation¶

You can locate contacts or places and generate routes on a map in Odoo.

To use the feature, open the Settings app, and, under the Integrations, section, activate Geo Localization. Then, choose between using the OpenStreetMap or Google Places API.

OpenStreetMap is a free, open geographic database updated and maintained by volunteers. To use it, select Open Street Map.

OpenStreetMap might not always be accurate. You can join the OpenStreetMap community to fix any issues encountered.

Google Places API map

The Google Places API map provides detailed info on places, businesses, and points of interest. It supports location-based features like search, navigation, and recommendations.

Using the Google Places API could require payment to Google.

To use it, select Google Place Map and enter your API Key.

---

## Event templates¶

**URL:** https://www.odoo.com/documentation/19.0/applications/marketing/events/event_templates.html

**Contents:**
- Event templates¶
- Event templates page¶
- Create event template¶
  - Event template form¶
    - Booths tab¶
- Use event templates¶

The Odoo Events application provides the ability to customize and configure event templates, which can be used to expedite the event-creation process.

These templates can be created and personalized in the application, and then selected from an event form, in order to quickly apply a series of settings and elements to the new event, all of which can be further modified, if needed.

In the Odoo Events application, event templates can quickly be created and modified.

To begin, navigate to Events app ‣ Configuration ‣ Event Templates. Doing so reveals the Event Templates page. Here, find all the existing event templates in the database.

By default, Odoo provides three pre-configured event templates: Exhibition, Training, and Sport, which all have their own unique customizations applied to them.

To change how these event templates appear on the Template drop-down field on an event form, drag-and-drop them into any desired order, using the (draggable) icon, located to the left of each event template line on the Event Templates page.

To learn more about event forms, refer to the Create events documentation.

There are two ways to create and configure an event template in Odoo Events.

On the dashboard, by navigating to Events app ‣ Configuration ‣ Event Templates and clicking the New button in the upper-left corner. Doing so reveals a blank event template form that can be customized in a number of different ways.

On an event form itself. Start by typing the name of a new event template in the Template field, and click Create and edit… from the resulting drop-down menu. Doing so reveals a Create Template pop-up window, featuring all the same configurable fields and elements found on a standard event template form.

Clicking Create “[template name]” from the resulting drop-down menu, via the Template field on an event form creates the event template in the database, but does not present the user with the Create Template pop-up window.

The event template would have to be modified, by selecting it on the Event Templates page (Events app ‣ Configuration ‣ Event Templates).

All the fields on a standard Event Template form are also on the Create Template pop-up window, accessible via the Template field on an event form.

Start by providing the event template with a name in the Event Template field, located at the top of the form.

Beneath that field, there is a series of selectable checkboxes, all of which are related to how the event menu will be displayed on the event web page.

Website Submenu: enables a submenu on the event’s website. When this checkbox is ticked, every other checkbox in this series is automatically ticked, as well. Then, choose to untick (deselect) any of the checkbox options, as desired.

Tracks Menu Item: adds a submenu item to navigate to a page displaying all planned tracks for the event.

Track Proposals Menu Item: adds a submenu item to navigate to a page, in which visitors can fill out a form to propose a track (talk, lecture, presentation, etc.) to happen during the event.

Booth Menu Item: adds a submenu item that takes visitors to a separate page, where an event booth can be purchased. Event booths can be customized and configured in the Booths tab of the event template form, from the Booth Categories page (Events app ‣ Configuration ‣ Booth Categories).

First, users must create a booth product with the required Event Booth option set as the Product Type on the product form.

Exhibitors Menu Item: adds a submenu item that takes visitors to a separate page, showcasing all the exhibitors related to that specific event. Icons representing those exhibitors are also found on the footer of every event-specific web page, as well.

Community: adds a submenu item allowing attendees to access pre-configured virtual community rooms to meet with other attendees, and discuss various topics related to the event. When this checkbox is ticked, the Allow Room Creation feature becomes available.

Allow Room Creation: allow visitors to create community rooms of their own.

Register Button: adds a button at the end of the event submenu that takes visitors to the event-specific registration page when clicked.

Once the desired checkboxes have been ticked, select an appropriate Timezone for the event from the available drop-down menu.

Then, for organizational purposes, there is the option to add Tags to this event template.

There is also the option to Limit Registrations to this specific event template by ticking that checkbox. If ticked, proceed to enter the number of Attendees this template should be limited to.

Beneath those general information fields at the top of the event template form, there are five tabs:

The Booths tab on an event template form is the only tab that differentiates itself from a standard event form, where the other tabs (Tickets, Communication, Questions, and Notes) are present and configured using the same process. For more information about those tabs, refer to the Create events documentation.

To create a booth or booth category, an event booth product must be created in the database first, with the Product Type set to Event Booth. Only products with that specific configuration can be selected in the required Product field of a booth or booth category form.

Event booths can be created and customized in two ways in the Odoo Events application. Either in the Booths tab of an event template form, or by navigating to Events app ‣ Configuration ‣ Booth Categories, and click New.

To add a booth from the Booths tab of an event template form, click Add a line. Doing so reveals a blank Create Booths pop-up window.

Start by providing a Name for this booth in the corresponding field at the top of the pop-up window.

Then, select an appropriate Booth Category from the drop-down field below. Booth categories can be created and modified from the Booth Categories page in the Events application, which is accessible by navigating to Events app ‣ Configuration ‣ Booth Categories.

A Booth Category can be created directly from this field on the Create Booths pop-up window. To accomplish that, type the name of the new booth category in the Booth Category field, and select either Create or Create and edit… from the resulting drop-down menu.

Clicking Create merely creates the category, which can (and should be) customized at a later date. Clicking Create and edit… reveals a new Create Booth Category pop-up window, from which the category can be configured in a number of different ways.

From this pop-up window, proceed to name the Booth Category, modify its Booth Details settings, configure its Sponsorship options (if applicable), and leave an optional Description to explain any pertinent details related to this specific category of booths.

There is also the option to add a photo/visual representation of the booth category, via the (camera) icon in the upper-right corner.

When all desired configurations are complete, click the Save & Close button.

The same configurations and options are available by navigating to Events app ‣ Configuration ‣ Booth Categories, and clicking New.

Once the desired Booth Category is selected, the remaining fields on the Create Booths pop-up window (Currency, Product, and Price) autopopulate, based on information configured for that selected Booth Category.

These fields cannot be modified from the Create Booths pop-up window. They can only be modified from the specific booth category form page.

When all desired configurations are complete, click Save & Close to save the booth, and return to the event template form. Or, click Save & New to save the booth, and start creating another booth on a fresh Create Booths pop-up window. Click Discard to remove all changes, and return to the event template form.

Once the booth has been saved, it appears in the Booths tab on the event template form.

Once an event template is complete, it is accessible on all event forms in the Odoo Events application.

To use an event template, navigate to the Events app and click New to open a new event form.

From the event form, click the Template field to reveal all the existing event templates in the database. They appear in the same order as they are listed in on the Event Templates page (Events app ‣ Configuration ‣ Event Templates).

Select the desired event template from the Template drop-down field on the event form. Pre-configured settings automatically populate the event form, saving time during the event creation process.

Any of these pre-configured settings related to the selected event template chosen in the Template field on an event form can be modified, as desired.

---

## Email templates¶

**URL:** https://www.odoo.com/documentation/19.0/applications/general/companies/email_template.html

**Contents:**
- Email templates¶
- Editing email templates¶
  - Powerbox¶
  - XML/HTML code editor¶
  - Dynamic placeholders¶
  - Rich text editor¶
  - Resetting email templates¶
  - Default reply on email templates¶
- Transactional emails and corresponding URLs¶
  - Updating translations within email templates¶

Email templates are saved emails that are used repeatedly to send emails from the database. They allow users to send quality communications, without having to compose the same text repeatedly.

Creating different templates that are tailored to specific situations lets users choose the right message for the right audience. This increases the quality of the message and the overall engagement rate with the customer.

Email templates in Odoo use QWeb or XML, which allows for editing emails in their final rendering, making customizations more robust, without having to edit any code whatsoever. This means that Odoo can use a Graphical User Interface (GUI) to edit emails, which edits the backend code. When the received email is read by the end user’s program, different formatting and graphics will appear in the final form of it.

Access email templates in developer mode by navigating to Settings app ‣ Technical menu ‣ Email ‣ Email Templates.

The powerbox feature can be used when working with email templates. This feature provides the ability to directly edit the formatting and text in an email template, as well as the ability to add links, buttons, appointment options, or images.

Additionally, the XML/HTML code of the email template can be edited directly, via the </> icon. Dynamic placeholders (referencing fields within Odoo) are also available for use in the email template.

The powerbox feature is an enriched text editor with various formatting, layout, and text options. It can also be used to add XML/HTML features in an email template. The powerbox feature is activated by typing a forward slash / in the body of the email template.

When a forward slash / is typed in the body of an email template, a drop-down menu appears with the following options:

Bulleted list: Create a simple bulleted list.

Numbered list: Create a list with numbering.

Checklist: Track tasks with a checklist.

Table: Insert a table.

Separator: Insert a horizontal rule separator.

Quote: Add a blockquote section.

Code: Add a code section.

2 columns: Convert into two columns.

3 columns: Convert into three columns.

4 columns: Convert into four columns.

Heading 1: Big section heading.

Heading 2: Medium section heading.

Heading 3: Small section heading.

Switch direction: Switch the text’s direction.

Text: Paragraph block.

Image: Insert an image.

Article: Link an article.

Button: Add a button.

Appointment: Add a specific appointment.

Calendar: Schedule an appointment.

3 Stars: Insert a rating over three stars.

5 Stars: Insert a rating over five stars.

Signature: Insert your signature.

Dynamic Placeholders: Insert personalized content.

To use any of these options, click on the desired feature from the powerbox drop-down menu. To format existing text with a text-related option (e.g. Heading 1, Switch direction, etc.), highlight the text, then type in the activator key (forward slash) /, and select the desired option from the drop-down menu.

Using dynamic placeholders

To access the XML/HTML editor for an email template, first enter developer mode. Then, click the </> icon in the upper-right corner of the template, and proceed to edit the XML/HTML. To return to the standard text editor, click the </> icon again.

The XML/HTML editor should be accessed with caution as this is the backend code of the template. Editing the code can cause the email template to break immediately or when upgrading the database.

Dynamic placeholders reference certain fields within the Odoo database to produce unique data in the email template.

Many companies like to customize their emails with a personalized piece of customer information to grab attention. This can be accomplished in Odoo by referencing a field within a model by inserting a dynamic placeholder. For example, a customer’s name can be referenced in the email from the Customer field on the Sales Order model. The dynamic placeholder for this field is: {{ object.partner_id }}.

Dynamic placeholders are encoded to display fields from within the database. Dynamic placeholders can be used in the Body (Content Tab) of the email template. They can also be used in the fields present in the Email Configuration tab, the Subject of the email, and the Language.

To use the dynamic placeholders in the Body of an email open the powerbox feature by typing in / into the body of the email template under the Content tab. Scroll to the bottom of the options list, to Marketing Tools. Next, select Dynamic Placeholder. Then select the dynamic placeholder from a list of available options and follow the prompts to configure it with the desired corresponding Odoo field. Each dynamic placeholder will vary in configuration.

Each unique combination of Fields, Sub-models and Sub-fields creates a different dynamic placeholder. Imagine it as a combination to the field that is being created.

To search the available fields, simply type in the front-end name (on user-interface) of the field in the search. This will find a result from all of the available fields for the model that the email template is created for.

Customizing email templates are out of the scope of Odoo Support.

A rich text editor toolbar can be accessed by highlighting text in the email template. This can be used to change the heading, font size/style, color, add a list type, or a link.

Should the email template not work because the code has been altered it can be reset to restore it back to the out-of-box default template. Simply click on the Reset Template button in the upper left-hand of the screen and the template will be reset.

Under the Email Configuration tab on an email template, there is a Reply To field. In this field, add email addresses to which replies are redirected when sending emails en masse using this template.

Add multiple email addresses by adding a comma , between the addresses or dynamic placeholders.

The Reply To field is only used for mass mailing (sending emails in bulk). Bulk emails can be sent in almost every Odoo application that has a list view option.

To send mass mails, while in list view, check the boxes next to the desired records where the emails are to be sent, click the Action button (represented by a ⚙️ (gear) icon), and select the desired email option from the Action drop-down menu. Email options can vary by the particular list view and application.

If it is possible to send an email, a mail composer pop-up window appears, with values that can be defined and customized. This option will be available on the Action button on pages where emails can be sent in bulk—for example, on the Customers page of the CRM app. This action occurs throughout the Odoo database.

In Odoo, multiple events can trigger the sending of automated emails. These emails are known as transactional emails, and sometimes contain links redirecting to the Odoo database.

By default, links generated by the database use the dynamic web.base.url key defined in the system parameters. For more information about this, see system parameters.

If the Website application is not installed, the web.base.url key will always be the default parameter used to generate all the links.

The web.base.url key can only have a single value, meaning that, in a multi-website or multi-company database environment, even if there is a specific domain name for each website, the links generated to share a document (or the links within a transactional email) may remain the same, regardless of which website/company is related to the sending of the email/document.

If the Value of the web.base.url system parameter is equal to https://www.mycompany.com and there are two separate companies in Odoo with different website URLs: https://www.mycompany2.com and https://www.mycompany1.com, the links created by Odoo to share a document, or send a transactional email, come from the domain: https://www.mycompany.com, regardless of which company sends the document or email.

This is not always the case, as some Odoo applications (eCommerce, for example) have a link established in the database with the Website application. In that case, if a specific domain is defined for the website, the URL generated in the email template uses the domain defined on the corresponding website of the company.

When a customer makes a purchase on an Odoo eCommerce website, the order has an established link with that website. As a result, the links in the confirmation email sent to the customer use the domain name for that specific website.

A document shared using the Documents application will always use the web.base.url key, as the document shared is not associated with any particular website. This means that the URL will always be the same (the web.base.url key value), no matter what company it’s shared from. This is a known limitation.

For more information about how to configure domains, check out the domain name documentation.

In Odoo, email templates are automatically translated for all users in the database for all of the languages installed. Changing the translations shouldn’t be necessary. However, if for a specific reason, some of the translations need to be changed, it can be done.

Like any modification in the code, if translation changes are not done correctly (for example, modifications leading to bad syntax), it can break the template, and as a result, the template will appear blank.

In order to edit translations, first enter developer mode. Then, on the email template, click on the Edit button, and then click on the language button, represented by the initials of the language currently being used (e.g. EN for English).

If there aren’t multiple languages installed and activated in the database, or if the user does not have administration access rights, the language button will not appear.

A pop-up window with the different languages installed on the database appears. From this pop-up, editing of translations is possible. When the desired changes have been made, click the Save button to save the changes.

When editing the translations, the default language set in the database appears in bold.

---

## Project templates¶

**URL:** https://www.odoo.com/documentation/19.0/applications/services/project/project_management/project_templates.html

**Contents:**
- Project templates¶
- Creating templates¶
  - Project roles in templates¶
  - Task scheduling in templates¶
- Using templates¶

Templates allow you to create new projects with predefined settings, reducing the need to manually set up similar projects repeatedly.

To create a project template, an existing project is required and used as a base to be converted into a template. Converting a project into a template transfers the entire project’s properties to the template. This includes the project’s stages, tasks, sub-tasks, and their respective configurations, such as planned dates, statuses, assignees, and more.

First, access the settings of the project that you want to convert into a template by going to Projects, hovering your mouse over the project’s card, clicking the (vertical ellipsis) icon, and selecting Settings. Review and adjust the project’s properties to ensure it reflects your desired template setup.

Once your project is ready, click the (cog) icon and select Convert to Template. The Template banner indicates that the project has been successfully converted into a template.

Converting a project into a template will also archive the original project used to create the template. To keep using a project that you want to convert into a template, duplicate it first by hovering your mouse over the project’s card, clicking the (vertical ellipsis) icon, and selecting Duplicate.

To edit or delete a template, go to Projects ‣ New. Next to the name of the template, click the (pencil) icon to edit it or the (trash) icon to delete it. Editing or deleting a template does not affect the projects that were previously created from it.

Templates enable you to pre-select specific roles for tasks within your template, making the selection of assignees faster during the creation of a new project using a template.

Go to Projects ‣ New, and click the (pencil) icon next to the name of the template you want to edit. Then click on the Tasks smart button, and on one of the tasks. In the Project Roles field, type or select the roles that you want to perform this task, then click Save.

Create a project based on this template: go to Projects ‣ New, and click on the name of the template. The Create a project from template form then includes Project Roles. For each of them, you can select assignees by clicking on the Assignees field. This automatically dispatches the right tasks to the right employees.

In a project template, task scheduling can be automated according to the planned dates specified within the template.

Project and task planned dates are not saved when converting a project into a template. These require to be added to the template after it is created.

On the project template, define the Planned dates for both the project and each task. When tasks have planned start dates in the template, Odoo calculates the number of working days between the project’s start date and the first scheduled task. This time window is referred to as the delta.

When a new project is generated from this template:

The system uses the project’s start date as a reference.

Each task’s start date is automatically planned according to its delta.

If no start date is set on the new project, the current date is used as the default start date.

Task end dates are then determined automatically by Odoo’s scheduling algorithm.

To ensure that all project roles and tasks are planned without conflict according to the team’s availability and workload, the scheduling algorithm calculates the end date of each task based on the allocated time, while also considering task dependencies and assignee’s availability, working schedule, time off, and public holidays.

To create a new project from a template, go to Projects ‣ New, and click on a template in the Project Templates section. Enter a name for your project. Optionally, add a Customer, a Planned Date, and set up the task creation email. Then, click Create Project.

Templates can also be linked to specific products. To do so, the project template must be set as Billable:

Go to Projects ‣ New and click the (pencil) icon next to the name of the template you want to edit.

Click the Settings tab, tick the Billable checkbox, and click Save.

Once this is done, configure the product by:

Selecting Service as the Product Type.

Selecting Project or Project & Task in the Create on Order field.

Selecting a Project Template and clicking Save.

---

## Schedule interviews¶

**URL:** https://www.odoo.com/documentation/19.0/applications/hr/recruitment/schedule_interviews.html

**Contents:**
- Schedule interviews¶
- Recruitment team scheduled interviews¶
  - New event pop-up window¶
    - More options¶
  - Send meeting to attendees¶
- Applicant scheduled interviews¶
  - Modify stage¶
  - Send email¶
  - Self-scheduled interview¶

An in-person, virtual, or phone interview can be scheduled in one of two ways through the Recruitment app, either by the recruitment team, or by the applicant.

With one drag-and-drop, Odoo emails the candidate with a self-service link, the candidate books the time slot, and sends the meeting to everyone’s calendar. No more back-and-forth emails or calls.

When an applicant reaches the interview stage, the recruitment team should schedule the interview, by first coordinating a suitable date and time with the applicant and interviewers.

To schedule the interview, navigate to the applicant’s card, by first going to the Recruitment app, and clicking the relevant job card. This opens the Applications page for that job position. Then, click the desired applicant’s card to view their detailed applicant form.

To schedule an phone, virtual, or in-person interview, click the No Meeting smart button at the top of the applicant’s record.

The Meetings smart button displays No Meeting if no meetings are currently scheduled. For applicants who are new to the First Interview stage, this is the default.

If there is one meeting already scheduled, the smart button displays 1 Meeting, with the date of the upcoming meeting beneath it. If more than one meeting is scheduled, the button displays Next Meeting, with the date of the first upcoming meeting beneath it.

Clicking the Meetings smart button loads a calendar, showing the scheduled meetings and events for the currently signed-in user, as well as the employees who are listed under the Attendees section, located to the right of the calendar.

To change the currently loaded meetings and events being displayed, uncheck an attendee whose calendar events are to be hidden. Only the checked attendees are visible on the calendar.

To add a meeting to the calendar when in the Day or Week view, click on the start time of the meeting and drag down to the end time. Doing so selects the date, time, and the length of the meeting.

A meeting can also be added in this view by clicking on the desired day and time slot.

Both methods cause a New Event pop-up window to appear.

Clicking a grid, corresponding with the time and date, opens the New Event pop-up window to schedule a meeting.

Enter the information on the form. The only required fields to enter are a title for the meeting, along with the Start (and end date/time) fields.

Once the card details are entered, click Save & Close to save the changes and create the interview.

After entering in a required name for the meeting, the fields available to modify on the New Event card are as follows:

Meeting Title: Enter the subject for the meeting. This should clearly indicate the purpose of the meeting. The default subject is the Candidate name entered on the applicant’s card.

Start: Configure the start and end date and times for the meeting. Clicking either of these fields opens a calendar pop-up window. Click on the desired date to select it, and then enter the time in the corresponding field. Click Apply to close the window.

All Day: Tick the box to schedule an all-day interview. If this box is ticked, the Start field changes to Start Date.

Attendees: Select the people who should attend the meeting. The default attendees are the prospective candidate, and the assigned recruiter for the job position. Add as many other people as desired.

Videocall URL: If the meeting is virtual, or if there is a virtual option available, click Odoo meeting, and a URL is automatically created for the meeting, which populates the field.

Description: Enter a brief description in this field. There is an option to enter formatted text, such as numbered lists, headings, tables, links, photos, and more. Use the powerbox feature, by typing a / to reveal a list of options.

Scroll through the options and click on the desired item. The item appears in the field, and can be modified. Each command presents a different pop-up window. Follow the instructions for each command to complete the entry.

To add additional information to the meeting, click the More Options button in the lower-right corner of the New Event pop-up window. Enter any of the following additional fields:

Duration: this field auto populates based on the Start (and end) date and time. If the meeting time is adjusted, this field automatically adjusts to the correct duration length. The default length of a meeting is one hour.

Recurrent: if the meeting should repeat at a selected interval (not typical for a first interview), tick the checkbox next to Recurrent. Several additional fields appear when this is enabled:

Timezone: using the drop-down menu, select the Timezone for the recurrent meetings.

Repeat: choose Daily, Weekly, Monthly, Yearly, or Custom recurring meetings. If Custom is selected, a Repeat Every field appears beneath it, along with another time frequency parameter (Days, Weeks, Months, or Years). Enter a number in the blank field, then select the time period using the drop-down menu.

Repeat on: enabled when the Weekly option is selected in the Repeat field. Choose the day the weekly meeting falls on.

Day of Month: configure the two drop-down menu options to select a specific day of the month, irrespective of the date (e.g. the first Tuesday of every month). To set a specific calendar date, choose Date of Month and enter the calendar date in the field (e.g. 15 to set the meeting to occur on the fifteenth of every month).

Until: using the drop-down menu, select when the meetings stop repeating. The available options are Number of repetitions, End date, and Forever. If Number of repetitions is selected, enter the number of total meetings to occur in the blank field to the right. If End date is selected, specify the date using the calendar pop-up window, or type in a date in a MM/DD/YYYY format. Forever schedules meetings indefinitely.

Location: enter the location for the meeting.

Tags: select any tags for the meeting using the drop-down menu, or add a new tag by typing in the tag and clicking Create “tag”. There is no limit to the number of tags that can be used.

Privacy: select if the organizer appears either Available or Busy for the duration of the meeting. Next, select the visibility of this meeting, using the drop-down menu to the right of the first selection. Options are Public, Private, and Only internal users. Public allows for everyone to see the meeting, Private allows only the attendees listed on the meeting to see the meeting, and Only internal users allows anyone logged into the company database to see the meeting.

Organizer: the employee who created the meeting is populated in this field. Use the drop-down menu to change the selected employee.

Reminders: select a reminder from the drop-down menu. Default options include Notification, Email, and SMS Text Message, each with a specific time period before the event (hours, days, etc). The chosen reminder chosen alerts the meeting participants of the meeting, via the selected option at the specified time. Multiple reminders can be selected in this field.

Once changes have been entered on the New Event pop-up window, and the meeting details are correct, the meeting can be sent to the attendees, via email or text message, from the expanded event form (what is seen when the More Options button is clicked on in the New Event pop-up window).

To send the meeting via email, click the EMAIL button next to the Attendees field on the expanded meeting form.

A Contact Attendees email configurator pop-up window appears. A pre-formatted email, using the default Calendar: Event Update email template, populates the email body field.

The applicant, followers of the job application, as well as the user who created the meeting, are added to the To by default. Make any desired changes to the email.

To send the meeting via text message, click the SMS button next to the Attendees field on the expanded meeting form. A Send SMS pop-up window appears.

At the top, a blue banner appears if any attendees do not have valid mobile numbers, and lists how many records are invalid. If a contact does not have a valid mobile number listed, click Close, and edit the attendee’s record, then redo these steps.

When no warning message appears, type in the message to be sent to the attendees in the Message field. To add any emojis to the message, click the (smile add) icon on the right-side of the pop-up window.

The number of characters, and amount of text messages required to send the message (according to GSM7 criteria) appears beneath the Message field. Click Put in queue to have the text sent later, after any other messages are scheduled, or click Send Now to send the message immediately.

Sending text messages is not a default capability with Odoo. To send text messages, credits are required, which need to be purchased. For more information on IAP credits and plans, refer to the In-app purchases (IAP) documentation.

Coordinating interview times typically requires several email exchanges and can slow the recruitment process. Enabling Odoo’s self-service scheduling removes that bottleneck: when an applicant is moved to an interview stage, the system automatically sends a scheduling link, records the selected slot, and updates all relevant calendars.

This automation is turned off by default. To activate it, assign the Recruitment: Schedule Interview email template to either the First Interview or Second Interview stage (see Modify stage).

Modify either the First Interview or Second Interview stage so the stage’s Email Template field is set to Recruitment: Schedule interview.

After configuring the First Interview or Second Interview stages to send emails, drag-and-drop the applicant card into one of these stages to send the email.

When the applicant received the email, they click the Schedule my interview button at the bottom of the email. This navigates the applicant to a private online scheduling page, which is only accessible through the emailed link.

This page displays the MEETING DETAILS on the right side of the screen. This includes the format and length of the meeting. In this example. the interview is virtual ( Online) and the duration is a half hour ( 30 minutes).

Then the applicant clicks on an available day on the calendar, signified by purple text. Once a day is selected, they click on one of the available times to select that date and time.

Be sure to check the Timezone field, beneath the calendar, to ensure it is set to the correct time zone. Changing the time zone may alter the available times presented.

Once the date and time are selected, the applicant is navigated to an Add more details about you page. This page asks the applicant to enter their Full name, Email, and Phone number. The contact information entered on this form is how the applicant is contacted to remind them about the scheduled interview.

When everything is entered on the Add more details about you page, the applicant clicks the Confirm Appointment button, and the interview is scheduled.

After confirming the interview, the applicant is taken to a confirmation page, where all the details of the interview are displayed. The option to add the meeting to the applicant’s personal calendars is available, through the Add to iCal/Outlook and Add to Google Agenda buttons, beneath the interview details.

The applicant is also able to cancel or reschedule the interview, if necessary, with the Cancel your appointment link at the bottom of the confirmation.

---

## QWeb Templates¶

**URL:** https://www.odoo.com/documentation/19.0/developer/reference/frontend/qweb.html

**Contents:**
- QWeb Templates¶
- Data output¶
- Conditionals¶
- Loops¶
- attributes¶
- setting variables¶
- calling sub-templates¶
- Advanced Output¶
  - Python¶
  - forcing double-escaping¶

QWeb is the primary templating engine used by Odoo2. It is an XML templating engine1 and used mostly to generate HTML fragments and pages.

Template directives are specified as XML attributes prefixed with t-, for instance t-if for Conditionals, with elements and other attributes being rendered directly.

To avoid element rendering, a placeholder element <t> is also available, which executes its directive but doesn’t generate any output in and of itself:

if condition is true, but:

QWeb’s output directive out will automatically HTML-escape its input, limiting XSS risks when displaying user-provided content.

out takes an expression, evaluates it and injects the result in the document:

rendered with the value value set to 42 yields:

See Advanced Output for more advanced topics (e.g. injecting raw HTML, etc…).

QWeb has a conditional directive if, which evaluates an expression given as attribute value:

The element is rendered if the condition is true:

but if the condition is false it is removed from the result:

The conditional rendering applies to the bearer of the directive, which does not have to be <t>:

will give the same results as the previous example.

Extra conditional branching directives t-elif and t-else are also available:

QWeb has an iteration directive foreach which take an expression returning the collection to iterate on, and a second parameter t-as providing the name to use for the “current item” of the iteration:

Like conditions, foreach applies to the element bearing the directive’s attribute, and

is equivalent to the previous example.

foreach can iterate on an array (the current item will be the current value) or a mapping (the current item will be the current key). Iterating on an integer (equivalent to iterating on an array between 0 inclusive and the provided integer exclusive) is still supported but deprecated.

In addition to the name passed via t-as, foreach provides a few other variables for various data points:

$as will be replaced by the name passed to t-as

the object being iterated over

This variable is only available on JavaScript QWeb, not Python.

the current iteration value, identical to $as for lists and integers, but for mappings it provides the value (where $as provides the key)

the current iteration index (the first item of the iteration has index 0)

the size of the collection if it is available

whether the current item is the first of the iteration (equivalent to {$as}_index == 0)

whether the current item is the last of the iteration (equivalent to {$as}_index + 1 == {$as}_size), requires the iteratee’s size be available

either "even" or "odd", the parity of the current iteration round

a boolean flag indicating that the current iteration round is on an even index

a boolean flag indicating that the current iteration round is on an odd index

These extra variables provided and all new variables created into the foreach are only available in the scope of the foreach. If the variable exists outside the context of the foreach, the value is copied at the end of the foreach into the global context.

QWeb can compute attributes on-the-fly and set the result of the computation on the output node. This is done via the t-att (attribute) directive which exists in 3 different forms:

an attribute called $name is created, the attribute value is evaluated and the result is set as the attribute’s value:

same as previous, but the parameter is a format string instead of just an expression, often useful to mix literal and non-literal string (e.g. classes):

There are two equivalent syntaxes for format strings: "plain_text {{code}}" (aka jinja-style) and "plain_text #{code}" (aka ruby-style).

if the parameter is a mapping, each (key, value) pair generates a new attribute and its value:

if the parameter is a pair (tuple or array of 2 element), the first item of the pair is the name of the attribute and the second item is the value:

QWeb allows creating variables from within the template, to memoize a computation (to use it multiple times), give a piece of data a clearer name, …

This is done via the set directive, which takes the name of the variable to create. The value to set can be provided in two ways:

a t-value attribute containing an expression, and the result of its evaluation will be set:

if there is no t-value attribute, the node’s body is rendered and set as the variable’s value:

QWeb templates can be used for top-level rendering, but they can also be used from within another template (to avoid duplication or give names to parts of templates) using the t-call directive:

This calls the named template with the execution context of the parent, if other_template is defined as:

the call above will be rendered as <p/> (no content), but:

will be rendered as <p>1</p>.

However, this has the problem of being visible from outside the t-call. Alternatively, content set in the body of the call directive will be evaluated before calling the sub-template, and can alter a local context:

The body of the call directive can be arbitrarily complex (not just set directives), and its rendered form will be available within the called template as a magical 0 variable:

By default, out should HTML-escape content which needs to be escaped, protecting the system against XSS

Content which does not need to be escaped will instead be injected as-is in the document, and may become part of the document’s actual markup.

The only cross-platform “safe” content is the output of t-call or a t-set used with a “body” (as opposed to t-value or t-valuef).

Usually you should not have to care too much: APIs for which it makes sense should generate “safe” content automatically, and things should work transparently.

For the cases where things need to be clearer though the following APIs output safe content which will by default not be (re-)escaped when injected into templates:

html_escape() and markupsafe.escape() (they are aliases, and have no risk of double-escaping).

markupsafe.Markup is an unsafe API, it’s an assertion that you want the content to be markup-safe but necessarily can not check that, it should be used with care.

to_text() does not mark the content as safe, but will not strip that information from safe content.

If content is marked as safe but for some reason needs to be escaped anyway (e.g. printing the markup of an HTML fields), it can just be converted back to a normal string to “strip” the safety flag e.g. str(content) in Python and String(content) in Javascript.

Because Markup is a much richer type than Markup(), some operations will strip the safety information from a Markup() but not a Markup e.g. string concatenation ('' + content) in Python will result in a Markup with the other operand having been properly escaped, while in Javascript will yield a String() where the other operand was not escaped before the concatenation.

An alias for out, would originally HTML-escape its input. Not yet formally deprecated as the only difference between out and esc is that the latter is a bit unclear / incorrect.

A version of out which never escapes its content. Content is emitted as-is, whether it’s safe or not.

Use out with a markupsafe.Markup value instead.

t-raw was deprecated because as the code producting the content evolves it can be hard to track that it’s going to be used for markup, leading to more complicated reviews and more dangerous lapses.

The t-field directive can only be used when performing field access (a.b) on a “smart” record (result of the browse method). It is able to automatically format based on field type, and is integrated in the website’s rich text editing.

t-options can be used to customize fields, the most common option is widget, other options are field- or widget-dependent.

with an empty value, invokes the breakpoint() builtin function, which usually invokes a debugger (pdb by default).

The behaviour can be configured via PYTHONBREAKPOINT or sys.breakpointhook().

Most Python-side uses of QWeb are in controllers (and during HTTP requests), in which case templates stored in the database (as views) can be trivially rendered by calling odoo.http.HttpRequest.render():

This automatically creates a Response object which can be returned from the controller (or further customized to suit).

At a deeper level than the previous helper is the _render method on ir.qweb (use the datable) and the public module method render (don’t use the database):

Renders a QWeb view/template by database id or external id. Templates are automatically loaded from ir.qweb records.

_prepare_environment method sets up a number of default values in the rendering context. The http_routing and website addons, also default values they need. You can use minimal_qcontext=False option to avoid this default value like the public method render:

the current Request object, if any

whether the current request (if any) is in debug mode

url-encoding utility function

the corresponding standard library module

the corresponding standard library module

the corresponding standard library module

the keep_query helper function

values – context values to pass to QWeb for rendering

engine (str) – name of the Odoo model to use for rendering, can be used to expand or customize QWeb locally (by creating a “new” qweb based on ir.qweb with alterations)

returns etree object, ref

The t-name directive can only be placed at the top-level of a template file (direct children to the document root):

It takes no other parameter, but can be used with a <t> element or any other. With a <t> element, the <t> should have a single child.

The template name is an arbitrary string, although when multiple templates are related (e.g. called sub-templates) it is customary to use dot-separated names to indicate hierarchical relationships.

Alter existing templates in-place, e.g. to add information to templates

Create a new template from a given parent template

t-inherit which is the name of the template to inherit from,

t-inherit-mode which is the behaviour of the inheritance: it can either be set to primary to create a new child template from the parented one or to extension to alter the parent template in place.

An optional t-name directive can also be specified. It will be the name of the newly created template if used in primary mode, else it will be added as a comment on the transformed template to help retrace inheritances.

For the inheritance itself, the changes are done using xpaths directives. See the XPATH documentation for the complete set of available instructions.

Primary inheritance (child template):

Extension inheritance (in-place transformation):

Template inheritance is performed via the t-extend directive which takes the name of the template to alter as parameter.

The directive t-extend will act as a primary inheritance when combined with t-name and as an extension one when used alone.

In both cases the alteration is then performed with any number of t-jquery sub-directives:

The t-jquery directives takes a CSS selector. This selector is used on the extended template to select context nodes to which the specified t-operation is applied:

the node’s body is appended at the end of the context node (after the context node’s last child)

the node’s body is prepended to the context node (inserted before the context node’s first child)

the node’s body is inserted right before the context node

the node’s body is inserted right after the context node

the node’s body replaces the context node’s children

the node’s body is used to replace the context node itself

the nodes’s body should be any number of attribute elements, each with a name attribute and some textual content, the named attribute of the context node will be set to the specified value (either replaced if it already existed or added if not)

if no t-operation is specified, the template body is interpreted as javascript code and executed with the context node as this

while much more powerful than other operations, this mode is also much harder to debug and maintain, it is recommended to avoid it

The javascript QWeb implementation provides a few debugging hooks:

takes an expression parameter, evaluates the expression during rendering and logs its result with console.log:

will print 42 to the console

triggers a debugger breakpoint during template rendering:

will stop execution if debugging is active (exact condition depend on the browser and its development tools)

the node’s body is javascript code executed during template rendering. Takes a context parameter, which is the name under which the rendering context will be available in the t-js’s body:

(core is the web.core module) An instance of QWeb2.Engine() with all module-defined template files loaded, and references to standard helper objects _ (underscore), _t (translation function) and JSON.

core.qweb.render can be used to easily render basic module templates

The QWeb “renderer”, handles most of QWeb’s logic (loading, parsing, compiling and rendering templates).

Odoo Web instantiates one for the user in the core module, and exports it to core.qweb. It also loads all the template files of the various modules into that QWeb instance.

A QWeb2.Engine() also serves as a “template namespace”.

Renders a previously loaded template to a String, using context (if provided) to find the variables accessed during template rendering (e.g. strings to display).

template (String()) – the name of the template to render

context (Object()) – the basic namespace to use for template rendering

The engine exposes an other method which may be useful in some cases (e.g. if you need a separate template namespace with, in Odoo Web, Kanban views get their own QWeb2.Engine() instance so their templates don’t collide with more general “module” templates):

Loads a template file (a collection of templates) in the QWeb instance. The templates can be specified as:

QWeb will attempt to parse it to an XML document then load it.

QWeb will attempt to download the URL content, then load the resulting XML string.

QWeb will traverse the first level of the document (the child nodes of the provided root) and load any named template or template override.

A QWeb2.Engine() also exposes various attributes for behavior customization:

Prefix used to recognize directives during parsing. A string. By default, t.

Boolean flag putting the engine in “debug mode”. Normally, QWeb intercepts any error raised during template execution. In debug mode, it leaves all exceptions go through without intercepting them.

The jQuery instance used during template inheritance processing. Defaults to window.jQuery.

A Function. If present, called before compiling each DOM node to template code. In Odoo Web, this is used to automatically translate text content and some attributes in templates. Defaults to null.

it is similar in that to Genshi, although it does not use (and has no support for) XML namespaces

although it uses a few others, either for historical reasons or because they remain better fits for the use case. Odoo 9.0 still depends on Jinja and Mako.

**Examples:**

Example 1 (jsx):
```jsx
<t t-if="condition">
    <p>Test</p>
</t>
```

Example 2 (typescript):
```typescript
<p>Test</p>
```

Example 3 (jsx):
```jsx
<div t-if="condition">
    <p>Test</p>
</div>
```

Example 4 (typescript):
```typescript
<div>
    <p>Test</p>
</div>
```

---

## Double Opt-in¶

**URL:** https://www.odoo.com/documentation/19.0/applications/marketing/marketing_automation/campaign_templates/double_optin.html

**Contents:**
- Double Opt-in¶
- Use the Double Opt-in campaign template¶
  - Campaign configuration¶
- Double Opt-in use-case¶

A double opt-in, also referred to as a confirmed opt-in, may be required in some countries for marketing communications, due to anti-SPAM laws. Confirming consent has several other benefits, as well: it validates email addresses, avoids spam/robo subscribers, keeps mailing lists clean, and only includes engaged contacts in the mailing list.

When the Double Opt-in campaign template is used, a new mailing list titled, Confirmed contacts is created in the Email Marketing app, and any new mailing list contacts that are added to the default Newsletter mailing list are sent a confirmation email to double opt-in. The contacts that click on the confirmation link in the email are automatically added to the Confirmed contacts mailing list in Odoo.

When using the Double Opt-in campaign template, only the contacts in the Confirmed contacts mailing list are considered to have confirmed their consent.

Open the Marketing Automation app, and select the Double Opt-in campaign template to create a new campaign for confirming consent.

The campaign templates do not display, by default, when there are existing Marketing Automation campaigns. To display the campaign templates, type the name of a campaign (that does not exist in the database) into the Search… bar, then press Enter.

For example, searching for empty displays the campaign template cards again, as long as there is not a campaign with the name “empty” in the database.

Upon creation of the campaign, the campaign form loads with a new preconfigured campaign.

The Target and Filter configurations of the campaign are as follows:

Responsible*: The user who created the campaign.

Target: Mailing Contact

Unicity based on: Email (Mailing Contact)

Mailing lists contains Newsletter

* The Responsible field is only visible with Developer mode (debug mode) activated.

The Target model of the campaign should not be modified. Changing the Target model with activities in the Workflow invalidates the existing activities in the Workflow.

The Double Opt-in campaign template is intended to only use the Mailing Contact model.

The campaign loads two activities in the Workflow section of the campaign: an email activity, with a child server action activity that triggers on click.

By default, the Confirmation email activity is set to trigger 1 Hours after the beginning of the workflow. In other words, the email is sent 1 hour after a new contact is added to the Newsletter mailing list.

The email activity uses the preconfigured Confirmation email template, which contains a button for the contact to click to confirm their consent.

To modify the email template, select the Templates smart button at the top of the campaign form. Then, in the list of templates, select the Confirmation email template.

Be sure to personalize the contents of the email template; however, it is recommended to keep the contents of double opt-in confirmation emails short and to-the-point.

The default confirmation button, in the body of the template, links directly to the database’s website homepage. Click on the button to edit the button text and URL.

To provide a streamlined experience for the contact, consider creating a page on the website that expresses gratitude to the contact for confirming their subscription to the mailing list. Add the link to that page in the URL of the confirmation button.

The email template should only include a single call-to-action link for confirmation, other than an unsubscribe link.

Any click on a link (or button) included in the confirmation email, besides the unsubscribe button, triggers the Add to list server action.

The child activity Add to list server action’s On click trigger cannot differentiate between multiple URLs in an email, besides the /unsubscribe_from_list unsubscribe button that is included in any one of the footer blocks.

The Add to list server action activity triggers immediately after a click in the parent Confirmation email activity is detected.

When triggered, the Add to list activity executes the Add To Confirmed List server action, automatically adding the contact to the Confirmed contacts mailing list, if they are not already in the mailing list.

To modify the server action, select the title of the activity to open the Open: Activities pop-up window and edit the server action activities configuration.

Consider setting an Expiry Duration to prevent executing the activity after a specific amount of time.

It is not recommended to modify the preconfigured Python code in the Add To Confirmed List server action, as doing so may trigger a change in the database’s pricing plan.

Once the campaign configuration is complete, consider launching a test to verify the campaign executes as expected. If the campaign testing is successful, Start the campaign to begin sending double opt-in confirmation emails to Newsletter mailing list contacts, and fill the Confirmed contacts mailing list with engaged contacts.

To prepare for sending newsletter marketing emails on an Odoo database, a mailing contact list must be procured. One way of collecting subscribers is through a sign-up form on the website that adds contacts to the Newsletter mailing list on the form submission.

Before sending any marketing emails, use the Double Opt-in campaign template in the Marketing Automation app to confirm marketing email consent from the contacts in the Newsletter mailing list.

After launching the Double Opt-in campaign, view the contacts that have double opt-in in the Confirmed contacts mailing list (Email Marketing app ‣ Mailing Lists ‣ Mailing Lists).

Now, the Confirmed contacts mailing list is ready to be used for sending newsletter marketing emails from an Odoo database.

---

## SMS analysis¶

**URL:** https://www.odoo.com/documentation/19.0/applications/marketing/sms_marketing/sms_analysis.html

**Contents:**
- SMS analysis¶

On the Reporting page (accessible via the Reporting option in the header menu), there are options to apply different combinations of Filters and Measures to view metrics in a number of different layouts (e.g. Graph, List, and Cohort views.)

Each Reporting metric view option allows for more extensive performance analysis of SMS mailings.

For example, while in the default Graph view, SMS data is visualized as different graphs and charts, which can be sorted and grouped in various ways (e.g. Measures drop down menu).

SMS messages can be sent using automation rules in Odoo. Odoo Studio is required to use automation rules.

To install Odoo Studio, go to the Apps application. Then, using the Search… bar, search for studio.

If it is not already installed, click Install.

Adding the Studio application upgrades the subscription status to Custom, which increases the cost. Consult support, or reach out to the database’s customer success manager, with any questions on making the change.

To use automation rules, navigate in developer mode, to Settings app ‣ Technical menu ‣ Automation section ‣ Automation Rules. Then, click New to create a new rule.

Enter a name for the automation rule, and select a Model to implement this rule on.

Based on the selection for the Trigger, additional fields will populate below. Set the Trigger to one of the following options:

Other options may appear based on the Model selected. For example if the Calendar Event model is selected, then the following options appear in addition to those above:

Under the Before Update Domain field, set a condition to be met before updating the record. Click Edit Domain to set record parameters.

Under the Actions To Do tab, select Add an action. Next, in the resulting Create Actions pop-up window, select Send SMS, and set the Allowed Groups. Allowed Groups are the access rights groups that are allowed to execute this rule. Leave the field empty to allow all groups. See this documentation: Create and modify groups.

Next, set the SMS Template and choose whether the SMS message should be logged as a note, by making a selection in the drop-down menu: Send SMS as. Click Save and Close to save the changes to this new action.

Add any necessary notes under the Notes tab. Finally, navigate away from the completed automation rule, or manually save (by clicking the ☁️ (cloud) icon), to implement the change.

---

## Conduct appraisals¶

**URL:** https://www.odoo.com/documentation/19.0/applications/hr/appraisals/new_appraisals.html

**Contents:**
- Conduct appraisals¶
- Employee self-assessment¶
  - Employee skills¶
  - Complete the self-assessment¶
- Manager feedback¶
  - Ask for feedback¶
- Appraisal review¶
  - Schedule appraisal review¶
  - Review employee skills¶
- Complete an appraisal¶

This guide explains the end-to-end appraisal workflow in Odoo, from creation to final rating, showing how managers and employees collaborate at each stage.

Employee self-assessment: The employee completes the Employee’s Feedback template and updates their skills. Responses remain hidden until the employee sets the form to Visible to Manager.

Manager feedback: While the employee works on their section, the manager reviews goals, gathers peer input if needed, and fills out the Manager’s Feedback template. Feedback can remain hidden until the appraisal meeting.

Appraisal review: Manager and employee meet to discuss both feedback sections, validate skill changes, and agree on next steps. The meeting can be scheduled directly from the appraisal or the calendar.

Completion and rating: After the discussion, the manager assigns a final rating, adds any private notes, and marks the appraisal Done. The record then locks unless it is reopened for further edits.

Throughout the process, optional actions, such as requesting peer feedback or logging private manager notes, enhance the appraisal’s accuracy and relevance.

Once an appraisal is confirmed, the employee is required to fill out the self-assessment.

Only confirmed appraisals can be worked on. If an appraisal is not confirmed, the fields on the appraisal form cannot be edited, and feedback cannot be recorded.

After the employee receives a notification via email that an appraisal is confirmed and scheduled, they are requested to fill out their half of the default appraisal template, and update any skills.

Employee’s can click on the link in the confirmation email to navigate to the appraisal, or they can open their appraisal in the Appraisals app. To do this, open the Appraisals app, then click on the appraisal card.

The Employee’s Feedback half of the template includes the following sections: My work, My future, and My feelings. Each of these sections consists of several questions for the employee to answer, allowing the employee to perform a self-assessment, and provide feedback on how they feel about the company and their role.

After completing the Employee’s Feedback section in the Appraisal tab, the employee next updates their skills in the Skills tab.

Any skills that were present on the employee’s record when the appraisal was confirmed, appear in this tab. If a Skill Level has changed since the last appraisal, the level must be updated.

The Skills tab does not appear on the appraisal until the appraisal is confirmed.

Click on the Skill Level for the skill that has changed, revealing a drop-down of all available levels. Click on the new level for the skill. Once selected, the Progress field updates accordingly. Next, click into the Justification field for the skill, and enter any relevant details explaining the change. This field is not necessary, but may aid management when reviewing the employee’s skills.

The employee feedback remains hidden from management while the employee is performing their self-assessment. Once the employee has completed their half of the appraisal, and updated any skills, they tick the gray Not Visible to Manager toggle. This changes the toggle text to Visible to Manager, the color changes to green, and their responses are then visible to management.

Additionally, a green dot appears on the appraisal card on the Appraisals app dashboard, indicating the employee has completed their assessment, and marked their half of the appraisal as done.

While the employee is completing their Employee’s Feedback section, the manager fills out the Manager’s Feedback section.

Before the manager fills out their portion of the appraisal, managers typically review the employee’s goals and skills, and ask for additional feedback from the employee’s coworkers, to better understand all the achievements and challenges for the employee.

Once the manager has all the information they need to evaluate the employee, they fill out the Manager’s Feedback section of the appraisal form. The manager’s half has the following sections: Feedback, Evaluation, and Improvements.

The manager’s appraisal focuses on the employee’s accomplishments, as well as identifying areas of improvements, with actionable steps to help the employee achieve their goals in both the long and short term.

When the feedback section is completed, the manager can tick the Not Visible to Employee toggle. This changes the toggle text to Visible to Employee, the color changes to green, and their responses are then visible to the employee.

Some managers prefer to keep their feedback hidden from the employee until they meet with the employee to discuss the appraisal.

As part of the appraisal process, the manager can request feedback for an employee from anyone in the company about an employee. In Odoo, this is referred to as 360 Feedback.

Feedback is requested from coworkers and anyone else who works with the employee. This is to get a more well-rounded view of the employee, and aid in the manager’s overall assessment.

To request feedback, the appraisal must be confirmed. Once confirmed, an Ask Feedback button appears in the upper-left corner of the form.

Once both portions of an appraisal are completed (the employee and manager feedback sections), it is time for the employee and manager to meet and discuss the appraisal.

During the appraisal meeting, the manager reviews both the Employee’s Feedback section as well as their own Manager feedback.

Additionally, the employee’s skills and goals are reviewed at this time, and updated as needed.

A meeting can be scheduled in one of two ways: either from the Appraisals app dashboard, or from an individual appraisal card.

To schedule an appraisal from the dashboard of the Appraisals app, first navigate to Appraisals app ‣ Appraisals.

Click the activity icon beneath the appraisal date on the desired appraisal card, and an activity pop-up window appears. Click Schedule an activity, and set the Activity Type` to Meeting. For more information on scheduling activities, refer to the activities documentation.

Doing so opens a New Event pop-up form. From this pop-up form, make any desired modifications, such as designating a Start time.

The employee populates the Attendees section by default. Add anyone else who should be in the meeting, if necessary.

To make the meeting a video call, instead of an in-person meeting, click Odoo meeting, and a Videocall URL link appears in the field.

Once all the desired changes are complete, click Save & Close.

The meeting now appears on the calendar, and the invited parties are informed, via email.

The other way to schedule a meeting is from the individual appraisal form. To do this, navigate to the Appraisal app dashboard, then click on an appraisal card.

Next, click on the Meeting smart button, and the calendar loads. Follow the same directions above to create the meeting.

For more detailed information on how to schedule activities, refer to the activities documentation.

If no meetings are scheduled, the Meeting smart button reads No Meeting.

Part of an appraisal is evaluating an employee’s skills, and tracking their progress over time. The Skills tab of the appraisal form auto-populates with the skills from the employee form, once an appraisal is confirmed.

Each skill is grouped with like skills, and the Skill Level, Progress, and Justification are displayed for each skill.

Update any skills, or add any new skills to the Skills tab.

If a skill level has increased, enter a reason for the improved rating in the Justification field, such as took a fluency language test or received Javascript certification.

After an appraisal is completed, and the skills have been updated, the next time an appraisal is confirmed, the updated skills populate the Skills tab.

The Skills tab can be modified after the employee and their manager have met and discussed the employee’s appraisal.

This is a common situation as the manager may not have all the necessary information to assess and update the employee’s skills before meeting.

After the appraisal has been filled out by both the employee and the manager, and both parties have met and discussed the employee’s performance, the manager then logs any notes, and assigns a rating.

When completed, click the Mark as Done button in the upper-left corner of the appraisal form.

Once the appraisal is marked as Done, the status changes from Confirmed to Done, and the Mark as Done button changes to a Reopen button.

Modifications are not possible once the appraisal is marked as done.

To make any changes to an appraisal with a status of Done, click the Reopen button, then, click Confirm, and make any modifications needed. Once all modifications are complete, click the Mark as Done button again.

Managers can log notes on an appraisal that are only visible to other managers. Enter any notes in the Private Note tab. This can be done anytime during the appraisal process.

The employee being evaluated does not have access to this tab, and the tab does not appear on their appraisal.

The tab is optional and does not affect the final rating.

After both the manager and employee have met to discuss the employee’s performance, the appraisal must be given a Final Rating.

Using the drop-down menu, select the rating for the employee. The default options are: Needs improvement, Meets expectations, Exceeds expectations, Strongly Exceeds Expectations, and Good.

To add a new rating, navigate to Appraisals app ‣ Configuration ‣ Evaluation Scale. The default ratings are presented in a list view. To add a new rating, click the New button in the upper-left corner, and a blank line appears at the bottom of the list. Enter the new rating, then press the enter key, or click away from the line. Add as many new ratings as needed.

Click Appraisals in the top menu to return to the Appraisals dashboard, click on the appraisal, then select the desired Final Rating.

---

## Reporting¶

**URL:** https://www.odoo.com/documentation/19.0/applications/essentials/reporting.html

**Contents:**
- Reporting¶
- Selecting a view¶
  - Graph view¶
  - Pivot view¶
- Choosing measures¶
- Using the pivot view¶
- Using the graph view¶

You can find several reports under the Reporting menu of most apps that let you analyze and visualize the data of your records.

Depending on the report, Odoo can display the data in various ways. Sometimes, a unique view fully tailored to the report is available, while several views are available for others. However, two generic views are dedicated to reporting: the graph and pivot views.

The graph view is used to visualize your records’ data, helping you identify patterns and trends. The view is often found under the Reporting menu of apps but can be found elsewhere. Click the graph view button located at the top right to access it.

The pivot view is used to aggregate your records’ data and break it down for analysis. The view is often found under the Reporting menu of apps but can be found elsewhere. Click the pivot view button located at the top right to access it.

After selecting a view, you should ensure only the relevant records are filtered. Next, you should choose what is measured. By default, a measure is always selected. If you wish to edit it, click Measures and choose one or, only for pivots, multiple measures.

When you select a measure, Odoo aggregates the values recorded on that field for the filtered records. Only numerical fields (integer, decimal, monetary) can be measured. In addition, the Count option is used to count the total number of filtered records.

After choosing what you want to measure, you can define how the data should be grouped depending on the dimension you want to analyze. By default, the data is often grouped by Date > Month, which is used to analyze the evolution of a measure over the months.

When you filter a single time period, the option to compare it against another one appears.

Among other measures, you could add the Margin and Count measures to the Sales Analysis report. By default, the Untaxed Amount measure is selected.

You could group the measures by Product Category at the level of rows on the previous Sales Analysis report example.

Grouping data is quintessential to the pivot view. It enables drilling down the data to gain deeper insights. While you can use the Group By option to quickly add a group at the level of rows, as shown in the example above, you can also click the plus button (➕) next to the Total header at the level of rows and columns, and then select one of the preconfigured groups. To remove one, click the minus button (➖).

Once you have added a group, you can add new ones on the opposite axis or the newly created subgroups.

You could further divide the measures on the previous Sales Analysis report example by the Salesperson group at the level of columns and by the Order Date > Month group on the All / Saleable / Office Furniture product category.

Switch the rows and columns’ groups by clicking the flip axis button (⇄).

Click on a measure’s label to sort the values by ascending (⏶) or descending (⏷) order.

Download a .xlsx version of the pivot by clicking the download button (⭳).

Three graphs are available: the bar, line, and pie charts.

Bar charts are used to show the distribution or a comparison of several categories. They are especially useful as they can deal with larger data sets.

Line charts are useful to show changing time series and trends over time.

Pie charts are used to show the distribution or a comparison of a small number of categories when they form a meaningful whole.

For bar and line charts, you can use the stacked option when you have at least two groups, which then appear on top of each other instead of next to each other.

For line charts, you can use the cumulative option to sum values, which is especially useful to show the change in growth over a time period.

---

## Task creation¶

**URL:** https://www.odoo.com/documentation/19.0/applications/services/project/tasks/task_creation.html

**Contents:**
- Task creation¶
- Manual task creation¶
  - Task configuration¶
- Creating tasks from an email alias¶
- Creating tasks from a website form¶

Tasks in Odoo Project can be created manually or automatically, including from emails or website forms.

Open the Project app and choose the desired project. Create a new task by doing one of the following:

Clicking the (plus) button in the upper left corner. This creates a new task in the first stage of your Kanban view.

Pressing the (plus) button next to the Kanban stage name. This creates a new task in this Kanban stage.

Fill in the Task Title and add one or more Assignees, then click Add.

Click the task to open it. The task form includes the following fields that you can fill in:

Task Title: title of the task.

(Star): click the (star) icon to mark the task as high priority. The icon will turn yellow. Click it again to remove the high priority.

Project: the project that this task belongs to.

Assignees: the person(s) in charge of handling the work on this task.

Tags: custom labels allowing to categorize and filter your tasks.

Customer: the person or company that will be billed for this task. This field only appears in tasks that belong to billable projects.

Sales Order Item: this can be either the sales order that was used to create this task, or a sales order that was linked to this task manually. This field only appears in tasks linked to billable projects.

Allocated Time: the amount of time that the work on this task is expected to last, tracked by timesheets.

Deadline: the expected end date of the task. Click the Deadline field to select the task’s end date in the dropdown calendar. To define the task’s duration, including its start and end dates (and optionally specific times), click the (fa-calendar-plus-o) icon in the dropdown calendar, select the dates and times, then click Apply to save.

You can also create new tasks by switching to the list or Gantt view and clicking New.

The following fields can also be edited directly from the Kanban view without opening the individual task: (priority), Allocated hours, Assignees, and task status. You can also color code or Set a Cover image to your task by clicking the (vertical ellipsis).

You can use the following keyboard shortcuts in the task title to configure new tasks (modify the values in the examples below according to your needs):

30h: to allocate 30 hours to the task.

#tags: to add tags to the task.

@user: to assign the task to a user.

!: to star the task as high priority.

Along with using the correct format, follow this order: the task’s name, followed by the allocated time, the tags, the assignee, and then the priority.

For example, if you want to create a task named “Prepare workshop”, allocate 5h hours to it, add the “School” tag, assign it to Audrey and set its priority to High, enter the following task title: Prepare workshop 5h #school @Audrey !

This feature allows for project tasks to be automatically created once an email is delivered to a designated email address.

To configure it, open the Project app, then click the (vertical ellipsis) icon next to the desired project’s name. Select Settings, then open the Settings tab.

Fill in the Create tasks by sending an email to field as follows:

Section of the alias before the @ symbol: type the name of the email alias, e.g. contact, help, jobs.

Domain: in most cases, this is filled in by default with your domain.

Accept Emails From: refine the senders whose emails will create tasks in the project.

Once configured, the email alias can be seen under the name of your project on the Kanban dashboard.

When an email is sent to the alias, the email is automatically converted into a project task. The following rules apply:

The email sender is displayed in the Customer field.

The email subject is displayed in the Task Title field.

The email body is displayed in the Description field.

The whole content of the email is additionally displayed in the chatter.

All the recipients of the email (To/Cc/Bcc) that are Odoo users are automatically added as followers of the task.

If you have the Website app installed in your database, you can configure any form on your website to trigger the creation of tasks in a project.

Go to the website page where you wish to add the form and add the Form building block.

In the website editor, edit the following fields:

Action: select Create a Task.

Project: choose the project that you want the new tasks to be created in.

When the form is submitted, it automatically creates a project task. The task’s content is defined by the form’s corresponding fields.

---

## Set up a content delivery network (CDN)¶

**URL:** https://www.odoo.com/documentation/19.0/applications/websites/website/configuration/cdn.html

**Contents:**
- Set up a content delivery network (CDN)¶
- Deploying with KeyCDN¶
  - Create a pull zone in the KeyCDN dashboard¶
  - Configure the Odoo instance with the new zone¶
  - Prevent security issues by activating cross-origin resource sharing (CORS)¶

A CDN or content distribution network, is a geographically distributed network of servers that provides high speed internet content. The CDN provides quick, high-quality content delivery for content-heavy websites.

This document will guide you through the setup of a KeyCDN account with an Odoo powered website.

On the KeyCDN dashboard, start by navigating to the Zones menu item on the left. On the form, give a value to the Zone Name, which will appear as part of the CDN’s URL. Then, set the Zone Status to active to engage the zone. For the Zone Type set the value to Pull, and then, finally, under the Pull Settings, enter the Origin URL— this address should be the full Odoo database URL.

Use https://yourdatabase.odoo.com and replace the yourdatabase subdomain prefix with the actual name of the database. A custom URL can be used, as well, in place of the Odoo subdomain that was provided to the database.

Under the General Settings heading below the zone form, click the Show all settings button to expand the zone options. This should be the last option on the page. After expanding the General Settings ensure that the CORS option is enabled.

Next, scroll to the bottom of the zone configuration page and Save the changes. KeyCDN will indicate that the new zone will be deployed. This can take about 10 minutes.

A new Zone URL has been generated for your Zone, in this example it is pulltest-xxxxx.kxcdn.com. This value will differ for each database.

Copy this Zone URL to a text editor for later, as it will be used in the next steps.

In the Odoo Website app, go to the Settings and then activate the Content Delivery Network (CDN) setting and copy/paste the Zone URL value from the earlier step into the CDN Base URL field. This field is only visible and configurable when the developer mode is activated.

Ensure that there are two forward slashes (//) before the CDN Base URL and one forward slash (/) after the CDN Base URL.

Save the settings when complete.

Now the website is using the CDN for the resources matching the CDN filters regular expressions.

In the HTML of the Odoo website, the CDN integration is evidenced as working properly by checking the URL of images. The CDN Base URL value can be seen by using your web browser’s Inspect feature on the Odoo website. Look for it’s record by searching within the Network tab inside of devtools.

A security restriction in some browsers (such as Mozilla Firefox and Google Chrome) prevents a remotely linked CSS file to fetch relative resources on this same external server.

If the CORS option isn’t enabled in the CDN Zone, the more obvious resulting problem on a standard Odoo website will be the lack of Font Awesome icons because the font file declared in the Font Awesome CSS won’t be loaded from the remote server.

When these cross-origin resource issues occur, a security error message similar to the output below will appear in the web browser’s developer console:

Font from origin 'http://pulltest-xxxxx.kxcdn.com' has been blocked from loading /shop:1 by Cross-Origin Resource Sharing policy: No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin 'http://yourdatabase.odoo.com' is therefore not allowed access.

Enabling the CORS option in the CDN settings fixes this issue.

---

## View architectures¶

**URL:** https://www.odoo.com/documentation/19.0/developer/reference/user_interface/view_architectures.html

**Contents:**
- View architectures¶
- Generic architecture¶
- Python expression¶
- Form¶
  - Root attributes¶
  - Semantic components¶
    - field: display field values¶
    - label: display field labels¶
    - button: display action buttons¶
    - Chatter widget¶

The architecture of a view is defined by XML data interpreted by the JavaScript framework.

For most views, there is a *.rng file defining the attributes and possible architectures. Some views are not ruled by such a file either because they accept HTML content, or for performance reasons.

The current context and user access rights may impact the view abilities.

When evaluating node attributes, e.g. the readonly modifier, it is possible to provide a Python expression that will be executed in an environment that has access to the following variables:

The names of all fields present in the current view, containing the value of the current record, except for column_invisible in list view; relational fields are given as a list of IDs;

The ID of the current record;

parent: the record that refers to the container; only inside sub-views of relational fields;

context (dict): the current view’s context;

uid (int): the id of the current user;

today (str): the current local date in the YYYY-MM-DD format;

now (str): the current local datetime in the YYYY-MM-DD hh:mm:ss format.

Form views are used to display the data from a single record. They are composed of regular HTML with additional semantic and structural components.

The root element of form views is form.

Optional attributes can be added to the root element form to customize the view.

The view title. It is displayed only if you open an action that has no name and whose target is new (opening a dialog).

Disable/enable record creation on the view.

Disable/enable record edition on the view.

Disable/enable record duplication on the view through the Action dropdown.

Disable/enable record deletion on the view through the Action dropdown.

The name of the JavaScript component the webclient will instantiate instead of the form view.

Disable automatic focusing on the first field in the view.

Semantic components tie into the Odoo system and allow interaction with it.

Form views accept the following children semantic components: field, label, button, Chatter widget, and Attachments preview widget.

Placeholders are denoted in all caps.

The field element renders (and allows editing of, possibly) a single field of the current record.

Using the same field multiple times in a form view is supported, and the fields can receive different values for the invisible and readonly attributes. These fields may have the same values but can be displayed differently. However, the behavior is not guaranteed when several fields exist with different values for the required attribute.

The field element can have the following attributes:

The name of the field to render.

The widget used to represent the field. The selected widget can change the way the field is rendered and/or the way it can be edited. It refers to a Javascript implementation (an Owl component) registered to the fields registry.

The node id. Useful when there are several occurrences of the same field in the view (see label: display field labels).

The label of the field.

The string attribute of the model’s field

The tooltip displayed when hovering the field or its label.

The configuration options for the field’s widget (including default widgets), as a Python expression that evaluates to a dict.

For relation fields, the following options are available: no_create, no_quick_create, no_open, and no_create_edit.

Whether the field can be modified by the user (False) or is read-only (True), as a Python expression that evaluates to a bool.

Whether the field can be left empty (False) or must be set (True), as a Python expression that evaluates to a bool.

Whether the element is visible (False) or hidden (True), as a Python expression that evaluates to a bool.

There are two uses for the invisible attribute:

Usability: to avoid overloading the view and to make it easier for the user to read, depending on the content.

Technical: a field must be present (invisible is enough) in the view to be used in a Python expression.

The comma-separated list of user groups to whom the element is displayed. Users who do not belong to at least one of these groups are unable to see the element. Groups can be prefixed with the negative ! operator to exclude them.

The filters to apply when displaying existing records for selection, as a Python expression that evaluates to a domain.

The context to use when fetching possible values and creating or searching records, as a Python expression that evaluates to a dict.

Whether the field label should be hidden.

Fields that are a direct child of a group element

The help message to display on empty fields. It can replace field labels in complex forms. However, it should not be an example of data, as users may confuse placeholder text with filled fields.

The comma-separated list of display modes (view types) to use for the field’s linked records. Allowed modes are: list, form, kanban, and graph.

One2many and Many2many fields

The HTML class to set on the generated element.

The styling uses the Bootstrap framework and UI icons. Common Odoo classes include:

oe_inline: prevents the usual line break following fields, and limits their span;

oe_left, oe_right: floats the element to the corresponding direction;

oe_read_only, oe_edit_only: only displays the element in the corresponding form mode;

oe_avatar: for image fields, displays images as an “avatar” (max 90x90 square);

oe_stat_button: defines a particular rendering to dynamically display information while being clickable to target an action.

The name of the related field providing the name of the file.

Whether the field stores a password and thus its data should not be displayed.

The XMLID of the specific Kanban view record that should be used when selecting records in a mobile environment.

Whether the field is focused when the view opens. It can be applied to only one field of a view.

Relational fields nodes can contain specific subviews.

When a field component is not placed directly inside a group, or when its nolabel attribute is set, the field’s label is not automatically displayed alongside its value. The label component is the manual alternative of displaying the label of a field.

The label element can have the following attributes:

The reference to the field associated with the label. It can be either the name of the field, or its id (the id attribute set on the field).

When there are several occurrences of the same field in the view, and there are several label components associated with these field nodes, these labels must have unique for attribute; in this case, referencing the id attribute of the corresponding field nodes.

The label to display.

The field’s label coming from the field definition on the model

The HTML class to set on the generated element.

The styling uses the Bootstrap framework and UI icons. Common Odoo classes include:

oe_inline: prevents the usual line break following fields, and limits their span;

oe_left, oe_right: floats the element to the corresponding direction;

oe_read_only, oe_edit_only: only displays the element in the corresponding form mode;

oe_avatar: for image fields, displays images as an “avatar” (max 90x90 square);

oe_stat_button: defines a particular rendering to dynamically display information while being clickable to target an action.

Whether the element is visible (False) or hidden (True), as a Python expression that evaluates to a bool.

There are two uses for the invisible attribute:

Usability: to avoid overloading the view and to make it easier for the user to read, depending on the content.

Technical: a field must be present (invisible is enough) in the view to be used in a Python expression.

The button element can have the following attributes:

The type of the button indicating how it behaves. It can have two different values:

Call a method on the view’s model. The button’s name is the method that is called with the current record ID and the current context.

Load and execute an ir.actions action record. The button’s name is the XMLID of the action to load. The context is extended with the view’s model (as active_model) and with the current record (as active_id).

Mandatory if the special attribute is not set

The method to call if the type is object. The XMLID of the action to load if the type is action, either in raw format or in %(XMLID)d format.

The button’s text if there is no icon, the alt text for the icon otherwise.

The icon to use to display the button. See icons for the reference list.

The tooltip message shown when hovering with the mouse cursor.

The context that is merged into the view’s context when performing the button’s call, as a Python expression that evaluates to a dict.

The comma-separated list of user groups to whom the element is displayed. Users who do not belong to at least one of these groups are unable to see the element. Groups can be prefixed with the negative ! operator to exclude them.

Whether the element is visible (False) or hidden (True), as a Python expression that evaluates to a bool.

There are two uses for the invisible attribute:

Usability: to avoid overloading the view and to make it easier for the user to read, depending on the content.

Technical: a field must be present (invisible is enough) in the view to be used in a Python expression.

The HTML class to set on the generated element.

The styling uses the Bootstrap framework and UI icons. Common Odoo classes include:

oe_inline: prevents the usual line break following fields, and limits their span;

oe_left, oe_right: floats the element to the corresponding direction;

oe_read_only, oe_edit_only: only displays the element in the corresponding form mode;

oe_avatar: for image fields, displays images as an “avatar” (max 90x90 square);

oe_stat_button: defines a particular rendering to dynamically display information while being clickable to target an action.

The behavior of the button for form views opened in dialog. It can have two different values:

Save the record and close the dialog.

Close the dialog without saving.

The confirmation message to display (and for the user to accept) before performing the button’s action.

The hotkey (keyboard_shortcut, similar to an accesskey) that is bound to the button. It is enabled when the alt key is pressed together with the selected character, or together with the shift key and the selected character when shift+ is prepended to the value.

The chatter widget is the communication and log tool allowing to email colleagues and customers directly from a record (task, order, invoice, event, note…).

It is added with a div element with the class oe_chatter when the model inherits the mail.thread mixin.

The attachment preview widget is added with an empty div element with the class o_attachment_preview.

Structural components provide structure or “visual” features with little logic. They are used as elements or sets of elements in form views.

Form views accept the following children structural components: group, sheet, notebook, notebook, newline, separator, header, footer, Buttons container, and Title container.

Placeholders are denoted in all caps.

The group element is used to define column layouts in forms. By default, groups define 2 columns, and most direct children of groups take a single column.

field elements that are direct children of groups display a label by default, and the label and the field itself have a colspan of 1 each.

Children are laid out horizontally (they try to fill the next column before changing row).

The group element can have the following attributes:

The title displayed for the group.

The number of columns in a group.

The number of columns taken by a child element.

Whether the element is visible (False) or hidden (True), as a Python expression that evaluates to a bool.

There are two uses for the invisible attribute:

Usability: to avoid overloading the view and to make it easier for the user to read, depending on the content.

Technical: a field must be present (invisible is enough) in the view to be used in a Python expression.

Possible structure and representation of its rendering

The sheet element can be used as a direct child of the form root element for a narrower and more responsive form layout (centered page, margin…). It usually contains group elements.

The notebook element defines a tabbed section. Each tab is defined through a page child element.

The notebook element should not be placed within group elements.

The page element can have the following attributes:

The title of the tab.

Whether the element is visible (False) or hidden (True), as a Python expression that evaluates to a bool.

There are two uses for the invisible attribute:

Usability: to avoid overloading the view and to make it easier for the user to read, depending on the content.

Technical: a field must be present (invisible is enough) in the view to be used in a Python expression.

Possible structure and representation of its rendering

The newline element is used within group elements to end the current row early and immediately switch to a new row, without filling any remaining column beforehand.

Possible structure and representation of its rendering

The separator element adds vertical spacing between elements within a group.

The <separator> element can have the following attributes:

The title as a section title.

Possible structure and representation of its rendering

The separator element can be used to achieve visual separation between elements within the same inner group element while keeping them horizontally aligned.

The header element combined with the sheet element provides a full-width location above the sheet itself generally used to display workflow button elements and a field element rendered as status widget.

The footer element is used to display buttons elements at the end of dialogs.

When no footer element is specified, the view’s standard buttons (like Save or Discard) will be present by default. It is also possible to avoid replacing the standard buttons in form or x2many dialogs by using the replace attribute. This attribute defaults to True if not specified but setting it to False (or 0) will make it so that the specified footer will be added next to the default buttons instead of replacing them.

A button elements container can be created with a div element with the class button_box.

Possible structure and representation of its rendering

A title field element container can be created with a div element with the class oe_title.

Settings views are a customization of the form view. They are used to display settings in a centralized place. They differ from generic form views in that they have a search bar and a sidebar.

Settings views accept the field, label and button elements of form views, as well as three additional children elements: app, block, and setting.

Placeholders are denoted in all caps.

The app element is used to declare the application on the settings view. It creates an entry with the logo of the application on the sidebar of the view. It also acts as delimiter when searching.

The app element can have the following attributes:

The name of the application.

The technical name of the application (the name of the module).

The relative path to the logo.

A path computed with the name attribute: /name/static/description/icon.png

The comma-separated list of user groups to whom the element is displayed. Users who do not belong to at least one of these groups are unable to see the element. Groups can be prefixed with the negative ! operator to exclude them.

Whether the element is visible (False) or hidden (True), as a Python expression that evaluates to a bool.

There are two uses for the invisible attribute:

Usability: to avoid overloading the view and to make it easier for the user to read, depending on the content.

Technical: a field must be present (invisible is enough) in the view to be used in a Python expression.

The block element is used to declare a group of settings. This group can have a title and a description.

The block element can have the following attributes:

The title of the block of settings. One can search on its value.

The description of the block of settings. One can search on its value.

The comma-separated list of user groups to whom the element is displayed. Users who do not belong to at least one of these groups are unable to see the element. Groups can be prefixed with the negative ! operator to exclude them.

Whether the element is visible (False) or hidden (True), as a Python expression that evaluates to a bool.

There are two uses for the invisible attribute:

Usability: to avoid overloading the view and to make it easier for the user to read, depending on the content.

Technical: a field must be present (invisible is enough) in the view to be used in a Python expression.

The setting element is used to declare the setting itself.

The first field element in the setting is used as the main field. It is placed on the left panel if it is a boolean field, and on the top of the right panel otherwise. The field is also used to create the setting label if a string attribute is not defined.

The setting element can also contain additional elements (e.g., HTML). All of those elements are rendered in the right panel.

The <setting> element can have the following attributes:

By default, a setting is visually separated on two panels (left and right), and is used to edit a given field. By defining type="header", a special kind of setting is rendered instead. This setting is used to modify the scope of the other settings. For example, on the Website application, this setting is used to indicate to which website the other settings apply. The header setting is visually represented as a banner on top of the screen.

The text used as the label of the setting.

The first field’s label

The text used as a tooltip.

The description of the setting. This text is displayed just below the setting label (with the class text-muted).

Whether the setting is company-specific. If set, an icon is displayed next to the setting label.

It accepts only the value '1'.

The path to the documentation on the setting. If set, a clickable icon is displayed next to the setting label. The path can be both an absolute or a relative path. In the latter case, it is relative to https://www.odoo.com/documentation/<version>.

The comma-separated list of user groups to whom the element is displayed. Users who do not belong to at least one of these groups are unable to see the element. Groups can be prefixed with the negative ! operator to exclude them.

Whether the element is visible (False) or hidden (True), as a Python expression that evaluates to a bool.

There are two uses for the invisible attribute:

Usability: to avoid overloading the view and to make it easier for the user to read, depending on the content.

Technical: a field must be present (invisible is enough) in the view to be used in a Python expression.

The root element of list views is list (the previous name was tree).

Possible structure and representation of its rendering

Optional attributes can be added to the root element list to customize the view.

The view title. It is displayed only if you open an action that has no name and whose target is new (opening a dialog).

Disable/enable record creation on the view.

Disable/enable record edition on the view.

Disable/enable record deletion on the view through the Action dropdown.

Disable/enable record import from data on the view.

Disable/enable record export to data on the view.

Make the view’s records editable in-place, and allow creating new records from a row of the list. It can have two different values:

New records are created from the top of the list.

New records are created from the bottom of the list.

The architecture for the inline form view is derived from the list view. Most attributes valid on a form view’s fields and buttons are thus accepted by list views, although they may not have any meaning if the list view is non-editable.

This behavior is disabled if the edit attribute is set to False.

Activate the multi-editing feature that allows updating a field to the same value for multiple records at once.

It accepts only the value '1'.

Display a button at the end of each row to open the record in a form view.

It has no effect if the view is non-editable.

The name of the field on which the records should be grouped by default if no grouping is specified via the action or the current search.

A comma-separated list of fields names that overrides the ordering defined on the model through the _order attribute.

To inverse the sorting order of a field, postfix it with desc, separated by a space.

The style that should be applied to matching records’ rows, as a Python expression that evaluates to a bool.

<style> must be replaced by one of bf (bold), it (italic), info, warning, danger, muted, primary, and success.

The default size of a page. It must be strictly positive.

80 for list views, 40 for X2many lists in form views

The default number of groups on a page when the list view is grouped. It must be strictly positive.

80 for list views, 40 for X2many lists in form views

Whether the first level of groups should be opened by default when the list view is grouped.

It may be slow, depending on the number of groups.

Whether the view should be populated with a set of sample records if none are found for the current model.

These fake records have heuristics for certain field names/models. For example, a field display_name on the model res.users will be populated with sample people names, while an email field will be in the form firstname.lastname@sample.demo.

The user is unable to interact with these data, and they will be discarded as soon as an action is performed (record created, column added, etc.).

List views accept the following children elements: field, button, groupby, header, control, and create.

Placeholders are denoted in all caps.

The field element renders (and allows editing of, possibly) a single field of all current records as a column.

Using the same field multiple times in a list view is not supported

The field element can have the following attributes:

The name of the field to render.

The widget used to represent the field. The selected widget can change the way the field is rendered and/or the way it can be edited. It refers to a Javascript implementation (an Owl component) registered to the fields registry.

The label of the field.

The string attribute of the model’s field

Make the visibility of the field optional. The field’s column can be hidden or shown through a button on the view’s header.

It can have two different values:

The field is shown by default.

The field is hidden by default.

Whether the field can be modified by the user (False) or is read-only (True), as a Python expression that evaluates to a bool.

Whether the field can be left empty (False) or must be set (True), as a Python expression that evaluates to a bool.

Whether the element is visible (False) or hidden (True), as a Python expression that evaluates to a bool.

There are two uses for the invisible attribute:

Usability: to avoid overloading the view and to make it easier for the user to read, depending on the content.

Technical: a field must be present (invisible is enough) in the view to be used in a Python expression.

Whether the column is visible (False) or hidden (True), as a Python expression that evaluates to a bool.

Unlike invisible, it affects the entire column, and is evaluated without the subtree values.

The comma-separated list of user groups to whom the element is displayed. Users who do not belong to at least one of these groups are unable to see the element. Groups can be prefixed with the negative ! operator to exclude them.

The style that should be applied to matching records’ field, as a Python expression that evaluates to a bool.

<style> must be replaced by one of bf (bold), it (italic), info, warning, danger, muted, primary, and success.

The aggregate to display at the bottom of the column. The aggregation is computed on only records that are currently displayed. The aggregation operation must match the corresponding field’s aggregator.

The list view always tries to optimize the available space among columns. For some field types, this is done by enforcing a width, depending on the field type. For instance, we know exactly the number of pixels required to display a date, so we can ensure that a column for a date field doesn’t take more space than what is strictly necessary, thus leaving the extra space for the other columns. However, the framework can’t guess the adequate width for every field types. For instance, char fields can be used to encode large values, or 3-letter country codes. In the latter case, one can set the width directly in the arch (e.g. width="40px"). It represents the width (always in pixels) required to render the values inside the cells. The width of the column will then be the sum of the given value and the cells’ left and right paddings.

Whether the field’s column header should remain empty. If set, the column will not be sortable.

It accepts only the value '1'

When a list view is grouped, numeric fields are aggregated and displayed for each group. Also, if there are too many records in a group, a pager appears on the right of the group row. For this reason, it is a bad practice to have a numeric field in the last column when the list view is in a situation where it can be grouped. However, it does not pose a problem for X2many fields in a form view, as they cannot be grouped.

Possible structure and representation of its rendering

The button element can have the following attributes:

The type of the button indicating how it behaves. It can have two different values:

Call a method on the view’s model. The button’s name is the method that is called with the current record ID and the current context.

Load and execute an ir.actions action record. The button’s name is the XMLID of the action to load. The context is extended with the view’s model (as active_model) and with the current record (as active_id).

Mandatory if the special attribute is not set

The method to call if the type is object. The XMLID of the action to load if the type is action, either in raw format or in %(XMLID)d format.

The button’s text if there is no icon, the alt text for the icon otherwise.

The icon to use to display the button. See icons for the reference list.

The tooltip message shown when hovering with the mouse cursor.

The context that is merged into the view’s context when performing the button’s call, as a Python expression that evaluates to a dict.

The comma-separated list of user groups to whom the element is displayed. Users who do not belong to at least one of these groups are unable to see the element. Groups can be prefixed with the negative ! operator to exclude them.

Whether the element is visible (False) or hidden (True), as a Python expression that evaluates to a bool.

There are two uses for the invisible attribute:

Usability: to avoid overloading the view and to make it easier for the user to read, depending on the content.

Technical: a field must be present (invisible is enough) in the view to be used in a Python expression.

Whether the column is visible (False) or hidden (True), as a Python expression that evaluates to a bool.

Unlike invisible, it affects the entire column, and is evaluated without the subtree values.

The HTML class to set on the generated element.

The styling uses the Bootstrap framework and UI icons. Common Odoo classes include:

oe_inline: prevents the usual line break following fields, and limits their span;

oe_left, oe_right: floats the element to the corresponding direction;

oe_read_only, oe_edit_only: only displays the element in the corresponding form mode;

oe_avatar: for image fields, displays images as an “avatar” (max 90x90 square);

oe_stat_button: defines a particular rendering to dynamically display information while being clickable to target an action.

Possible structure and representation of its rendering

The groupby element is used to define group headers with button elements when grouping records on Many2one fields. It also accepts field elements, which can be used for modifiers. These fields thus belong on the Many2one co-model. These extra fields are fetched in batch.

The groupby element can have the following attributes:

The name of the a Many2one field to use as header.

A special button element with type="edit" can be defined to open the Many2one field’s form view.

Possible structure and representation of its rendering

Fields inside the groupby element are used only to fetch and store the value, but they are never displayed.

The header element accepts the following children elements:

The button element allows defining buttons in the control panel. It is the same element as the button element in list views, but it accepts one more attribute when placed inside a header element:

Make the button available at all time, without having to select records.

It accepts only the value always.

Possible structure and representation of its rendering

The control element allows to customize the create and delete actions. In particular, it allows to add special create buttons with specific contexts, to use regular view buttons as create actions, and to make the create and delete actions available only under certain conditions.

The control element is only supported in list views inside One2many or Many2many fields.

The control element takes no attributes. It accepts the following children elements:

The given create elements replace the default Add a line button. A create element can have the following attributes:

The context that is merged into the view’s context when performing the button’s call, as a Python expression that evaluates to a dict.

Whether the element is visible (False) or hidden (True), as a Python expression that evaluates to a bool. The parent record and the context can be used in the expresion.

Like regular view buttons

The delete element allows to conditionnaly hide the delete icon, row by row. There can only be one child of this type. It can have only one attribute:

Same as for create, except that in this case the record itself can also be used in the expresion.

Possible structure and representation of its rendering

Search views are different from other view types in that they are not used to display content. Although they apply to a specific model, they are used to filter another view’s content (usually aggregated views; e.g., List and Graph).

The root element of search views is search.

It takes no attributes.

Possible structure and representation of its rendering

Search views accept the following children elements: field, filter, separator, group, and searchpanel.

Placeholders are denoted in all caps.

The field element defines domains or contexts with user-provided values. When search domains are generated, field domains are joined with each other and with filters using the AND operator.

The field element can have the following attributes:

The name of the field to filter on.

The label of the field.

The string attribute of the model’s field

By default, fields generate domains of the form [(name, {operator}, value)], where name is the field’s name and value is the value provided by the user, possibly filtered or transformed (e.g., a user is expected to provide the label of a selection field’s value, not the value itself).

The operator attribute allows overriding the default operator, which depends on the field’s type (e.g., = for float fields, but ilike for char fields and child_of for many2one).

The domain to use as the field’s search domain, as a Python expression that evaluates to a domain.

It can use the self variable to inject the provided value in the custom domain. It can be used to generate significantly more flexible domains than with the operator attribute alone (e.g., search on multiple fields at once).

If both the operator and filter_domain attributes are provided, filter_domain takes precedence.

The context to merge into the context of the view that the search view is targeting, as a Python expression that evaluates to a dict.

It can contain user-provided values, which are available under the self variable.

The filters to apply to the completion results for fields that allow for auto-completion (e.g., Many2one).

The comma-separated list of user groups to whom the element is displayed. Users who do not belong to at least one of these groups are unable to see the element. Groups can be prefixed with the negative ! operator to exclude them.

Whether the element is visible (False) or hidden (True), as a Python expression that evaluates to a bool.

There are two uses for the invisible attribute:

Usability: to avoid overloading the view and to make it easier for the user to read, depending on the content.

Technical: a field must be present (invisible is enough) in the view to be used in a Python expression.

Possible structure and representation of its rendering

The filter element is used to create pre-defined filters that can be toggled in the search view. It allows adding data to the search context the context passed to the data view for searching/filtering, or appending new sections to the search filter.

The filter element can have the following attributes:

The technical name of the filter. It can be used to enable it by default or as an inheritance hook.

The label of the filter.

The tooltip displayed when hovering the filter.

The domain to append to the action’s domain as part of the search domain.

The name of the date or datetime field to filter on.

When used, this attribute creates a set of filters available in a sub-menu of the Filters menu. The available filters are time-dependent but not dynamic in the sense that their domains are evaluated at the time of the control panel instantiation.

By default, these filters contain a dropdown with different sub-filters that allow you to filter based on months, quarters and years. Additionally, you can create custom sub-filters that allow filtering using domains. These custom filters must have the following attributes: name, string and domain.

Note that all custom filters defined this way are mutually exclusive with each other and with the other sub-filters.

The earliest month that will show up in the dropdown of a date filter, as an offset relative to the current month.

If the current month is February, the earliest month selectable in the dropdown will be November.

Filters with a non-empty date attribute

The latest month that will show up in the dropdown of a date filter, as an offset relative to the current month.

If the current month is February, the latest month selectable in the dropdown will be March.

Filters with a non-empty date attribute

The earliest year that will show up in the dropdown of a date filter, as an offset relative to the current year.

If the current year is 2024, the earliest year selectable in the dropdown will be 2021.

Filters with a non-empty date attribute

The latest year that will show up in the dropdown of a date filter, as an offset relative to the current year.

If the current year is 2024, the latest year selectable in the dropdown will be 2025.

Filters with a non-empty date attribute

The default period of the time-based filter (with a date attribute). It must be one of, or a comma-separated list of valid filter ids.

Valid filter ids include the following:

first_quarter, second_quarter, third_quarter and fourth_quarter.

One of month, month-x and month+x, where x is a non-zero integer value between start_month and end_month.

One of year, year-x and year+x, where x is a non-zero integer value between start_year and end_year.

The name of any custom filter defined within the filter, prepended with custom_.

The filter must be in the default set of filters activated at the view initialization.

month, or the closest value to the current month if unavailable

Filters with a non-empty date attribute

Whether the element is visible (False) or hidden (True), as a Python expression that evaluates to a bool.

There are two uses for the invisible attribute:

Usability: to avoid overloading the view and to make it easier for the user to read, depending on the content.

Technical: a field must be present (invisible is enough) in the view to be used in a Python expression.

The comma-separated list of user groups to whom the element is displayed. Users who do not belong to at least one of these groups are unable to see the element. Groups can be prefixed with the negative ! operator to exclude them.

The context merged into the action’s domain to generate the search domain

The context key group_by set with a field as value can be used to define a group available in the Group By menu. When the field is of type date or datetime, the filter generates a submenu of the Group By menu with the following interval options available: Year, Quarter, Month, Week, and Day.

The results of formatted_read_group grouped on a field may be influenced by its group_expand attribute, allowing to display empty groups when needed. For more information, please refer to Field.

Sequences of filters (without non-filters elements separating them) are treated as inclusively composited: they will be composed with OR rather than the usual AND.

Records whose state field is draft or done are shown.

Records whose state field is draft and delay field is below 15.

Possible structure and representation of its rendering

The separator element is used to separates groups of filters in simple search views. For more complex search views, the group element is recommended.

The separator element takes no attributes.

The group element is used to separate groups of filters in cluttered search views. In simpler search views, it can be substituted for the separator element.

The group element takes no attributes.

The searchpanel element displays a search panel to the left of multi-records views. It allows for quickly filtering data on the basis of given fields.

The searchpanel element accepts only field children elements.

The field element used as a child element of a searchpanel element can have the following attributes:

The name of the field to filter on.

The label of the field.

The string attribute of the model’s field

The behavior and display of the field. It can have two different values:

At most one value can be selected. Supported field types are many2one and selection.

Several values can be selected. Supported field types are many2one, many2many and selection.

The comma-separated list of user groups to whom the element is displayed. Users who do not belong to at least one of these groups are unable to see the element. Groups can be prefixed with the negative ! operator to exclude them.

The icon of the field.

The color of the field.

When the field element has the select=one attribute set, it can have the following additional attributes:

Whether child categories should appear under their parent category, or at the same hierarchy level.

If set to a non zero integer, the hierarchy (if any) will be unfold up to the given level.

When the field element has the select=multi attribute set, it can have the following additional attributes:

Whether the record counters is computed and displayed if non-zero.

This attribute exists to avoid impacting performance. Another way to address performance issues is to override the search_panel_select_range and search_panel_select_multi_range methods.

Whether categories and filters with no records should be shown.

The maximal number of values to fetch for the field. If the limit is reached, no values are displayed on the search panel, and an error message is shown instead. If set to 0, all values are fetched.

The conditions that the records have to satisfy.

The name of the field name on which values should be grouped.

Many2one and Many2many fields

Search fields and filters can be configured through the action’s context using search_default_{name} keys. For fields, the value must be the value to set to the field. For filters, it must be a boolean value or a number.

With foo, a field, and bar, a filter, the following action context will search foo on acro and enable bar by default:

A numeric value (between 1 and 99) can be used to define the order of default groupby filters.

With foo and bar, two groupby filters, the following action context will first enable bar, then foo.

Kanban views are used as a kanban board visualisation: they display records as “cards”, halfway between a list and a form view.

Records may be grouped in columns for use in workflow visualisation or manipulation (e.g., tasks or work-progress management), or ungrouped (used simply to visualize records).

The root element of Kanban views is kanban.

Possible structure and representation of its rendering

Kanban views load and display a maximum of ten columns. Any column after that is closed but can still be opened by the user.

Optional attributes can be added to the root element kanban to customize the view.

The view title. It is displayed only if you open an action that has no name and whose target is new (opening a dialog).

Disable/enable record creation on the view.

Disable/enable record edition on the view.

Disable/enable record deletion on the view through the Action dropdown.

The name of the field on which the records should be grouped by default if no grouping is specified via the action or the current search.

A comma-separated list of fields names that overrides the ordering defined on the model through the _order attribute.

To inverse the sorting order of a field, postfix it with desc, separated by a space.

Add HTML classes to the root HTML element of the view.

The key in the KanbanExamplesRegistry of the examples that can be browsed when creating a new column in the grouped kanban view.

Use of the examples attribute in the utm module

Whether the Add a new column bar is visible.

Whether columns can be deleted via the cog menu.

Whether columns can be edited via the cog menu.

Whether columns can be reordered.

Whether records can be dragged when the kanban view is grouped.

Whether records belonging to a column can be archived and unarchived when the active field is defined on the model.

Whether it should be possible to create records without switching to the form view.

True when the kanban view is grouped by many2one, selection, char, or boolean fields, otherwise False

The reference of the form view to open when using the quick creation of records.

The custom action to call when clicking on Create.

If set to 'quick_create', the quick creation of records is used instead. If the quick creation is disabled, the standard create action is called.

By default, clicking on a kanban card opens the corresponding record in a form view. This behavior can be disabled by setting the attribute can_open to False.

Name of the integer field used to color the left border of the kanban cards.

Whether the view should be populated with a set of sample records if none are found for the current model.

These fake records have heuristics for certain field names/models. For example, a field display_name on the model res.users will be populated with sample people names, while an email field will be in the form firstname.lastname@sample.demo.

The user is unable to interact with these data, and they will be discarded as soon as an action is performed (record created, column added, etc.).

Kanban views accept the following children elements: templates, field, header, progressbar.

The templates element is used to define the QWeb templates that structure the kanban cards.

The definition of a card’s structure can be split into multiple templates for clarity, but at least one root card template must be defined.

An additional template can be defined: menu. If defined, it is rendered inside a dropdown that can be toggled with a vertical ellipsis (⋮) on the top right of the card.

The templates are written in JavaScript QWeb.

These are QWeb templates, not Owl templates, meaning that directives like t-on-click aren’t available.

Inside those templates, the field element allows to render a field. It can have the following attributes:

The name of the field to render.

The widget used to represent the field. The selected widget can change the way the field is rendered and/or the way it can be edited. It refers to a Javascript implementation (an Owl component) registered to the fields registry.

By default, field nodes are replaced by a span containing their formatted value, unless the widget attribute is specified, in which case their rendering and behavior depends on the corresponding widget. The widget attribute can have different values including:

Allows reordering records with a drag and drop, using the corresponding field as order.

Allows editing a color (integer) field. Combined with the root attribute highlight_color, allows editing the color of the cards.

See the Field section to discover various widgets and their options.

Kanban templates being rendered with the QWeb engine, they have a rendering context, a set of variables available in the templates, containing useful information and tools. Here’re the available variables:

An object with all the fields defined in the view. Each field has two attributes: value and raw_value. The former is formatted according to current user parameters, while the latter is the raw value (e.g. the id for a many2one field). This object is useful for instance, for using field values inside t-if conditions. For display purposes, we recommend using the <field> tag.

An object with 2 keys defining the available actions for the user:

editable: true if the user can edit records, false otherwise;

deletable: true if the user can delete records, false otherwise.

This is useful to conditionally display elements requiring specific access rights.

The current context propagated from either the action that opens the kanban view, or the one2many or many2many field that embeds the kanban view in a form view.

Indicates that the view is readonly.

Whether the kanban view is opened when selecting a many2one or many2many field (in mobile environment).

The luxon object, allowing to manipulate date and datetime field values.

The Javascript JSON namespace object containing a parse method allowing to parse json field values into Javascript Objects.

While most of the kanban templates are standard QWeb templates, the kanban view processes button and a elements is a special way. Buttons and links with a type attribute perform different operations than their standard HTML function. The type attribute can have the values action and object of regular buttons, or the following values:

Clicking the element opens the card’s record in form view.

Clicking the element deletes the card’s record and removes the card.

Clicking the element archives the card’s record and removes the card.

Clicking the element unarchives the card’s record and removes the card.

Clicking the element allows to select an image to set as cover image of the record.

The widget element allows to insert dynamically generated (in Javascript) html inside the cards. It has a mandatory name attribute, referring to a Javascript implementation (an Owl component) registered to the view_widgets registry.

See the Widget section to discover various widgets and their options.

Several card layouts can be easily obtained using standard html elements and Bootstrap utility classes. By default, the card is a flexbox container with column direction.

The footer html element is styled to stick to the bottom of the card, and is as a flexbox container with row direction, allowing to easily display several fields on the same line.

To display some content, like an image, on the side of the card, one can use aside and main html elements, with the flex-row classname on the card. The main node is a flexbox container like the card is when there’s no aside.

The classname o_kanban_aside_full set on the aside element removes the padding so that the image spreads to the borders of the card.

The field element can also be used outside the kanban templates. In that case, it allows to declare fields that are not displayed in the card, but still need to be fetched, for instance because their value is used in a t-if condition.

The header element is used to insert custom buttons in the control panel.

The header element accepts only button children elements, similar to list views’ button elements.

The button element used as a child element of the header element can have the following additional attributes:

The display mode of the button. It can have two different values:

The button is displayed only when some records are selected; their action applies to the selected records.

The button is displayed at all times, even if no records are selected.

Only the always display mode is available because it is not yet possible to select records in a kanban view.

The progressbar element is used to define a progress bar to display on top of kanban columns in grouped kanban views.

The progressbar element can have the following attributes:

The name of the field on which the progress bar’s sub-groups are based.

The mapping of the progress bar’s field values to the color values muted, success, warning, and danger.

The name of the field to use in a sum displayed next to the progress bar. If not set, the total number of records is displayed instead.

Possible structure and representation of its rendering

QWeb views are standard QWeb Templates templates inside a view’s arch. They don’t have a specific root element. Because QWeb views don’t have a specific root element, their type must be specified explicitly (it can not be inferred from the root element of the arch field).

QWeb views have two use cases:

they can be used as frontend templates, in which case template should be used as a shortcut.

they can be used as actual qweb views (opened inside an action), in which case they should be defined as regular view with an explicit type (it can not be inferred) and a model.

The main additions of qweb-as-view to the basic qweb-as-template are:

qweb-as-view has a special case for a <nav> element bearing the CSS class o_qweb_cp_buttons: its contents should be buttons and will be extracted and moved to the control panel’s button area, the <nav> itself will be removed, this is a work-around to control panel views not existing yet

qweb-as-view rendering adds several items to the standard qweb rendering context:

the model to which the qweb view is bound

the domain provided by the search view

the context provided by the search view

a lazy proxy to model.search(domain), this can be used if you just want to iterate the records and not perform more complex operations (e.g. grouping)

qweb-as-view also provides additional rendering hooks:

_qweb_prepare_context(view_id, domain) prepares the rendering context specific to qweb-as-view

qweb_render_view(view_id, domain) is the method called by the client and will call the context-preparation methods and ultimately env['ir.qweb'].render().

The graph view is used to visualize aggregations over a number of records or record groups. Its root element is <graph> which can take the following attributes:

one of bar (default), pie and line, the type of graph to use

only used for bar charts. Set to 0 to prevent the bars within a group to be stacked initially.

set to 1 to prevent from redirecting clicks on graph to list view

if set, x-axis values will be sorted by default according their measure with respect to the given order (asc or desc). Only used for bar and pie charts.

string displayed in the breadcrumbs when redirecting to list view.

Whether the view should be populated with a set of sample records if none are found for the current model.

These fake records have heuristics for certain field names/models. For example, a field display_name on the model res.users will be populated with sample people names, while an email field will be in the form firstname.lastname@sample.demo.

The user is unable to interact with these data, and they will be discarded as soon as an action is performed (record created, column added, etc.).

The only allowed element within a graph view is field which can have the following attributes:

the name of a field to use in the view. If used for grouping (rather than aggregating)

if true, the field will not appear either in the active measures nor in the selectable measures.

if set to measure, the field will be used as an aggregated value within a group instead of a grouping criteria. It only works for the last field with that attribute but it is useful for other fields with string attribute (see below).

on date and datetime fields, groups by the specified interval (day, week, month, quarter or year) instead of grouping on the specific datetime (fixed second resolution) or date (fixed day resolution). Default is month.

only used for field with type="measure". The name that will be used to display the field in the graph view, overrides the default python String attribute of the field.

The measures are automatically generated from the model fields; only the aggregatable fields are used. Those measures are also alphabetically sorted on the string of the field.

graph view aggregations are performed on database content, non-stored function fields can not be used in graph views

In Graph views, a field can have a widget attribute to dictate its format. The widget should be a field formatter, of which the most interesting are float_time, and monetary.

The pivot view is used to visualize aggregations as a pivot table. Its root element is <pivot> which can take the following attributes:

Set to 1 to remove table cell’s links to list view.

Set to 1 to display the Quantity column by default.

The name of the measure and the order (asc or desc) to use as default order in the view.

The only allowed element within a pivot view is field which can have the following attributes:

the name of a field to use in the view. If used for grouping (rather than aggregating)

the name that will be used to display the field in the pivot view, overrides the default python String attribute of the field.

indicates whether the field should be used as a grouping criteria or as an aggregated value within a group. Possible values are:

groups by the specified field, each group gets its own row.

creates column-wise groups

field to aggregate within a group

on date and datetime fields, groups by the specified interval (day, week, month, quarter or year) instead of grouping on the specific datetime (fixed second resolution) or date (fixed day resolution).

if true, the field will not appear either in the active measures nor in the selectable measures (useful for fields that do not make sense aggregated, such as fields in different units, e.g. € and $).

Whether the view should be populated with a set of sample records if none are found for the current model.

These fake records have heuristics for certain field names/models. For example, a field display_name on the model res.users will be populated with sample people names, while an email field will be in the form firstname.lastname@sample.demo.

The user is unable to interact with these data, and they will be discarded as soon as an action is performed (record created, column added, etc.).

The measures are automatically generated from the model fields; only the aggregatable fields are used. Those measures are also alphabetically sorted on the string of the field.

like the graph view, the pivot aggregates data on database content which means that non-stored function fields can not be used in pivot views

In Pivot view a field can have a widget attribute to dictate its format. The widget should be a field formatter, of which the most interesting are date, datetime, float_time, and monetary.

For instance a timesheet pivot view could be defined as:

Calendar views display records as events in a daily, weekly, monthly or yearly calendar.

By default the calendar view will be centered around the current date (today). You can pass a specific initial date to the context of the action in order to set the initial focus of the calendar on the period (see mode) around this date (the context key to use being initial_date)

Their root element is <calendar>. Available attributes on the root node are:

Name of the record’s field holding the start date for the event.

Name of the record’s field holding the end date for the event.

Alternative to date_stop. Provides the duration of the event instead of its end date (unit: hour).

Comma-separated list of available scales, among day, week, month, year. By default, all scales are available.

"day,week,month,year"

Default scale of the calendar.

"day", "week", "month" or "year"

Name of the record’s field to use for color segmentation. Records in the same color segment are allocated the same highlight color in the calendar.

Name of the record’s boolean field indicating whether the corresponding event is flagged as day-long, in which case duration is irrelevant.

Name of the record’s field to use to display aggregated values next to filters, in the filter side panel. The aggregator can be explicitly given (e.g. aggregate="expected_revenue:sum"). If not given, the aggregator of the field is used. Supported aggregators are sum, avg, min, max, count and count_distinct.

Limits the number of events displayed in calendar cells, in month scale, and for all-day events in week and day scales. If there are more events than the limit, a “more” button is added to show the rest of the events in a popover.

If set to true weekend days and public holidays have a greyed out background.

Set it to true to hide the date part in the record’s popover.

Set it to true to hide the time part in the record’s popover.

By default, in month mode, the last days of the previous month and the first days of the next months are displayed, as well as their events. This option allows to disable this such that those sibling days are greyed out and their events aren’t displayed.

By default, a mini calendar (in month mode) is displayed in the side panel, next to the main calendar. This option allows to remove it by setting it to False.

If true, open events in dialog to edit them, otherwise, open them in a classical form view.

View to open when the user creates or edits an event. By default, uses the form view of the current action, if any.

Enables quick event creation on click: only asks the user for a name and tries to create a new event with just that and the clicked event time. Falls back to a full form view if the quick creation fails.

Name of the record’s field holding the display name of the record. This field is used when creating records through the ‘quick create’ mechanism.

Id of the form view to open when the attribute quick_create is set and the user creates an event, instead of the default dialog which only allows to specify a name.

Reference of a form view. When set, in month scale, allows to create (and delete) records in batches by clicking on calendar cells or by selecting an area of calendar cells. One record is then created for each selected cell and for each active filter’s value, with the values of the multi create form view, which is displayed in the side panel. In delete mode, all records displayed in selected cells are deleted.

Note: if the side panel contains multiple filter sections, only the first one is used, to avoid a combinatorial explosion.

Disable/enable record creation on the view.

Disable/enable record edition on the view.

Disable/enable record deletion on the view.

The view title. It is displayed only if you open an action that has no name and whose target is new (opening a dialog).

Calendar views accept a single type of child elements: <field>. Those fields are displayed in a popover, in the given order, which opens when a record (a calendar event) is clicked.

Fields in the popover are readonly. If the edit action is available, an Edit button is displayed in the popover, to open a form view where fields can be edited.

Field nodes can have the following attributes:

The name of the field to render.

The widget used to represent the field. The selected widget can change the way the field is rendered and/or the way it can be edited. It refers to a Javascript implementation (an Owl component) registered to the fields registry.

Python expression indicating whether the field should be displayed or not. Other fields of the model can be used in the expression, as long as those fields are also declared here.

Python expression encoding an object of options for the field. In particular, the option icon, allowing to specify, as classnames, which icon to display in front of the field in the popover (for instance, fa fa-users). If no icon is given, the option string can be set, and its value is then used as label, displayed in front of the field.

Specifying a <field> in a calendar arch also allows to customize the filter side panel, by setting the filters attribute. Extra attributes are then available for that matter:

If set to true, the field can be used as filter, from the side panel of the calendar.

Only for relational fields. Specify the name of the field on the co-model to use to display as avatar in front of field values in the side panel filters.

Specify which field to use to colorize the checkbox in the side panel filters. By default, the color attribute of the calendar view is used to match events with values in the filters.

Allows to create new filter values on the fly. The given model is then used as model for those filters. To combine with write_field.

Combined with write_model, specifies the field name, in the given model, to use to encode the filter value.

Combined with write_model, specifies the field name, in the given model, to use to encode the status of the filter (whether it is checked or not).

The Activity view is used to display the activities linked to the records. The data are displayed in a chart with the records forming the rows and the activity types the columns. The first cell of each row displays a (customizable, see templates, quite similarly to Kanban) card representing the corresponding record. When clicking on others cells, a detailed description of all activities of the same type for the record is displayed.

The Activity view is only available when the mail module is installed, and for the models that inherit from the mail.activity.mixin.

The root element of the Activity view is <activity>, it accepts the following attributes:

A title, which should describe the view

Possible children of the view element are:

declares fields to use in activity logic. If the field is simply displayed in the activity view, it does not need to be pre-declared.

Possible attributes are:

the name of the field to fetch

defines the QWeb Templates templates. Cards definition may be split into multiple templates for clarity, but activity views must define at least one root template activity-box, which will be rendered once for each record.

The activity view uses mostly-standard javascript qweb and provides the following context variables (see Kanban for more details):

the current ActivityRecord(), can be used to fetch some meta-information. These methods are also available directly in the template context and don’t need to be accessed via widget

an object with all the requested fields as its attributes. Each field has two attributes value and raw_value

The cohort view is used to display and understand the way some data changes over a period of time. For example, imagine that for a given business, clients can subscribe to some service. The cohort view can then display the total number of subscriptions each month, and study the rate at which client leave the service (churn). When clicking on a cell, the cohort view will redirect you to a new action in which you will only see the records contained in the cell’s time interval; this action contains a list view and a form view.

By default the cohort view will use the same list and form views as those defined on the action. You can pass a list view and a form view to the context of the action in order to set/override the views that will be used (the context keys to use being form_view_id and list_view_id)

For example, here is a very simple cohort view:

The root element of the Cohort view is <cohort>, it accepts the following attributes:

A title, which should describe the view

A valid date or datetime field. This field is understood by the view as the beginning date of a record

A valid date or datetime field. This field is understood by the view as the end date of a record. This is the field that will determine the churn.

Set to 1 to prevent from redirecting clicks on cohort cells to list view.

A string to describe the mode. It should be either ‘churn’ or ‘retention’ (default). Churn mode will start at 0% and accumulate over time whereas retention will start at 100% and decrease over time.

A string to describe the timeline. It should be either ‘backward’ or ‘forward’ (default). Forward timeline will display data from date_start to date_stop, whereas backward timeline will display data from date_stop to date_start (when the date_start is in future / greater than date_stop).

A string to describe a time interval. It should be ‘day’, ‘week’, ‘month’’ (default) or ‘year’.

A field that can be aggregated. This field will be used to compute the values for each cell. If not set, the cohort view will count the number of occurrences.

allows to specify a particular field in order to manage it from the available measures, it’s main use is for hiding a field from the selectable measures:

the name of the field to use in the view.

the name that would be used to display the field in the cohort view, overrides the default python String attribute of the field.

if true, the field will not appear either in the active measures nor in the selectable measures (useful for fields that do not make sense aggregated, such as fields in different units, e.g. € and $). If the value is a domain, the domain is evaluated in the context of the current row’s record, if True the corresponding attribute is set on the cell.

alternate representations for a field’s display.

Whether the view should be populated with a set of sample records if none are found for the current model.

These fake records have heuristics for certain field names/models. For example, a field display_name on the model res.users will be populated with sample people names, while an email field will be in the form firstname.lastname@sample.demo.

The user is unable to interact with these data, and they will be discarded as soon as an action is performed (record created, column added, etc.).

This view is a work in progress and may have to be expanded or altered.

only date column fields have been tested, selection and many2one are nominally implemented and supported but have not been tested, datetime is not implemented at all.

column cells are hardly configurable and must be numerical

cell adjustment is disabled by default and must be configured to be enabled

create, edit and delete ACL metadata doesn’t get automatically set on the view root due to limitations in fields_view_get post-processing (there’s a fixed explicit list of the view types getting those attributes)

The grid view has its own schema and additional validation in this module. The view architecture is:

architecture root element

mandatory string attribute

optional create, edit and delete attributes

optional adjustment and adjust_name attributes

adjustment can be either object or action to indicate whether a cell’s adjustment should be performed through a method call or an action execution. adjust_name provides respectively the method name and the action id.

In both cases, the adjustment parameters are provided as a grid_adjust context member, in the object case, the parameters are also provided as positional function parameters (next to an empty list of ids):

the domain matching the entire row of the adjusted cell

the name of the column for the adjusted cell

the value of the column for the adjusted cell

the measure field of the adjusted cell

the difference between the old value of the cell and the adjusted one, may be positive or negative

optional hide_line_total and hide_column_total attributes

set to true to hide total line (default false)

set to true to hide total column (default false)

optional barchart_total attribute

set to true in order to display a bar chart at the bottom of the grid, based on the totals of the columns (default false).

optional create_inline and display_empty attributes

set to true in order to display an additional row at bottom of the grid with an Add a line button (default false). When this option is set to true, the Add a line button from the control panel is hidden. When no data is available and when display_empty is not set (so when the help content is displayed), the the Add a line button from the control panel is shown in order to let the user create a first record.

set to true in order to keep displaying the grid when there is no data (default false). This can be useful when you want the user to be able to keep track of the current period (as dates are displayed in the columns headers). As a reminder, when no data are present and when this attribute is no set, the help content is displayed instead of the grid.

Regular Odoo action buttons, displayed in the view header

mandatory string attribute (the button label)

mandatory type attribute, either object or action

workflow buttons are not supported

mandatory name attribute, either the name of the method to call, or the ID of the action to execute

The server callback is provided with all the record ids displayed in the view, either as the ids passed to the method (object button) or as the context’s active_ids (action buttons)

Row grouping fields, will be replaced by the search view’s groupby filter if any.

The order of row fields in the view provides their grouping depth: if the first field is school and the second is age the records will be grouped by school first and by age within each school.

Column grouping field.

The col field can contain 0+ <range> elements which specify customisable column ranges. range elements have the following mandatory attributes

can be used to override the default range (the first one by default) through the grid_range context value

the range button’s label (user-visible)

symbolic name of the span of all columns to display at once in the view, may trigger pagination.

For date fields, valid spans are currently week and month.

symbolic name of the step between one column and the previous/next

For date fields, the only valid span is currently day.

Cell field, automatically accumulated (by read_group).

The measure field can take a widget attribute to customise its display.

Aside from optional buttons, the grid view currently calls two methods:

read_grid (provided on all models by the module) returns almost the entirety of the grid’s content as a dict:

the row titles is a list of dictionaries with the following keys:

this maps to a dictionary with a key per row field, the values are always of the form [value, label].

the domain of any record at the source of this row, in case it’s necessary to copy a record during cell adjustment

the column titles is a list of dictionaries with at least one key:

see column domain value

boolean, marks/highlights a column

the grid data as a list (of rows) of list (of cells) of cell dicts each with the following keys:

the numeric value associated with the cell

the domain matching the cell’s records (should be assumed opaque)

the number of records grouped in the cell

a boolean indicating that this specific cell should not be client-editable

a list of classes (as strings) to add on the cell’s container (between the cell’s TD and the cell’s potentially-editable element).

In case of conflicts between this list and the base classes (prefixed with o_grid_cell_), the classes in this list are ignored.

Note that the grid data is dense, if querying the database yields no group matching a cell a cell will generate an “empty” cell with default values for required keys.

prev and next which can be either falsy (no pagination) or a context item to merge into the view’s own context to read_grid the previous or next page, it should be assumed to be opaque

read_grid_domain(field, range) (provided on al models by the module) returns the domain matching the current configured “span” of the grid. This is also done internally by read_grid, but can be useful or necessary to call independently to use with separate e.g. search_count or read_group.

adjust_grid, for which there currently isn’t a blanket implementation and whose semantics are likely to evolve with time and use cases

read_grid calls a number of hooks allowing the customisation of its operations from within without having to override the entire method:

converts the output of a read_group (group-by-group) into cells in the format described above (as part of “the grid data”)

generates an empty version of a cell (if there is no corresponding group)

generates a ColumnMetadata object based on the column type, storing values either returned directly (as part of read_grid) or used query and reformat read_group into read_grid:

the actual grouping field/query for the columns

domain to apply to read_group in case the column field is paginated, can be an empty list

context segments which will be sent to read_grid for pages before and after the current one. If False, disables pagination in that direction

column values to display on the “current page”, each value is a dictionary with the following keys:

dictionary mapping field names to values for the entire column, usually just name -> a value

domain matching this specific column

True if the current column should be specifically outlined in the grid, False otherwise

how to format the values of that column/type from read_group formatting to read_grid formatting (matching values in ColumnInfo)

if the view is not editable, individual cells won’t be editable

if the view is not creatable, the Add a Line button will not be displayed (it currently creates a new empty record)

selects which range should be used by default if the view has multiple ranges

if applicable, used as the default anchor of column ranges instead of whatever read_grid defines as its default.

For date fields, the reference date around which the initial span will be computed. The default date anchor is “today” (in the user’s timezone)

Gantt views appropriately display Gantt charts (for scheduling).

The root element of gantt views is <gantt/>, it has no children but can take the following attributes:

This view title is displayed only if you open an action that has no name and whose target is ‘new’ (opening a dialog)

Disable/enable record creation on the view.

Disable/enable record edition on the view.

Disable/enable record deletion on the view through the Action dropdown.

name of the field providing the start datetime of the event for each record.

name of the field providing the end duration of the event for each record.

name of the many2many field that provides the dependency relation between two records. If B depends on A, dependency_field is the field that allows getting A from B. Both this field and dependency_inverted_field field are used to draw dependency arrows between pills and reschedule them.

name of the many2many field that provides the invert dependency relation than dependency_field. If B depends on A, dependency_inverted_field is the field that allows getting B from A.

name of the field used to color the pills according to its value

python expression that evaluates to a bool

allow changing the style of a cell’s text based on the corresponding record’s attributes.

{$name} can be one of the following bootstrap contextual color (danger, info, secondary, success or warning).

Define a conditional display of a record in the style of a row’s text based on the corresponding record’s attributes.

Values are Python expressions. For each record, the expression is evaluated with the record’s attributes as context values and if true, the corresponding style is applied to the row. Here are some of the other values available in the context:

uid: the id of the current user,

today: the current local date as a string of the form YYYY-MM-DD,

now: same as today with the addition of the current time. This value is formatted as YYYY-MM-DD hh:mm:ss.

name of a field to group tasks by

if set to true, the gantt view will not have any drag&drop support

field name to display consolidation value in record cell

dictionary with the “group by” field as key and the maximum consolidation value that can be reached before displaying the cell in red (e.g. {"user_id": 100})

name of the field that describes if the task has to be excluded from the consolidation if set to true it displays a striped zone in the consolidation line

allows disabling the corresponding action in the view by setting the corresponding attribute to false (default: true).

create: If enabled, an Add button will be available in the control panel to create records.

cell_create: If enabled and create enabled, a “+” button will be displayed while hovering on a time slot cell to create a new record on that slot.

edit: If enabled, the opened records will be in edit mode (thus editable).

plan: If enabled and edit enabled, a “magnifying glass” button will be displayed on time slots to plan unassigned records into that time slot.

When you do not want to create records on the gantt view and the beginning and end dates are required on the model, the planning feature should be disabled because no record will ever be found.

Depending on the scale, the number of units to add to today to compute the default period. Examples: An offset of +1 in default_scale week will open the gantt view for next week, and an offset of -2 in default_scale month will open the gantt view of 2 months ago.

name of a field providing the completion percentage for the record’s event, between 0 and 100

title of the gantt view

JSON object specifying snapping precisions for the pills in each scale.

Possible values for scale day are (default: hour):

hour: records times snap to full hours (ex: 7:12 becomes 8:00)

hour:half: records times snap to half hours (ex: 7:12 becomes 7:30)

hour:quarter: records times snap to half hours (ex: 7:12 becomes 7:15)

Possible values for scale week are (default: day:half):

day: records times snap to full days (ex: 7:28 AM becomes 11:59:59 PM of the previous day, 10:32 PM becomes 12:00 PM of the current day)

day:half: records times snap to half hours (ex: 7:28 AM becomes 12:00 PM)

Possible values for scale month are (default: day:half):

day: records times snap to full days (ex: 7:28 AM becomes 11:59:59 PM of the previous day, 10:32 PM becomes 12:00 PM of the current day)

day:half: records times snap to half hours (ex: 7:28 AM becomes 12:00 PM)

Scale year always snap to full day.

Example of precision attribute: {"day": "hour:quarter", "week": "day:half", "month": "day"}

boolean to control whether the row containing the total count of records should be displayed. (default: false)

boolean to control whether it is possible to collapse each row if grouped by one field. (default: false, the collapse starts when grouping by two fields)

boolean to mark the dates returned by the gantt_unavailability function of the model as available inside the gantt view. Records can still be scheduled in them, but their unavailability is visually displayed. (default: false)

default scale when rendering the view. Possible values are (default: month):

comma-separated list of allowed scales for this view. By default, all scales are allowed. For possible scale values to use in this list, see default_scale.

defines the QWeb Templates template gantt-popover which is used when the user hovers over one of the records in the gantt view.

The gantt view uses mostly-standard javascript qweb and provides the following context variables:

the current GanttRow(), can be used to fetch some meta-information. The getColor method to convert in a color integer is also available directly in the template context without using widget.

If specified when clicking the add button on the view, instead of opening a generic dialog, launch a client action. this should hold the xmlid of the action (eg: on_create="%(my_module.my_wizard)d"

view to open when the user create or edit a record. Note that if this attribute is not set, the gantt view will fall back to the id of the form view in the current action, if any.

if set to true, the gantt view will start at the first record, instead of starting at the beginning of the year/month/day.

If set to true, the time appears in the pill label when the scale is set on week or month. (e.g. 7:00 AM - 11:00 AM (4h) - DST Task 1)

This allows to display a thumbnail next to groups name if the group is a relationnal field. This expects a python dict which keys are the name of the field on the active model. Values are the names of the field holding the thumbnail on the related model.

Example: tasks have a field user_id that reference res.users. The res.users model has a field image that holds the avatar, then:

will display the users avatars next to their names when grouped by user_id.

Whether the view should be populated with a set of sample records if none are found for the current model.

These fake records have heuristics for certain field names/models. For example, a field display_name on the model res.users will be populated with sample people names, while an email field will be in the form firstname.lastname@sample.demo.

The user is unable to interact with these data, and they will be discarded as soon as an action is performed (record created, column added, etc.).

This view is able to display records on a map and the routes between them. The records are represented by pins. It also allows the visualization of fields from the model in a popup tied to the record’s pin.

The model on which the view is applied should contain a res.partner many2one since the view relies on the res.partner’s address and coordinates fields to localize the records.

The view uses location data platforms’ API to fetch the tiles (the map’s background), do the geoforwarding (converting addresses to a set of coordinates) and fetch the routes. The view implements two API, OpenStreetMap and MapBox. OpenStreetMap is used by default and is able to fetch tiles and do geoforwarding. This API does not require a token. As soon as a valid MapBox token is provided in the general settings the view switches to the MapBox API. This API is faster and allows the computation of routes. A token can be obtained by signing up to MapBox.

The view’s root element is <map>. It can have the following attributes:

Contains the res.partner many2one. If not provided the view resorts to create an empty map.

If a field is provided the view overrides the model’s default order. The field must be part of the model on which the view is applied, not from res.partner.

if 1 display the routes between the records. The view needs a valid MapBox token and at least two located records (i.e the records have a res.partner many2one and the partner has an address or valid coordinates).

if 1 hide the name from the pin’s popup (default: 0).

if 1 hide the address from the pin’s popup (default: 0).

if 1 hide the title from the pin list (default: 0).

String to display as title of the pin list. If not provided, the title is the action’s name or “Items” if the view is not in an action.

Maximum number of records to fetch (default: 80). It must be a positive integer.

The <map> element can contain multiple <field> elements. Each <field> element is interpreted as a line in the pin’s popup. The field’s attributes are the following:

The field to display.

String to display before the field’s content. It can be used as a description.

**Examples:**

Example 1 (jsx):
```jsx
<field name="field_a" readonly="True"/>
<field name="field_b" invisible="context.get('show_me') and field_a == 4"/>
```

Example 2 (jsx):
```jsx
<field name="field_a"/>
<field name="x2m">
    <!-- sub-view -->
    <form>
        <field name="field_b" invisible="parent.field_a"/>
    </form>
</field>
```

Example 3 (typescript):
```typescript
<form>
    ...
</form>
```

Example 4 (jsx):
```jsx
<form>
    <field name="FIELD_NAME"/>
</form>
```

---

## Appraisal templates¶

**URL:** https://www.odoo.com/documentation/19.0/applications/hr/appraisals/appraisal_templates.html

**Contents:**
- Appraisal templates¶
- Modify appraisal templates¶
- Create appraisal templates¶

The Appraisals app uses a preconfigured default template, which is general enough to be applicable to all employees. If desired, the default template can be modified, or, if different templates are needed, such as department-specific appraisal templates, new templates can be created.

If needed, changes can be made to the default template. To view the default template, navigate to Appraisals app ‣ Configuration ‣ Appraisal Templates.

The default template appears in a list view, named Default Template. Click on the template to view the template details. Make any desired changes to the template.

The default template consists of the following questions:

What are my best achievement(s) since my last appraisal?

Describe something that made you proud, a piece of work positive for the company.

What has been the most challenging aspect of my work this past year and why?

Did you face new difficulties? Did you confront yourself to new obstacles?

What would I need to improve my work?

How can the company help you with your needs and objectives so you can reach your goals and foster better collaboration.

What are my short and long-term goals with the company, and for my career?

Give an example of a short-term objective (< 6 months)

Give an example of a long-term objective (> 6 months)

Which parts of my job do I most / least enjoy?

Every job has strong points - which tasks do you enjoy the most and the least?

How do I feel about the company…

Internal Communication:

How do I feel about my own…

Give one positive achievement that convinced you of the employee’s value.

Some achievements that illustrate their strengths in addressing job challenges.

How could the employee improve?

From a manager’s point of view, how could you help them overcome their weaknesses?

Short term (6-months) actions / decisions / objectives

Do you need a rapid response to the current situation?

Long term (> 6 months) career discussion, where does the employee want to go, how can you help them reach this path?

How do you see the employee’s future - does your vision align with their aspirations?

Large companies with many departments may prefer department-specific appraisal templates, instead of using a default universal template. Creating and using department-specific templates can be helpful if specific feedback is needed.

An appliance repair company has two main types of employees, office workers who handle administrative tasks and scheduling, and repair technicians who work in the field, performing repairs at customers’ homes.

This type of company may create two different appraisal templates, one for the office workers, and one for the on-site repair technicians.

To create a new appraisal template, click the New button in the upper-left corner. Next, configure the appraisal by adding questions to the template.

Additionally, a new appraisal template can be created by duplicating the default template, and making modifications to the copy. To duplicate the template, navigate to Appraisals app ‣ Configuration ‣ Appraisal Templates, then click on the template being duplicated. Click the (Actions) icon, then click Duplicate.

First, change the name of the template, then make any desired changes to the template.

Appraisal templates are housed in the Surveys app. Any appraisal template created in the Appraisals app must be configured to be used with the Appraisals app, or it will not be available for appraisals.

To ensure a new template is available for the Appraisals app, navigate to the Surveys app, and click on the appraisal template.

Tick the radio button next to Appraisal at the top of the survey. This setting allows the survey to be used in the Appraisals app, and is visible in the Appraisal Template drop-down menu.

---

## Quotation templates¶

**URL:** https://www.odoo.com/documentation/19.0/applications/sales/sales/sales_quotations/quote_template.html

**Contents:**
- Quotation templates¶
- Configuration¶
- Create quotation templates¶
  - Lines tab¶
  - Optional Products tab¶
  - Terms & Conditions tab¶
- Use quotation templates¶
- Mass cancel quotations/sales orders¶

Reusable quotation templates can be made in Odoo’s Sales app for common products or services.

By using these templates, quotations can be tailored and sent to customers at a quicker pace, without having to create new quotations from scratch every time a sales negotiation occurs.

To use quotation templates, begin by activating the setting in Sales app ‣ Configuration ‣ Settings, and scroll to the Quotations _Orders heading.

Under the heading, tick the Quotation Templates checkbox. Doing so reveals a new Default Template field, in which a default quotation template can be chosen from the drop-down menu.

Upon activating the Quotation Template feature, an internal Quotation Templates link appears beneath the Default Template field.

Clicking this link reveals the Quotation Templates page, from which templates can be created, viewed, and edited.

Before leaving the Settings page, do not forget to click the Save button to save all changes made during the session.

To create a quotation template, click the Quotation Templates link on the Settings page once Quotation templates are enabled, or navigate to Sales app ‣ Configuration ‣ Quotation Templates. Both options reveal the Quotation Templates page, where quotation templates can be created, viewed, and edited.

To create a new quotation template, click the New button, located in the upper-left corner. Doing so reveals a blank quotation template form that can be customized.

Start by entering a name for the template in the Quotation Template field.

Then, in the Quotation Validity field, designate how many days the quotation template will remain valid for, or leave the field on the default 0 to keep the template valid indefinitely.

Next, in the Confirmation Mail field, click the blank drop-down menu to select a preconfigured email template to be sent to customers upon confirmation of an order.

To create a new email template directly from the Confirmation Mail field, start typing the name of the new email template in the field, and select either: Create or Create and edit… from the drop-down menu that appears.

Selecting Create creates the email template, which can be edited later.

Selecting Create and edit… creates the email template, and a Create Confirmation Mail pop-up window appears, in which the email template can be customized and configured immediately.

When all modifications are complete, click Save & Close to save the email template and return to the quotation form.

If working in a multi-company environment, use the Company field to designate to which company this quotation template applies.

If a journal is set in the Invoicing Journal field, all sales orders with this template will invoice in that specified journal. If no journal is set in this field, the sales journal with the lowest sequence is used.

If the Online Signature and/or Online Payment features are activated in the Settings (Sales app ‣ Configuration ‣ Settings), those options are available on quotation template forms.

Check the box beside Online Signature to request an online signature from the customer to confirm an order.

Check the box beside Online Payment to request an online payment from the customer to confirm an order. When Online Payment is checked, a new percentage field appears, in which a specific percentage of payment can be entered.

Both options, Online Signature and Online Payment can be enabled simultaneously, in which case the customer must provide both a signature and a payment to confirm an order.

In the Lines tab, products can be added to the quotation template by clicking Add a product, organized by clicking Add a section (and dragging/dropping section headers), and further explained with discretionary information (such as warranty details, terms, etc.) by clicking Add a note.

To add a product to a quotation template, click Add a product in the Lines tab of a quotation template form. Doing so reveals a blank field in the Product column.

When clicked, a drop-down menu with existing products in the database appears. Select the desired product from the drop-down menu to add it to the quotation template.

If the desired product is not readily visible, type the name of the desired product in the Product field, and the option appears in the drop-down menu. Products can also be found by clicking Search More… from the drop-down menu.

It is possible to add event-related products (booths and registrations) to quotation templates. To do so, click the Product field, type in Event, and select the desired event-related product from the resulting drop-down menu.

When a product is added to a quotation template, the default Quantity is 1, but that can be edited at any time.

Then, drag and drop the product to the desired position, via the six squares icon, located to the left of each line item.

To add a section, which serves as a header to organize the lines of a sales order, click Add a section in the Lines tab. When clicked, a blank field appears, in which the desired name of the section can be typed. When the name has been entered, click away to secure the section name.

Then, drag and drop the section name to the desired position, via the (six squares) icon, located to the left of each line item.

To add a note, which appears as a piece of text for the customer on the quotation, click Add a note in the Lines tab. When clicked, a blank field appears, in which the desired note can be typed. When the note has been entered, click away to secure the note.

Then, drag and drop the note to the desired position, via the (six squares) icon.

To delete any line item from the Lines tab (product, section, and/or note), click the (remove record) icon on the far-right side of the line.

Using optional products is a marketing strategy that involves the cross-selling of products along with a core product. The aim is to offer useful and related products to customers, which may result in an increased sale.

If a customer wants to buy a car, they have the choice to order massaging seats as an additional product that compliments the car, or ignore the offer and buy the car alone.

Optional products appear as a section on the bottom of sales orders and eCommerce pages. Customers can immediately add them to their online sales orders themselves, if desired.

In the Optional Products tab, Add a line for each cross-selling product related to the original items in the Lines tab, if applicable.

Clicking Add a line reveals a blank field in the Product column.

When clicked, a drop-down menu with products from the database appear. Select the desired product from the drop-down menu to add it as an optional product to the quotation template.

To delete any line item from the Optional Products tab, click the (remove record) icon.

Optional products are not required to create a quotation template.

The Terms & Conditions tab provides the opportunity to add terms and conditions to the quotation template. To add terms and conditions, type the desired terms and conditions in this tab.

Default terms and conditions (T&C)

Terms and conditions are not required to create a quotation template.

When creating a quotation (Sales app ‣ New), choose a preconfigured template in the Quotation Template field.

The order of the templates in the Quotation Template field is determined by the order of the templates in the Quotation Templates form. The order of the quotations in the Quotation Templates form does not affect anything else.

To view what the customer will see, click the Preview button at the top of the page to see how the quotation template appears on the front-end of the website through Odoo’s customer portal.

When all blocks and customizations are complete, click the Save button to save the configuration.

The blue banner located at the top of the quotation template preview can be used to quickly return Back to edit mode. When clicked, Odoo returns to the quotation form in the back-end of the Sales application.

Cancel multiple quotations (or sales orders) by navigating to the Sales app ‣ Orders ‣ Quotations dashboard, landing, by default, in the list view. Then, on the left side of the table, tick the checkboxes for the quotations to be canceled.

Select all records in the table by selecting the checkbox column header at the top-left of the table; the total number of selected items are displayed at the top of the page.

Then, with the desired quotations (or sales orders) selected from the list view on the Quotations page, click the Actions button to reveal a drop-down menu.

From this drop-down menu, select Cancel quotations.

This action can be performed for quotations in any stage, even if it is confirmed as a sales order.

Upon selecting the Cancel quotations option, a Cancel quotations confirmation pop-up window appears. To complete the cancellation, click the Cancel quotations button.

An error pop-up message appears when attempting to cancel an order for an ongoing subscription that has an invoice.

Online signatures for order confirmations

Online payment order confirmation

---

## UI icons¶

**URL:** https://www.odoo.com/documentation/19.0/developer/reference/user_interface/icons.html

**Contents:**
- UI icons¶
- Odoo UI icons¶
  - RTL adaptations¶
- Odoo Spreadsheet icons¶

Odoo’s user interface mostly relies on FontAwesome4 icons.

To cover FontAwesome’s lack of iconography for specific functionalities, we designed our own icons: Odoo UI icons and Odoo Spreadsheet icons.

The Odoo UI icons can be rendered using the main oi class in conjunction with the specific icon class.

Directional icons have RTL adaptations which flip the icons by 180 degrees when an RTL language is selected.

The Odoo Spreadsheet icons are defined as <svg> elements and rendered using QWeb templates.

**Examples:**

Example 1 (jsx):
```jsx
<i class="oi oi-odoo"/>
```

Example 2 (jsx):
```jsx
<t t-name="o-spreadsheet-Icon.GLOBAL_FILTERS">
    <svg width="20" height="20" viewbox="0 0 20 20">
        <path fill="currentColor" d="M1 3h12L7 9M5.5 6h3v11l-3-3M14 4h4v2h-4m-3 3h7v2h-7m0 3h7v2h-7"/>
    </svg>
</t>
```

---

## Campaign templates¶

**URL:** https://www.odoo.com/documentation/19.0/applications/marketing/marketing_automation/campaign_templates.html

**Contents:**
- Campaign templates¶

---

## Create quotations¶

**URL:** https://www.odoo.com/documentation/19.0/applications/sales/sales/sales_quotations/create_quotations.html

**Contents:**
- Create quotations¶
- Quotation settings¶
- Quotations dashboard¶
- Create quotation¶
  - Order Lines tab¶
  - Optional Products tab¶
  - Other Info tab¶
    - Sales section¶
    - Delivery section¶
    - Invoicing section¶

In Odoo Sales, quotations can be created and sent to customers. Once a quotation has been confirmed, it officially turns into a sales order, which can then be invoiced and paid for.

To access these setting options, navigate to Sales app ‣ Configuration ‣ Settings, and scroll to the Quotations & Orders section.

Quotation Templates: Enable this option to create quotation templates featuring standard product offers, which are then selectable on quotation forms. When this checkbox is ticked, an additional field, Default Template, appears, along with a link to the Quotation Templates page.

Online Signature: Request an online signature to confirm orders.

Online Payment: Request an online prepayment from customers to confirm orders. Request a full or partial payment (via down payment). When this checkbox is ticked, an additional field, Prepayment amount (%), appears. There is also a link to the Payment Providers page.

Default Quotation Validity: Determine a set amount (in days) that quotations can remain valid for.

Default Recurrence: Select a default period from the drop-down menu to use as a recurrence period for a new quotation.

Sale Warnings: Get warning messages about orders that include specific products or customers.

PDF Quote builder: Customize the look of quotations with header pages, product descriptions, footer pages, and more.

Lock Confirmed Sales: Ensure no further edits can be made to confirmed orders.

Pro-Forma Invoice: Send pro-forma invoices to customers.

To activate any of these settings, tick the checkbox beside the desired option(s). Then, click Save.

The Quotations dashboard is the page that appears when the Sales app is opened.

By default, the Quotations dashboard displays all quotations in the database related to the current user, as indicated by the default My Quotations filter present in the search bar.

To view all quotations in the database, remove the My Quotations filter from the search bar.

Quotations on this page appear in a default list view, but can also be viewed in a Kanban view, Calendar, Pivot table, Graph, or Activity view.

To view and/or modify any listed quotation from the Quotations dashboard, click on the desired quotation line from the list, and Odoo reveals the specific form for that selected quotation.

To create a quotation, open the Sales app, and click the New button, located in the upper-left corner of the main Quotations dashboard.

The New button is only present if the Quotations dashboard is in list or Kanban view.

Clicking the New button reveals a blank quotation form, with various fields and tabs to configure.

Begin by entering the customer’s name in the Customer field at the top of the form. This is a required field.

If the customer’s information is already in the database, the Invoice Address and Delivery Address fields auto-populate with the saved information for those respective fields, based on the data from that customer’s contact record (found in the Contacts application).

If the customer was referred by another customer or contact, enter their name in the Referrer field.

If a Referrer is selected, a new field, Commission Plan appears, in which a commission can be selected from the drop-down menu. This commission is rewarded to the contact selected in the Referrer field.

Next, if they have not already been auto-populated with the customer’s information, enter the appropriate addresses in the Invoice Address and Delivery Address fields. Both of these fields are required.

Then, if desired, choose a Quotation Template from the drop-down field to apply to this quotation. It should be noted that some additional fields may appear, depending on the template selected.

The default date that appears in the Expiration field is based on the number configured in the Default Quotation Validity setting (in Sales app ‣ Configuration ‣ Settings).

When using a quotation template, the date in the Expiration field is based off the Quotation Validity figure on the template form.

If the quotation is for a recurring product or subscription, select the desired Recurring Plan from that specific drop-down menu.

If desired, select a specific Pricelist to be applied to this quotation.

Lastly, select any specific Payment Terms to be used for this quotation.

The first tab on the quotation form is the Order Lines tab.

In this tab, select products, and quantities of those products, to add them to the quotation.

There are two ways to add products to the quotation from this tab.

Click Add a product, select the desired item from the Product drop-down field, and proceed to adjust the quantity of that selected product, if necessary.

Or, click Catalog to reveal a separate page, showcasing every item (and every potential product variant) in an organized catalog display, with items organizable by Product Category and Attributes.

From here, simply locate the desired items, click the Add button on the product card, and adjust the quantity, if needed. When complete, click the Back to Quotation button in the upper-left corner to return to the quotation, where the newly-selected catalog items can be found in the Order Lines tab.

If multiple items should be presented in a more organized way on the quotation, click Add a section, enter a name for the section, and drag-and-drop that section heading in the desired location amongst the items in the Order Lines tab. The section heading appears in bold and a sub-total for all products in a section is displayed.

If needed, click Add a note beneath a certain product line to add a custom note about that specific product. The note appears in italics. Then, if needed, proceed to drag-and-drop the note beneath the desired product line.

Beneath the product lines, there are buttons that can be clicked to apply any of the following: Coupon Code, Promotions, Discount, and/or Add shipping.

Use eWallets and gift cards

Discount and loyalty programs

Open the Optional Products tab to select related products that can be presented to the customer, which may result in an increased sale.

For example, if the customer wants to buy a car, an optional product that could be offered is a Trailer Hitch.

In the Other Info tab, there are various quotation-related configurations separated into four different sections: Sales, Delivery, Invoicing, and Tracking.

Some fields only appear if specific settings and options have been configured.

In the Sales section of the Other Info tab, there are sales specific fields that can be configured.

Salesperson: Assign a salesperson from the drop-down menu to be associated with this quotation. The user who originally created the quotation is selected in this field, by default.

Sales Team: Assign a specific sales team to this quotation. If the selected Salesperson is a member of a sales team, that team is auto-populated in the field.

Company: Select a company from the drop-down menu this quotation should be associated with. This field only appears when working in a multi-company environment.

Online signature: Tick this checkbox to request an online signature from the customer to confirm the order. This field only appears if the Online Signature setting has been enabled.

Online payment: Tick this checkbox, and enter a desired percentage in the adjacent field, to request an online payment from the customer (for that designated percentage of the total amount) to confirm the order. This field only appears if the Online Payment setting has been enabled.

Customer Reference: Enter a custom reference ID for this customer. The entered reference ID can contain letters, numbers, or a mix of both.

Tags: Add specific tags to the quotation for added organization and enhanced searchability in the Odoo Sales application. Multiple tags can be added, if necessary.

In the Delivery section of the Other Info tab, there are delivery-specific fields that can be configured.

Shipping Weight: Displays the weight of the items being shipped. This field is not modifiable. Product weight is configured on individual product forms.

Incoterm: Select an Incoterm (International Commerical Term) to use as predefined commerical terms for international transactions.

Incoterm Location: If an Incoterm is being used, enter the international location in this field.

Shipping Policy: Select a desired shipping policy from the drop-down menu. If all products are delivered at once, the delivery order is scheduled, based on the greatest product lead time. Otherwise, it is based on the shortest lead time. The available options are: As soon as possible or When all products are ready.

Delivery Date: Click into the empty field to reveal a calendar popover, from which a customer delivery date can be selected. If no custom date is required, refer to the Expected date listed to the right of that field.

In the Invoicing section of the Other Info tab, there are invoicing specific fields that can be configured.

Fiscal Position: Select a fiscal position to be used to adapt taxes and accounts for particular customers or sales orders/invoices. The default value comes from the customer. If a selection is made in this field, an Update Taxes clickable link and icon appear. When clicked, the taxes for this partiuclar customer and quotation are updated. A confirmation window appears, as well.

Analytic Account: Select an analytic account to apply to this customer/quotation.

In the Tracking section of the Other Info tab, there are tracking specific fields that can be configured.

Source Document: Enter the reference of the document that generated the quotation/sales order, if applicable.

Opportunity: Select the specific opportunity (from the CRM app) related to this quotation, if applicable.

Campaign: Select the marketing campaign related to this quotation, if applicable.

Medium: Select the method by which this quotation originated (e.g. Email), if applicable.

Source: Select the source of the link used to generate this quotation (e.g. Facebook), if applicable.

In the Notes tab of the quotation form, enter any specific internal notes about the quotation and/or customer, if desired.

Once all the necessary fields and tabs have been configured, it is time to send the quotation to the customer for confirmation. Upon confirmation, the quotation turns into an official sales order.

At the top of the form, there is a series of buttons:

Send by Email: When clicked, a pop-up window appears with the customer’s name and email address in the Recipients field, the quotation (and reference ID) in the Subject field, and a brief default message in the body of the email, which can be modified, if needed.

Below that, a PDF copy of the quotation is attached. When ready, click Send to send the quotation, via email, to the customer, so they can review and confirm it.

Send PRO-FORMA Invoice: This button only appears if the Pro-Forma Invoice setting has been enabled. When clicked, a pop-up window appears with the customer’s name and email address in the Recipients field, the Proforma invoice (and reference ID) in the Subject field, and a brief default message in the body of the email, which can be modified, if needed.

Below that, a PDF copy of the quotation is attached. When ready, click Send to send the quotation, via email, to the customer, so they can review and confirm it.

Confirm: When clicked, the quotation is confirmed, and the status changes to Sales Order.

Preview: When clicked, Odoo reveals a preview of the quotation the customer sees when they log into their customer portal. Click the Back to edit mode link at the top of the preview page, in the blue banner, to return to the quotation form.

Cancel: When clicked, the quotation is canceled.

If the Lock Confirmed Sales setting is enabled, the sales order becomes Locked, and is indicated as such on the sales order form.

At this point, the quotation has been confirmed, turned into a sales order, and is now ready to be invoiced and paid for.

For more information about invoicing, refer to the Invoice based on delivered or ordered quantities

Online signatures for order confirmations

Online payment order confirmation

---

## Schedule appraisals¶

**URL:** https://www.odoo.com/documentation/19.0/applications/hr/appraisals/schedule_appraisals.html

**Contents:**
- Schedule appraisals¶
- Automatic scheduling¶
  - Appraisals plans¶
  - Appraisals automation¶
- Manually schedule an appraisal¶

Odoo’s Appraisals app helps managers run recurring performance reviews. Each review can include a self-assessment and can follow any schedule the company sets.

Regular appraisals turn day-to-day work into clear goals and measurable skill targets. They also supply the objective evidence HR needs for raises or promotions, and keep individual performance aligned with company KPIs.

Reviews can be scheduled automatically through an appraisal plan that triggers evaluations at set intervals, or created manually whenever needed—such as before a promotion or department transfer.

To ensure no appraisal is missed, enable automatic scheduling by going to Appraisals app ‣ Configuration ‣ Settings.

The Appraisals Plan settings determines the frequency that appraisals are scheduled.

By default, appraisals are preconfigured to be automatically created six months after an employee is hired, with a second appraisal exactly six months after that.

Once those two initial appraisals have been completed in the employee’s first year, following appraisals are only created once a year (every twelve months).

To modify this schedule, change the number of months in the blank fields under the Appraisals Plans section.

Changing the Appraisals Plans field updates every employee record whose Next Appraisal Dates is empty.

Tick the checkbox next to Appraisals Automation to have Odoo automatically schedule and confirm appraisals.

Appraisals are scheduled according to the appraisal plan.

Managers can schedule an appraisal at any time, outside the regular cycle.

For example, if an employee is promoted, or transfers to a new role or a new department, an appraisal is scheduled to assess performance in the current role.

To create a new appraisal, open the Appraisals app, and click the New button in the upper-left corner. This opens a blank Appraisals form.

First, using the drop-down menu, select the employee being evaluated, in the first field on the form. Once an employee is selected, the employee’s Manager, Job Position, and Department fields are populated according to the information on the employee record.

The current date populates the Appraisal Date field, which is the date the appraisal is scheduled to be completed. Using the calendar selector, adjust the date, if desired. This field is typically updated when the manager submits their final rating at the end of the appraisal process.

If there is an appraisal plan configured, the Next Appraisal Date field displays Ongoing. This indicates that the following appraisal will be scheduled according to the appraisal schedule. Once the appraisal is marked as complete, the Next Appraisal Date is updated with the date of the next appraisal.

Last, select the desired Appraisal Template. The Default Template populates this field, by default, and is created when the Appraisals app is installed. Using the drop-down menu, select a different template, if desired.

Once the information in the top-half of the Appraisals form is complete, click the Confirm button in the upper-left corner, and the appraisal is scheduled, and the employee is notified.

Once the appraisal is confirmed, both the employee and manager can start to fill out the appraisal.

---

## Owl components¶

**URL:** https://www.odoo.com/documentation/19.0/developer/reference/frontend/owl_components.html

**Contents:**
- Owl components¶
- Using Owl components¶
- Best practices¶
- Reference List¶
  - ActionSwiper¶
    - Location¶
    - Description¶
    - Props¶
    - Example: Extending existing components¶
  - CheckBox¶

The Odoo Javascript framework uses a custom component framework called Owl. It is a declarative component system, loosely inspired by Vue and React. Components are defined using QWeb templates, enriched with some Owl specific directives. The official Owl documentation contains a complete reference and a tutorial.

Although the code can be found in the web module, it is maintained from a separate GitHub repository. Any modification to Owl should therefore be made through a pull request on https://github.com/odoo/owl.

Currently, all Odoo versions (starting in version 14) share the same Owl version.

The Owl documentation already documents in detail the Owl framework, so this page will only provide Odoo specific information. But first, let us see how we can make a simple component in Odoo.

This example shows that Owl is available as a library in the global namespace as owl: it can simply be used like most libraries in Odoo. Note that we defined here the template as a static property, but without using the static keyword, which is not available in some browsers (Odoo javascript code should be Ecmascript 2019 compliant).

We define here the template in the javascript code, with the help of the xml helper. However, it is only useful to get started. In practice, templates in Odoo should be defined in an xml file, so they can be translated. In that case, the component should only define the template name.

In practice, most components should define 2 or 3 files, located at the same place: a javascript file (my_component.js), a template file (my_component.xml) and optionally a scss (or css) file (my_component.scss). These files should then be added to some assets bundle. The web framework will take care of loading the javascript/css files, and loading the templates into Owl.

Here is how the component above should be defined:

And the template is now located in the corresponding xml file:

Template names should follow the convention addon_name.ComponentName.

First of all, components are classes, so they have a constructor. But constructors are special methods in javascript that are not overridable in any way. Since this is an occasionally useful pattern in Odoo, we need to make sure that no component in Odoo directly uses the constructor method. Instead, components should use the setup method:

Another good practice is to use a consistent convention for template names: addon_name.ComponentName. This prevents name collision between odoo addons.

The Odoo web client is built with Owl components. To make it easier, the Odoo javascript framework provides a suite of generic components that can be reused in some common situations, such as dropdowns, checkboxes or datepickers. This page explains how to use these generic components.

a swiper component to perform actions on touch swipe

a simple checkbox component with a label next to it

a list of colors to choose from

full-featured dropdown

a component to navigate between pages using tabs

a small component to handle pagination

a dropdown component to choose between different options

a list of tags displayed in rounded pills

@web/core/action_swiper/action_swiper

This is a component that can perform actions when an element is swiped horizontally. The swiper is wrapping a target element to add actions to it. The action is executed once the user has released the swiper passed a portion of its width.

The simplest way to use the component is to use it around your target element directly in an xml template as shown above. But sometimes, you may want to extend an existing element and would not want to duplicate the template. It is possible to do just that.

If you want to extend the behavior of an existing element, you must place the element inside, by wrapping it directly. Also, you can conditionnally add props to manage when the element might be swipable, its animation and the minimum portion to swipe to perform the action.

You can use the component to interact easily with records, messages, items in lists and much more.

The following example creates a basic ActionSwiper component. Here, the swipe is enabled in both directions.

Actions are permuted when using right-to-left (RTL) languages.

optional boolean to determine if a translate effect is present during the swipe

optional animation that is used after the swipe ends (bounce or forwards)

if present, the actionswiper can be swiped to the left

if present, the actionswiper can be swiped to the right

optional minimum width ratio that must be swiped to perform the action

You can use both onLeftSwipe and onRightSwipe props at the same time.

The Object’s used for the left/right swipe must contain:

action, which is the callable Function serving as a callback. Once the swipe has been completed in the given direction, that action is performed.

icon is the icon class to use, usually to represent the action. It must be a string.

bgColor is the background color, given to decorate the action. can be one of the following bootstrap contextual color (danger, info, secondary, success or warning).

Those values must be given to define the behavior and the visual aspect of the swiper.

In the following example, you can use xpath’s to wrap an existing element in the ActionSwiper component. Here, a swiper has been added to mark a message as read in mail.

@web/core/checkbox/checkbox

This is a simple checkbox component with a label next to it. The checkbox is linked to the label: the checkbox is toggled whenever the label is clicked.

if true, the checkbox is checked, otherwise it is unchecked

if true, the checkbox is disabled, otherwise it is enabled

@web/core/colorlist/colorlist

The ColorList let you choose a color from a predefined list. By default, the component displays the current selected color, and is not expandable until the canToggle props is present. Different props can change its behavior, to always expand the list, or make it act as a toggler once it is clicked, to display the list of available colors until a choice is selected.

optional. Whether the colorlist can expand the list on click

list of colors to display in the component. Each color has a unique id

optional. If true, the list is always expanded

optional. If true, the list is expanded by default

callback executed once a color is selected

optional. The color id that is selected

Color id’s are the following:

@web/core/dropdown/dropdown and @web/core/dropdown/dropdown_item

The Dropdown lets you show a menu with a list of items when a toggle is clicked on. They can be combined with DropdownItems to invoke callbacks and close the menu when items are selected.

Dropdowns are surprisingly complicated components, the list of features they provide is as follow:

Toggle the item list on click

Close on outside click

Call a function when items are selected

Optionally close the item list when an item is selected

SIY: style it yourself

Support sub dropdowns, up to any level

Configurable hotkey to open/close a dropdown or select a dropdown item

Keyboard navigation (arrows, tab, shift+tab, home, end, enter and escape)

Reposition itself whenever the page scrolls or is resized

Smartly chose the direction it should open (right-to-left direction is automatically handled).

Direct siblings dropdowns: when one is open, toggle others on hover

To properly use a <Dropdown/> component, you need to populate two OWL slots :

default slot: it contains the toggle elements of your dropdown. By default, click events will be attached to this element to open and close the dropdown.

content slot: it contains the elements of the dropdown menu itself and is rendered inside a popover. Although it is not mandatory, you can put some DropdownItem inside this slot, the dropdown will automatically close when these items are selected.

Optional classname added to the dropdown’s menu

Optional, if true, disables the dropdown so the user is not able to open it anymore. (default: false)

Optional list of items to be displayed as DropdownItems inside the dropdown’s menu

Optionally defines the desired menu opening position. RTL direction is automatically applied. Should be a valid usePosition hook position. (default: bottom-start)

Optional function called just before opening. May be asynchronous.

Optional function called just after opening.

Optional function called after opening or closing (gives a boolean as single argument that represents whether the dropdown is open or not).

Optional object with open(), close() and isOpen properties to manually control when the dropdown opens and closes.

Optional, when true, the Dropdown component will not add click event listeners to the toggler. This allows for more control as when to open the dropdown. (This should be used in tandem with the state prop)

Optionally overrides the navigation options of the dropdown, (see web/core/navigation/navigation).

Optional, if true, keeps the Dropdown’s menu at the same position while the mouse is hovering it, creating a better UX when the menu’s content changes.

Optional, allows to get a ref of the dropdown’s menu, (expects a function returned from useChildRef)

Optional value added to the root span classname (supports both strings and OWL classname object notation).

Optional function called when the dropdown item is selected.

"none" | "closest" | "all"

Optional, controls which parent dropdown should close when the item is selected: none: the dropdown will not close, closest: the direct parent will close, all: every nested parent dropdown will close (default: all)

Optional object representing attributes that are added to the root element. <DropdownItem attrs="{ title: 'A tooltip', 'data-hotkey': 'shift+a' }">. (If href is set, the element will automatically become an a element).

When writing custom css for you components, do not forget that the menu elements are not next to the toggle but inside the overlay container, at the bottom of the document. Thus, use the menuClass and class props to more easily write your selectors. (This DOM magic let us avoid lots of z-index issues.)

Dropdown can be nested, to do this simply put new Dropdown components inside other dropdown’s content slot. When the parent dropdown is open, child dropdowns will open automatically on hover.

By default, selecting a DropdownItem will close the whole Dropdown tree.

This example shows how one could make a nested File dropdown menu, with submenus for the New sub elements.

In the example bellow, we recursively call a template to display a tree-like structure.

If needed, you can also open or close the dropdown using code. To do this you must use the useDropdownState hook along with the state prop. useDropdownState returns an object that has an open and a close method (as well as an isOpen getter). Give the object to the state prop of the dropdown you want to control and calling the respective functions should now open and close your dropdown.

You can also set manual to true if you don’t want the default click handlers to be added on the toggle.

The following example shows a dropdown that opens automatically when mounted and only has a 50% chance of closing when clicking on the button inside.

Location: @web/core/dropdown/dropdown_group

You can use the DropdownGroup component to make Dropdowns share a common group, this means that when one of these Dropdown is open, the others will automatically open themselves on mouse hover, without the need for a click.

To do this, either surround all the Dropdowns with a single DropdownGroup or surround them with DropdownGroups with the same group key.

In the example bellow, all dropdown in the snippet bellow will share the same group:

Whereas in the following snippet, only the first, second and fourth dropdown share the same group:

@web/core/notebook/notebook

The Notebook is made to display multiple pages in a tabbed interface. Tabs can be located at the top of the element to display horizontally, or at the left for a vertical layout.

There are two ways to define your Notebook pages to instanciate, either by using slot’s, or by passing a dedicated props.

A page can be disabled with the isDisabled attribute, set directly on the slot node, or in the page declaration, if the Notebook is used with the pages given as props. Once disabled, the corresponding tab is greyed out and set as inactive as well.

optional. Allow anchors navigation to elements inside tabs that are not visible.

optional. Classname set on the root of the component.

optional. Page id to display by default.

optional. List of icons used in the tabs.

optional. Whether tabs direction is horizontal or vertical.

optional. Callback executed once the page has changed.

optional. Contain the list of page’s to populate from a template.

The first approach is to set the pages in the slots of the component.

The other way to define your pages is by passing the props. This can be useful if some pages share the same structure. Create first a component for each page template that you may use.

Both examples are shown here:

@web/core/pager/pager

The Pager is a small component to handle pagination. A page is defined by an offset and a limit (the size of the page). It displays the current page and the total number of elements, for instance, “9-12 / 20”. In the previous example, offset is 8, limit is 4 and total is 20. It has two buttons (“Previous” and “Next”) to navigate between pages.

The pager can be used anywhere but its main use is in the control panel. See the usePager hook in order to manipulate the pager of the control panel.

Index of the first element of the page. It starts with 0 but the pager displays offset + 1.

Size of the page. The sum of offset and limit corresponds to the index of the last element of the page.

Total number of elements the page can reach.

Function that is called when page is modified by the pager. This function can be async, the pager cannot be edited while this function is executing.

Allows to click on the current page to edit it (true by default).

Binds access key p on the previous page button and n on the next page one (true by default).

@web/core/select_menu/select_menu

This component can be used when you want to do more than using the native select element. You can define your own option template, allowing to search between your options, or group them in subsections.

Prefer the native HTML select element, as it provides by default accessibility features, and has a better user interface on mobile devices. This component is designed to be used for more complex use cases, to overcome limitations of the native element.

optional. List of choice’s to display in the dropdown.

optional. Classname set on the root of the SelectMenu component.

optional. List of group’s, containing choices to display in the dropdown.

optional. Enable multiple selections. When multiple selection is enabled, selected values are displayed as tag’s in the SelectMenu input.

optional. classname set on the toggler button.

optional. Whether the selected value can be unselected.

optional. Whether a search box is visible in the dropdown.

optional. Text displayed as the search box placeholder.

optional. Current selected value. It can be from any kind of type.

optional. Callback executed when an option is chosen.

The shape of a choice is the following:

value is actual value of the choice. It is usually a technical string, but can be from any type.

label is the displayed text associated with the option. This one is usually a more friendly and translated string.

The shape of a group is the following:

choices is the list of choice’s to display for this group.

label is the displayed text associated with the group. This is a string displayed at the top of the group.

In the following example, the SelectMenu will display four choices. One of them is displayed on top of the options, since no groups are associated with it, but the other ones are separated by the label of their group.

You can also customize the appearance of the toggler and set a custom template for the choices, using the appropriate component slot’s.

When SelectMenu is used with multiple selection, the value props must be an Array containing the values of the selected choices.

For more advanced use cases, you can customize the bottom area of the dropdown, using the bottomArea slot. Here, we choose to display a button with the corresponding value set in the search input.

@web/core/tags_list/tags_list

This component can display a list of tags in rounded pills. Those tags can either simply list a few values, or can be editable, allowing the removal of items. It can be possible to limit the number of displayed items using the itemsVisible props. If the list is longer than this limit, the number of additional items is shown in a circle next to the last tag.

optional. Whether the tag is displayed as a badge.

optional. Whether the tag is displayed with a text or not.

optional. Limit of visible tags in the list.

list of tag’s elements given to the component.

The shape of a tag is the following:

colorIndex is an optional color id.

icon is an optional icon displayed just before the displayed text.

id is a unique identifier for the tag.

img is an optional image displayed in a circle, just before the displayed text.

onClick is an optional callback that can be given to the element. This allows the parent element to handle any functionality depending on the tag clicked.

onDelete is an optional callback that can be given to the element. This makes the removal of the item from the list of tags possible, and must be handled by the parent element.

text is the displayed string associated with the tag.

In the next example, a TagsList component is used to display multiple tags. It’s at the developer to handle from the parent what would happen when the tag is pressed, or when the delete button is clicked.

Depending the attributes given to each tag, their appearance and behavior will differ.

**Examples:**

Example 1 (jsx):
```jsx
import { Component, xml, useState } from "@odoo/owl";

class MyComponent extends Component {
    static template = xml`
        <div t-on-click="increment">
            <t t-esc="state.value">
        </div>
    `;

    setup() {
        this.state = useState({ value: 1 });
    }

    increment() {
        this.state.value++;
    }
}
```

Example 2 (gdscript):
```gdscript
import { Component, useState } from "@odoo/owl";

class MyComponent extends Component {
    static template = 'myaddon.MyComponent';

    ...
}
```

Example 3 (xml):
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<templates xml:space="preserve">

<t t-name="myaddon.MyComponent">
  <div t-on-click="increment">
    <t t-esc="state.value"/>
  </div>
</t>

</templates>
```

Example 4 (vue):
```vue
// correct:
class MyComponent extends Component {
    setup() {
        // initialize component here
    }
}

// incorrect. Do not do that!
class IncorrectComponent extends Component {
    constructor(parent, props) {
        // initialize component here
    }
}
```

---

## Purchase templates¶

**URL:** https://www.odoo.com/documentation/19.0/applications/inventory_and_mrp/purchase/manage_deals/purchase_templates.html

**Contents:**
- Purchase templates¶
- Configuration¶
- Create a new template¶
  - Create a new RFQ from a purchase template¶

Purchase templates are an agreement type that allow for the repeated creation of requests for quotations (RFQs) for recurring purchases. Products can then be added and quantities can be changed, as needed. Purchase templates can be used for multiple vendors, saving time and simplifying the RFQ process.

Purchase templates differ from blanket orders in that a blanket order is a large order split into several deliveries, therefore all RFQs must be for the same vendor. Purchase templates can be replicated for multiple vendors, and can copy over quantities, which is useful when placing frequent orders.

First, navigate to Purchase app ‣ Configuration ‣ Settings. Under the Orders section, tick the Purchase Agreements checkbox. Click Save to save the changes.

Navigate Purchase app ‣ Orders ‣ Purchase Agreements and click New.

Select a Vendor from the drop-down list.

To make this template available to use with multiple vendors, leave the Vendor field blank.

In the Agreement Type field, select Purchase Template from the drop-down.

Confirm the information in the remaining fields is correct, or update as needed.

On the Products tab, click Add a line, and select the desired product. Update the Quantity, and set the Unit Price.

When adding products to a new blanket order, the pre-existing prices of products are not automatically added to the product lines. Instead, the prices must be manually assigned, by changing the value in the Unit Price column to an agreed-upon price with the listed vendor. Otherwise, the price will remain 0.

After adding all necessary products, click Confirm.

After confirming a purchase template, new quotations can be created directly from the purchase template form. RFQs using this form are pre-populated with information based on the rules set in the form. Additionally, new quotations are automatically linked to this purchase template form, via the RFQs/Orders smart button at the top of the form.

To create a new quotation, click New Quotation. This opens a new RFQ, that is pre-populated with the correct information, depending on the settings configured on the purchase template form.

If there was no vendor identified on the purchase template, choose a Vendor from the drop-down list. Products can be added to the RFQ by clicking Add a product in the Products tab. To remove a product, click the (trash) icon at the far-right of the product line.

From the new RFQ form, click Send by Email to compose and send an email to the listed vendor. Click Print RFQ to generate a printable PDF of the quotation; or, once ready, click Confirm Order to confirm the purchase order.

After confirming the order, return to the purchase template via the breadcrumbs. The RFQs/Orders smart button has been updated to list the confirmed order.

---

## Google Search Console¶

**URL:** https://www.odoo.com/documentation/19.0/applications/websites/website/configuration/google_search_console.html

**Contents:**
- Google Search Console¶
- Domain property¶
- URL prefix property¶
- Site ownership verification¶
  - HTML file upload¶
  - HTML tag¶

Google Search Console is a free web service provided by Google that allows website owners to monitor, maintain, and troubleshoot their site’s presence in Google Search results. It offers valuable insights into how Google views and interacts with your site, helping you optimize its performance.

To enable Google Search Console for your website, go to Google Search Console. Then, select the property type Domain property or URL prefix property.

A domain property in Search Console tracks all versions of your website, including subdomains and protocols (http/https). This comprehensive view allows you to analyze your overall website’s search performance and make informed decisions to optimize its visibility. Enter the domain, e.g., example.com and click Continue.

The domain property type can only be verified via DNS record.

Google suggests creating at least one domain property to represent your site, as it is the most complete view of your website information.

This type of verification is usually simpler as you have multiple verification methods, such as using your existing Google Analytics or Tag Manager account. It also makes sense to view a section of your website separately. For example, if you work with a consultant on a specific part of your website, you might want to verify this part separately to limit access to your data. Enter the URL, e.g., https://example.odoo.com/ and click Continue.

Before using Google Search Console for your website, you must verify your site ownership. This verification process is a security measure that protects both you and Google. It ensures that only authorized users have access to sensitive data and that you have control over how your website is treated in Google Search.

Five methods are available to do this:

Google Analytics tracking code

Google Tag Manager container snippet

The best method for you depends on your comfort level and technical expertise. For beginners, using a file upload or HTML tag might be easiest. Those options are convenient if you already use Google Analytics or Google Tag Manager. You need to access your domain registrar’s settings for domain verification.

This method involves uploading an HTML file provided by Google containing the verification code you have to put in your Odoo’s Website Settings. Google verifies ownership by checking for this code.

Once you added your website URL under the URL prefix option and clicked continue, expand the HTML file section where you find a download button.

Download your HTML verification file and copy the verification code (e.g., google123abc.html).

In your Odoo database, go to Website ‣ Configuration ‣ Settings, and enable Google Search Console in the SEO section. Paste the verification code (e.g., google123abc.html) in the dedicated field.

In Google Search Console, click Verify. If you perform the steps above correctly, verification should be done immediately.

This method involves copying a meta tag provided by Google and pasting it into your Odoo website. To verify your site ownership using an HTML tag, follow these instructions:

Expand the HTML tag section.

Copy the HTML tag to clipboard.

On your Odoo website, click Edit in the upper-right corner, go to the Theme tab, scroll down to the Advanced section, then click <head> and </body> next to Code Injection. Paste the copied tag into the first field (<head>), and click Save.

Return to GSC and click Verify.

---

## Rental¶

**URL:** https://www.odoo.com/documentation/19.0/applications/sales/rental.html

**Contents:**
- Rental¶
    - Manage deposits
- Dashboard¶
- Settings¶
- Rental products¶
- Rental pricing¶
  - Pricing¶
  - Reservations¶
  - Price computing¶

The Odoo Rental application provides comprehensive solutions to configure and manage rentals.

Send quotations, confirm orders, schedule rentals, register products when they are picked up and returned, and invoice customers from this single platform.

Odoo Rental: product page

Odoo Tutorials: Rental

Learn how to create a refundable deposit for rental products.

Upon opening the Rental application, the Rental Orders dashboard is revealed.

In the default Kanban view, all rentals are visible. Each rental card displays the customer name, the price of the rental, the related sales order number, along with the status of the rental.

On the left sidebar, the Rental Status for each rental can be found. Beneath that, the Invoice Status of the rentals is accessible. Clicking any option in the left sidebar filters the displayed rentals on the dashboard.

To configure additional rental delay costs, availability of rental items, or minimum time of rental, navigate to Rental app ‣ Configuration ‣ Settings.

In the Rental section, there are options to configure Default Delay Costs and Default Padding Time. Also, there is the option to activate Rental Transfers.

Default Delay Costs are additional costs for late returns.

Default Padding Time represents the minimum amount of time between two rentals.

Rental Transfers means stock deliveries and receipts can be used for rental orders.

In the Rent Online section, there are options to configure a Minimal Rental Duration and designate Unavailability days, or days during which pickup and return are not possible.

To view all products that can rented in the database, navigate to Rentals app ‣ Products. By default, the Rental search filter appears in the search bar.

Each product Kanban card displays that product’s name, rental price, and product image (if applicable).

To adjust the rental pricing on a product, go to the Products page in the Rental app, then select the desired product or click New to create a new product from scratch.

On the product form, ensure the Rental checkbox is ticked. Then, open the Rental prices tab.

Under the Pricing section of the Rental prices tab, designate custom rental prices and rental periods for the product.

To add pricing for a rental, click Add a price. Then, choose a pricing period (the unit of duration of the rental) in the Period column, or create a new pricing period by typing in the name and clicking Create.

Next, decide whether or not to apply this custom rental price to a specific Pricelist.

Lastly, enter the desired Price for that specific Period.

No limit exists on how many pricing lines can be added. Multiple pricing options for rental products are typically used to give discounts for customers who agree to longer rental durations.

Remember when considering pricing that renting for a day is usually limited to operating hours, not 24 hours. When determining pricing, keep in mind that a rental day typically refers to operating hours, not a full 24-hour period.

Under the Reservations section of the Rental prices tab, there is the option to configure additional fines for any Hourly Fine or Daily Fine that the customer takes to return a rental.

Also, there is the option to set the Reserve product time, expressed in hours, to make the rental product temporarily unavailable between two rental orders. Such a feature may prove useful if maintenance or cleaning is required between rentals.

Odoo always uses two rules to compute the price of a product when a rental order is created:

Only one price line is used.

The cheapest line is selected.

Consider the following rental pricing configuration for a product:

A customer wants to rent this product for eight days. What price will they pay?

After an order is created, Odoo selects the second line as this is the cheapest option. The customer has to pay three times ‘3 days’ to cover the rental’s eight days, for a total of $750.

---

## Customize a view type¶

**URL:** https://www.odoo.com/documentation/19.0/developer/howtos/javascript_view.html

**Contents:**
- Customize a view type¶
- Subclass an existing view¶
- Create a new view from scratch¶

Assume we need to create a custom version of a generic view. For example, a kanban view with some extra ribbon-like widget on top (to display some specific custom information). In that case, this can be done in a few steps:

Extend the kanban controller/renderer/model and register it in the view registry.

In our custom kanban, we defined a new template. We can either inherit the kanban controller template and add our template pieces or we can define a completely new template.

Use the view with the js_class attribute in arch.

The possibilities for extending views are endless. While we have only extended the controller here, you can also extend the renderer to add new buttons, modify how records are presented, or customize the dropdown, as well as extend other components such as the model and buttonTemplate.

Creating a new view is an advanced topic. This guide highlight only the essential steps.

Create the controller.

The primary role of a controller is to facilitate the coordination between various components of a view, such as the Renderer, Model, and Layout.

The template of the Controller displays the control panel with Layout and also the renderer.

The primary function of a renderer is to generate a visual representation of data by rendering the view that includes records.

The role of the model is to retrieve and manage all the necessary data in the view.

For advanced cases, instead of creating a model from scratch, it is also possible to use RelationalModel, which is used by other views.

Create the arch parser.

The role of the arch parser is to parse the arch view so the view has access to the information.

Create the view and combine all the pieces together, then register the view in the views registry.

Declare the view in the arch.

**Examples:**

Example 1 (javascript):
```javascript
import { KanbanController } from "@web/views/kanban/kanban_controller";
import { kanbanView } from "@web/views/kanban/kanban_view";
import { registry } from "@web/core/registry";

// the controller usually contains the Layout and the renderer.
class CustomKanbanController extends KanbanController {
    static template = "my_module.CustomKanbanView";

    // Your logic here, override or insert new methods...
    // if you override setup(), don't forget to call super.setup()
}

export const customKanbanView = {
    ...kanbanView, // contains the default Renderer/Controller/Model
    Controller: CustomKanbanController,
};

// Register it to the views registry
registry.category("views").add("custom_kanban", customKanbanView);
```

Example 2 (xml):
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<templates>
    <t t-name="my_module.CustomKanbanView" t-inherit="web.KanbanView">
        <xpath expr="//Layout" position="before">
            <div>
                Hello world !
            </div>
        </xpath>
    </t>
</templates>
```

Example 3 (jsx):
```jsx
<kanban js_class="custom_kanban">
    <templates>
        <t t-name="kanban-card">
            <!--Your comment-->
        </t>
    </templates>
</kanban>
```

Example 4 (jsx):
```jsx
import { Layout } from "@web/search/layout";
import { useService } from "@web/core/utils/hooks";
import { Component, onWillStart, useState} from "@odoo/owl";

export class BeautifulController extends Component {
    static template = "my_module.View";
    static components = { Layout };

    setup() {
        this.orm = useService("orm");

        // The controller create the model and make it reactive so whenever this.model is
        // accessed and edited then it'll cause a rerendering
        this.model = useState(
            new this.props.Model(
                this.orm,
                this.props.resModel,
                this.props.fields,
                this.props.archInfo,
                this.props.domain
            )
        );

        onWillStart(async () => {
            await this.model.load();
        });
    }
}
```

---

## Views¶

**URL:** https://www.odoo.com/documentation/19.0/applications/studio/views.html

**Contents:**
- Views¶
- General views¶
  - Form¶
  - Activity¶
  - Search¶
- Multiple records views¶
  - Kanban¶
  - List¶
  - Map¶
- Timeline views¶

Views are the interface that allows displaying the data contained in a model. One model can have several views, which are simply different ways to show the same data. In Studio, views are organized into four categories: general, multiple records, timeline, and reporting.

To change the default view of a model, access Studio, go to Views, click the (ellipsis) icon next to the desired view, and click Set as Default.

You can modify views using the built-in XML editor: Activate the Developer mode, go to the view you want to edit, select the View tab, and click </> XML.

If you are editing a view using the XML editor, avoid making changes directly to standard and inherited views, as these would be reset and lost during updates or module upgrades. Always make sure you select the right Studio inherited views: When you modify a view in Studio by dragging and dropping a new field, for example, a specific Studio inherited view and its corresponding XPath, which defines the modified part of the view, are automatically generated.

The settings described below are found under the view’s View tab unless specified otherwise.

The Form view is used when creating and editing records, such as contacts, sales orders, products, etc.

To structure a form, drag and drop the Tabs and Columns element found under the + Add tab.

To prevent users from creating, editing, deleting or duplicating records, untick Can Create, Can Edit, Can Delete or Can Duplicate.

To add a button, click Add a button at the top of the form, enter a Label, and select the button’s action:

Run a Server Action: select the server action to be executed from the dropdown list;

Call a method: specify an existing Python method already defined in Odoo.

To change a button’s label or style, click the button and edit its Label or Class (either btn-primary for a primary button or btn-secondary for a secondary button) in the Properties tab.

Primary buttons represent the main action(s) the user can take in a specific view, e.g., Send a request for quotation, and are more visually prominent. Secondary buttons offer alternative or less common actions, e.g., Print or Preview a request for quotation, and are less visually prominent. By default, a new button is styled as a secondary button.

To add a smart button, click the (plus) icon in the top-right corner of the form. Enter a Label, choose an Icon, and select a related field.

The Activity view is used to schedule and have an overview of activities (emails, calls, etc.) linked to records.

This view can only be modified within Studio by editing the XML code.

The Search view is added on top of other views to filter, group, and search records.

To add custom Filters and structure them using Separators, go to the + Add tab and drag and drop them under Filters.

To add an existing field under the search dropdown menu, go to the + Add tab and drag and drop it under Autocompletion Fields.

The settings described below are found under the view’s View tab unless specified otherwise.

The Kanban view is often used to support business flows by moving records across stages or as an alternative way to display records inside cards.

If the Kanban view exists, it is used by default to display data on mobile devices instead of the List view.

To prevent users from creating new records, untick Can Create.

To create records directly within the view, in a minimalistic form, enable Quick Create.

To set a default grouping for records, select a field under Default Group By.

The List view is used to overview many records at once, look for records, and edit simple records.

To prevent users from creating, editing, deleting or duplicating records, untick Can Create, Can Edit, Can Delete, or Can Duplicate.

To create and edit records directly within the view, select either Add record at the bottom, Add record on top or Open form view under When Creating Record.

This prevents users from opening records in Form view from the List view.

To edit several records at once, tick Enable Mass Editing.

To change the way records are sorted by default, select a field under Sort By.

To set a default grouping for records, select a field under Default Group By.

To add a button, click Add a button at the top of the list, enter a Label, and select the button’s action:

Run a Server Action: select the server action to be executed from the dropdown list;

Call a method: specify an existing Python method already defined in Odoo.

The widths of columns in a List view are computed automatically to provide the optimal user experience. However, it is also possible to set a fixed column width per field. To do so, click on the relevant column to open the field’s Properties tab, then enter the desired number of pixels in the Column Width (px) field.

To add a (drag handle) icon to reorder records manually, add an Integer field with the Handle widget.

The Map view is used to display records on a map. For example, it is used in the Field Service app to plan an itinerary between different tasks.

A Many2One field linked to the Contact model is required to activate the view, as the contact address is used to position records on the map.

To select which kind of contact should be used on the map, select it under Contact Field.

To hide the name or the address of the record, tick Hide Name or Hide Address.

To add information from other fields, select them under Additional Fields.

To have a route suggested between the different records, tick Enable Routing and select which field should be used to sort records for the routing.

When you first activate one of the timeline views, you need to select which Date or Date & Time fields on your model should be used to define when the records start and stop in order to display them on the view. You can modify the Start Date Field and Stop Date Field after activating the view.

The settings described below are found under the view’s View tab unless specified otherwise.

The Calendar view is used to overview and manage records inside a calendar.

To create records directly within the view instead of opening the Form view, enable Quick Create.

This only works on specific models that can be quick-created using only a name. However, most models do not support quick creation and open the Form view to fill in the required fields.

To color records on the calendar, select a field under Color. All the records sharing the same value for that field are displayed using the same color.

As the number of colors is limited, the same color can end up being assigned to different values.

To display events lasting the whole day at the top of the calendar, select a Checkbox field that specifies if the event lasts the whole day.

To choose the default time scale used to display events, select Day, Week, Month, or Year under Default Display Mode.

You can also use a Delay Field to display the duration of the event in hours by selecting a Decimal or Integer field on the model which specifies the duration of the event. However, if you set an End Date Field, the Delay Field will not be taken into account.

The Cohort view is used to examine the life cycle of records over a time period. For example, it is used in the Subscriptions app to view the subscriptions’ retention rate.

To display a measure (i.e., the aggregated value of a given field) by default on the view, select a Measure Field.

To choose which time interval is used by default to group results, select Day, Week, Month, or Year under Interval.

To change the cohort Mode, select either Retention the percentage of records staying over a period of time, it starts at 100% and decreases with time or Churn the percentage of records moving out over a period of time - it starts at 0% and increases with time.

To change the way the Timeline (i.e., the columns) progresses, select either Forward (from 0 to +15) or Backward (from -15 to 0). For most purposes, the Forward timeline is used.

The Gantt view is used to forecast and examine the overall progress of records. Records are represented by a bar under a time scale.

To prevent users from creating or editing records, untick Can Create or Can Edit.

To fill cells in gray whenever a record should not be created there (e.g., on weekends for employees), tick Display Unavailability.

The underlying model must support this feature, and support for it cannot be added using Studio. It is supported for the Project, Time Off, Planning, and Manufacturing apps.

To show a total row at the bottom, tick Display Total row.

To collapse multiple records in a single row, tick Collapse First Level.

To choose which way records are grouped by default on rows (e.g., per employee or project), select a field under Default Group by.

To define a default time scale to view records, select Day, Week, Month, or Year under Default Scale.

To color records on the view, select a field under Color. All the records sharing the same value for that field are displayed using the same color.

As the number of colors is limited, the same color can be assigned to different values.

To specify with which degree of precision each time scale should be divided by, select Quarter Hour, Half Hour, or Hour under Day Precision, Half Day or Day under Week Precision, and Month Precision.

The settings described below are found under the view’s View tab unless specified otherwise.

The Pivot view is used to explore and analyze the data contained in records in an interactive manner. It is especially useful to aggregate numeric data, create categories, and drill down the data by expanding and collapsing different levels of data.

To access all records whose data is aggregated under a cell, tick Access records from cell.

To divide the data into different categories, select field(s) under Column grouping, Row grouping - First level, or Row grouping - Second level.

To add different types of data to be measured using the view, select a field under Measures.

To display a count of records that made up the aggregated data in a cell, tick Display count.

The Graph view is used to showcase data from records in a bar, line, or pie chart.

To change the default chart, select Bar, Line, or Pie under Type.

To choose a default data dimension (category), select a field under First dimension and, if needed, another under Second dimension.

To select a default type of data to be measured using the view, select a field under Measure.

For Bar and Line charts only: To sort the different data categories by their value, select Ascending (from lowest to highest value) or Descending (from highest to lowest) under Sorting.

For Bar and Pie charts only: To access all records whose data is aggregated under a data category on the chart, tick Access records from graph.

For Bar charts only: When using two data dimensions (categories), display the two columns on top of each other by default by ticking Stacked graph.

---
