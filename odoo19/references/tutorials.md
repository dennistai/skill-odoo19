# Odoo19 - Tutorials

**Pages:** 46

---

## Chapter 3: Models And Basic Fields¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/server_framework_101/03_basicmodel.html

**Contents:**
- Chapter 3: Models And Basic Fields¶
- Object-Relational Mapping¶
- Model fields¶
  - Types¶
  - Common Attributes¶
  - Automatic Fields¶

At the end of the previous chapter, we were able to create an Odoo module. However, at this point it is still an empty shell which doesn’t allow us to store any data. In our real estate module, we want to store the information related to the properties (name, description, price, living area…) in a database. The Odoo framework provides tools to facilitate database interactions.

Before moving forward in the exercise, make sure the estate module is installed, i.e. it must appear as ‘Installed’ in the Apps list.

Do not use mutable global variables.

A single Odoo instance can run several databases in parallel within the same python process. Distinct modules might be installed on each of these databases, therefore we cannot rely on global variables that would be updated depending on installed modules.

Reference: the documentation related to this topic can be found in the Models API.

Goal: at the end of this section, the table estate_property should be created:

A key component of Odoo is the ORM layer. This layer avoids having to manually write most SQL and provides extensibility and security services2.

Business objects are declared as Python classes extending Model, which integrates them into the automated persistence system.

Models can be configured by setting attributes in their definition. The most important attribute is _name, which is required and defines the name for the model in the Odoo system. Here is a minimum definition of a model:

This definition is enough for the ORM to generate a database table named test_model. By convention all models are located in a models directory and each model is defined in its own Python file.

Take a look at how the crm_recurring_plan table is defined and how the corresponding Python file is imported:

The model is defined in the file crm/models/crm_recurring_plan.py (see here)

The file crm_recurring_plan.py is imported in crm/models/__init__.py (see here)

The folder models is imported in crm/__init__.py (see here)

Define the real estate properties model.

Based on example given in the CRM module, create the appropriate files and folder for the estate_property table.

When the files are created, add a minimum definition for the estate.property model.

Any modification of the Python files requires a restart of the Odoo server. When we restart the server, we will add the parameters -d and -u:

-u estate means we want to upgrade the estate module, i.e. the ORM will apply database schema changes. In this case it creates a new table. -d rd-demo means that the upgrade should be performed on the rd-demo database. -u should always be used in combination with -d.

During the startup you should see the following warnings:

If this is the case, then you should be good! To be sure, double check with psql as demonstrated in the Goal.

Add a _description to your model to get rid of one of the warnings.

Reference: the documentation related to this topic can be found in the Fields API.

Fields are used to define what the model can store and where they are stored. Fields are defined as attributes in the model class:

The name field is a Char which will be represented as a Python unicode str and a SQL VARCHAR.

Goal: at the end of this section, several basic fields should have been added to the table estate_property:

There are two broad categories of fields: ‘simple’ fields, which are atomic values stored directly in the model’s table, and ‘relational’ fields, which link records (of the same or different models).

Simple field examples are Boolean, Float, Char, Text, Date and Selection.

Add basic fields to the Real Estate Property table.

Add the following basic fields to the table:

The garden_orientation field must have 4 possible values: ‘North’, ‘South’, ‘East’ and ‘West’. The selection list is defined as a list of tuples, see here for an example.

When the fields are added to the model, restart the server with -u estate

Connect to psql and check the structure of the table estate_property. You’ll notice that a couple of extra fields were also added to the table. We will revisit them later.

Goal: at the end of this section, the columns name and expected_price should be not nullable in the table estate_property:

Much like the model itself, fields can be configured by passing configuration attributes as parameters:

Some attributes are available on all fields, here are the most common ones:

The label of the field in UI (visible by users).

If True, the field can not be empty. It must either have a default value or always be given a value when creating a record.

Provides long-form help tooltip for users in the UI.

Requests that Odoo create a database index on the column.

Set attributes for existing fields.

Add the following attributes:

After restarting the server, both fields should be not nullable.

Reference: the documentation related to this topic can be found in Automatic fields.

You may have noticed your model has a few fields you never defined. Odoo creates a few fields in all models1. These fields are managed by the system and can’t be written to, but they can be read if useful or necessary:

The unique identifier for a record of the model.

Creation date of the record.

User who created the record.

Last modification date of the record.

User who last modified the record.

Now that we have created our first model, let’s add some security!

it is possible to disable the automatic creation of some fields

writing raw SQL queries is possible, but requires caution as this bypasses all Odoo authentication and security mechanisms.

**Examples:**

Example 1 (sql):
```sql
$ psql -d rd-demo
rd-demo=# SELECT COUNT(*) FROM estate_property;
count
-------
    0
(1 row)
```

Example 2 (python):
```python
from odoo import models

class TestModel(models.Model):
    _name = "test_model"
```

Example 3 (unknown):
```unknown
$ ./odoo-bin --addons-path=addons,../enterprise/,../tutorials/ -d rd-demo -u estate
```

Example 4 (unknown):
```unknown
...
WARNING rd-demo odoo.models: The model estate.property has no _description
...
WARNING rd-demo odoo.modules.loading: The model estate.property has no access rules, consider adding one...
...
```

---

## Chapter 3: Customize a kanban view¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/master_odoo_web_framework/03_customize_kanban_view.html

**Contents:**
- Chapter 3: Customize a kanban view¶
- 1. Create a new kanban view¶
- 2. Create a CustomerList component¶
- 3. Load and display data¶
- 4. Update the main kanban view¶
- 5. Only display customers which have an active order¶
- 6. Add a search bar to the customer list¶
- 7. Refactor the code to use t-model¶
- 8. Paginate customers!¶

We have gained an understanding of the numerous capabilities offered by the web framework. As a next step, we will customize a kanban view. This is a more complicated project that will showcase some non trivial aspects of the framework. The goal is to practice composing views, coordinating various aspects of the UI, and doing it in a maintainable way.

Bafien had the greatest idea ever: a mix of a kanban view and a list view would be perfect for your needs! In a nutshell, he wants a list of customers on the left of the CRM kanban view. When you click on a customer on the left sidebar, the kanban view on the right is filtered to only display leads linked to that customer.

The solutions for each exercise of the chapter are hosted on the official Odoo tutorials repository.

Since we are customizing the kanban view, let us start by extending it and using our extension in the kanban view of CRM.

Create a new empty component that extends the KanbanController component from @web/views/kanban/kanban_controller.

Create a new view object and assign all keys and values from kanbanView from @web/views/kanban/kanban_view. Override the Controller key by putting your newly created controller.

Register it in the views registry under awesome_kanban.

Update the crm kanban arch in awesome_kanban/views/views.xml to use the extended view. This can be done by specifying the js_class attribute in the kanban node.

Example: Create a new view by extending a pre-existing one

We will need to display a list of customers, so we might as well create the component.

Create a CustomerList component which only displays a div with some text for now.

It should have a selectCustomer prop.

Create a new template extending (XPath) the kanban controller template web.KanbanView to add the CustomerList next to the kanban renderer. Give it an empty function as selectCustomer for now.

You can use this xpath inside the template to add a div before the renderer component.

Subclass the kanban controller to add CustomerList in its sub-components.

Make sure you see your component in the kanban view.

Modify the CustomerList component to fetch a list of all customers in onWillStart.

Display the list in the template with a t-foreach.

Whenever a customer is selected, call the selectCustomer function prop.

Example: fetching records from a model

Implement selectCustomer in the kanban controller to add the proper domain.

Since it is not trivial to interact with the search view, here is a snippet to create a filter:

By clicking on multiple customers, you can see that the old customer filter is not replaced. Make sure that by clicking on a customer, the old filter is replaced by the new one.

You can use this snippet to get the customers filters and toggle them.

Modify the template to give the real function to the CustomerList selectCustomer prop.

You can use Symbol to make sure that the custom isFromAwesomeKanban key will not collide with keys any other code might add to the object.

There is a opportunity_ids field on res.partner. Let us allow the user to filter results on customers with at least one opportunity.

Add an input of type checkbox in the CustomerList component, with a label “Active customers” next to it.

Changing the value of the checkbox should filter the list of customers.

Add an input above the customer list that allows the user to enter a string and to filter the displayed customers, according to their name.

You can use the fuzzyLookup from @web/core/utils/search function to perform the filter.

Code: The fuzzylookup function

Example: Using fuzzyLookup

To solve the previous two exercises, it is likely that you used an event listener on the inputs. Let us see how we could do it in a more declarative way, with the t-model directive.

Make sure you have a reactive object that represents the fact that the filter is active (something like this.state = useState({ displayActiveCustomers: false, searchString: ''})).

Modify the code to add a getter displayedCustomers which returns the currently active list of customers.

Modify the template to use t-model.

Add a pager in the CustomerList, and only load/render the first 20 customers.

Whenever the pager is changed, the customer list should update accordingly.

This is actually pretty hard, in particular in combination with the filtering done in the previous exercise. There are many edge cases to take into account.

**Examples:**

Example 1 (jsx):
```jsx
<xpath expr="//t[@t-component='props.Renderer']" position="before">
   ...
</xpath>
```

Example 2 (css):
```css
this.env.searchModel.createNewFilters([{
      description: partner_name,
      domain: [["partner_id", "=", partner_id]],
      isFromAwesomeKanban: true, // this is a custom key to retrieve our filters later
}])
```

Example 3 (javascript):
```javascript
const customerFilters = this.env.searchModel.getSearchItems((searchItem) =>
      searchItem.isFromAwesomeKanban
);

for (const customerFilter of customerFilters) {
   if (customerFilter.isActive) {
         this.env.searchModel.toggleSearchItem(customerFilter.id);
   }
}
```

---

## Reuse code with mixins¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/mixins.html

**Contents:**
- Reuse code with mixins¶

If you need to interface with common Odoo features such as the chatter, you can rely on mixins. They are Odoo models exposing useful methods through inheritance.

To learn and play with mixins, visit this repository. This module for a plant nursery is training material developed for the OXP 2018. You don’t need to code it on your side. But you can check the presentations in the /static/pdf directory and play with the module to discover some magic features in Odoo.

---

## Chapter 14: A Brief History Of QWeb¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/server_framework_101/14_qwebintro.html

**Contents:**
- Chapter 14: A Brief History Of QWeb¶
- Concrete Example: A Kanban View¶

So far the interface design of our real estate module has been rather limited. Building a list view is straightforward since only the list of fields is necessary. The same holds true for the form view: despite the use of a few tags such as <group> or <page>, there is very little to do in terms of design.

However, if we want to give a unique look to our application, it is necessary to go a step further and be able to design new views. Moreover, other features such as PDF reports or website pages need another tool to be created with more flexibility: a templating engine.

You might already be familiar with existing engines such as Jinja (Python), ERB (Ruby) or Twig (PHP). Odoo comes with its own built-in engine: QWeb Templates. QWeb is the primary templating engine used by Odoo. It is an XML templating engine and used mostly to generate HTML fragments and pages.

You probably already have come across the kanban board in Odoo where the records are displayed in a card-like structure. We will build such a view for our real estate module.

Reference: the documentation related to this topic can be found in Kanban.

Goal: at the end of this section a Kanban view of the properties should be created:

In our estate application, we would like to add a Kanban view to display our properties. Kanban views are a standard Odoo view (like the form and list views), but their structure is much more flexible. In fact, the structure of each card is a mix of form elements (including basic HTML) and QWeb. The definition of a Kanban view is similar to the definition of the list and form views, except that their root element is <kanban>. In its simplest form, a Kanban view looks like:

Let’s break down this example:

<templates>: defines a list of QWeb Templates templates. Kanban views must define at least one root template kanban-card, which will be rendered once for each record.

<t t-name="kanban-card">: <t> is a placeholder element for QWeb directives. In this case, it is used to set the name of the template to kanban-card

<field name="name"/>: this will add the name field to the view.

Make a minimal kanban view.

Using the simple example provided, create a minimal Kanban view for the properties. The only field to display is the name.

Tip: you must add kanban in the view_mode of the corresponding ir.actions.act_window.

Once the Kanban view is working, we can start improving it. If we want to display an element conditionally, we can use the t-if directive (see Conditionals).

We added a few things:

t-if: the <div> element is rendered if the condition is true.

record: an object with all the requested fields as its attributes. Each field has two attributes value and raw_value. The former is formatted according to current user parameters and the latter is the direct value from a read().

In the above example, the field name was added in the <templates> element, but state is outside of it. When we need the value of a field but don’t want to display it in the view, it is possible to add it outside of the <templates> element.

Improve the Kanban view.

Add the following fields to the Kanban view: expected price, best price, selling price and tags. Pay attention: the best price is only displayed when an offer is received, while the selling price is only displayed when an offer is accepted.

Refer to the Goal of the section for a visual example.

Let’s give the final touch to our view: the properties must be grouped by type by default. You might want to have a look at the various options described in Kanban.

Add default grouping.

Use the appropriate attribute to group the properties by type by default. You must also prevent drag and drop.

Refer to the Goal of the section for a visual example.

Kanban views are a typical example of how it is always a good idea to start from an existing view and fine tune it instead of starting from scratch. There are many options and classes available, so… read and learn!

**Examples:**

Example 1 (jsx):
```jsx
<kanban>
    <templates>
        <t t-name="kanban-card">
            <div>
                <field name="name"/>
            </div>
        </t>
    </templates>
</kanban>
```

Example 2 (jsx):
```jsx
<kanban>
    <field name="state"/>
    <templates>
        <t t-name="kanban-card">
            <div>
                <field name="name"/>
                <div t-if="record.state.raw_value == 'new'">
                    This is new!
                </div>
            </div>
        </t>
    </templates>
</kanban>
```

---

## Discover the web framework¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/discover_js_framework.html

**Contents:**
- Discover the web framework¶
- Setup¶
- Content¶

This two parts tutorial is designed to introduce you to the basics of the web framework. Whether you are new to the framework or have some prior experience, this tutorial will provide you with a solid foundation for using the web framework in your projects.

The first part covers the basics of Owl components, which are a key part of the web framework. Owl components are reusable UI components that can be used to build complex web interfaces quickly and efficiently. We will explore how to create and use Owl components in Odoo. Then, in the second part of this tutorial, we focus on creating a dashboard using various features of Odoo. Dashboards are an essential part of any web application, and provide a nice starting point to use and interact with the Odoo codebase.

This tutorial assumes that you have some basic knowledge of development with Odoo in general (models, controllers, QWeb, …). If you are new to Odoo, we recommend that you start with the Server framework 101 tutorial before proceeding with this one.

Each chapter of this tutorial is an independant project. If you feel comfortable with Owl, you can start directly with chapter 2.

Clone the official Odoo tutorials repository and switch to the branch 19.0.

Add the cloned repository to your --addons-path.

Start a new Odoo database and install the modules awesome_owl (for chapter 1) and awesome_dashboard (for chapter 2).

Chapter 1: Owl components

Chapter 2: Build a dashboard

---

## Server framework 101¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/server_framework_101.html

**Contents:**
- Server framework 101¶

Welcome to the Server framework 101 tutorial! If you reached this page that means you are interested in the development of your own Odoo module. It might also mean that you recently joined the Odoo company for a rather technical position. In any case, your journey to the technical side of Odoo starts here.

The goal of this tutorial is for you to get an insight of the most important parts of the Odoo development framework while developing your own Odoo module to manage real estate assets. The chapters should be followed in their given order since they cover the development of a new Odoo application from scratch in an incremental way. In other words, each chapter depends on the previous one.

Before going further, make sure you have prepared your development environment with the setup guide.

Ready? Let’s get started!

Chapter 1: Architecture Overview

Chapter 2: A New Application

Chapter 3: Models And Basic Fields

Chapter 4: Security - A Brief Introduction

Chapter 5: Finally, Some UI To Play With

Chapter 6: Basic Views

Chapter 7: Relations Between Models

Chapter 8: Computed Fields And Onchanges

Chapter 9: Ready For Some Action?

Chapter 10: Constraints

Chapter 11: Add The Sprinkles

Chapter 12: Inheritance

Chapter 13: Interact With Other Modules

Chapter 14: A Brief History Of QWeb

Chapter 15: The final word

---

## Safeguard your code with unit tests¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/unit_tests.html

**Contents:**
- Safeguard your code with unit tests¶
- Running Tests¶
- Integration Bots¶
  - Runbot¶
  - Robodoo¶
  - Mergebot¶
- Modules¶
- Writing a test¶

This tutorial is an extension of the Server framework 101 tutorial. Make sure you have completed it and use the estate module you have built as a base for the exercises in this tutorial.

Reference: Odoo’s Test Framework: Learn Best Practices (Odoo Experience 2020) on YouTube.

Writing tests is a necessity for multiple reasons. Here is a non-exhaustive list:

Ensure code will not be broken in the future

Define the scope of your code

Give examples of use cases

It is one way to technically document the code

Help your coding by defining your goal before working towards it

Before knowing how to write tests, we need to know how to run them.

This section is only for Odoo employees and people that are contributing to github.com/odoo. We highly recommend having your own CI otherwise.

When a test is written, it is important to make sure it always passes when modifications are applied to the source code. To automate this task, we use a development practice called Continuous Integration (CI). This is why we have some bots running all the tests at different moments. Whether you are working at Odoo or not, if you are trying to merge something inside odoo/odoo, odoo/enterprise, odoo/upgrade or on odoo.sh, you will have to go through the CI. If you are working on another project, you should think of adding your own CI.

Reference: the documentation related to this topic can be found in Runbot FAQ.

Most of the tests are run on Runbot every time a commit is pushed on GitHub.

You can see the state of a commit/branch by filtering on the runbot dashboard.

A bundle is created for each branch. A bundle consists of a configuration and batches.

A batch is a set of builds, depending on the parameters of the bundle. A batch is green (i.e. passes the tests) if all the builds are green.

A build is when we launch a server. It can be divided in sub-builds. Usually there are builds for the community version, the enterprise version (only if there is an enterprise branch but you can force the build), and the migration of the branch. A build is green if every sub-build is green.

A sub-build only does some parts of what a full build does. It is used to speed up the CI process. Generally it is used to split the post install tests in 4 parallel instances. A sub-build is green if all the tests are passing and there are no errors/warnings logged.

All tests are run regardless of the modifications done. Correcting a typo in an error message or refactoring a whole module triggers the same tests. All modules will be installed as well. This means something might not work even if the Runbot is green, i.e. your changes depend on a module that the module the changes are in doesn’t depend on.

The localization modules (i.e. country-specific modules) are not installed on Runbot (except the generic one). Some modules with external dependencies can also be excluded.

There is a nightly build running additional tests: module operations, localization, single module installs, multi-builds for nondeterministic bugs, etc. These are not kept in the standard CI to shorten the time of execution.

You can also login to a build built by Runbot. There are 3 users usable: admin, demo and portal. The password is the same as the login. This is useful to quickly test things on different versions without having to build it locally. The full logs are also available; these are used for monitoring.

You will most likely have to gain a little bit more experience before having the rights to summon robodoo, but here are a few remarks anyways.

Robodoo is the guy spamming the CI status as tags on your PRs, but he is also the guy that kindly integrates your commits into the main repositories.

When the last batch is green, the reviewer can ask robodoo to merge your PR (it is more a rebase than a merge). It will then go to the mergebot.

Mergebot is the last testing phase before merging a PR.

It will take the commits in your branch not yet present on the target, stage it and rerun the tests one more time, including the enterprise version even if you are only changing something in community.

This step can fail with a Staging failed error message. This could be due to

a nondeterministic bug that is already on the target. If you are an Odoo employee, you can check those here: https://runbot.odoo.com/runbot/errors

a nondeterministic bug that you introduced but wasn’t detected in the CI before

an incompatibility with another commit merged right before and what you are trying to merge

an incompatibility with the enterprise repository if you only did changes in the community repo

Always check that the issue does not come from you before asking the merge bot to retry: rebase your branch on the target and rerun the tests locally.

Because Odoo is modular, the tests need to be also modular. This means tests are defined in the module that adds the functionality you are adding in, and tests cannot depend on functionality coming from modules your module doesn’t depend on.

Reference: the documentation related to this topic can be found in Special Tags.

If the behavior you want to test can be changed by the installation of another module, you need to ensure that the tag at_install is set; otherwise, you can use the tag post_install to speed up the CI and ensure it is not changed if it shouldn’t.

Reference: the documentation related to this topic can be found in Python unittest and Testing Odoo.

Here are a few things to take into consideration before writing a test

The tests should be independent of the data currently in the database (including demo data)

Tests should not impact the database by leaving/changing residual data. This is usually done by the test framework by doing a rollback. Therefore, you must never call cr.commit in a test (nor anywhere else in the business code).

For a bug fix, the test should fail before applying the fix and pass after.

Don’t test something that is already tested elsewhere; you can trust the ORM. Most of the tests in business modules should only test the business flows.

You shouldn’t need to flush data into the database.

Remember that onchange only applies in the Form views, not by changing the attributes in python. This also applies in the tests. If you want to emulate a Form view, you can use odoo.tests.Form.

The tests should be in a tests folder at the root of your module. Each test file name should start with test_ and be imported in the __init__.py of the test folder. You shouldn’t import the test folder/module in the __init__.py of the module.

All the tests should extend odoo.tests.common.TransactionCase. You usually define a setUpClass and the tests. After writing the setUpClass, you have an env available in the class and can start interacting with the ORM.

These test classes are built on top of the unittest python module.

For better readability, split your tests into multiple files depending on the scope of the tests. You can also have a Common class that most of the tests should inherit from; this common class can define the whole setup for the module. For instance, in account.

Update the code so no one can:

Create an offer for a sold property

Sell a property with no accepted offers on it

and create tests for both of these cases. Additionally check that selling a property that can be sold is correctly marked as sold after selling it.

Someone keeps breaking the reset of Garden Area and Orientation when you uncheck the Garden checkbox. Make sure it doesn’t happen again.

Tip: remember the note about Form a little bit above.

**Examples:**

Example 1 (julia):
```julia
$ odoo-bin -h
Usage: odoo-bin [options]

Options:
--version             show program's version number and exit
-h, --help            show this help message and exit

[...]

Testing Configuration:
  --test-file=TEST_FILE
                      Launch a python test file.
  --test-enable       Enable unit tests.
  --test-tags=TEST_TAGS
                      Comma-separated list of specs to filter which tests to
                      execute. Enable unit tests if set. A filter spec has
                      the format: [-][tag][/module][:class][.method] The '-'
                      specifies if we want to include or exclude tests
                      matching this spec. The tag will match tags added on a
                      class with a @tagged decorator (all Test classes have
                      'standard' and 'at_install' tags until explicitly
                      removed, see the decorator documentation). '*' will
                      match all tags. If tag is omitted on include mode, its
                      value is 'standard'. If tag is omitted on exclude
                      mode, its value is '*'. The module, class, and method
                      will respectively match the module name, test class
                      name and test method name. Example: --test-tags
                      :TestClass.test_func,/test_module,external  Filtering
                      and executing the tests happens twice: right after
                      each module installation/update and at the end of the
                      modules loading. At each stage tests are filtered by
                      --test-tags specs and additionally by dynamic specs
                      'at_install' and 'post_install' correspondingly.
  --screencasts=DIR   Screencasts will go in DIR/{db_name}/screencasts.
  --screenshots=DIR   Screenshots will go in DIR/{db_name}/screenshots.
                      Defaults to /tmp/odoo_tests.

$ # run all the tests of account, and modules installed by account
$ # the dependencies already installed are not tested
$ # this takes some time because you need to install the modules, but at_install
$ # and post_install are respected
$ odoo-bin -i account --test-enable
$ # run all the tests in this file
$ odoo-bin --test-file=addons/account/tests/test_account_move_entry.py
$ # test tags can help you filter quite easily
$ odoo-bin --test-tags=/account:TestAccountMove.test_custom_currency_on_account_1
```

Example 2 (python):
```python
from odoo.tests.common import TransactionCase
from odoo.tests import tagged

# The CI will run these tests after all the modules are installed,
# not right after installing the one defining it.
@tagged('post_install', '-at_install')  # add `post_install` and remove `at_install`
class PostInstallTestCase(TransactionCase):
    def test_01(self):
        ...

@tagged('at_install')  # this is the default
class AtInstallTestCase(TransactionCase):
    def test_01(self):
        ...
```

Example 3 (unknown):
```unknown
estate
├── models
│   ├── *.py
│   └── __init__.py
├── tests
│   ├── test_*.py
│   └── __init__.py
├── __init__.py
└── __manifest__.py
```

Example 4 (python):
```python
from odoo.tests.common import TransactionCase
from odoo.exceptions import UserError
from odoo.tests import tagged

# The CI will run these tests after all the modules are installed,
# not right after installing the one defining it.
@tagged('post_install', '-at_install')
class EstateTestCase(TransactionCase):

    @classmethod
    def setUpClass(cls):
        # add env on cls and many other things
        super(EstateTestCase, cls).setUpClass()

        # create the data for each tests. By doing it in the setUpClass instead
        # of in a setUp or in each test case, we reduce the testing time and
        # the duplication of code.
        cls.properties = cls.env['estate.property'].create([...])

    def test_creation_area(self):
        """Test that the total_area is computed like it should."""
        self.properties.living_area = 20
        self.assertRecordValues(self.properties, [
           {'name': ..., 'total_area': ...},
           {'name': ..., 'total_area': ...},
        ])


    def test_action_sell(self):
        """Test that everything behaves like it should when selling a property."""
        self.properties.action_sold()
        self.assertRecordValues(self.properties, [
           {'name': ..., 'state': ...},
           {'name': ..., 'state': ...},
        ])

        with self.assertRaises(UserError):
            self.properties.forbidden_action_on_sold_property()
```

---

## Chapter 1: Architecture Overview¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/server_framework_101/01_architecture.html

**Contents:**
- Chapter 1: Architecture Overview¶
- Multitier application¶
- Odoo modules¶
  - Composition of a module¶
  - Module structure¶
- Odoo Editions¶

Odoo follows a multitier architecture, meaning that the presentation, the business logic and the data storage are separated. More specifically, it uses a three-tier architecture (image from Wikipedia):

The presentation tier is a combination of HTML5, JavaScript and CSS. The logic tier is exclusively written in Python, while the data tier only supports PostgreSQL as an RDBMS.

Depending on the scope of your module, Odoo development can be done in any of these tiers. Therefore, before going any further, it may be a good idea to refresh your memory if you don’t have an intermediate level in these topics.

In order to go through this tutorial, you will need a very basic knowledge of HTML and an intermediate level of Python. Advanced topics will require more knowledge in the other subjects. There are plenty of tutorials freely accessible, so we cannot recommend one over another since it depends on your background.

For reference this is the official Python tutorial.

Since version 15.0, Odoo is actively transitioning to using its own in-house developed OWL framework as part of its presentation tier. The legacy JavaScript framework is still supported but will be deprecated over time. This will be discussed further in advanced topics.

Both server and client extensions are packaged as modules which are optionally loaded in a database. A module is a collection of functions and data that target a single purpose.

Odoo modules can either add brand new business logic to an Odoo system or alter and extend existing business logic. One module can be created to add your country’s accounting rules to Odoo’s generic accounting support, while a different module can add support for real-time visualisation of a bus fleet.

Everything in Odoo starts and ends with modules.

Terminology: developers group their business features in Odoo modules. The main user-facing modules are flagged and exposed as Apps, but a majority of the modules aren’t Apps. Modules may also be referred to as addons and the directories where the Odoo server finds them form the addons_path.

An Odoo module can contain a number of elements:

A business object (e.g. an invoice) is declared as a Python class. The fields defined in these classes are automatically mapped to database columns thanks to the ORM layer.

XML or CSV files declaring the model data:

configuration data (modules parametrization, security rules),

Handle requests from web browsers

Images, CSS or JavaScript files used by the web interface or website

None of these elements are mandatory. Some modules may only add data files (e.g. country-specific accounting configuration), while others may only add business objects. During this training, we will create business objects, object views and data files.

Each module is a directory within a module directory. Module directories are specified by using the --addons-path option.

An Odoo module is declared by its manifest.

When an Odoo module includes business objects (i.e. Python files), they are organized as a Python package with a __init__.py file. This file contains import instructions for various Python files in the module.

Here is a simplified module directory:

Odoo is available in two versions: Odoo Enterprise (licensed & shared sources) and Odoo Community (open-source). In addition to services such as support or upgrades, the Enterprise version provides extra functionalities to Odoo. From a technical point-of-view, these functionalities are simply new modules installed on top of the modules provided by the Community version.

Ready to start? It is now time to write your own application!

**Examples:**

Example 1 (unknown):
```unknown
module
├── models
│   ├── *.py
│   └── __init__.py
├── data
│   └── *.xml
├── __init__.py
└── __manifest__.py
```

---

## Restrict access to data¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/restrict_data_access.html

**Contents:**
- Restrict access to data¶
- Groups¶
- Access Rights¶
- Record Rules¶
- Security Override¶
  - Bypassing Security¶
  - Programmatically checking security¶
- Multi-company security¶
- Visibility != security¶

This tutorial is an extension of the Server framework 101 tutorial. Make sure you have completed it and use the estate module you have built as a base for the exercises in this tutorial.

So far we have mostly concerned ourselves with implementing useful features. However in most business scenarios security quickly becomes a concern: currently,

Any employee (which is what group_user stands for) can create, read, update or delete properties, property types, or property tags.

If estate_account is installed then only agents allowed to interact with invoicing can confirm sales as that’s necessary to create an invoice.

We do not want third parties to be able to access properties directly.

Not all our employees may be real-estate agents (e.g. administrative personnel, property managers, …), we don’t want non-agents to see the available properties.

Real-estate agents don’t need or get to decide what property types or tags are available.

Real-estate agents can have exclusive properties, we do not want one agent to be able to manage another’s exclusives.

All real-estate agents should be able to confirm the sale of a property they can manage, but we do not want them to be able to validate or mark as paid any invoice in the system.

We may actually be fine with some or most of these for a small business.

Because it’s easier for users to disable unnecessary security rules than it is to create them from nothing, it’s better to err on the side of caution and limit access: users can relax that access if necessary or convenient.

The documentation related to this topic can be found in the security reference.

Coding guidelines document the format and location of master data items.

At the end of this section,

We can make employees real-estate agents or real-estate managers.

The admin user is a real-estate manager.

We have a new real-estate agent employee with no access to invoicing or administration.

It would not be practical to attach individual security rules to employees any time we need a change so groups link security rules and users. They correspond to roles that can be assigned to employees.

For most Odoo applications 1 a good baseline is to have user and manager (or administrator) roles: the manager can change the configuration of the application and oversee the entirety of its use while the user can well, use the application 2.

This baseline seems sufficient for us:

Real estate managers can configure the system (manage available types and tags) as well as oversee every property in the pipeline.

Real estate agents can manage the properties under their care, or properties which are not specifically under the care of any agent.

In keeping with Odoo’s data-driven nature, a group is no more than a record of the res.groups model. They are normally part of a module’s master data, defined in one of the module’s data files.

As simple example can be found here.

Create the security.xml file in the appropriate folder and add it to the __manifest__.py file.

If not already, add a 'category' field to your __manifest__.py with value Real Estate/Brokerage.

Add a record creating a group with the id estate_group_user, the name “Agent” and the category base.module_category_real_estate_brokerage.

Below that, add a record creating a group with the id estate_group_manager, the name “Manager” and the category base.module_category_real_estate_brokerage. The estate_group_manager group needs to imply estate_group_user.

Where does that category comes from ? It’s a module category. Here we used the category id base.module_category_real_estate_brokerage which was automatically generated by Odoo based on the category set in the __manifest__.py of the module. You can also find here the list of default module categories provided by Odoo.

Since we modified data files, remember to restart Odoo and update the module using -u estate.

If you go to Settings ‣ Manage Users and open the admin user (“Mitchell Admin”), you should see a new section:

Set the admin user to be a Real Estate manager.

Via the web interface, create a new user with only the “real estate agent” access. The user should not have any Invoicing or Administration access.

Use a private tab or window to log in with the new user (remember to set a password), as the real-estate agent you should only see the real estate application, and possibly the Discuss (chat) application:

The documentation related to this topic can be found at Access Rights.

At the end of this section,

Employees who are not at least real-estate agents will not see the real-estate application.

Real-estate agents will not be able to update the property types or tags.

Access rights were first introduced in Chapter 4: Security - A Brief Introduction.

Access rights are a way to give users access to models via groups: associate an access right to a group, then all users with that group will have the access.

For instance we don’t want real-estate agents to be able to modify what property types are available, so we would not link that access to the “user” group.

Access rights can only give access, they can’t remove it: when access is checked, the system looks to see if any access right associated with the user (via any group) grants that access.

A user with the groups A and C will be able to do anything but delete the object while one with B and C will be able to read and update it, but not create or delete it.

The group of an access right can be omitted, this means the ACL applies to every user, this is a useful but risky fallback as depending on the applications installed it can grant even non-users access to the model.

If no access right applies to a user, they are not granted access (default-deny).

If a menu item points to a model to which a user doesn’t have access and has no submenus which the user can see, the menu will not be displayed.

Update the access rights file to:

Give full access to all objects to your Real Estate Manager group.

Give agents (real estate users) only read access to types and tags.

Give nobody the right to delete properties.

Check that your agent user is not able to alter types or tags, or to delete properties, but that they can otherwise create or update properties.

Remember to give different xids to your ir.model.access records otherwise they will overwrite one another.

Since the “demo” user was not made a real-estate agent or manager, they should not even be able to see the real-estate application. Use a private tab or window to check for this (the “demo” user has the password “demo”).

The documentation related to this topic can be found at Record Rules.

At the end of this section, agents will not be able to see the properties exclusive to their colleagues; but managers will still be able to see everything.

Access rights can grant access to an entire model but often we need to be more specific: while an agent can interact with properties in general we may not want them to update or even see properties managed by one of their colleagues.

Record rules provide that precision: they can grant or reject access to individual records:

The Search domains is how access is managed: if the record passes then access is granted, otherwise access is rejected.

Because rules tends to be rather complex and not created in bulk, they’re usually created in XML rather than the CSV used for access rights.

Only applies to the “create”, “update” (write) and “delete” (unlink) operations: here we want every employee to be able to see other users’ records but only the author / assignee can update a record.

Is non-global so we can provide an additional rule for e.g. managers.

Allows the operation if the current user (user.id) is set (e.g. created, or is assigned) on the record, or if the record has no associated user at all.

If no rule is defined or applies to a model and operation, then the operation is allowed (default-allow), this can have odd effects if access rights are not set up correctly (are too permissive).

Define a rule which limits agents to only being able to see or modify properties which have no salesperson, or for which they are the salesperson.

You may want to create a second real-estate agent user, or create a few properties for which the salesperson is a manager or some other user.

Verify that your real estate manager(s) can still see all properties. If not, why not? Remember:

The estate_group_manager group needs to imply estate_group_user.

At the end of this section, agents should be able to confirm property sales without needing invoicing access.

If you try to mark a property as “sold” as the real estate agent, you should get an access error:

This happens because estate_account tries to create an invoice during the process, but creating an invoice requires the right to all invoice management.

We want agents to be able to confirm a sale without them having full invoicing access, which means we need to bypass the normal security checks of Odoo in order to create an invoice despite the current user not having the right to do so.

There are two main ways to bypass existing security checks in Odoo, either wilfully or as a side-effect:

The sudo() method will create a new recordset in “sudo mode”, this ignores all access rights and record rules (although hard-coded group and user checks may still apply).

Performing raw SQL queries will bypass access rights and record rules as a side-effect of bypassing the ORM itself.

Update estate_account to bypass access rights and rules when creating the invoice.

These features should generally be avoided, and only used with extreme care, after having checked that the current user and operation should be able to bypass normal access rights validation.

Operations performed in such modes should also rely on user input as little as possible, and should validate it to the maximum extent they can.

At the end of this section, the creation of the invoice should be resilient to security issues regardless to changes to estate.

In Odoo, access rights and record rules are only checked when performing data access via the ORM e.g. creating, reading, searching, writing, or unlinking a record via ORM methods. Other methods do not necessarily check against any sort of access rights.

In the previous section, we bypassed the record rules when creating the invoice in action_sold. This bypass can be reached by any user without any access right being checked:

Add a print to action_sold in estate_account before the creation of the invoice (as creating the invoice accesses the property, therefore triggers an ACL check) e.g.:

You should see reached in your Odoo log, followed by an access error.

Just because you’re already in Python code does not mean any access right or rule has or will be checked.

Currently the accesses are implicitly checked by accessing data on self as well as calling super() (which does the same and updates self), triggering access errors and cancelling the transaction “uncreating” our invoice.

However if this changes in the future, or we add side-effects to the method (e.g. reporting the sale to a government agency), or bugs are introduced in estate, … it would be possible for non-agents to trigger operations they should not have access to.

Therefore when performing non-CRUD operations, or legitimately bypassing the ORM or security, or when triggering other side-effects, it is extremely important to perform explicit security checks.

Explicit security checks can be performed by:

Checking who the current user is (self.env.user) and match them against specific models or records.

Checking that the current user has specific groups hard-coded to allow or deny an operation (self.env.user.has_group).

Calling check_access(operations) on a recordset, this verifies that the current user is allowed to perform the operation on every record of the set. As a special case, when the recordset is empty, it verifies that the current user has some access rights to perform the operation on the model in general.

Before creating the invoice, use check_access to ensure that the current user can update the property the invoice is for.

Re-run the bypass script, check that the error occurs before the print.

Multi-company Guidelines for an overview of multi-company facilities in general, and multi-company security rules in particular.

Documentation on rules in general can, again, be found at Record Rules.

At the end of this section, agents should only have access to properties of their agency (or agencies).

For one reason or another we might need to manage our real-estate business as multiple companies e.g. we might have largely autonomous agencies, a franchise setup, or multiple brands (possibly from having acquired other real-estate businesses) which remain legally or financially separate from one another.

Odoo can be used to manage multiple companies inside the same system, however the actual handling is up to individual modules: Odoo itself provides the tools to manage the issue of company-dependent fields and multi-company rules, which is what we’re going to concern ourselves with.

We want different agencies to be “siloed” from one another, with properties belonging to a given agency and users (whether agents or managers) only able to see properties linked to their agency.

As before, because this is based on non-trivial records it’s easier for a user to relax rules than to tighten them so it makes sense to default to a relatively stronger security model.

Multi-company rules are simply record rules based on the company_ids or company_id fields:

company_ids is all the companies to which the current user has access

company_id is the currently active company (the one the user is currently working in / for).

Multi-company rules will usually use the former i.e. check if the record is associated with one of the companies the user has access to:

Multi-company rules are usually global, otherwise there is a high risk that additional rules would allow bypassing the multi-company rules.

Add a company_id field to estate.property, it should be required (we don’t want agency-less properties), and should default to the current user’s current company.

Create a new company, with a new estate agent in that company.

The manager should be a member of both companies.

The old agent should only be a member of the old company.

Create a few properties in each company (either use the company selector as the manager or use the agents). Unset the default salesman to avoid triggering that rule.

All agents can see all companies, which is not desirable, add the record rule restricting this behaviour.

remember to --update your module when you change its model or data

At the end of this section, real-estate agents should not see the Settings menu of the real-estate application, but should still be able to set the property type or tags.

Specific Odoo models can be associated directly with groups (or companies, or users). It is important to figure out whether this association is a security or a visibility feature before using it:

Visibility features mean a user can still access the model or record otherwise, either through another part of the interface or by performing operations remotely using RPC, things might just not be visible in the web interface in some contexts.

Security features mean a user can not access records, fields or operations.

Here are some examples:

Groups on model fields (in Python) are a security feature, users outside the group will not be able to retrieve the field, or even know it exists.

Example: in server actions, only system users can see or update Python code.

Groups on view elements (in XML) are a visibility feature, users outside the group will not be able to see the element or its content in the form but they will otherwise be able to interact with the object (including that field).

Example: only managers have an immediate filter to see their teams’ leaves.

Groups on menus and actions are visibility features, the menu or action will not be shown in the interface but that doesn’t prevent directly interacting with the underlying object.

Example: only system administrators can see the elearning settings menu.

Real Estate agents can not add property types or tags, but can see their options from the Property form view when creating it.

The Settings menu just adds noise to their interface, make it only visible to managers.

Despite not having access to the Property Types and Property Tags menus anymore, agents can still access the underlying objects since they can still select tags or a type to set on their properties.

An Odoo Application is a group of related modules covering a business area or field, usually composed of a base module and a number of expansions on that base to add optional or specific features, or link to other business areas.

For applications which would be used by most or every employees, the “application user” role might be done away with and its abilities granted to all employees directly e.g. generally all employees can submit expenses or take time off.

**Examples:**

Example 1 (jsx):
```jsx
<record id="rule_id" model="ir.rule">
    <field name="name">A description of the rule's role</field>
    <field name="model_id" ref="model_to_manage"/>
    <field name="perm_read" eval="False"/>
    <field name="groups" eval="[Command.link(ref('base.group_user'))]"/>
    <field name="domain_force">[
        '|', ('user_id', '=', user.id),
             ('user_id', '=', False)
    ]</field>
</record>
```

Example 2 (unknown):
```unknown
print(" reached ".center(100, '='))
```

Example 3 (jsx):
```jsx
<record model="ir.rule" id="hr_appraisal_plan_comp_rule">
    <field name="name">Appraisal Plan multi-company</field>
    <field name="model_id" ref="model_hr_appraisal_plan"/>
    <field name="domain_force">[
        '|', ('company_id', '=', False),
             ('company_id', 'in', company_ids)
    ]</field>
</record>
```

---

## Branches¶

**URL:** https://www.odoo.com/documentation/19.0/administration/odoo_sh/getting_started/branches.html

**Contents:**
- Branches¶
- Stages¶
  - Production¶
  - Staging¶
  - Development¶
  - Merging branches¶
- Tabs¶
  - History¶
  - Mails¶
  - Shell¶

The Branches view provides an overview of the different branches in your repository.

Odoo.sh offers three different branch stages:

You can change the stage of a branch by dragging and dropping it under the desired stage.

Development branches can be moved under Staging. If you try to move a development branch under Production, a warning message will be displayed explaining that you can only have one production branch per project.

Staging branches can be moved under Development, but it is not possible to move them under Production.

The production branch can only be moved under Development. If you try to move it under Staging, you can only perform a merge. Refer to the merging section for a detailed explanation of this process.

The production branch contains the code used to run the production database. There can be only one production branch.

When you push a new commit to this branch, the production server is updated with the revised code and restarted.

If the changes require a module update, such as changing a form view, and you want the update to be performed automatically, you can increase the module’s version number in its manifest file (__manifest__.py). The platform then performs the update, during which the instance will be held temporarily unavailable for maintenance reasons.

This method is equivalent to upgrading the module using the Apps menu or the -u switch on the command line.

If the changes prevent the server from restarting or if the module update fails, the server is automatically reverted to the previous successful code revision, and the database is rolled back to its previous state. Access to the failed update’s log to troubleshoot it.

The demo data is not loaded, as it is not intended for use on a production database. The unit tests are not performed, as it would increase the unavailability time of the production database during the update.

Odoo.sh automatically backs up the production database. It keeps seven daily, four weekly, and three monthly backups. Each backup includes the database dump, the filestore (attachments and binary fields), logs, and sessions.

When using trial projects, the production branch and all staging branches are automatically set back to the development stage after 30 days.

Staging branches are meant to test new features using production data without compromising the actual production database with test records. They create neutralized duplicates of the production database.

The neutralization disables:

To test them, trigger them manually or re-enable them. Be aware that the platform will trigger them less often if no one is using the database in order to save resources.

They are instead intercepted using a mail catcher. An interface to view the emails sent by the database is provided in your Odoo.sh project. That way, no emails are sent to your contacts.

Payment providers and shipping connectors

They are put into test mode.

If you configure or view changes in a staging database, make sure to record them (noting them step by step, reproducing in production, etc.) or write them directly in the branch’s modules, using XML data files to override the default configuration or views. Check the first module documentation to view examples.

Unit tests are not performed. They rely on demo data, which is not loaded into the production and staging databases. If Odoo starts supporting running the units without demo data, Odoo.sh will then consider running the tests on staging databases.

Staging databases are not automatically backed up. Nevertheless, you can restore a backup of the production database in a staging branch for testing purposes or to manually recover data that has been accidentally deleted from the production database. It is possible to create manual backups of staging databases.

Databases created for staging branches are intended to last up to three months. After that, they can be automatically blocked without prior notice. Only rebuilding the branch will allow you to use that specific branch again.

Development branches create new databases using demo data to run the unit tests. The installed modules are those included in the branch. You can change this list of modules to install in the project settings.

When pushing a commit to a development branch, a new server is started, with a database created from scratch, and the branch is updated. The demo data is loaded, and the unit tests are performed by default to verify that the changes do not break any of the features being tested. You can disable the tests or allow specific tests to be run with custom tags by going to the branch’s settings.

Similarly to staging branches, emails are not sent but are intercepted by a mail catcher, and scheduled actions are not triggered as long as the database is not in use.

Development databases are not automatically backed up, and manual backups are not possible.

Databases created for development branches are intended to last approximately three days. After that, they can be automatically garbage-collected to make room for new databases without prior notice.

You can merge your branches by dragging and dropping them into each other.

To test the changes of development branches with the production data, you can either:

Merge the development branch into a staging branch by dragging and dropping it onto the desired branch; or

Drag and drop the development branch under the Staging section to make it a staging branch.

When the changes are ready for production, drag and drop the staging branch into the production branch to merge and deploy them.

You can merge development branches into the production branch directly. However, changes will not be validated against the production data through a staging branch, so there is a higher risk of encountering issues in the production database.

You can merge development branches into each other, and staging branches into each other.

You can also use git merge directly on your workstation to merge your branches. Odoo.sh is notified when new revisions are pushed to your branches.

Merging a staging branch into the production branch only merges the source code. Any changes made to the staging database are not passed to the production database. However, if you modify the code in the repository, it will be passed to the production branch when merging.

If you test configuration changes in staging branches, and want them to be applied to the production branch, you have to, either:

Write the configuration changes in XML data files to overide the default configuration or views in the branch, and then increase the version of the module in its manifest (__manifest__.py) to trigger the module update when merging the staging branch in the production branch.

This method is recommended for better scalability of your developments, as you will use the Git versioning features for all configuration changes, thereby ensuring traceability of your changes.

Pass them manually from the staging database to the production one by copying and pasting them.

The History tab gives an overview of the branch history:

The commit messages and their authors

The various events linked to the platform, such as stage changes, database imports, and backup restores

A status in the top right corner of each event indicates the current operation on the database (e.g., installation, update, backup import) or its outcome (e.g., test feedback, successful backup import). If an operation is successful, a Connect button appears, allowing you to access the database.

The Mails tab contains the mail catcher, which provides an overview of emails sent by the database.

The mail catcher is available for development and staging branches. Emails from the production database are actually sent and are not intercepted by the mail catcher.

The Shell tab provides shell access to the container.

Clicking Shell opens a new browser tab where you can run basic Linux commands (ls, top). You can open a shell on the database by running psql.

You can open multiple shell tabs at once and arrange their layout by dragging and dropping them.

Production instance shells are highlighted in red to emphasize the danger of manipulating production instances directly, while staging/development instance shells are highlighted in yellow.

Long-running shell instances/idle shell sessions can be terminated at any time to free up resources.

Here is an overview of useful commands that you can run an Odoo.sh database terminal:

odoo-bin shell: to open an Odoo shell

odoo-update: to update modules in the database

odoosh-restart: to restart Odoo.sh services (http or cron)

odoosh-storage: to check the storage usage of your instance’s container filesystem

psql: to open a database shell

mutt: to check how emails appear on text clients (staging and development instances)

lnav ~/logs/odoo.log: to navigate in your instance’s odoo.log file

ncdu: to launch the disk usage analyzer with an interactive interface

grep: to filter and find information in log or configuration files

Clicking Editor opens a new browser tab to access an online integrated development environment (IDE) to edit the source code. You can also open terminals, Python consoles, and Odoo shell consoles.

You can open multiple tabs and drag and drop them to arrange the layout as you wish.

Online editor documentation.

The Monitor tab displays various performance monitoring metrics of the current build.

Zoom in with your cursor to adjust the time range or select it manually from the time range selector. It is also possible to change the time zone.

Technical logs always use the UTC. To analyze these logs together with your monitoring metrics, ensure UTC is selected in the monitoring tool.

Similarly, when sending a support ticket, ensure the information you share is based on UTC, as Odoo uses this time zone to investigate performance issues.

The information is aggregated periodically. When this is the case, a blue dotted line is displayed, along with the tag Aggregate Date. This means that the data before this date will appear flattened when compared to the data after this date. Therefore, when using the monitoring tool, it is recommended to focus on recent events to get the most detailed information possible.

Dotted Lines of other colors help you relate to other changes on the build (database import, git push, etc.).

On each graph, an 𝕚 (information) icon is displayed in the top-left corner. Hover your mouse over it to get more details about what the graph represents.

The Memory graph displays information about memory consumption:

Memory container represents Odoo workers and container processes.

Memory postgresql represents the database.

The CPU graph displays information about CPU consumption:

CPU http represents Odoo workers.

CPU cron/mail represents scheduled actions and incoming emails.

CPU postgresql (database processes)

CPU other represents webshells, the editor, etc.

The Storage graph displays information about the storage used:

Container represents the filestore, log files, and user files.

Postgresql represents the database and indexes.

The Requests graph displays information about the number of HTTP requests per second:

HTTP successes represents successful requests.

HTTP errors represents failed requests (check odoo.log).

HTTP rate limited represents declined requests, possibly due to lack of workers.

The Concurrent requests (max) graph displays the maximum number of concurrent HTTP requests per second.

Database workers determine the number of concurrent requests that can be managed simultaneously. It is essential to have enough workers to handle all incoming requests as they arrive. However, having additional workers beyond this does not improve the speed at which requests are processed.

The Average Response time displays the average response time to HTTP requests (in milliseconds).

The Incoming graph displays data about the daily number of incoming emails:

Received Emails represents emails successfuly received.

Received Emails bounced represents emails unsuccessfully received.

The Outgoing graph displays data about the daily number of outgoing emails:

Sent Emails represents emails successfully sent.

Sent Emails bounced represents emails unsuccessfully sent.

The Logs tab offers a real-time view of your server’s logs.

Different logs are available:

pip.log: the Python dependencies installation

install.log: the database installation (for development branches, tests are included)

odoosh-import-database.log: the last imported dump process

odoo.log: the running server

update.log: the database updates

pg_slow_queries.log: psql queries that take an unusual amount of time

sh_webshell.log: the actions taken in the webshell

sh_editor.log: the actions taken in the editor

neutralize.log: the neutralization of the database (only staging)

When new lines are added to the logs, they are displayed automatically. If you scroll to the bottom, the browser scrolls automatically each time a new line is added.

You can pause the logs fetching process by clicking the (pause) button in the upper right corner. Otherwise, the process stops after five minutes. You can restart it by clicking the (play) button.

The Backups tab lists the available backups to download and restore, lets you perform a manual backup and import a database.

The production database is automatically backed up daily. Seven daily, four weekly, and three monthly backups are kept. Each backup includes the database dump, the filestore (attachments and binary fields), logs, and sessions.

You can refer to the estimated scheduling of automatic backups to gain a better understanding of how the system works. This file is updated daily, taking the current day as the departure point.

Staging and development databases are not automatically backed up. However, you can restore a backup of the production database in your staging branches, for testing purposes, or manually recover data that has been accidentally deleted from the production database.

The list contains the backups kept on the server of your production database. This server only keeps one month of backups: seven daily and four weekly backups.

Dedicated backup servers keep the same backups, as well as three additional monthly backups. To restore or download one of these monthly backups, contact Odoo Support.

When merging a commit updating the version of one or several modules (in __manifest__.py), or their linked Python dependencies (in requirements.txt), then Odoo.sh performs an automatic backup (flagged with type Update in the list), as either the container will be changed by the installation of new pip packages, either the database itself will be changed with the module update triggered afterwards. In these two cases, a backup is triggered as it may break something.

If the merged commit does not update the version of a module or linked dependencies, then no backup is triggered by Odoo.sh, as neither the container nor the database is modified; therefore, the platform considers this safe enough. As an extra precaution, you can make a manual backup before modifiyng production sources.

The purpose of manual backups is to create a specific snapshot of production or staging databases (not available for development). These remain available for seven days. However, there is a limit of five daily manual backups.

The Import Database feature accepts database archives from:

the standard Odoo database manager (available for on-premise Odoo servers under /web/database/manager)

the Odoo Online databases manager

the Odoo.sh Backups tab (using the (Download Options) button)

the Odoo.sh Builds view (by clicking Download DB dump)

The Upgrade tab can be used to upgrade production and staging branches of valid projects. For more information about the upgrade process, refer to the Upgrade documentation.

The Tools tab contains the code profiler. It is used to start a profiling session, recording the activities of Odoo workers running in the instance for a maximum of five minutes. You can choose to terminate the session earlier, as running the tool for a shorter duration reduces the amount of noise in the report.

After each session, an interactive flame graph is created to help you visualize how the Odoo workers allocate their time.

Running the profiler consumes a lot of server resources, so avoid letting it run for too long. The goal is to record a specific action in your database.

The Settings tab lists the configuration options available for the currently selected branch. The options vary for each stage.

You can change the branch’s behavior upon receiving a new commit for development and staging branches.

By default, a development branch creates a new build and a staging branch updates the previous build. This is useful if the feature you are working on requires a specific configuration, as you would not need to manually configure it again after every commit.

If you select New build for a staging branch, a fresh copy of the production build is created every time a commit is pushed.

A branch that is moved from staging to development is set automatically to Do nothing.

You can choose which modules should be installed automatically for development branches.

To change the default behavior, untick the Use Default option under Development build behavior and select one of the following options under Module Installation:

Install only my modules (does not include submodules): only installs the branch’s modules, excluding submodules. This is the default option.

Full installation (no test suite): installs the branch’s modules, submodules, and all standard Odoo modules. When running the full installation, the test suite is disabled.

Install a list of modules: installs the specified modules. To do so, enter their technical name, and separate them using commas (e.g., sale_management,website,accountant).

If the test suite is enabled, installing all standard Odoo modules can take up to one hour.

By default, the test suite for development branches is enabled. You can restrict which tests are run by entering test tags and separating them using commas (e.g., custom_tags,at_install,post_install).

To disable the test suite entirely, untick Validate the test suite on new builds.

You can change the version of Odoo for development branches, for example, to test upgraded code or develop features while your production database is in the process of being upgraded to a newer version, by selecting another Version.

By default, Latest is selected as the Revision, and the sources of your Odoo server are updated weekly automatically to benefit from the latest bug, security, and performance fixes.

To choose a specific revision instead, select it using the Revision field.

Revisions expire after three months. You will be notified by email when the revision’s expiration date approaches. If you have not taken any action when it expires, the Revision field is automatically set back to Latest.

You can configure additional <name>.odoo.com domains or your own custom domains for all branch types.

To use your own custom domain, it is necessary to:

Own or purchase the domain name.

Enter the domain name under Custom domains (e.g., www.mycompany.com), then click Add domain.

Configure the domain name (e.g., www.mycompany.com) using your registrar’s domain name manager with a CNAME record value set to your production database domain name (e.g., mycompany.odoo.com).

Bare domains (e.g., mycompany.com) are not accepted. They can only be configured using A records, which only accept IP addresses as their value. Therefore, a bare domain could suddenly cease to function, as the IP address of a database can change (e.g., following an upgrade, a hardware failure, a change of database hosting location).

To have both your bare domain (e.g., mycompany.com) and www domain (e.g., www.mycompany.com) working, it is necessary to redirect the bare domain to the www domain. .com. Most domain managers provide a way to configure this redirection, commonly referred to as a web redirection.

If the redirection is correctly set up, an SSL certificate is automatically generated using Let’s Encrypt within the hour, meaning your domain will be accessible through HTTPS.

If the domain of your email addresses uses the SPF or DKIM authentication protocol, it is necessary to authorize Odoo as a sending host in the domain name settings to increase the deliverability of outgoing emails. For more information, refer to the Configure DNS records to send emails in Odoo documentation.

If Odoo is not authorized as a sending host, your outgoing emails may be flagged as spam.

In the top right corner of the view, several shell commands are displayed. The commands can be copied using the clipboard button and then used in a terminal. In addition, some of them can be used directly from Odoo.sh’s interface.

The clone command is used to create a local copy of your Git repository.

--recurse-submodules to download the submodules of your repository

--branch main to check out to a specific branch of the repository (e.g., development)

The run button is not available as the command is used to create a local copy on your machine.

The fork command is used to create a new branch based on the current one.

git checkout -b main-1 main a command to create a new branch (e.g., development-1) based on the current branch (e.g., development)

git push -u origin development-1 a command to upload the new branch (e.g., development-1) to the remote repository

The merge command is used to combine changes on one branch into another branch.

git merge staging-1 a command to merge the changes of the current branch into another branch (e.g., staging-1)

git push -u origin staging a command to upload the merged changes to the remote repository branch (e.g., staging)

The SSH command is used to connect to a build using SSH.

To use the SSH command, it is necessary to set up an SSH key first. To do so:

Generate a new SSH key.

Copy the SSH key to your clipboard.

On Odoo.sh, click your GitHub user in the top-right corner and select Profile.

Paste the SSH key under the Add a key manually field and click Add.

25004381 the build ID

my-user-my-repository-staging-25004381.dev.odoo.com the domain used to connect to the build

Provided you have the necessary access rights on the project, you will be granted SSH access to the build.

Long-running SSH connections are not guaranteed. Idle connections can be disconnected to free up resources.

The submodule command is used to add a branch from another repository to your current branch as a submodule.

Submodules documentation

git submodule add -b master <URL> <PATH> a command to add a specific branch (e.g., master) of a repository (<URL>) as a submodule under the specified path (<PATH>) in your current branch.

git commit -a a command to commit all current changes

git push -u origin staging a command to upload the changes of the current branch (e.g., staging) to the remote repository.

The delete command is used to delete a branch from your repository.

Once you delete a branch, there is no way to retrieve it unless a backup exists. Staging branches are not automatically backed up, but can be manually. Development branches cannot be backed up.

git push origin :staging a command to delete a specific branch (e.g., staging) on the remote repository

git branch -D staging a command to delete the specific branch on your local copy of the repository

Before deleting a branch, refer to the Backups section to better understand how they work and when you should create a manual backup.

**Examples:**

Example 1 (python):
```python
git clone --recurse-submodules --branch development git@github.com:my-organization/my-repository.git
```

Example 2 (unknown):
```unknown
git checkout -b main-1 development && git push -u origin development-1
```

Example 3 (unknown):
```unknown
git merge staging-1 && git push -u origin staging
```

Example 4 (python):
```python
ssh 25004381@my-user-my-repository-staging-25004381.dev.odoo.com
```

---

## Settings¶

**URL:** https://www.odoo.com/documentation/19.0/administration/odoo_sh/getting_started/settings.html

**Contents:**
- Settings¶
- Project name¶
- Collaborators¶
  - Feature access by stage and role¶
- Public access¶
- GitHub commit statuses¶
- GitHub key and webhook¶
- Submodules¶
- Production database size¶
- Database workers¶

The Settings view allow you to manage the configuration of your project.

The name of your project defines the address used to access your production database. The addresses of your staging and development builds are automatically derived from this name. If you change the project name, only future builds will use the new name.

To grant access to a GitHub user, enter their username and click Add. By default, the user is is granted the Developer role. Click the dropdown menu to select another one:

Admin: full access to all Odoo.sh features and tools. This role is dedicated to project management and has exclusive access to the project’s settings.

Tester: access to staging and development databases and their tools. This role is intended for users performing user acceptance testing (UAT). Testers can work with copies of production data, but they cannot access the production database through Odoo.sh’s tools.

Developer: no access to the production and staging databases. This role is intended for users who modify the code but should not access production data. Developers cannot connect to the production or staging databases and do not have access to the web shell or server logs.

Connect + / Connect as

Connect + / Connect as

Connect + / Connect as

Only admins can access the audit logs and the project settings.

All roles can access the builds page, but the features available are specific to each role.

When enabling Allow public access, the builds page becomes publicly accessible, allowing visitors to connect to development builds. Visitors can also access logs, the shell, and mails for development builds. Production and staging builds remain private; visitors can only view their status.

You can add a GitHub token to allow Odoo.sh to push commit statuses back to GitHub. The token must have the commit statuses (write) repository permission.

For more information, refer to GitHub’s documentation on managing access tokens.

A deploy key and a webhook are automatically created on your GitHub repository to allow Odoo.sh to fetch code and receive commit notifications. As they can be unintentionally modified or deleted, you can use the Verify Deploy Key and Verify Webhook buttons below to verify their configuration.

Administrative rights on the GitHub repository are necessary.

The git submodule command allows you to integrate other Git projects into your codebase without copying the code directly.

A Git repository containing Odoo modules, for example from the Odoo Apps Store or community modules, is necessary.

Before adding private GitHub repository as a submodule, it is necessary to add a deploy key:

Paste the SSH URL of the sub-repository (e.g., git@github.com:USERNAME/REPOSITORY.git) and click Add.

Copy the generated Public Key.

On the sub-repository’s GitHub, go to Settings ‣ Deploy keys.

Click Add deploy key, enter a Title, paste the public key into the Key field, and click Add key.

To add a public repository or private repository with a deploy key as a submodule:

Add the submodule to your project.

Commit and push the change.

Wait for Odoo.sh to rebuild the project.

This section displays the total storage used by the project. It includes the PostgreSQL database size and disk files in your container (database filestore, session storage, etc.). If the production database exceeds the storage included in your subscription, the plan will be automatically adjusted.

To analyze disk usage, run the Ncdu tool in the web shell.

Additional database workers can be configured to allow your production database to handle higher concurrent loads.

To add more workers, contact your account manager. After payment, the new worker(s) will be added to your project.

Adding more workers does not automatically fix performance issues. It only increases the number of concurrent connections the server can handle. If some operations remain slow, the issue is likely code-related. If it is not due to your customizations, contact Odoo Support.

Additional staging branches allow you to develop and test multiple features simultaneously. To add more staging branches, request a product increase directly from your Odoo.sh project. A widget will guide you to the subscription portal to complete the purchase. After payment, synchronization with Odoo.sh will occur automatically, and the number of available staging branches will be updated.

This section shows the activation status of the project. You can change the activation code if necessary, provided the new code is not already assigned to another project.

You cannot change the activation code to:

A code already used in another project

A trial code (downgrading from paid to trial is not allowed)

An invalid code (not linked to an Odoo.sh custom plan)

For any other issue, contact Odoo Support.

**Examples:**

Example 1 (python):
```python
git submodule add -b BRANCH git@github.com:USERNAME/REPOSITORY.git PATH
```

Example 2 (unknown):
```unknown
git commit -a && git push -u origin master
```

---

## Chapter 5 - Dynamic templates¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/website_theme/05_dynamic_templates.html

**Contents:**
- Chapter 5 - Dynamic templates¶
- Adapt the shop template¶
- Adapt the product page template¶

Now, let’s adapt the dynamic sections of the website. As you may know, some pages such as those for eCommerce are automatically generated. Pages like the shop, product, and checkout are automatically generated when the website_sale application is installed. These template pages pull their displayed information from the backend.

To modify these pages, we need to edit the standard Odoo template. This can be done using SCSS, presets, and especially XPath. Locate the standard Odoo template you want to modify and extend it using XPath. Following the Airproof design, let’s begin by modifying the shop view.

First, locate the standard template in Odoo : website_sale ‣ templates.xml ‣ id=”products”.

Apply all changes in your website_sale_templates.xml file. Start by:

Adapt the filtering on the left.

Remove the search bar (you can remove it from both the shop and the product pages at the same time).

Hide the list or grid view option.

Display only 3 products per row.

Create the appropriate design and information for the product cards.

Apply your modifications using presets, XPath and SCSS.

To enable attribute/variant filtering, activate the Product variants option in the website backend settings and configure attributes and variants for the products.

Find the solution in our Airproof example on presets.xml, website.xml, website_sale_templates.xml part shop page, and shop.scss.

The client is thrilled with the shop modifications. Next, let’s apply our design to the product pages. Based on the Airproof design below, adapt a few elements including:

Remove the search bar (if not done with the previous exercise).

Remove the quantity selector, Terms and Conditions, and share icons.

Update the Add to cart button icon.

Enable the accordion “More information”.

Insert the product specifications (section that only appears when the product has just one variant per attribute) into the accordion containing the “more information” section.

Design the appropriate layout for the product images carousel.

Add a title and apply the previously created product template to the Alternative products section (ensure alternative products are assigned on the product in the backend for this section to appear).

Implement a new drop zone below product details, visible on all products. As a use case, add an Text-Image building block using the Website Builder (e.g.: See Airproof product page screenshot with “Six reasons to buy…”).

See reference documentation on how to create a Drop zone.

Make your modifications using presets, XPath, and SCSS. Be sure to comment your code properly so you can find your way around.

The drop zone will be visible on all products. To create a specific dropzone per product, you need to add a new field to the product model.

Find the solution in our Airproof example on presets.xml, website_sale_templates.xml part product page, and product_page.scss.

---

## Chapter 4: Security - A Brief Introduction¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/server_framework_101/04_securityintro.html

**Contents:**
- Chapter 4: Security - A Brief Introduction¶
- Data Files (CSV)¶
- Access Rights¶

In the previous chapter, we created our first table intended to store business data. In a business application such as Odoo, one of the first questions to consider is who1 can access the data. Odoo provides a security mechanism to allow access to the data for specific groups of users.

The topic of security is covered in more detail in Restrict access to data. This chapter aims to cover the minimum required for our new module.

Odoo is a highly data driven system. Although behavior is customized using Python code, part of a module’s value is in the data it sets up when loaded. One way to load data is through a CSV file. One example is the list of country states which is loaded at installation of the base module.

id is an external identifier. It can be used to refer to the record (without knowing its in-database identifier).

country_id:id refers to the country by using its external identifier.

name is the name of the state.

code is the code of the state.

These three fields are defined in the res.country.state model.

By convention, a file importing data is located in the data folder of a module. When the data is related to security, it is located in the security folder. When the data is related to views and actions (we will cover this later), it is located in the views folder. Additionally, all of these files must be declared in the data list within the __manifest__.py file. Our example file is defined in the manifest of the base module.

Also note that the content of the data files is only loaded when a module is installed or updated.

The data files are sequentially loaded following their order in the __manifest__.py file. This means that if data A refers to data B, you must make sure that B is loaded before A.

In the case of the country states, you will note that the list of countries is loaded before the list of country states. This is because the states refer to the countries.

Why is all this important for security? Because all the security configuration of a model is loaded through data files, as we’ll see in the next section.

Reference: the documentation related to this topic can be found in Access Rights.

Goal: at the end of this section, the following warning should not appear anymore:

When no access rights are defined on a model, Odoo determines that no users can access the data. It is even notified in the log:

Access rights are defined as records of the model ir.model.access. Each access right is associated with a model, a group (or no group for global access) and a set of permissions: create, read, write and unlink2. Such access rights are usually defined in a CSV file named ir.model.access.csv.

Here is an example for our previous test_model:

id is an external identifier.

name is the name of the ir.model.access.

model_id/id refers to the model which the access right applies to. The standard way to refer to the model is model_<model_name>, where <model_name> is the _name of the model with the . replaced by _. Seems cumbersome? Indeed it is…

group_id/id refers to the group which the access right applies to.

perm_read,perm_write,perm_create,perm_unlink: read, write, create and unlink permissions

Create the ir.model.access.csv file in the appropriate folder and define it in the __manifest__.py file.

Give the read, write, create and unlink permissions to the group base.group_user.

Tip: the warning message in the log gives you most of the solution ;-)

Restart the server and the warning message should have disappeared!

It’s now time to finally interact with the UI!

meaning which Odoo user (or group of users)

‘unlink’ is the equivalent of ‘delete’

**Examples:**

Example 1 (unknown):
```unknown
"id","country_id:id","name","code"
state_au_1,au,"Australian Capital Territory","ACT"
state_au_2,au,"New South Wales","NSW"
state_au_3,au,"Northern Territory","NT"
state_au_4,au,"Queensland","QLD"
...
```

Example 2 (unknown):
```unknown
WARNING rd-demo odoo.modules.loading: The models ['estate.property'] have no access rules...
```

Example 3 (julia):
```julia
WARNING rd-demo odoo.modules.loading: The models ['estate.property'] have no access rules in module estate, consider adding some, like:
id,name,model_id:id,group_id:id,perm_read,perm_write,perm_create,perm_unlink
```

Example 4 (unknown):
```unknown
id,name,model_id/id,group_id/id,perm_read,perm_write,perm_create,perm_unlink
access_test_model,access_test_model,model_test_model,base.group_user,1,0,0,0
```

---

## Setup guide¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/setup_guide.html

**Contents:**
- Setup guide¶
- Adapt the environment for the tutorials¶
- Run the server¶
  - Launch with odoo-bin¶
  - Log in to Odoo¶
- Enable the developer mode¶
- Extra tools¶
  - Useful Git commands¶
  - Code Editor¶
  - Administrator tools for PostgreSQL¶

Depending on the intended use case, there are multiple ways to install Odoo. For developers of the Odoo community and Odoo employees alike, the preferred way is to perform a source install (running Odoo from the source code).

Follow the Environment setup section of the contributing guide to prepare your environment for pushing local changes to the Odoo repositories.

By now, you should have downloaded the source code into two local repositories, one for odoo/odoo and one for odoo/enterprise. These repositories are set up to push changes to pre-defined forks on GitHub. This will prove to be convenient when you start contributing to the codebase, but in the scope of following a tutorial, we want to avoid polluting them with training material. Let’s then push your changes in a third repository: odoo/tutorials. Like the first two repositories, it will be part of the addons-path that references all directories containing Odoo modules.

Depending on the tutorial that you are following, you might not need to install all the modules that this repository contains.

Following the same process as with the odoo/odoo and odoo/enterprise repositories, clone the odoo/tutorials repository on your machine with:

Configure your fork and Git to push changes to your fork rather than to the main codebase. If you work at Odoo, configure Git to push changes to the shared fork created on the account odoo-dev.

Visit github.com/odoo/tutorials and click the Fork button to create a fork of the repository on your account.

In the command below, replace <your_github_account> with the name of the GitHub account on which you created the fork.

That’s it! Your environment is now prepared to run Odoo from the sources, and you have successfully created a repository to serve as an addons directory. This will allow you to push your work to GitHub.

For Odoo employees only:

Make sure to read very carefully Make your first contribution. In particular, your branch name must follow our conventions.

Once you have pushed your first change to the shared fork on odoo-dev, create a PR. Please put your quadrigram in the PR title (e.g., “abcd - Technical Training”).

This will enable you to share your upcoming work and receive feedback from your coaches. To ensure a continuous feedback loop, we recommend pushing a new commit as soon as you complete a chapter of the tutorial. Note that the PR is automatically updated with commits you push to odoo-dev, you don’t need to open multiple PRs.

At Odoo we use Runbot extensively for our CI tests. When you push your changes to odoo-dev, Runbot creates a new build and test your code. Once logged in, you will be able to see your branches Tutorials project.

The specific location of the repositories on your file system is not crucial. However, for the sake of simplicity, we will assume that you have cloned all the repositories under the same directory. If this is not the case, make sure to adjust the following commands accordingly, providing the appropriate relative path from the odoo/odoo repository to the odoo/tutorials repository.

Once all dependencies are set up, Odoo can be launched by running odoo-bin, the command-line interface of the server.

There are multiple command-line arguments that you can use to run the server. In this training you will only need some of them.

The database that is going to be used.

A comma-separated list of directories in which modules are stored. These directories are scanned for modules.

Prevent the worker from using more than <limit> CPU seconds for each request.

Prevent the worker from taking longer than <limit> seconds to process a request.

The --limit-time-cpu and --limit-time-real arguments can be used to prevent the worker from being killed when debugging the source code.

Other commonly used arguments are:

-i: Install some modules before running the server (comma-separated list). This is equivalent to going to Apps in the user interface, and installing the module from there.

-u: Update some modules before running the server (comma-separated list). This is equivalent to going to Apps in the user interface, selecting a module, and upgrading it from there.

Open http://localhost:8069/ on your browser. We recommend using Chrome, Firefox, or any other browser with development tools.

To log in as the administrator user, use the following credentials:

The developer or debug mode is useful for training as it gives access to additional (advanced) tools. Enable the developer mode now. Choose the method that you prefer; they are all equivalent.

Here are some useful Git commands for your day-to-day work.

If you are working at Odoo, many of your colleagues are using VSCode, VSCodium (the open source equivalent), PyCharm, or Sublime Text. However, you are free to choose your preferred editor.

It is important to configure your linters correctly. Using a linter helps you by showing syntax and semantic warnings or errors. Odoo source code tries to respect Python’s and JavaScript’s standards, but some of them can be ignored.

For Python, we use PEP8 with these options ignored:

E301: expected 1 blank line, found 0

E302: expected 2 blank lines, found 1

For JavaScript, we use ESLint and you can find a configuration file example here.

You can manage your PostgreSQL databases using the command line as demonstrated earlier or using a GUI application such as pgAdmin or DBeaver.

To connect the GUI application to your database we recommend you connect using the Unix socket.

Host name/address: /var/run/postgresql

When facing a bug or trying to understand how the code works, simply printing things out can go a long way, but a proper debugger can save a lot of time.

You can use a classic Python library debugger (pdb, pudb or ipdb), or you can use your editor’s debugger.

In the following example we use ipdb, but the process is similar with other libraries.

Place a trigger (breakpoint):

Here is a list of commands:

Print the list of available commands if not argument is supplied. With a command as an argument, print the help about that command.

The value of the expression is pretty-printed using the pprint module.

Print a stack trace with the most recent frame at the bottom.

Move the current frame one level down in the stack trace (to a newer frame).

Move the current frame one level up in the stack trace (to an older frame).

Continue the execution until the next line in the current function is reached or it returns.

Continue the execution and only stop when a breakpoint is encountered.

Execute the current line. Stop at the first possible occasion (either in a function that is called or on the next line in the current function).

Quit the debugger. The program being executed is aborted.

**Examples:**

Example 1 (python):
```python
$ git clone git@github.com:odoo/tutorials.git
```

Example 2 (typescript):
```typescript
$ cd /TutorialsPath
$ git remote add dev git@github.com:<your_github_account>/tutorials.git
```

Example 3 (powershell):
```powershell
$ cd /tutorials
$ git remote add dev git@github.com:odoo-dev/tutorials.git
$ git remote set-url --push origin you_should_not_push_on_this_repository
```

Example 4 (bash):
```bash
$ cd $HOME/src/odoo/
$ ./odoo-bin --addons-path="addons/,../enterprise/,../tutorials" -d rd-demo
```

---

## Chapter 5: Finally, Some UI To Play With¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/server_framework_101/05_firstui.html

**Contents:**
- Chapter 5: Finally, Some UI To Play With¶
- Data Files (XML)¶
- Actions¶
- Menus¶
- Fields, Attributes And View¶
  - Some New Attributes¶
  - Default Values¶
  - Reserved Fields¶

Now that we’ve created our new model and its corresponding access rights, it is time to interact with the user interface.

At the end of this chapter, we will have created a couple of menus in order to access a default list and form view.

Reference: the documentation related to this topic can be found in Data Files.

In Chapter 4: Security - A Brief Introduction, we added data through a CSV file. The CSV format is convenient when the data to load has a simple format. When the format is more complex (e.g. load the structure of a view or an email template), we use the XML format. For example, this help field contains HTML tags. While it would be possible to load such data through a CSV file, it is more convenient to use an XML file.

The XML files must be added to the same folders as the CSV files and defined similarly in the __manifest__.py. The content of the data files is also sequentially loaded when a module is installed or updated, therefore all remarks made for CSV files hold true for XML files. When the data is linked to views, we add them to the views folder.

In this chapter, we will load our first action and menus through an XML file. Actions and menus are standard records in the database.

When performance is important, the CSV format is preferred over the XML format. This is the case in Odoo where loading a CSV file is faster than loading an XML file.

In Odoo, the user interface (actions, menus and views) is largely defined by creating and composing records defined in an XML file. A common pattern is Menu > Action > View. To access records the user navigates through several menu levels; the deepest level is an action which triggers the opening of a list of the records.

Reference: the documentation related to this topic can be found in Actions.

Goal: at the end of this section, an action should be loaded in the system. We won’t see anything yet in the UI, but the file should be loaded in the log:

Actions can be triggered in three ways:

by clicking on menu items (linked to specific actions)

by clicking on buttons in views (if these are connected to actions)

as contextual actions on object

We will only cover the first case in this chapter. The second case will be covered in a later chapter while the last is the focus of an advanced topic. In our Real Estate example, we would like to link a menu to the estate.property model, so we are able to create a new record. The action can be viewed as the link between the menu and the model.

A basic action for our test_model is:

id is an external identifier. It can be used to refer to the record (without knowing its in-database identifier).

model has a fixed value of ir.actions.act_window (Window Actions (ir.actions.act_window)).

name is the name of the action.

res_model is the model which the action applies to.

view_mode are the views that will be available; in this case they are the list and form views. We’ll see later that there can be other view modes.

Examples can be found everywhere in Odoo, but this is a good example of a simple action. Pay attention to the structure of the XML data file since you will need it in the following exercise.

Create the estate_property_views.xml file in the appropriate folder and define it in the __manifest__.py file.

Create an action for the model estate.property.

Restart the server and you should see the file loaded in the log.

Reference: the documentation related to this topic can be found in Shortcuts.

Goal: at the end of this section, three menus should be created and the default view is displayed:

To reduce the complexity in declaring a menu (ir.ui.menu) and connecting it to the corresponding action, we can use the <menuitem> shortcut .

A basic menu for our test_model_action is:

The menu test_model_menu_action is linked to the action test_model_action, and the action is linked to the model test_model. As previously mentioned, the action can be seen as the link between the menu and the model.

However, menus always follow an architecture, and in practice there are three levels of menus:

The root menu, which is displayed in the App switcher (the Odoo Community App switcher is a dropdown menu)

The first level menu, displayed in the top bar

The easiest way to define the structure is to create it in the XML file. A basic structure for our test_model_action is:

The name for the third menu is taken from the name of the action.

Create the estate_menus.xml file in the appropriate folder and define it in the __manifest__.py file. Remember the sequential loading of the data files ;-)

Create the three levels of menus for the estate.property action created in the previous exercise. Refer to the Goal of this section for the expected result.

Restart the server and refresh the browser1. You should now see the menus, and you’ll even be able to create your first real estate property advertisement!

Goal: at the end of this section, the selling price should be read-only and the number of bedrooms and the availability date should have default values. Additionally the selling price and availability date values won’t be copied when the record is duplicated.

The reserved fields active and state are added to the estate.property model.

So far we have only used the generic view for our real estate property advertisements, but in most cases we want to fine tune the view. There are many fine-tunings possible in Odoo, but usually the first step is to make sure that:

some fields have a default value

some fields are read-only

some fields are not copied when duplicating the record

In our real estate business case, we would like the following:

The selling price should be read-only (it will be automatically filled in later)

The availability date and the selling price should not be copied when duplicating a record

The default number of bedrooms should be 2

The default availability date should be in 3 months

Before moving further with the view design, let’s step back to our model definition. We saw that some attributes, such as required=True, impact the table schema in the database. Other attributes will impact the view or provide default values.

Add new attributes to the fields.

Find the appropriate attributes (see Field) to:

set the selling price as read-only

prevent copying of the availability date and the selling price values

Restart the server and refresh the browser. You should not be able to set any selling prices. When duplicating a record, the availability date should be empty.

Any field can be given a default value. In the field definition, add the option default=X where X is either a Python literal value (boolean, integer, float, string) or a function taking a model and returning a value:

The name field will have the value ‘Unknown’ by default while the last_seen field will be set as the current time.

Add the appropriate default attributes so that:

the default number of bedrooms is 2

the default availability date is in 3 months

Tip: this might help you: today()

Check that the default values are set as expected.

Reference: the documentation related to this topic can be found in Reserved Field names.

A few field names are reserved for pre-defined behaviors. They should be defined on a model when the related behavior is desired.

Add the active field to the estate.property model.

Restart the server, create a new property, then come back to the list view… The property will not be listed! active is an example of a reserved field with a specific behavior: when a record has active=False, it is automatically removed from any search. To display the created property, you will need to specifically search for inactive records.

Set a default value for active field.

Set the appropriate default value for the active field so it doesn’t disappear anymore.

Note that the default active=False value was assigned to all existing records.

Add a state field to the estate.property model. Five values are possible: New, Offer Received, Offer Accepted, Sold and Cancelled. It must be required, should not be copied and should have its default value set to ‘New’.

Make sure to use the correct type!

The state will be used later on for several UI enhancements.

Now that we are able to interact with the UI thanks to the default views, the next step is obvious: we want to define our own views.

A refresh is needed since the web client keeps a cache of the various menus and views for performance reasons.

**Examples:**

Example 1 (unknown):
```unknown
INFO rd-demo odoo.modules.loading: loading estate/views/estate_property_views.xml
```

Example 2 (jsx):
```jsx
<record id="test_model_action" model="ir.actions.act_window">
    <field name="name">Test action</field>
    <field name="res_model">test_model</field>
    <field name="view_mode">list,form</field>
</record>
```

Example 3 (jsx):
```jsx
<menuitem id="test_model_menu_action" action="test_model_action"/>
```

Example 4 (jsx):
```jsx
<menuitem id="test_menu_root" name="Test">
    <menuitem id="test_first_level_menu" name="First Level">
        <menuitem id="test_model_menu_action" action="test_model_action"/>
    </menuitem>
</menuitem>
```

---

## Chapter 8: Computed Fields And Onchanges¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/server_framework_101/08_compute_onchange.html

**Contents:**
- Chapter 8: Computed Fields And Onchanges¶
- Computed Fields¶
  - Dependencies¶
  - Inverse Function¶
  - Additional Information¶
- Onchanges¶
  - Additional Information¶
- How to use them?¶

The relations between models are a key component of any Odoo module. They are necessary for the modelization of any business case. However, we may want links between the fields within a given model. Sometimes the value of one field is determined from the values of other fields and other times we want to help the user with data entry.

These cases are supported by the concepts of computed fields and onchanges. Although this chapter is not technically complex, the semantics of both concepts is very important. This is also the first time we will write Python logic. Until now we haven’t written anything other than class definitions and field declarations.

Reference: the documentation related to this topic can be found in Computed Fields.

Goal: at the end of this section:

In the property model, the total area and the best offer should be computed:

In the property offer model, the validity date should be computed and can be updated:

In our real estate module, we have defined the living area as well as the garden area. It is then natural to define the total area as the sum of both fields. We will use the concept of a computed field for this, i.e. the value of a given field will be computed from the value of other fields.

So far fields have been stored directly in and retrieved directly from the database. Fields can also be computed. In this case, the field’s value is not retrieved from the database but computed on-the-fly by calling a method of the model.

To create a computed field, create a field and set its attribute compute to the name of a method. The computation method should set the value of the computed field for every record in self.

By convention, compute methods are private, meaning that they cannot be called from the presentation tier, only from the business tier (see Chapter 1: Architecture Overview). Private methods have a name starting with an underscore _.

The value of a computed field usually depends on the values of other fields in the computed record. The ORM expects the developer to specify those dependencies on the compute method with the decorator depends(). The given dependencies are used by the ORM to trigger the recomputation of the field whenever some of its dependencies have been modified:

self is a collection.

The object self is a recordset, i.e. an ordered collection of records. It supports the standard Python operations on collections, e.g. len(self) and iter(self), plus extra set operations such as recs1 | recs2.

Iterating over self gives the records one by one, where each record is itself a collection of size 1. You can access/assign fields on single records by using the dot notation, e.g. record.name.

Many examples of computed fields can be found in Odoo. Here is a simple one.

Compute the total area.

Add the total_area field to estate.property. It is defined as the sum of the living_area and the garden_area.

Add the field in the form view as depicted on the first image of this section’s Goal.

For relational fields it’s possible to use paths through a field as a dependency:

The example is given with a Many2one, but it is valid for Many2many or a One2many. An example can be found here.

Let’s try it in our module with the following exercise!

Compute the best offer.

Add the best_price field to estate.property. It is defined as the highest (i.e. maximum) of the offers’ price.

Add the field to the form view as depicted in the first image of this section’s Goal.

Tip: you might want to try using the mapped() method. See here for a simple example.

You might have noticed that computed fields are read-only by default. This is expected since the user is not supposed to set a value.

In some cases, it might be useful to still be able to set a value directly. In our real estate example, we can define a validity duration for an offer and set a validity date. We would like to be able to set either the duration or the date with one impacting the other.

To support this Odoo provides the ability to use an inverse function:

An example can be found here.

A compute method sets the field while an inverse method sets the field’s dependencies.

Note that the inverse method is called when saving the record, while the compute method is called at each change of its dependencies.

Compute a validity date for offers.

Add the following fields to the estate.property.offer model:

Where date_deadline is a computed field which is defined as the sum of two fields from the offer: the create_date and the validity. Define an appropriate inverse function so that the user can set either the date or the validity.

Tip: the create_date is only filled in when the record is created, therefore you will need a fallback to prevent crashing at time of creation.

Add the fields in the form view and the list view as depicted on the second image of this section’s Goal.

Computed fields are not stored in the database by default. Therefore it is not possible to search on a computed field unless a search method is defined. This topic is beyond the scope of this training, so we won’t cover it. An example can be found here.

Another solution is to store the field with the store=True attribute. While this is usually convenient, pay attention to the potential computation load added to your model. Lets re-use our example:

Every time the partner name is changed, the description is automatically recomputed for all the records referring to it! This can quickly become prohibitive to recompute when millions of records need recomputation.

It is also worth noting that a computed field can depend on another computed field. The ORM is smart enough to correctly recompute all the dependencies in the right order… but sometimes at the cost of degraded performance.

In general performance must always be kept in mind when defining computed fields. The more complex is your field to compute (e.g. with a lot of dependencies or when a computed field depends on other computed fields), the more time it will take to compute. Always take some time to evaluate the cost of a computed field beforehand. Most of the time it is only when your code reaches a production server that you realize it slows down a whole process. Not cool :-(

Reference: the documentation related to this topic can be found in onchange():

Goal: at the end of this section, enabling the garden will set a default area of 10 and an orientation to North.

In our real estate module, we also want to help the user with data entry. When the ‘garden’ field is set, we want to give a default value for the garden area as well as the orientation. Additionally, when the ‘garden’ field is unset we want the garden area to reset to zero and the orientation to be removed. In this case, the value of a given field modifies the value of other fields.

The ‘onchange’ mechanism provides a way for the client interface to update a form without saving anything to the database whenever the user has filled in a field value. To achieve this, we define a method where self represents the record in the form view and decorate it with onchange() to specify which field it is triggered by. Any change you make on self will be reflected on the form:

In this example, changing the partner will also change the name and the description values. It is up to the user whether or not to change the name and description values afterwards. Also note that we do not loop on self, this is because the method is only triggered in a form view, where self is always a single record.

Set values for garden area and orientation.

Create an onchange in the estate.property model in order to set values for the garden area (10) and orientation (North) when garden is set to True. When unset, clear the fields.

Onchanges methods can also return a non-blocking warning message (example).

There is no strict rule for the use of computed fields and onchanges.

In many cases, both computed fields and onchanges may be used to achieve the same result. Always prefer computed fields since they are also triggered outside of the context of a form view. Never ever use an onchange to add business logic to your model. This is a very bad idea since onchanges are not automatically triggered when creating a record programmatically; they are only triggered in the form view.

The usual pitfall of computed fields and onchanges is trying to be ‘too smart’ by adding too much logic. This can have the opposite result of what was expected: the end user is confused from all the automation.

Computed fields tend to be easier to debug: such a field is set by a given method, so it’s easy to track when the value is set. Onchanges, on the other hand, may be confusing: it is very difficult to know the extent of an onchange. Since several onchange methods may set the same fields, it easily becomes difficult to track where a value is coming from.

When using stored computed fields, pay close attention to the dependencies. When computed fields depend on other computed fields, changing a value can trigger a large number of recomputations. This leads to poor performance.

In the next chapter, we’ll see how we can trigger some business logic when buttons are clicked.

**Examples:**

Example 1 (python):
```python
from odoo import api, fields, models

class TestComputed(models.Model):
    _name = "test.computed"

    total = fields.Float(compute="_compute_total")
    amount = fields.Float()

    @api.depends("amount")
    def _compute_total(self):
        for record in self:
            record.total = 2.0 * record.amount
```

Example 2 (python):
```python
description = fields.Char(compute="_compute_description")
partner_id = fields.Many2one("res.partner")

@api.depends("partner_id.name")
def _compute_description(self):
    for record in self:
        record.description = "Test for partner %s" % record.partner_id.name
```

Example 3 (python):
```python
from odoo import api, fields, models

class TestComputed(models.Model):
    _name = "test.computed"

    total = fields.Float(compute="_compute_total", inverse="_inverse_total")
    amount = fields.Float()

    @api.depends("amount")
    def _compute_total(self):
        for record in self:
            record.total = 2.0 * record.amount

    def _inverse_total(self):
        for record in self:
            record.amount = record.total / 2.0
```

Example 4 (python):
```python
description = fields.Char(compute="_compute_description", store=True)
partner_id = fields.Many2one("res.partner")

@api.depends("partner_id.name")
def _compute_description(self):
    for record in self:
        record.description = "Test for partner %s" % record.partner_id.name
```

---

## Write importable modules¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/importable_modules.html

**Contents:**
- Write importable modules¶
- Problem statement¶
- Module structure¶
- Deploying the module¶
- Models and basic fields¶
  - Default values¶
- Security¶
- Views¶
- Relations¶
  - Many-to-one¶

This tutorial assumes familiarity with the Server framework 101 tutorial and the Define module data tutorial.

Although, as developers, we prefer to have the full power of Python to write our modules, it is sometimes not possible to do so; typically on managed hosting solutions which do not allow the deployment of custom Python code like the Odoo.com platform.

However, the flexible nature of Odoo is meant to allow customizations out of the box. Whilst a lot is possible with Studio, it is also possible to define models, fields and logic in XML Data Files. This makes it easier to develop, maintain and deploy these customizations.

In this tutorial, we will learn how to define models, fields and logic in XML data files and bundle them into a module. These are sometimes called importable modules, or data modules. We will also see the limitations of this approach to module development.

Like in the Server framework 101 tutorial, we will be working on Real Estate concepts.

Our goal is to create a new application to manage Real Estate properties in a similar (albeit simpler) way to the Server framework 101 tutorial. We will define the models, fields and logic in XML data files instead of Python files.

At the end of this tutorial, we will be able to achieve the following in our app:

Manage Real Estate properties that are for sale

Publish these properties on a website

Accept offers online from the website

Invoice the buyer when the property is sold

Like in any development project, a clear structure makes it easier to manage and maintain the code.

Unlike standard Odoo modules that use both Python and XML files, data modules use only XML files. Therefore, it is expected that your work tree will look something like this:

The only Python files you will have are the __init__.py and __manifest__.py files. The __manifest__.py file will be the same as for any Odoo module, but will also import its models in the data list.

Remember to list files in the data section of __manifest__.py in order of dependency, typically starting with model files.

The __init__.py file is empty, but is required for Odoo to recognize the module if you ever want to deploy your module in the classic way (by adding it in an addons path). It is not strictly necessary for modules that will be imported, but it is a good practice to keep it.

To deploy the module, you will need to create a zip file of the module and upload it to your Odoo instance. Make sure that the module base_import_module is installed on your instance, then go to the Apps ‣ Import Module and upload the zip file. You must be in developer mode to see the Import Module menu item.

If you modify the module, you will need to create a new zip file and upload it again, which will reload all the data in the module. Note however that some operations are not possible, like changing the type of a field you created previously. Data created by previous versions of the module (like removed fields) will not be automatically deleted. In general, the simplest way to handle this is to start with a fresh database or to uninstall the module prior to uploading the new version.

When uploading a module, the wizard will accept two options:

Force init: if your module is already installed and you upload it again; checking this option will force the update of all data marked as noupdate="1" in the XML files.

Import demo data: self explanatory

It is also possible to deploy the module using the odoo-bin command line tool with the deploy command:

This command also accepts the --force option, which is equivalent to the Force init option in the wizard.

Note that the user you use to deploy the module must have Administration/Settings access rights.

Create the following folders and files:

/home/$USER/src/tutorials/estate/__init__.py

/home/$USER/src/tutorials/estate/__manifest__.py

The __manifest__.py file should only define the name and the dependencies of our modules. The only necessary framework module for now is base (and base_import_module - although your module does not depend on it strictly speaking, you need it to be able to import your module).

Create a zip file of your module and upload it to your Odoo instance.

As you can imagine, defining models and fields in XML files is not as straightforward as in Python.

Since data files are read sequentially, you must define the elements in the right order. For example, you must define a model before you can define a field on that model, and you must define fields before adding them to a view.

In addition, XML is simply much more verbose than Python.

Let’s start by defining a simple model to represent a Real Estate property in the models directory of our module.

Odoo models are stored in database as ir.model records. Like any other record, they can be defined in XML files:

Note that all models and fields defined in data files must be prefixed with x_; this is mandatory and is used to differentiate them from models and fields defined in Python files.

Like for classic models defined in Python, Odoo will automatically add several fields to the model:

id (Id) The unique identifier for a record of the model.

create_date (Datetime) Creation date of the record.

create_uid (Many2one) User who created the record.

write_date (Datetime) Last modification date of the record.

write_uid (Many2one) User who last modified the record.

We can also add several fields to our new model. Let’s add some simple fields, like a name (string), selling price (float), a description (as html), and a postcode (as a char).

Like for models, fields are simply records of the ir.model.fields model and can be defined as such in data files:

You can set various attributes for your new field. For basic fields, these include:

name: the technical name of the field (must begin with x_)

field_description: the label of the field

help: a help text for the field, displayed in the interface

ttype: the type of the field (e.g. char, integer, float, html, etc.)

required: whether the field is required or not (default: False)

readonly: whether the field is read-only or not (default: False)

index: whether the field is indexed or not (default: False)

copied: whether the field is copied when duplicating a record or not (default: True for non-relational non-computed fields, False for relational and computed fields)

translate: whether the field is translatable or not (default: False)

Attributes are also available to control HTML sanitization as well as other, more advanced features; for a complete list, refer to the ir.model.fields model in the database available in the Settings ‣ Technical ‣ Database Structure ‣ Fields menu or see the ir.model.fields model definition in the base module.

Add the following basic fields to the table:

The x_garden_orientation field must have 4 possible values: ‘North’, ‘South’, ‘East’ and ‘West’. The selection list must be created by first creating the ir.model.fields record for the field itself, then creating the ir.model.fields.selection records. These records take three fields: field_id, name (the name in the UI) and value (the value in the database). A sequence field can also be set, which controls the order in which the selections are displayed in the UI (lower sequence values are displayed first).

In Python, default values can be set on fields using the default argument in the field declaration. In data modules, default values are set by creating an ir.default record for each field. For example, it is possible to set the default value of the x_selling_price field to 100000 for all properties by creating the following record:

For more details, refer to the ir.default model in the database available in the Settings ‣ Technical ‣ Actions ‣ User-defined Defaults menu or see the ir.default model definition in the base module.

These defaults are static but can be set by company and/or user using the user_id and company_id fields of the ir.default record. This means that having a dynamic default value of “today” for the x_date_availability field is not possible, for example.

Security in data modules is exactly the same as for Python modules and can be found in Chapter 4: Security - A Brief Introduction.

Refer to that tutorial for details.

Create the ir.model.access.csv file in the appropriate folder and define it in the __manifest__.py file.

Give the read, write, create and unlink permissions to the group base.group_user.

The warning message in the log gives you most of the solution ;-)

Views are the UI components that allow users to interact with the data. They are defined in XML files and can be found in the views directory of your module.

Since views and actions are already defined in Chapter 5: Finally, Some UI To Play With and Chapter 6: Basic Views, we will not go into details here.

Add a basic UI to the estate module.

Add a basic UI to the estate module to allow users to view, create, edit and delete Real Estate properties.

Create an action for the model x_estate.property.

Create a tree view for the model x_estate.property.

Create a form view for the model x_estate.property.

Add the views to the action.

Add a menu item to the main menu to allow users to access the action.

The real power of relational systems like Odoo lies in the ability to link records together. In a normal Python module, one could define new fields on a model to link it to other models in a single line of code. In a data module, this is still possible but requires a bit more legwork since we can’t use the same syntax as in Python.

As in Chapter 7: Relations Between Models, we will add some relations to our estate module. We will add links to:

the customer who bought the property

the real estate agent who sold the property

the property type: house, apartment, penthouse, castle…

a list of tags characterizing the property: cozy, renovated…

a list of the offers received

A many-to-one is a simple link to another object. For example, in order to define a link to the res.partner, we can define a new field in our model:

In the case of many-to-one fields, several attributes can be set to detail the relation:

relation: the name of the model to link to (required)

ondelete: the action to perform when the record is deleted (default: set null)

domain: a domain filter to apply to the relation

Create a new model x_estate.property.type with the following fields:

Add an action, list view and menu item for the x_estate.property.type model.

Add Access Rights to the x_estate.property.type model to give access to users.

Create the following fields on the x_estate.property model:

Many2one (x_estate.property.type)

Many2one (res.partner)

x_user_id (salesperson)

Include the new fields in the form view of the x_estate.property model.

A many-to-many is a relation to a list of objects. In our example, we will define a many-to-many relation towards a new x_estate.property.tag model. This tag represents a characteristic of the property, for example: renovated, cozy, etc.

A property can have many tags and a tag can be assigned to many properties - this is the typical many-to-many relationship.

Many-to-many fields are defined in the same way as many-to-one fields, but with the ttype set to many2many. The relation attribute is also set to the name of the model to link to. Other attributes can be set to control the relation:

relation_table: the name of the table to use for the relation

column1 and column2: the names of the columns to use for the relation

These attributes are optional, and should usually be specified only when there are multiple many-to-many fields between two models to avoid conflict; in most cases, the Odoo ORM will be able to determine the correct relation table and columns to use.

Create a new model x_estate.property.tag with the following fields:

Add an action, list view and menu item for the x_estate.property.tag model.

Add Access Rights to the x_estate.property.tag model to allow access to users.

Create the following fields on the x_estate.property model:

Many2many (x_estate.property.tag)

Include the new field in the form view of the x_estate.property model.

A one-to-many is a relation to a list of objects. In our example, we will define a one-to-many relation towards a new x_estate.property.offer model. This offer represent an offer made by a customer to buy a property.

One-to-many fields are defined in the same way as many-to-one fields, but with the ttype set to one2many. The relation attribute is also set to the name of the model to link to. Another attribute must be set to control the relation:

relation_field: the name of the field on the related model that contains the reference to the parent model (many-to-one field). This is used to link the two models together.

Create a new model x_estate.property.offer with the following fields:

Many2one (res.partner)

Many2one (x_estate.property)

Add Access Rights to the x_estate.property.offer model to allow access to users.

Add the field x_offer_ids to your x_estate.property model and in its form view.

Computed fields are a core concept in Odoo and are used to define fields that are computed based on other fields. This is useful for fields that are derived from other fields, like a sum of sub-records (adding up the price of all the items in a sale order).

Reference: the documentation related to this topic can be found in Computed Fields.

Data modules can define computed fields of any type, but are quite limited compared to Python modules. Indeed, since data modules are meant to be deployed on systems that do not allow arbitrary code to run, the Python code that is allowed is very limited.

All Python code written for data modules is executed in a sandboxed environment that limits the operations that can be performed. For example, you cannot import libraries, you cannot access any OS files, and you cannot even print to the console. Some utilities are provided, but this varies with the type of sandboxed environment that is used.

In the case of compute methods, the sandbox is very limited and only provides the bare minimum of utilities to allow the execution of the code. In addition to the Python builtins, you also have access to the datetime, dateutil and time modules (e.g., to help with date calculations).

Note also that “dot assignation” is disabled in the sandbox, so you cannot write property.x_total_area = 1 in the compute method. You have to use dictionary access: property['x_total_area'] = 1. Dot notation for field access works normally: property.x_garden_area will return the value of the x_garden_area field.

We previously defined two “area” fields on our x_estate.property model: living_area and garden_area. To define a computed field on the model that returns the sum of the two areas, we can add the following code to our data module:

Whilst in server actions, you iterate on a records variable, in the case of a computed field, you iterate on a self variable that contains the recordset on which the field is computed.

The depends attribute is used to define the fields that the computed field depends on and the compute attribute is used to define the code that is executed to compute the field (using Python code).

Unlike in Python modules, computed fields are stored by default. If you wish for a computed field to not be stored (e.g., for performance reasons or to avoid database bloat), you can set the store attribute to False. The same applies to readonly: if you want your computed field to not be editable, you need to set the readonly attribute to True.

The CDATA section is used to specify to XML parsers that the content is a string and not XML; this prevents the parser from trying to interpret the Python code as XML, or the addition of extra space, etc. when the code gets inserted into the database at module install time.

Add a computed field to the x_estate.property model that returns the sum of the x_living_area and x_garden_area fields, as shown above.

Include the field in the form view of the x_estate.property model.

Unlike in Python modules, it is not possible to define an inverse or search method for computed fields.

Related fields are a simplified form of computed fields that mirror the value of another field through a many2one relationship.

Reference: the documentation related to this topic can be found in Related fields.

Related fields can be of any type (the type of the field at the other end of the relation traversal). They are defined as if one were adding the field directly to the model with the addition of a related attribute that specifies the target field on the related model that contains the value to be mirrored.

For example, if we want to access the country of the buyer directly from the x_estate.property model, we can add the following code to our data module:

The related attribute is used to specify the target field on the related model that contains the value to be mirrored. This must be a dot-separated list of field names.

In a Python module, you are free to define any method on your model. One common usage pattern is to add so-called “actions” methods to your model then bind these methods to buttons in the UI (e.g to confirm a quote, post an invoice, etc.).

In a data module, you can achieve the same effect by defining Server Actions bound to your model. Server actions represent pieces of logic that are run dynamically on the server. These actions can be configured manually in the database directly via the Settings ‣ Technical ‣ Actions ‣ Server Actions menu and can be of different types; in our case, we will use the code type which allows us to run any Python code in a sandboxed environment.

This environment contains several utilities to help you interact with the Odoo database:

env: the environment of the record

model: the model of the record

user and uid: the current user and their id

datetime, dateutil, timezone and time: libraries to help with date/time calculations

float_compare: a utility function to compare two float values with a given precision

b64encode and b64decode: utility functions to encode and decode values in base64

Command: a utility class to help build complex expressions and commands (see the Command class in the ORM reference)

In addition, you have access to the recordset on which the action is executed (typically a single record when the action is executed from a form view, and multiple records when the action is executed from a list view) via the record and records variables.

If your action needs to return an action to the client (for example to redirect the user to another view), you can assign it to a an action variable inside your server action’s code. The code sandbox will inspect the variables defined in your code after its execution and will automatically return it if it detects the presence of an action variable.

If the website module is installed, the request object will be available in the code sandbox and you can assign a response object to the response variable to return a response to the client in a similar way. This is explored in more details in the Website controllers section.

For example, we could define an action on the x_estate.property model that sets the x_status field of all its offers to Refused:

To include this action as a button in the form view of the x_estate.property model, we can add the following button node in the header of our form view:

It is also possible to add an entry in the gear icon () to run this action (e.g. to avoid adding buttons to views that are already crowded). To do so, you can bind your server action to the model and to specific types of views:

This will make the action available in the gear icon () of the x_estate.property model, in the list (when one or more records are selected via the checkbox) and form views.

Add a server action to the x_estate.property.offer model that sets the x_status field of an offer to Accepted and updates the selling price and buyer of the property to which the offer is attached accordingly. This action should also mark all the other offers on the same property as Refused.

Include a button in the embedded list view of offers that allows to execute this action

Unlike in Python modules, it is not possible to override a Python model’s method cleanly.

However, it is possible (in some cases) to replace the elements of the UI that call these methods and to intercept the calls to these methods in a server action.

A typical example would be an integration with the Sales app of Odoo. Let’s imagine that your Real Estate module integrates with the Sales application so that when a specific product is sold (e.g., a quote for managing the sale of a property), you want to automatically create a new property record in your module.

To achieve this, you will need to:

create a server action that calls the original method of the button and add custom logic before or after that method call

replace the button in the view with a custom button that calls the server action

Automations rules are a way to automatically execute actions on records in the database based on specific triggers, like state changes, addition of a tag, etc. They can be useful to tie behaviour to life-cycle events of records, for example by sending an email when an offer is accepted.

Using automation rules for extending a standard behaviour can be more robust than the UI-based approach since it will also run if the life-cycle event is triggered in another way than via a button (e.g., via a webhook or a direct call to the method; for example when a quote is confirmed via the portal or the e-commerce). They are however a bit more finicky to set up properly, as one needs to ensure that the automation will only run at the proper moment by setting up specific fields to watch, etc.

Documentation: a more complete documentation related to this topic can be found in Automation rules.

Automation Rules are not part of the base module; they come with the base_automation module; so if you define automation rules in your data module, you need to make sure that base_automation is part of your module’s dependencies.

Once installed, Automation Rules are managed via the Settings ‣ Technical ‣ Automations ‣ Automation Rules menu.

Automation Rules are particularly useful to tie a data module to an existing standard Odoo module. Since data modules cannot override methods, tying automation to life-cycle changes of standard models is a common way to extend standard modules.

If we were to rewrite our example from the previous section using automation rules, a few changes would be needed:

the server action should no longer call the original method of the button (instead, the original method will trigger the change that will fire the automation rule)

the view extension is not needed

we need to define an Automation Rule to trigger the server action on the appropriate event

Note that the XML IDs to standard Odoo models, fields, selection values, etc. can be found in the Odoo instance itself by navigating to the appropriate record in the technical menus and using the View Metadata menu entry of the debug menu. XML IDs for models are simply the model name with dots replaced by underscores and prefixed by model_ (e.g., sale.model_sale_order is sale.order as defined in the sale module); XML IDs for fields are the model name with dots replaced by underscores and prefixed by field_, the model’s name and the field name (e.g., sale.field_sale_order__name is the XML ID for the name field of the sale.order model which is defined in the sale module).

HTTP Controllers in Odoo are usually defined in the controllers directory of a module. In data modules, it is possible to define server actions that behave as controllers if the website module is installed.

When the website module is installed, server actions can be marked as Available on the website and given a path (the full path is always prefixed with /website/action to avoid URL collisions); the global request object is made available in the local scope of the code server action.

The request object provides several methods to access the body of the request:

request.get_http_params(): extract key-value pairs from the query string and the forms present in the body (both application/x-www-form-urlencoded and multipart/form-data).

request.get_json_data(): extract the JSON data from the body of the request.

Since it is not possible to return a value from within a server action, to define the response to return, one can assign a response-like object to the response variable, which will be returned to the website automatically.

Here is an example of a simple website controller that will return a list of properties when the URL /website/action/estate is called:

Several useful methods are available in the request object to facilitate the generation of the response object:

request.render(template, qcontext=None, lazy=True, **kw) to render a QWeb template using its xmlid; the extra keyword arguments are forwarded to the werkzeug.Response object (e.g. to set cookies, headers, etc.)

request.redirect(location, code=303, local=True) to redirect to a different URL; the local argument is used to specify whether the redirection should be relative to the website or not (default: True).

request.notfound() to return a werkzeug.HTTPException exception to signal a 404 error to the website.

request.make_response(data, headers=None, cookies=None, status=200) to manually create a werkzeug.Response object; the status argument is the HTTP status code to return (default: 200).

request.make_json_response(data, headers=None, cookies=None, status=200) to manually create a JSON response; the data will be json-serialized using json.dumps utility; this can be useful to set up server-to-server communications via API calls.

For implementation details or other (less common) methods, refer to the Request object’s implementation in the odoo.http module.

Note that security concerns are left to the developer (typically through security rules or by using sudo to access records).

The model used in the model_id field of the server action must be accessible to the public user for the write operation for this server action to run; otherwise the server action will return a 403 error. A way to avoid giving access is to link your server action to a model that is already accessible to the public user, a typical (if weird) example is to link the server action to the ir.filters model.

Add a JSON API to your module so that external services can retrieve a list of properties for sale.

add a new x_api_published field to the model to control whether the properties are published on the API or not

add an access right record to allow public users to read and write the model

prevent any write from the public user by adding a record rule for the write operation with an impossible domain (e.g. [('id', '=', False)])

add a record rule so that properties marked as x_api_published can be read by the public user

add a server action to return a list of properties in JSON format when the URL /website/action/estate is called

Whilst importable modules cannot include Python files, no such restriction exists for JavaScript files. Adding JavaScript files to your importable module is exactly the same as adding them to a standard Odoo module.

This means that an importable module can include new field components or even entirely new views.

As an example, let’s add a simple ‘tour’ to the Estate module. Tours are a standard mechanism in Odoo used to onboard users by guiding them through your application.

A very minimal tour with a single step can be added by adding this file in static/src/js/tour.js:

You then need to include the file in the appropriate bundle in the manifest file:

You also need to add an xml record in a new estate_tour.xml file in the data folder so that your tour is displayed:

Unlike normal Python modules, glob expansion is not supported in importable modules; so you need to list each file you want to include in the module specifically.

**Examples:**

Example 1 (unknown):
```unknown
estate
├── actions
│   └── *.xml
├── models
│   └── *.xml
├── security
│   └── ir.model.access.csv
│   └── estate_security.xml
├── views
│   └── *.xml
├── __init__.py
└── __manifest__.py
```

Example 2 (typescript):
```typescript
$ odoo-bin deploy <path_to_your_module> https://<your_odoo_instance> --login <your_login> --password <your_password>
```

Example 3 (xml):
```xml
<?xml version="1.0" encoding="utf-8"?>
<odoo>
    <record id="model_real_estate_property" model="ir.model">
        <field name="name">Real Estate Property</field>
        <field name="model">x_estate.property</field>
    </record>
</odoo>
```

Example 4 (xml):
```xml
<?xml version="1.0" encoding="utf-8"?>
<odoo>
    <!-- ...model definition from before... -->
    <record id="field_real_estate_property_name" model="ir.model.fields">
        <field name="model_id" ref="estate.model_real_estate_property" />
        <field name="name">x_name</field>
        <field name="field_description">Name</field>
        <field name="ttype">char</field>
        <field name="required">True</field>
    </record>

    <record id="field_real_estate_property_selling_price" model="ir.model.fields">
        <field name="model_id" ref="estate.model_real_estate_property" />
        <field name="name">x_selling_price</field>
        <field name="field_description">Selling Price</field>
        <field name="ttype">float</field>
        <field name="required">True</field>
    </record>

    <record id="field_real_estate_property_description" model="ir.model.fields">
        <field name="model_id" ref="estate.model_real_estate_property" />
        <field name="name">x_description</field>
        <field name="field_description">Description</field>
        <field name="ttype">html</field>
    </record>

    <record id="field_real_estate_property_postcode" model="ir.model.fields">
        <field name="model_id" ref="estate.model_real_estate_property" />
        <field name="name">x_postcode</field>
        <field name="field_description">Postcode</field>
        <field name="ttype">char</field>
    </record>
</odoo>
```

---

## Chapter 1: Owl components¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/discover_js_framework/01_owl_components.html

**Contents:**
- Chapter 1: Owl components¶
- Example: a Counter component¶
- 1. Displaying a counter¶
- 2. Extract Counter in a sub component¶
- 3. A simple Card component¶
- 4. Using markup to display html¶
- 5. Props validation¶
- 6. The sum of two Counter¶
- 7. A todo list¶
- 8. Use dynamic attributes¶

This chapter introduces the Owl framework, a tailor-made component system for Odoo. The main building blocks of OWL are components and templates.

In Owl, every part of user interface is managed by a component: they hold the logic and define the templates that are used to render the user interface. In practice, a component is represented by a small JavaScript class subclassing the Component class.

To get started, you need a running Odoo server and a development environment setup. Before getting into the exercises, make sure you have followed all the steps described in this tutorial introduction.

If you use Chrome as your web browser, you can install the Owl Devtools extension. This extension provides many features to help you understand and profile any Owl application.

Video: How to use the DevTools

In this chapter, we use the awesome_owl addon, which provides a simplified environment that only contains Owl and a few other files. The goal is to learn Owl itself, without relying on Odoo web client code.

The solutions for each exercise of the chapter are hosted on the official Odoo tutorials repository. It is recommended to try to solve them first without looking at the solution!

First, let us have a look at a simple example. The Counter component shown below is a component that maintains an internal number value, displays it, and updates it whenever the user clicks on the button.

The Counter component specifies the name of a template that represents its html. It is written in XML using the QWeb language:

As a first exercise, let us modify the Playground component located in awesome_owl/static/src/ to turn it into a counter. To see the result, you can go to the /awesome_owl route with your browser.

Modify playground.js so that it acts as a counter like in the example above. Keep Playground for the class name. You will need to use the useState hook so that the component is re-rendered whenever any part of the state object that has been read by this component is modified.

In the same component, create an increment method.

Modify the template in playground.xml so that it displays your counter variable. Use t-esc to output the data.

Add a button in the template and specify a t-on-click attribute in the button to trigger the increment method whenever the button is clicked.

The Odoo JavaScript files downloaded by the browser are minified. For debugging purpose, it’s easier when the files are not minified. Switch to debug mode with assets so that the files are not minified.

This exercise showcases an important feature of Owl: the reactivity system. The useState function wraps a value in a proxy so Owl can keep track of which component needs which part of the state, so it can be updated whenever a value has been changed. Try removing the useState function and see what happens.

For now we have the logic of a counter in the Playground component, but it is not reusable. Let us see how to create a sub-component from it:

Extract the counter code from the Playground component into a new Counter component.

You can do it in the same file first, but once it’s done, update your code to move the Counter in its own folder and file. Import it relatively from ./counter/counter. Make sure the template is in its own file, with the same name.

Use <Counter/> in the template of the Playground component to add two counters in your playground.

By convention, most components code, template and css should have the same snake-cased name as the component. For example, if we have a TodoList component, its code should be in todo_list.js, todo_list.xml and if necessary, todo_list.scss

Components are really the most natural way to divide a complicated user interface into multiple reusable pieces. But to make them truly useful, it is necessary to be able to communicate some information between them. Let us see how a parent component can provide information to a sub component by using attributes (most commonly known as props).

The goal of this exercise is to create a Card component, that takes two props: title and content. For example, here is how it could be used:

The above example should produce some html using bootstrap that look like this:

Create a Card component

Import it in Playground and display a few cards in its template

If you used t-esc in the previous exercise, then you may have noticed that Owl automatically escapes its content. For example, if you try to display some html like this: <Card title="'my title'" content="this.html"/> with this.html = "<div>some content</div>"", the resulting output will simply display the html as a string.

In this case, since the Card component may be used to display any kind of content, it makes sense to allow the user to display some html. This is done with the t-out directive.

However, displaying arbitrary content as html is dangerous, it could be used to inject malicious code, so by default, Owl will always escape a string unless it has been explicitely marked as safe with the markup function.

Update Card to use t-out

Update Playground to import markup, and use it on some html values

Make sure that you see that normal strings are always escaped, unlike markuped strings.

The t-esc directive can still be used in Owl templates. It is slightly faster than t-out.

The Card component has an implicit API. It expects to receive two strings in its props: the title and the content. Let us make that API more explicit. We can add a props definition that will let Owl perform a validation step in dev mode. You can activate the dev mode in the App configuration (but it is activated by default on the awesome_owl playground).

It is a good practice to do props validation for every component.

Add props validation to the Card component.

Rename the title props into something else in the playground template, then check in the Console tab of your browser’s dev tools that you can see an error.

We saw in a previous exercise that props can be used to provide information from a parent to a child component. Now, let us see how we can communicate information in the opposite direction: in this exercise, we want to display two Counter components, and below them, the sum of their values. So, the parent component (Playground) need to be informed whenever one of the Counter value is changed.

This can be done by using a callback prop: a prop that is a function meant to be called back. The child component can choose to call that function with any argument. In our case, we will simply add an optional onChange prop that will be called whenever the Counter component is incremented.

Add prop validation to the Counter component: it should accept an optional onChange function prop.

Update the Counter component to call the onChange prop (if it exists) whenever it is incremented.

Modify the Playground component to maintain a local state value (sum), initially set to 2, and display it in its template

Implement an incrementSum method in Playground

Give that method as a prop to two (or more!) sub Counter components.

There is a subtlety with callback props: they usually should be defined with the .bind suffix. See the documentation.

Let us now discover various features of Owl by creating a todo list. We need two components: a TodoList component that will display a list of TodoItem components. The list of todos is a state that should be maintained by the TodoList.

For this tutorial, a todo is an object that contains three values: an id (number), a description (string) and a flag isCompleted (boolean):

Create a TodoList and a TodoItem components.

The TodoItem component should receive a todo as a prop, and display its id and description in a div.

For now, hardcode the list of todos:

Use t-foreach to display each todo in a TodoItem.

Display a TodoList in the playground.

Add props validation to TodoItem.

Since the TodoList and TodoItem components are so tightly coupled, it makes sense to put them in the same folder.

The t-foreach directive is not exactly the same in Owl as the QWeb python implementation: it requires a t-key unique value, so that Owl can properly reconcile each element.

For now, the TodoItem component does not visually show if the todo is completed. Let us do that by using a dynamic attributes.

Add the Bootstrap classes text-muted and text-decoration-line-through on the TodoItem root element if it is completed.

Change the hardcoded this.todos value to check that it is properly displayed.

Even though the directive is named t-att (for attribute), it can be used to set a class value (and html properties such as the value of an input).

Owl let you combine static class values with dynamic values. The following example will work as expected:

See also: Owl: Dynamic class attributes

So far, the todos in our list are hard-coded. Let us make it more useful by allowing the user to add a todo to the list.

Remove the hardcoded values in the TodoList component:

Add an input above the task list with placeholder Enter a new task.

Add an event handler on the keyup event named addTodo.

Implement addTodo to check if enter was pressed (ev.keyCode === 13), and in that case, create a new todo with the current content of the input as the description and clear the input of all content.

Make sure the todo has a unique id. It can be just a counter that increments at each todo.

Bonus point: don’t do anything if the input is empty.

So far, we have seen one example of a hook function: useState. A hook is a special function that hook into the internals of the component. In the case of useState, it generates a proxy object linked to the current component. This is why hook functions have to be called in the setup method, and no later!

An Owl component goes through a lot of phases: it can be instantiated, rendered, mounted, updated, detached, destroyed… This is the component lifecycle. The figure above show the most important events in the life of a component (hooks are shown in purple). Roughly speaking, a component is created, then updated (potentially many times), then is destroyed.

Owl provides a variety of built-in hooks functions. All of them have to be called in the setup function. For example, if you want to execute some code when your component is mounted, you can use the onMounted hook:

All hook functions start with use or on. For example: useState or onMounted.

Let’s see how we can access the DOM with t-ref and useRef. The main idea is that you need to mark the target element in the component template with a t-ref:

Then you can access it in the JS with the useRef hook. However, there is a problem if you think about it: the actual html element for a component does not exist when the component is created. It only exists when the component is mounted. But hooks have to be called in the setup method. So, useRef return an object that contains a el (for element) key that is only defined when the component is mounted.

Focus the input from the previous exercise. This this should be done from the TodoList component (note that there is a focus method on the input html element).

Bonus point: extract the code into a specialized hook useAutofocus in a new awesome_owl/utils.js file.

Refs are usually suffixed by Ref to make it obvious that they are special objects:

Now, let’s add a new feature: mark a todo as completed. This is actually trickier than one might think. The owner of the state is not the same as the component that displays it. So, the TodoItem component needs to communicate to its parent that the todo state needs to be toggled. One classic way to do this is by adding a callback prop toggleState.

Add an input with the attribute type="checkbox" before the id of the task, which must be checked if the state isCompleted is true.

Owl does not create attributes computed with the t-att directive if it evaluates to a falsy value.

Add a callback props toggleState to TodoItem.

Add a change event handler on the input in the TodoItem component and make sure it calls the toggleState function with the todo id.

The final touch is to let the user delete a todo.

Add a new callback prop removeTodo in TodoItem.

Insert <span class="fa fa-remove"/> in the template of the TodoItem component.

Whenever the user clicks on it, it should call the removeTodo method.

If you’re using an array to store your todo list, you can use the JavaScript splice function to remove a todo from it.

In a previous exercise, we built a simple Card component. But it is honestly quite limited. What if we want to display some arbitrary content inside a card, such as a sub-component? Well, it does not work, since the content of the card is described by a string. It would however be very convenient if we could describe the content as a piece of template.

This is exactly what Owl’s slot system is designed for: allowing to write generic components.

Let us modify the Card component to use slots:

Remove the content prop.

Use the default slot to define the body.

Insert a few cards with arbitrary content, such as a Counter component.

(bonus) Add prop validation.

Bootstrap: documentation on cards

Finally, let’s add a feature to the Card component, to make it more interesting: we want a button to toggle its content (show it or hide it)

Add a state to the Card component to track if it is open (the default) or not

Add a t-if in the template to conditionally render the content

Add a button in the header, and modify the code to flip the state when the button is clicked

**Examples:**

Example 1 (jsx):
```jsx
import { Component, useState } from "@odoo/owl";

export class Counter extends Component {
    static template = "my_module.Counter";

    setup() {
        this.state = useState({ value: 0 });
    }

    increment() {
        this.state.value++;
    }
}
```

Example 2 (typescript):
```typescript
<templates xml:space="preserve">
   <t t-name="my_module.Counter">
      <p>Counter: <t t-esc="state.value"/></p>
      <button class="btn btn-primary" t-on-click="increment">Increment</button>
   </t>
</templates>
```

Example 3 (jsx):
```jsx
<Card title="'my title'" content="'some content'"/>
```

Example 4 (jsx):
```jsx
<div class="card d-inline-block m-2" style="width: 18rem;">
    <div class="card-body">
        <h5 class="card-title">my title</h5>
        <p class="card-text">
         some content
        </p>
    </div>
</div>
```

---

## Chapter 9: Ready For Some Action?¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/server_framework_101/09_actions.html

**Contents:**
- Chapter 9: Ready For Some Action?¶
- Object Type¶
- Action Type¶

So far we have mostly built our module by declaring fields and views. We just introduced business logic in the previous chapter thanks to computed fields and onchanges. In any real business scenario, we would want to link some business logic to action buttons. In our real estate example, we would like to be able to:

cancel or set a property as sold

accept or refuse an offer

One could argue that we can already do these things by changing the state manually, but this is not really convenient. Moreover, we want to add some extra processing: when an offer is accepted we want to set the selling price and the buyer for the property.

Reference: the documentation related to this topic can be found in Actions and Error management.

Goal: at the end of this section:

You should be able to cancel or set a property as sold:

A cancelled property cannot be sold and a sold property cannot be cancelled. For the sake of clarity, the state field has been added on the view.

You should be able to accept or refuse an offer:

Once an offer is accepted, the selling price and the buyer should be set:

In our real estate module, we want to link business logic with some buttons. The most common way to do this is to:

Add a button in the view, for example in the header of the view:

and link this button to business logic:

By assigning type="object" to our button, the Odoo framework will execute a Python method with name="action_do_something" on the given model.

The first important detail to note is that our method name isn’t prefixed with an underscore (_). This makes our method a public method, which can be called directly from the Odoo interface (through an RPC call). Until now, all methods we created (compute, onchange) were called internally, so we used private methods prefixed by an underscore. You should always define your methods as private unless they need to be called from the user interface.

Also note that we loop on self. Always assume that a method can be called on multiple records; it’s better for reusability.

Finally, a public method should always return something so that it can be called through XML-RPC. When in doubt, just return True.

There are hundreds of examples in the Odoo source code. One example is this button in a view and its corresponding Python method

Cancel and set a property as sold.

Add the buttons ‘Cancel’ and ‘Sold’ to the estate.property model. A cancelled property cannot be set as sold, and a sold property cannot be cancelled.

Refer to the first image of the Goal for the expected result.

Tip: in order to raise an error, you can use the UserError function. There are plenty of examples in the Odoo source code ;-)

Add the buttons ‘Accept’ and ‘Refuse’ to the estate.property.offer model.

Refer to the second image of the Goal for the expected result.

Tip: to use an icon as a button, have a look at this example.

When an offer is accepted, set the buyer and the selling price for the corresponding property.

Refer to the third image of the Goal for the expected result.

Pay attention: in real life only one offer can be accepted for a given property!

In Chapter 5: Finally, Some UI To Play With, we created an action that was linked to a menu. You may be wondering if it is possible to link an action to a button. Good news, it is! One way to do it is:

We use type="action" and we refer to the external identifier in the name.

In the next chapter we’ll see how we can prevent encoding incorrect data in Odoo.

**Examples:**

Example 1 (jsx):
```jsx
<form>
    <header>
        <button name="action_do_something" type="object" string="Do Something"/>
    </header>
    <sheet>
        <field name="name"/>
    </sheet>
</form>
```

Example 2 (python):
```python
from odoo import fields, models

class TestAction(models.Model):
    _name = "test.action"

    name = fields.Char()

    def action_do_something(self):
        for record in self:
            record.name = "Something"
        return True
```

Example 3 (jsx):
```jsx
<button type="action" name="%(test.test_model_action)d" string="My Action"/>
```

---

## Chapter 3 - Customisation, Part I¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/website_theme/03_customisation_part1.html

**Contents:**
- Chapter 3 - Customisation, Part I¶
- Add custom SCSS¶
- Add custom JS¶
- Create a custom header¶
- Create a custom footer¶
- Create your custom building blocks¶
- Create a new dynamic snippets template¶

You’ve adjusted Odoo and Bootstrap variables and set presets, yet you still notice disparities between your website and the client’s design. The only solution is to incorporate custom SCSS.

In theme.scss, reproduce the following design elements:

Add a green underline on active nav items.

Modify the arrow for collapsible nav items.

Modify the slider’s arrows by adding a green background and changing their design.

You will find the various media here.

See reference documentation on how to add your SCSS rules.

Find the solution in our Airproof example on header.scss and caroussel.scss.

Now, let’s add a mouse follower to the website. This interactive element will enhance the browsing experience, making it more engaging and visually appealing.

Use your JavaScript skills to implement this.

See reference documentation on how to add Javascript code.

Find the solution in our Airproof example on mouse_follower.js and mouse_follower.scss.

With variables, presets, and custom SCSS in place, it’s time to refine the layout and add key cross-page elements, starting with the header.

Based on the Airproof design, create a custom header with the following elements:

A custom shopping cart icon.

A login/user as a button.

Navigation text to 14px.

You can find the cart icon and template illustration.

In order for the client to find the mega menu you previously created for him, turn it into a template that can be found through the website builder.

Place your new header over the content of your homepage.

See reference documentation on how to:

create custom headers,

create a mega menu template,

create a header overlay.

Base yourself on the code of existing header templates that you can find in odoo/addons/website/views/website_templates.xml.

A good practise should be to create different files to manage your custom views and templates. For example, everything concerning the general layout (header, footer…) in website_templates.xml, everything related to blog in website_blog_templates.xml, to event in website_event_templates.xml, etc.

Don’t forget to continue making as many modifications as you can through the Bootstrap variables and primary variables (font, colors, size…). You can use them to help you with this exercise.

Ensure that you properly call every template in your header (like the shopping cart, language selector, call to action, etc.), so everything will remain editable via the website builder and all options will be available.

Find the solution in our Airproof example for:

the xml structure and how to add the header and mega menu template to the options list on website_template.xml.

disable the default header:

declare your website_templates.xml file along with all the new ones in your manifest.

disable the options you don’t want in your header via the presets.

make the use of primaries like header-template, navbar-font, header-font-size…

use bootstrap_overridden like $navbar-light-color, $navbar-light-hover-color, $navbar-padding-y…

to place the header over the content, add the right field to the home page:

The client is delighted with the new header, as it aligns perfectly with the provided design. Now, he wants a matching custom footer.

Based on the Airproof design, create a custom footer with the following elements:

A section for newsletter subscription.

A section for the copyright and social media.

You will find the icons here.

See reference documentation on how to create a custom footer and adapt the Copyright.

You can enable or disable the copyright section via the presets.

For the newsletter section to work, you need to install the website_mass_mailing application.

To complete this exercise, you need to:

add mass mailing to your depends:

find the xml structure and add the template to the options list on website_template.xml.

disable the default footer and enable the copyright:

make the use of primaries like footer-template, footer, o-cc4-link…

add a little scss rule for the newsletter section.

To allow your client to further customize his website, create tailor-made building blocks that he can freely drag & drop onto different pages.

Based on the Airproof design, create a custom carousel snippet to showcase drones. Then, add it as cover section on your homepage.

Create the snippet template. Then, add it to the list of building blocks available in the website builder. Place it in its own category. Here you will find the images and snippet illustration.

See reference documentation on how to create a custom building blocks.

Add two options in the Website Builder for the bubbles:

Allow users to choose only between a blue or green shadow.

Offer a range of possible margins between the bubbles.

See reference documentation on how to add snippet options.

Add the snippet on your homepage.

Don’t forget to always properly declare your new files in your __manifest__.py and follow the good folder structure seen previously.

To complete this exercise, you need to:

Create your template.

You can find all the necessary information in s_airproof_carousel.xml file and s_airproof_carousel/000.scss file from our example module.

Record your images in images.xml.

Declare your files in the __manifest__.py.

Add it to the list of building blocks. In our example, it looks like this:

Add the option to the Website Builder. In our example, it looks like this:

Additionally, the SCSS related to the bubbles in the s_airproof_carousel/000.scss file.

Add your snippet to the homepage. You can find all the necessary information in the home.xml file from our example module.

Based on the Airproof design, create a custom template that you will apply to a product dynamic snippet on the homepage.

First, create a custom template that will be added to the list of dynamic products templates. It has to include the following elements:

Add a Discover more link.

Add a hover effect on cards.

Move the navigation arrows.

You will find the icons here.

See reference documentation on how to create a template for dynamic snippets.

You can verify in the Website Builder that your template appears in the list of available templates for the product dynamic snippet.

Then, add a product dynamic snippet with the template you just created to the homepage.

See reference documentation on how to call a template.

To complete this exercise, you need to:

Create your snippet template. You can find all the necessary information in the options.xml file and caroussel.scss file from our example module.

Apply the template to a product dynamic snippet on the homepage. You can find all the necessary information in the home.xml file from our example module.

**Examples:**

Example 1 (jsx):
```jsx
<!-- Disable default header -->
<record id="website.template_header_default" model="ir.ui.view">
   <field name="active" eval="False"/>
</record>
```

Example 2 (jsx):
```jsx
<!-- Home -->
<record id="page_home" model="website.page">
   <field name="header_overlay" eval="True"/>
```

Example 3 (json):
```json
'depends': ['website_sale', 'website_sale_wishlist', 'website_blog',
'website_mass_mailing'],
```

Example 4 (jsx):
```jsx
<!-- Disable Default Footer -->
<record id="website.footer_custom" model="ir.ui.view">
   <field name="active" eval="False"/>
</record>
<!-- Enable Copyright -->
<record id="website.footer_no_copyright" model="ir.ui.view">
   <field name="active" eval="False"/>
</record>
```

---

## Chapter 7: Relations Between Models¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/server_framework_101/07_relations.html

**Contents:**
- Chapter 7: Relations Between Models¶
- Many2one¶
- Many2many¶
- One2many¶

The previous chapter covered the creation of custom views for a model containing basic fields. However, in any real business scenario we need more than one model. Moreover, links between models are necessary. One can easily imagine one model containing the customers and another one containing the list of users. You might need to refer to a customer or a user on any existing business model.

In our real estate module, we want the following information for a property:

the customer who bought the property

the real estate agent who sold the property

the property type: house, apartment, penthouse, castle…

a list of tags characterizing the property: cozy, renovated…

a list of the offers received

Reference: the documentation related to this topic can be found in Many2one.

Goal: at the end of this section:

a new estate.property.type model should be created with the corresponding menu, action and views.

three Many2one fields should be added to the estate.property model: property type, buyer and seller.

In our real estate module, we want to define the concept of property type. A property type is, for example, a house or an apartment. It is a standard business need to categorize properties according to their type, especially to refine filtering.

A property can have one type, but the same type can be assigned to many properties. This is supported by the many2one concept.

A many2one is a simple link to another object. For example, in order to define a link to the res.partner in our test model, we can write:

By convention, many2one fields have the _id suffix. Accessing the data in the partner can then be easily done with:

In practice a many2one can be seen as a dropdown list in a form view.

Add the Real Estate Property Type table.

Create the estate.property.type model and add the following field:

Add the menus as displayed in this section’s Goal

Add the field property_type_id into your estate.property model and its form, list and search views

This exercise is a good recap of the previous chapters: you need to create a model, set the model, add an action and a menu, and create a view.

Tip: do not forget to import any new Python files in __init__.py, add new data files in __manifest.py__ or add the access rights ;-)

Once again, restart the server and refresh to see the results!

In the real estate module, there are still two missing pieces of information we want on a property: the buyer and the salesperson. The buyer can be any individual, but on the other hand the salesperson must be an employee of the real estate agency (i.e. an Odoo user).

In Odoo, there are two models which we commonly refer to:

res.partner: a partner is a physical or legal entity. It can be a company, an individual or even a contact address.

res.users: the users of the system. Users can be ‘internal’, i.e. they have access to the Odoo backend. Or they can be ‘portal’, i.e. they cannot access the backend, only the frontend (e.g. to access their previous orders in eCommerce).

Add the buyer and the salesperson.

Add a buyer and a salesperson to the estate.property model using the two common models mentioned above. They should be added in a new tab of the form view, as depicted in this section’s Goal.

The default value for the salesperson must be the current user. The buyer should not be copied.

Tip: to get the default value, check the note below or look at an example here.

The object self.env gives access to request parameters and other useful things:

self.env.cr or self._cr is the database cursor object; it is used for querying the database

self.env.uid or self._uid is the current user’s database id

self.env.user is the current user’s record

self.env.context or self._context is the context dictionary

self.env.ref(xml_id) returns the record corresponding to an XML id

self.env[model_name] returns an instance of the given model

Now let’s have a look at other types of links.

Reference: the documentation related to this topic can be found in Many2many.

Goal: at the end of this section:

a new estate.property.tag model should be created with the corresponding menu and action.

tags should be added to the estate.property model:

In our real estate module, we want to define the concept of property tags. A property tag is, for example, a property which is ‘cozy’ or ‘renovated’.

A property can have many tags and a tag can be assigned to many properties. This is supported by the many2many concept.

A many2many is a bidirectional multiple relationship: any record on one side can be related to any number of records on the other side. For example, in order to define a link to the account.tax model on our test model, we can write:

By convention, many2many fields have the _ids suffix. This means that several taxes can be added to our test model. It behaves as a list of records, meaning that accessing the data must be done in a loop:

A list of records is known as a recordset, i.e. an ordered collection of records. It supports standard Python operations on collections, such as len() and iter(), plus extra set operations like recs1 | recs2.

Add the Real Estate Property Tag table.

Create the estate.property.tag model and add the following field:

Add the menus as displayed in this section’s Goal

Add the field tag_ids to your estate.property model and in its form and list views

Tip: in the view, use the widget="many2many_tags" attribute as demonstrated here. The widget attribute will be explained in detail in a later chapter of the training. For now, you can try adding and removing it and see the result ;-)

Reference: the documentation related to this topic can be found in One2many.

Goal: at the end of this section:

a new estate.property.offer model should be created with the corresponding form and list view.

offers should be added to the estate.property model:

In our real estate module, we want to define the concept of property offers. A property offer is an amount a potential buyer offers to the seller. The offer can be lower or higher than the expected price.

An offer applies to one property, but the same property can have many offers. The concept of many2one appears once again. However, in this case we want to display the list of offers for a given property so we will use the one2many concept.

A one2many is the inverse of a many2one. For example, we defined on our test model a link to the res.partner model thanks to the field partner_id. We can define the inverse relation, i.e. the list of test models linked to our partner:

The first parameter is called the comodel and the second parameter is the field we want to inverse.

By convention, one2many fields have the _ids suffix. They behave as a list of records, meaning that accessing the data must be done in a loop:

Because a One2many is a virtual relationship, there must be a Many2one field defined in the comodel.

Add the Real Estate Property Offer table.

Create the estate.property.offer model and add the following fields:

Many2one (res.partner)

Many2one (estate.property)

Create a list view and a form view with the price, partner_id and status fields. No need to create an action or a menu.

Add the field offer_ids to your estate.property model and in its form view as depicted in this section’s Goal.

There are several important things to notice here. First, we don’t need an action or a menu for all models. Some models are intended to be accessed only through another model. This is the case in our exercise: an offer is always accessed through a property.

Second, despite the fact that the property_id field is required, we did not include it in the views. How does Odoo know which property our offer is linked to? Well that’s part of the magic of using the Odoo framework: sometimes things are defined implicitly. When we create a record through a one2many field, the corresponding many2one is populated automatically for convenience.

Still alive? This chapter is definitely not the easiest one. It introduced a couple of new concepts while relying on everything that was introduced before. The next chapter will be lighter, don’t worry ;-)

**Examples:**

Example 1 (unknown):
```unknown
partner_id = fields.Many2one("res.partner", string="Partner")
```

Example 2 (unknown):
```unknown
print(my_test_object.partner_id.name)
```

Example 3 (unknown):
```unknown
tax_ids = fields.Many2many("account.tax", string="Taxes")
```

Example 4 (python):
```python
for tax in my_test_object.tax_ids:
    print(tax.name)
```

---

## Build a website theme¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/website_theme.html

**Contents:**
- Build a website theme¶

To start, you must have installed Odoo locally. You will also need some knowledge in:

JavaScript (not mandatory)

QWeb (Odoo’s own templating system)

OWL (JavaScript framework, not mandatory)

Throughout this tutorial, you will find “See also” sections leading to parts of the How-to guide: Website themes documentation. Be sure to read this documentation thoroughly each time! With it, you will find the solution to every exercise.

Ready? Let’s get started!

---

## Chapter 4 - Customisation, Part II¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/website_theme/04_customisation_part2.html

**Contents:**
- Chapter 4 - Customisation, Part II¶
- Create a custom background shape¶
- Create a custom gradient¶
- Animations¶
- Forms¶
- Create a page template¶

Shapes are decorative elements that can be applied to backgrounds or images. They are SVG files that can be animated and customized with different colors.

To better align with the website’s desired atmosphere, create a custom background shape that the client can reuse on different blocks.

Create your custom shape using the following setup:

Declare your shape. You can find the original SVG shape here.

Set the base color of the shape to the theme’s green, and add it to the list of available shapes.

See reference documentation on how to add a custom background shapes.

Find the solution in our Airproof example for:

the shape declaration on shapes.xml.

adding the shape to the list thanks to primary_variable.scss and option.xml.

Based on the Airproof design, apply the shape you just added to a Text-Image building block on the homepage:

Ensure the shape is in the right position.

Set its color to the theme’s light blue.

See reference documentation on how to use background shapes.

Unlike a standard Odoo shapes, when applying a custom shape to a section, replace web_editor with illustration in the shape class.

Next, let’s add gradients to backgrounds. To enhance the page’s dynamism, apply a gradient from blue rgb(11, 142, 230) to dark blue rgb(41, 128, 187) to your ”Latest products” block. But before that, add the gradient to the website builder so that the client can easily reuse it.

See reference documentation on how to apply gradients and how to create custom gradients.

Create and declare a gradients.xml file and add the custom gradient.

Apply it to the ”Latest products” section.

The client loves the overall design but finds the page a bit static. Enhance page interactivity with animations such as fade-in, rotate, bounce, etc. These can be applied to columns, images, texts, buttons…

Based on the airproof design, animate the following elements:

the text of the first slide of the carousel.

the sticker and the photo of the drone from the first slide.

the 4 columns with icons.

Adjust animation delays for smoother transitions.

See reference documentation on how to apply Animations.

Find the solution in our Airproof example on home.xml.

The forms in Odoo are very powerful. They can send emails directly to a personal inbox or integrate directly with other Odoo applications. This is great, as one of your client’s main priorities is after-sales service. Therefore, the contact form must be properly configured.

Based on the airproof design, create a contact page. Remember to disable the default one and add the new page link to the menu. The client has the following requests for their contact form:

Name and email address field.

Conditional VAT field displayed only if Company name is filled in.

All fields should be mandatory, except for Company name.

Form submission must trigger an email.

After form submission, the thank-you message should remain visible on the contact page.

See reference documentation on how to:

deactivate default pages,

To determine the correct code for your form:

You can also find the original building block code in Odoo: odoo/addons/website/views/snippets/s_website_form.xml.

Find the solution in our Airproof example on contact.xml.

You don’t have the time to create all the service pages for the client. No worries! Create a template page that the client can use to build their own service pages.

This page should be composed as follows:

a Parallax building block,

a Key benefits building block with the title replaced by “Discover our service”,

a Call to action building block,

your custom carousel snippet.

See reference documentation on how to create page templates.

Create your new_page_template_templates.xml file and discover its content in our Airproof example.

Don’t forget to declare your file in the __manifest__.py file and define what the template page contains.

**Examples:**

Example 1 (json):
```json
<!-- Text-image block & Background shape -->
<section class="s_text_image o_cc o_cc3 o_colored_level pt120 pb96"
data-snippet="s_image_text" data-name="Image - Text" style="background-color: rgb(41, 128, 187);"
data-oe-shape-data="{'shape': 'illustration/airproof/waves', 'colors': {'c1': '#BBE1FA'}, 'flip': ['x']}">
   <div class="o_we_shape o_illustration_airproof_waves o_we_flip_x" style="background-image:
   url('/web_editor/shape/illustration%2Fairproof%2Fwaves.svg?c2=%23BBE1FA');
   background-position: 100% 100%;"/>
   [...]
</section>
```

Example 2 (json):
```json
'data': [
   # Gradients
  'data/gradients.xml',
]
```

Example 3 (jsx):
```jsx
<record id="colorpicker" model="ir.ui.view">
  <field name="key">website_airproof.colorpicker</field>
  <field name="name">Custom Gradients</field>
  <field name="type">qweb</field>
  <field name="inherit_id" ref="web_editor.colorpicker"/>
  <field name="arch" type="xml">
      <xpath expr="//div[@data-name='predefined_gradients']/t[@t-set='gradients']" position="after">
          <t t-set="gradients" t-value="gradients + ['linear-gradient(0deg, rgb(41, 128, 187) 0%,
          rgb(11, 142, 230) 100%)']"/>
      </xpath>
  </field>
</record>
```

Example 4 (jsx):
```jsx
<!-- Latest products section -->
<section data-snippet="s_dynamic_snippet_products" class="s_dynamic_snippet_products s_dynamic
s_dynamic_empty pt32 pb32 o_colored_level s_product_product_airproof o_dynamic_snippet_empty o_cc o_cc5"
data-custom-template-data="{}" data-name="Produits" data-product-category-id="all"
data-show-variants="" data-number-of-records="16" data-filter-id="3" data-carousel-interval="5000"
data-template-key="website_airproof.dynamic_filter_template_product_product_airproof"
style="background-image: linear-gradient(0deg, rgb(41, 128, 187) 0%, rgb(11, 142, 230) 100%) !important;">
   [...]
</section>
```

---

## Chapter 11: Add The Sprinkles¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/server_framework_101/11_sprinkles.html

**Contents:**
- Chapter 11: Add The Sprinkles¶
- Inline Views¶
- Widgets¶
- List Order¶
  - Model¶
  - View¶
  - Manual¶
- Attributes and options¶
  - Form¶
  - List¶

Our real estate module now makes sense from a business perspective. We created specific views, added several action buttons and constraints. However our user interface is still a bit rough. We would like to add some colors to the list views and make some fields and buttons conditionally disappear. For example, the ‘Sold’ and ‘Cancel’ buttons should disappear when the property is sold or cancelled since it is no longer allowed to change the state at this point.

This chapter covers a very small subset of what can be done in the views. Do not hesitate to read the reference documentation for a more complete overview.

Reference: the documentation related to this chapter can be found in View records and View architectures.

Goal: at the end of this section, a specific list of properties should be added to the property type view:

In the real estate module we added a list of offers for a property. We simply added the field offer_ids with:

The field uses the specific view for estate.property.offer. In some cases we want to define a specific list view which is only used in the context of a form view. For example, we would like to display the list of properties linked to a property type. However, we only want to display 3 fields for clarity: name, expected price and state.

To do this, we can define inline list views. An inline list view is defined directly inside a form view. For example:

In the form view of the test_model, we define a specific list view for test_model_line with fields field_1 and field_2.

An example can be found here.

Add an inline list view.

Add the One2many field property_ids to the estate.property.type model.

Add the field in the estate.property.type form view as depicted in the Goal of this section.

Reference: the documentation related to this section can be found in Fields.

Goal: at the end of this section, the state of the property should be displayed using a specific widget:

Four states are displayed: New, Offer Received, Offer Accepted and Sold.

Whenever we’ve added fields to our models, we’ve (almost) never had to worry about how these fields would look like in the user interface. For example, a date picker is provided for a Date field and a One2many field is automatically displayed as a list. Odoo chooses the right ‘widget’ depending on the field type.

However, in some cases, we want a specific representation of a field which can be done thanks to the widget attribute. We already used it for the tag_ids field when we used the widget="many2many_tags" attribute. If we hadn’t used it, then the field would have displayed as a list.

Each field type has a set of widgets which can be used to fine tune its display. Some widgets also take extra options. An exhaustive list can be found in Fields.

Use the status bar widget.

Use the statusbar widget in order to display the state of the estate.property as depicted in the Goal of this section.

Tip: a simple example can be found here.

Same field multiple times in a view

Add a field only once to a list or a form view. Adding it multiple times is not supported.

Reference: the documentation related to this section can be found in Models.

Goal: at the end of this section, all lists should display by default in a deterministic order. Property types can be ordered manually.

During the previous exercises, we created several list views. However, at no point did we specify which order the records had to be listed in by default. This is a very important thing for many business cases. For example, in our real estate module we would want to display the highest offers on top of the list.

Odoo provides several ways to set a default order. The most common way is to define the _order attribute directly in the model. This way, the retrieved records will follow a deterministic order which will be consistent in all views including when records are searched programmatically. By default there is no order specified, therefore the records will be retrieved in a non-deterministic order depending on PostgreSQL.

The _order attribute takes a string containing a list of fields which will be used for sorting. It will be converted to an order_by clause in SQL. For example:

Our records are ordered by descending id, meaning the highest comes first.

Define the following orders in their corresponding models:

estate.property.offer

Ordering is possible at the model level. This has the advantage of a consistent order everywhere a list of records is retrieved. However, it is also possible to define a specific order directly in a view thanks to the default_order attribute (example).

Both model and view ordering allow flexibility when sorting records, but there is still one case we need to cover: the manual ordering. A user may want to sort records depending on the business logic. For example, in our real estate module we would like to sort the property types manually. It is indeed useful to have the most used types appear at the top of the list. If our real estate agency mainly sells houses, it is more convenient to have ‘House’ appear before ‘Apartment’.

To do so, a sequence field is used in combination with the handle widget. Obviously the sequence field must be the first field in the _order attribute.

Add the following field:

Add the sequence to the estate.property.type list view with the correct widget.

Tip: you can find an example here: model and view.

It would be prohibitive to detail all the available features which allow fine tuning of the look of a view. Therefore, we’ll stick to the most common ones.

Goal: at the end of this section, the property form view will have:

Conditional display of buttons and fields

In our real estate module, we want to modify the behavior of some fields. For example, we don’t want to be able to create or edit a property type from the form view. Instead we expect the types to be handled in their appropriate menu. We also want to give tags a color. In order to add these behavior customizations, we can add the options attribute to several field widgets.

Add the appropriate option to the property_type_id field to prevent the creation and the editing of a property type from the property form view. Have a look at the Many2one widget documentation for more info.

Add the following field:

Then add the appropriate option to the tag_ids field to add a color picker on the tags. Have a look at the FieldMany2ManyTags widget documentation for more info.

In Chapter 5: Finally, Some UI To Play With, we saw that reserved fields were used for specific behaviors. For example, the active field is used to automatically filter out inactive records. We added the state as a reserved field as well. It’s now time to use it! A state field can be used in combination with an invisible attribute in the view to display buttons conditionally.

Add conditional display of buttons.

Use the invisible attribute to display the header buttons conditionally as depicted in this section’s Goal (notice how the ‘Sold’ and ‘Cancel’ buttons change when the state is modified).

Tip: do not hesitate to search for invisible= in the Odoo XML files for some examples.

More generally, it is possible to make a field invisible, readonly or required based on the value of other fields. Note that invisible can also be applied to other elements of the view such as button or group.

invisible, readonly and required can have any Python expression as value. The expression gives the condition in which the property applies. For example:

This means that the description field is invisible when is_partner is False. It is important to note that a field used in invisible must be present in the view. If it should not be displayed to the user, we can use the invisible attribute to hide it.

Make the garden area and orientation invisible in the estate.property form view when there is no garden.

Make the ‘Accept’ and ‘Refuse’ buttons invisible once the offer state is set.

Do not allow adding an offer when the property state is ‘Offer Accepted’, ‘Sold’ or ‘Cancelled’. To do this use the readonly attribute.

Using a (conditional) readonly attribute in the view can be useful to prevent data entry errors, but keep in mind that it doesn’t provide any level of security! There is no check done server-side, therefore it’s always possible to write on the field through a RPC call.

Goal: at the end of this section, the property and offer list views should have color decorations. Additionally, offers and tags will be editable directly in the list, and the availability date will be hidden by default.

When the model only has a few fields, it can be useful to edit records directly through the list view and not have to open the form view. In the real estate example, there is no need to open a form view to add an offer or create a new tag. This can be achieved thanks to the editable attribute.

Make list views editable.

Make the estate.property.offer and estate.property.tag list views editable.

On the other hand, when a model has a lot of fields it can be tempting to add too many fields in the list view and make it unclear. An alternative method is to add the fields, but make them optionally hidden. This can be achieved thanks to the optional attribute.

Make a field optional.

Make the field date_availability on the estate.property list view optional and hidden by default.

Finally, color codes are useful to visually emphasize records. For example, in the real estate module we would like to display refused offers in red and accepted offers in green. This can be achieved thanks to the decoration-{$name} attribute (see Fields for a complete list):

The records where is_partner is True will be displayed in green.

Add some decorations.

On the estate.property list view:

Properties with an offer received are green

Properties with an offer accepted are green and bold

Properties sold are muted

On the estate.property.offer list view:

Refused offers are red

Accepted offers are green

The state should not be visible anymore

Keep in mind that all fields used in attributes must be in the view!

If you want to test the color of the “Offer Received” and “Offer Accepted” states, add the field in the form view and change it manually (we’ll implement the business logic for this later).

Reference: the documentation related to this section can be found in Search and Search defaults.

Goal: at the end of this section, the available properties will be filtered by default, and searching on the living area returns results where the area is larger than the given number.

Last but not least, there are some tweaks we would like to apply when searching. First of all, we want to have our ‘Available’ filter applied by default when we access the properties. To make this happen, we need to use the search_default_{$name} action context, where {$name} is the filter name. This means that we can define which filter(s) will be activated by default at the action level.

Here is an example of an action with its corresponding filter.

Add a default filter.

Make the ‘Available’ filter selected by default in the estate.property action.

Another useful improvement in our module would be the ability to search efficiently by living area. In practice, a user will want to search for properties of ‘at least’ the given area. It is unrealistic to expect users would want to find a property of an exact living area. It is always possible to make a custom search, but that’s inconvenient.

Search view <field> elements can have a filter_domain that overrides the domain generated for searching on the given field. In the given domain, self represents the value entered by the user. In the example below, it is used to search on both name and description fields.

Change the living area search.

Add a filter_domain to the living area to include properties with an area equal to or greater than the given value.

Goal: at the end of this section, there will be a stat button on the property type form view which shows the list of all offers related to properties of the given type when it is clicked on.

If you’ve already used some functional modules in Odoo, you’ve probably already encountered a ‘stat button’. These buttons are displayed on the top right of a form view and give a quick access to linked documents. In our real estate module, we would like to have a quick link to the offers related to a given property type as depicted in the Goal of this section.

At this point of the tutorial we have already seen most of the concepts to do this. However, there is not a single solution and it can still be confusing if you don’t know where to start from. We’ll describe a step-by-step solution in the exercise. It can always be useful to find some examples in the Odoo codebase by looking for oe_stat_button.

The following exercise might be a bit more difficult than the previous ones since it assumes you are able to search for examples in the source code on your own. If you are stuck there is probably someone nearby who can help you ;-)

The exercise introduces the concept of Related fields. The easiest way to understand it is to consider it as a specific case of a computed field. The following definition of the description field:

Every time the partner name is changed, the description is modified.

Add a stat button to property type.

Add the field property_type_id to estate.property.offer. We can define it as a related field on property_id.property_type_id and set it as stored.

Thanks to this field, an offer will be linked to a property type when it’s created. You can add the field to the list view of offers to make sure it works.

Add the field offer_ids to estate.property.type which is the One2many inverse of the field defined in the previous step.

Add the field offer_count to estate.property.type. It is a computed field that counts the number of offers for a given property type (use offer_ids to do so).

At this point, you have all the information necessary to know how many offers are linked to a property type. When in doubt, add offer_ids and offer_count directly to the view. The next step is to display the list when clicking on the stat button.

Create a stat button on estate.property.type pointing to the estate.property.offer action. This means you should use the type="action" attribute (go back to the end of Chapter 9: Ready For Some Action? if you need a refresher).

At this point, clicking on the stat button should display all offers. We still need to filter out the offers.

On the estate.property.offer action, add a domain that defines property_type_id as equal to the active_id (= the current record, here is an example)

Looking good? If not, don’t worry, the next chapter doesn’t require stat buttons ;-)

**Examples:**

Example 1 (jsx):
```jsx
<field name="offer_ids"/>
```

Example 2 (python):
```python
from odoo import fields, models

class TestModel(models.Model):
    _name = "test_model"
    _description = "Test Model"

    description = fields.Char()
    line_ids = fields.One2many("test_model_line", "model_id")


class TestModelLine(models.Model):
    _name = "test_model_line"
    _description = "Test Model Line"

    model_id = fields.Many2one("test_model")
    field_1 = fields.Char()
    field_2 = fields.Char()
    field_3 = fields.Char()
```

Example 3 (jsx):
```jsx
<form>
    <field name="description"/>
    <field name="line_ids">
        <list>
            <field name="field_1"/>
            <field name="field_2"/>
        </list>
    </field>
</form>
```

Example 4 (python):
```python
from odoo import fields, models

class TestModel(models.Model):
    _name = "test_model"
    _description = "Test Model"
    _order = "id desc"

    description = fields.Char()
```

---

## Chapter 12: Inheritance¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/server_framework_101/12_inheritance.html

**Contents:**
- Chapter 12: Inheritance¶
- Python Inheritance¶
- Model Inheritance¶
- View Inheritance¶

A powerful aspect of Odoo is its modularity. A module is dedicated to a business need, but modules can also interact with one another. This is useful for extending the functionality of an existing module. For example, in our real estate scenario we want to display the list of a salesperson’s properties directly in the regular user view.

But before going through the specific Odoo module inheritance, let’s see how we can alter the behavior of the standard CRUD (Create, Retrieve, Update or Delete) methods.

Goal: at the end of this section:

It should not be possible to delete a property which is not new or cancelled.

When an offer is created, the property state should change to ‘Offer Received’

It should not be possible to create an offer with a lower price than an existing offer

In our real estate module, we never had to develop anything specific to be able to do the standard CRUD actions. The Odoo framework provides the necessary tools to do them. In fact, such actions are already included in our model thanks to classical Python inheritance:

Our class TestModel inherits from Model which provides create(), read(), write() and unlink().

These methods (and any other method defined on Model) can be extended to add specific business logic:

The decorator model() is necessary for the create() method because the content of the recordset self is not relevant in the context of creation, but it is not necessary for the other CRUD methods.

It is also important to note that even though we can directly override the unlink() method, you will almost always want to write a new method with the decorator ondelete() instead. Methods marked with this decorator will be called during unlink() and avoids some issues that can occur during uninstalling the model’s module when unlink() is directly overridden.

In Python 3, super() is equivalent to super(TestModel, self). The latter may be necessary when you need to call the parent method with a modified recordset.

It is very important to always call super() to avoid breaking the flow. There are only a few very specific cases where you don’t want to call it.

Make sure to always return data consistent with the parent method. For example, if the parent method returns a dict(), your override must also return a dict().

Add business logic to the CRUD methods.

Prevent deletion of a property if its state is not ‘New’ or ‘Cancelled’

Tip: create a new method with the ondelete() decorator and remember that self can be a recordset with more than one record.

At offer creation, set the property state to ‘Offer Received’. Also raise an error if the user tries to create an offer with a lower amount than an existing offer.

Tip: the property_id field is available in the vals, but it is an int. To instantiate an estate.property object, use self.env[model_name].browse(value) (example)

Reference: the documentation related to this topic can be found in Inheritance and extension.

In our real estate module, we would like to display the list of properties linked to a salesperson directly in the Settings / Users & Companies / Users form view. To do this, we need to add a field to the res.users model and adapt its view to show it.

Odoo provides two inheritance mechanisms to extend an existing model in a modular way.

The first inheritance mechanism allows modules to modify the behavior of a model defined in an another module by:

adding fields to the model,

overriding the definition of fields in the model,

adding constraints to the model,

adding methods to the model,

overriding existing methods in the model.

The second inheritance mechanism (delegation) allows every record of a model to be linked to a parent model’s record and provides transparent access to the fields of this parent record.

In Odoo, the first mechanism is by far the most used. In our case, we want to add a field to an existing model, which means we will use the first mechanism. For example:

A practical example where two fields are added to a model can be found here.

By convention, each inherited model is defined in its own Python file. In our example, it would be models/inherited_model.py.

Add a field to Users.

Add the following field to res.users:

One2many inverse of the field that references the salesperson in estate.property

Add a domain to the field so it only lists the available properties.

In the next section let’s add the field to the view and check that everything is working well!

Reference: the documentation related to this topic can be found in Inheritance.

Goal: at the end of this section, the list of available properties linked to a salesperson should be displayed in their user form view

Instead of modifying existing views in place (by overwriting them), Odoo provides view inheritance where children ‘extension’ views are applied on top of root views. These extension can both add and remove content from their parent view.

An extension view references its parent using the inherit_id field. Instead of a single view, its arch field contains a number of xpath elements that select and alter the content of their parent view:

An XPath expression selecting a single element in the parent view. Raises an error if it matches no element or more than one

Operation to apply to the matched element:

appends xpath’s body to the end of the matched element

replaces the matched element with the xpath’s body, replacing any $0 node occurrence in the new body with the original element

inserts the xpath’s body as a sibling before the matched element

inserts the xpaths’s body as a sibling after the matched element

alters the attributes of the matched element using the special attribute elements in the xpath’s body

When matching a single element, the position attribute can be set directly on the element to be found. Both inheritances below have the same result.

An example of a view inheritance extension can be found here.

Add fields to the Users view.

Add the property_ids field to the base.view_users_form in a new notebook page.

Tip: an example an inheritance of the users’ view can be found here.

Inheritance is extensively used in Odoo due to its modular concept. Do not hesitate to read the corresponding documentation for more info!

In the next chapter, we will learn how to interact with other modules.

**Examples:**

Example 1 (python):
```python
from odoo import fields, models

class TestModel(models.Model):
    _name = "test_model"
    _description = "Test Model"

    ...
```

Example 2 (python):
```python
from odoo import fields, models

class TestModel(models.Model):
    _name = "test_model"
    _description = "Test Model"

    ...

    @api.model
    def create(self, vals_list):
        # Do some business logic, modify vals...
        ...
        # Then call super to execute the parent method
        return super().create(vals_list)
```

Example 3 (python):
```python
from odoo import fields, models

class InheritedModel(models.Model):
    _inherit = ["inherited.model"]

    new_field = fields.Char(string="New Field")
```

Example 4 (jsx):
```jsx
<record id="inherited_model_view_form" model="ir.ui.view">
    <field name="name">inherited.model.form.inherit.test</field>
    <field name="model">inherited.model</field>
    <field name="inherit_id" ref="inherited.inherited_model_view_form"/>
    <field name="arch" type="xml">
        <!-- find field description and add the field
             new_field after it -->
        <xpath expr="//field[@name='description']" position="after">
          <field name="new_field"/>
        </xpath>
    </field>
</record>
```

---

## Chapter 2 - Build your website¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/website_theme/02_build_website.html

**Contents:**
- Chapter 2 - Build your website¶
- Create a page¶
- Add a media¶
- Add building blocks¶
- Navigation¶

Now that the theme has been set up, let’s move on to creating the content.

First of all, start by creating your first theme page: the home page. For now, only indicate “Hello” as content in the page.

You will need to deactivate the default homepage.

See reference documentation on how to desactivate a default page and how to start a new page.

If you want the client to be able to reuse certain pictures that you are going to add on the website, they must be added to the image library.

To do the test, declare the drone image to add it to the library. You will find the drone picture here.

See reference documentation on how to add a media.

Go to the Website Builder, double-click on the logo, and you will see the drone image in the library.

To complete this exercise, you need to:

Put your PNG in the right image folder.

Create your images.xml file. You can find all the necessary information in the images.xml file from our example module.

Declare your file in the __manifest__.py.

Now, let’s get into the real work. Start adding content to the pages.

In an Odoo website, we create the content of a page using building blocks. These can be compared to snippets editable by the user in the Website Builder. The standard main container for any snippet is a section.

Based on the Airproof design, add the following elements to the homepage :

Create a section with the 3 boxes using the Big boxes building block.

For this section, you don’t want the future user to be able to edit it via the Website Builder.

Put an opacity filter on the background image of the 3 boxes.

Create another section containing a title and icons using the grid system.

You can use these images and icons.

See reference documentation on how to write standard building blocks and how to use the grid layout.

To determine the code needed to create your building blocks :

You can also find the original building block code in Odoo : odoo/addons/website/views/snippets/**.xml.

Find the solution in our Airproof example on home.xml.

For now, the client is fine with the default header but has requested some navigation adjustments.

The client has requested the following changes:

Add the Airproof logo. At the same time, set the name and favicon of the website.

Remove the link to the homepage and the shop.

Add a link to the future “About us” page.

Replace the default blog item with a dropdown to display the different blogs: “Our latest news” and “Tutorials”.

Add a mega-menu “Waterproof drones” to display the different products.

See how to set up the website settings (including the logo).

You can find the original mega-menu templates code in Odoo : odoo/addons/website/views/snippets/s_mega_menu_**.xml

See reference documentation on how to modifiy the Navigation.

Make sure the Blog app is installed and create the two different blogs in the backend.

Create the different products via the backend. You can use these product pictures.

If you want to make the logo available in the Media dialog, you have to add the record image.

It could be interesting to put a noupdate on the navigation so that the changes made later by your client won’t be overwritten if you have to update the module.

To complete this exercise, you need to:

Add the logo and favicon in the right folder.

Put the website records in a website.xml file:

Declare your website.xml file in __manifest__.py.

Create your menu as in the menu.xml from our Airproof example.

**Examples:**

Example 1 (json):
```json
'data': [
   # Pages
   'data/pages/home.xml',
]
```

Example 2 (xml):
```xml
<?xml version="1.0" encoding="utf-8"?>
<odoo noupdate="1">
   <!-- Deactivate default homepage -->
   <record id="website.homepage" model="ir.ui.view">
      <field name="active" eval="False"/>
   </record>
   <!-- Home -->
   <record id="page_home" model="website.page">
      <field name="name">Home</field>
      <field name="is_published" eval="True"/>
      <field name="key">website_airproof.page_home</field>
      <field name="url">/</field>
      <field name="type">qweb</field>
      <field name="arch" type="xml">
         <t t-name="website_airproof.page_home">
            <t t-call="website.layout">
               <!-- Title -->
               <t t-set="additional_title">One step beyond the horizon | Airproof</t>
               <!-- Content -->
               <div id="wrap" class="oe_structure">
                  <p>Hello</p>
               </div>
            </t>
         </t>
      </field>
   </record>
</odoo>
```

Example 3 (jsx):
```jsx
<record id="website.default_website" model="website">
   <field name="name">Airproof</field>
   <field name="logo" type="base64" file="website_airproof/static/src/img/content/branding/airproof-logo.svg"/>
   <field name="favicon" type="base64" file="website_airproof/static/description/airproof-favicon.png" />
</record>
```

---

## Builds¶

**URL:** https://www.odoo.com/documentation/19.0/administration/odoo_sh/getting_started/builds.html

**Contents:**
- Builds¶
- Overview¶
- Stages¶
  - Production¶
  - Staging¶
  - Development¶
- Features¶

In Odoo.sh, a build is a database loaded by an Odoo server (odoo/odoo and odoo/enterprise) running on a specific revision of your project repository in a containerized environment. Its purpose is to test the proper behavior of the server, the database, and the features associated with that revision.

In the Builds view, a row represents a branch, and a cell within that row represents a build of that branch.

Most builds are created after pushes to your GitHub repository branches. They can also be created through other operations, such as importing a database on Odoo.sh or requesting a rebuild for a branch in your project.

Builds can have three possible statuses:

A build is considered successful if no errors or warnings occur during its creation. Successful builds are highlighted in green.

A build is considered almost successful if warnings occur, but there are no errors. Almost successful builds are highlighted in yellow.

A build is considered failed if errors occur during its creation. Failed builds are highlighted in red.

Builds do not always create a database from scratch. For instance, when pushing a change on the production branch, the created build starts the server with your new revision and tries to load the current production database on it.

The first build of a production branch creates a database from scratch. If this build is successful, this database will become the production database of your project.

From then on, pushes to the production branch will create new builds that attempt to load the database using a server running the new revision.

If the build is successful or almost successful, the production database will run with this build and its associated revision.

If the build fails to load or update the database, the previous successful build is reused to load the database. In that case, the database continues to run using the previous successful revision.

The build used to run the production database is always the first in the builds list. If a build fails, it is placed after the build currently running the production database.

Staging builds duplicate the production database and attempt to load this copy using the revisions of the staging branches.

Each time you push a new revision to a staging branch, the resulting build uses a fresh copy of the production database. Databases are not reused between builds of the same branch. This ensures that:

Staging builds use databases that closely match the current production state, so your tests are not performed on outdated data.

You can freely experiment within a staging database. When you want to start over with a new copy of the production database, you can request a rebuild.

However, this also means that if you make configuration changes in a staging database and do not apply them in production, those changes will not be present in the next build of the same staging branch.

Development builds create new databases, load the demo data, and run the unit tests.

A build will be considered failed if tests fail during installation, as they are designed to raise errors when something is wrong.

If all tests pass and no errors occur, the build is considered successful.

Depending on the list of modules to install and test, a development build can take up to one hour to be ready. This is due to the large number of tests included in the default Odoo module suite.

The production branch always appears first. Other branches are ordered by the time of their last created build. The stage highlighted in purple corresponds to the stage selected in the Branches menu.

You can filter branches using the search bar.

For each branch, you can:

Access the latest build’s database by clicking Connect.

Jump to the branch’s code by clicking Github.

Create a new build by clicking Rebuild. It uses the latest revision of the branch (it is not available if a build is already in progress for that branch).

For each build, you can:

View the revision changes by clicking the (GitHub) icon.

Access the build’s database as the administrator by clicking Connect or as another user by clicking the (More Actions) button next to Connect and selecting Connect as.

Access the same tools as in the branches view by clicking the (More Actions) button next to Connect and selecting Logs, Web Shell, Editor, Outgoing e-mails (for the staging and development stages), Monitoring, and Download DB dump (for the production and staging stages).

---

## Customizing the web client¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/web.html

**Contents:**
- Customizing the web client¶
- A Simple Module¶
- Odoo JavaScript Module¶
- Classes¶
- Widgets Basics¶
  - Your First Widget¶
  - Display Content¶
  - Widget Parents and Children¶
  - Destroying Widgets¶
- The QWeb Template Engine¶

This tutorial is outdated.

This guide is about creating modules for Odoo’s web client.

To create websites with Odoo, see Build a website theme; to add business capabilities or extend existing business systems of Odoo, see Building a Module.

This guide assumes knowledge of:

Javascript basics and good practices

It also requires an installed Odoo, and Git.

Let’s start with a simple Odoo module holding basic web component configuration and letting us test the web framework.

The example module is available online and can be downloaded using the following command:

This will create a petstore folder wherever you executed the command. You then need to add that folder to Odoo’s addons path, create a new database and install the oepetstore module.

If you browse the petstore folder, you should see the following content:

The module already holds various server customizations. We’ll come back to these later, for now let’s focus on the web-related content, in the static folder.

Files used in the “web” side of an Odoo module must be placed in a static folder so they are available to a web browser, files outside that folder can not be fetched by browsers. The src/css, src/js and src/xml sub-folders are conventional and not strictly necessary.

Currently empty, will hold the CSS for pet store content

Mostly empty, will hold QWeb Templates templates

The most important (and interesting) part, contains the logic of the application (or at least its web-browser side) as javascript. It should currently look like:

Which only prints a small message in the browser’s console.

The files in the static folder, need to be defined within the module in order for them to be loaded correctly. Everything in src/xml is defined in __manifest__.py while the contents of src/css and src/js are defined in petstore.xml, or a similar file.

All JavaScript files are concatenated and minified to improve application load time.

One of the drawback is debugging becomes more difficult as individual files disappear and the code is made significantly less readable. It is possible to disable this process by enabling the “developer mode”: log into your Odoo instance (user admin password admin by default) open the user menu (in the top-right corner of the Odoo screen) and select About Odoo then Activate the developer mode:

This will reload the web client with optimizations disabled, making development and debugging significantly more comfortable.

Javascript doesn’t have built-in modules. As a result variables defined in different files are all mashed together and may conflict. This has given rise to various module patterns used to build clean namespaces and limit risks of naming conflicts.

The Odoo framework uses one such pattern to define modules within web addons, in order to both namespace code and correctly order its loading.

oepetstore/static/js/petstore.js contains a module declaration:

In Odoo web, modules are declared as functions set on the global odoo variable. The function’s name must be the same as the addon (in this case oepetstore) so the framework can find it, and automatically initialize it.

When the web client loads your module it will call the root function and provide two parameters:

the first parameter is the current instance of the Odoo web client, it gives access to various capabilities defined by the Odoo (translations, network services) as well as objects defined by the core or by other modules.

the second parameter is your own local namespace automatically created by the web client. Objects and variables which should be accessible from outside your module (either because the Odoo web client needs to call them or because others may want to customize them) should be set inside that namespace.

Much as modules, and contrary to most object-oriented languages, javascript does not build in classes1 although it provides roughly equivalent (if lower-level and more verbose) mechanisms.

For simplicity and developer-friendliness Odoo web provides a class system based on John Resig’s Simple JavaScript Inheritance.

New classes are defined by calling the extend() method of odoo.web.Class():

The extend() method takes a dictionary describing the new class’s content (methods and static attributes). In this case, it will only have a say_hello method which takes no parameters.

Classes are instantiated using the new operator:

And attributes of the instance can be accessed via this:

Classes can provide an initializer to perform the initial setup of the instance, by defining an init() method. The initializer receives the parameters passed when using the new operator:

It is also possible to create subclasses from existing (used-defined) classes by calling extend() on the parent class, as is done to subclass Class():

When overriding a method using inheritance, you can use this._super() to call the original method:

_super is not a standard method, it is set on-the-fly to the next method in the current inheritance chain, if any. It is only defined during the synchronous part of a method call, for use in asynchronous handlers (after network calls or in setTimeout callbacks) a reference to its value should be retained, it should not be accessed via this:

The Odoo web client bundles jQuery for easy DOM manipulation. It is useful and provides a better API than standard W3C DOM2, but insufficient to structure complex applications leading to difficult maintenance.

Much like object-oriented desktop UI toolkits (e.g. Qt, Cocoa or GTK), Odoo Web makes specific components responsible for sections of a page. In Odoo web, the base for such components is the Widget() class, a component specialized in handling a page section and displaying information for the user.

The initial demonstration module already provides a basic widget:

It extends Widget() and overrides the standard method start(), which — much like the previous MyClass — does little for now.

This line at the end of the file:

registers our basic widget as a client action. Client actions will be explained later, for now this is just what allows our widget to be called and displayed when we select the Pet Store ‣ Pet Store ‣ Home Page menu.

because the widget will be called from outside our module, the web client needs its “fully qualified” name, not the local version.

Widgets have a number of methods and features, but the basics are simple:

format the widget’s data

The HomePage widget already has a start() method. That method is part of the normal widget lifecycle and automatically called once the widget is inserted in the page. We can use it to display some content.

All widgets have a $el which represents the section of page they’re in charge of (as a jQuery object). Widget content should be inserted there. By default, $el is an empty <div> element.

A <div> element is usually invisible to the user if it has no content (or without specific styles giving it a size) which is why nothing is displayed on the page when HomePage is launched.

Let’s add some content to the widget’s root element, using jQuery:

That message will now appear when you open Pet Store ‣ Pet Store ‣ Home Page

to refresh the javascript code loaded in Odoo Web, you will need to reload the page. There is no need to restart the Odoo server.

The HomePage widget is used by Odoo Web and managed automatically. To learn how to use a widget “from scratch” let’s create a new one:

We can now add our GreetingsWidget to the HomePage by using the GreetingsWidget’s appendTo() method:

HomePage first adds its own content to its DOM root

HomePage then instantiates GreetingsWidget

Finally it tells GreetingsWidget where to insert itself, delegating part of its $el to the GreetingsWidget.

When the appendTo() method is called, it asks the widget to insert itself at the specified position and to display its content. The start() method will be called during the call to appendTo().

To see what happens under the displayed interface, we will use the browser’s DOM Explorer. But first let’s alter our widgets slightly so we can more easily find where they are, by adding a class to their root elements:

If you can find the relevant section of the DOM (right-click on the text then Inspect Element), it should look like this:

Which clearly shows the two <div> elements automatically created by Widget(), because we added some classes on them.

We can also see the two message-holding divs we added ourselves

Finally, note the <div class="oe_petstore_greetings"> element which represents the GreetingsWidget instance is inside the <div class="oe_petstore_homepage"> which represents the HomePage instance, since we appended

In the previous part, we instantiated a widget using this syntax:

The first argument is this, which in that case was a HomePage instance. This tells the widget being created which other widget is its parent.

As we’ve seen, widgets are usually inserted in the DOM by another widget and inside that other widget’s root element. This means most widgets are “part” of another widget, and exist on behalf of it. We call the container the parent, and the contained widget the child.

Due to multiple technical and conceptual reasons, it is necessary for a widget to know who is its parent and who are its children.

can be used to get the parent of a widget:

can be used to get a list of its children:

When overriding the init() method of a widget it is of the utmost importance to pass the parent to the this._super() call, otherwise the relation will not be set up correctly:

Finally, if a widget does not have a parent (e.g. because it’s the root widget of the application), null can be provided as parent:

If you can display content to your users, you should also be able to erase it. This is done via the destroy() method:

When a widget is destroyed it will first call destroy() on all its children. Then it erases itself from the DOM. If you have set up permanent structures in init() or start() which must be explicitly cleaned up (because the garbage collector will not handle them), you can override destroy().

when overriding destroy(), _super() must always be called otherwise the widget and its children are not correctly cleaned up leaving possible memory leaks and “phantom events”, even if no error is displayed

In the previous section we added content to our widgets by directly manipulating (and adding to) their DOM:

This allows generating and displaying any type of content, but gets unwieldy when generating significant amounts of DOM (lots of duplication, quoting issues, …)

As many other environments, Odoo’s solution is to use a template engine. Odoo’s template engine is called QWeb Templates.

QWeb is an XML-based templating language, similar to Genshi, Thymeleaf or Facelets. It has the following characteristics:

It’s implemented fully in JavaScript and rendered in the browser

Each template file (XML files) contains multiple templates

It has special support in Odoo Web’s Widget(), though it can be used outside of Odoo’s web client (and it’s possible to use Widget() without relying on QWeb)

The rationale behind using QWeb instead of existing javascript template engines is the extensibility of pre-existing (third-party) templates, much like Odoo views.

Most javascript template engines are text-based which precludes easy structural extensibility where an XML-based templating engine can be generically altered using e.g. XPath or CSS and a tree-alteration DSL (or even just XSLT). This flexibility and extensibility is a core characteristic of Odoo, and losing it was considered unacceptable.

First let’s define a simple QWeb template in the almost-empty oepetstore/static/src/xml/petstore.xml file:

Now we can use this template inside of the HomePage widget. Using the QWeb loader variable defined at the top of the page, we can call to the template defined in the XML file:

QWeb.render() looks for the specified template, renders it to a string and returns the result.

However, because Widget() has special integration for QWeb the template can be set directly on the widget via its template attribute:

Although the result looks similar, there are two differences between these usages:

with the second version, the template is rendered right before start() is called

in the first version the template’s content is added to the widget’s root element, whereas in the second version the template’s root element is directly set as the widget’s root element. Which is why the “greetings” sub-widget also gets a red background

templates should have a single non-t root element, especially if they’re set as a widget’s template. If there are multiple “root elements”, results are undefined (usually only the first root element will be used and the others will be ignored)

QWeb templates can be given data and can contain basic display logic.

For explicit calls to QWeb.render(), the template data is passed as second parameter:

with the template modified to:

When using Widget()’s integration it is not possible to provide additional data to the template. The template will be given a single widget context variable, referencing the widget being rendered right before start() is called (the widget’s state will essentially be that set up by init()):

We’ve seen how to render QWeb templates, let’s now see the syntax of the templates themselves.

A QWeb template is composed of regular XML mixed with QWeb directives. A QWeb directive is declared with XML attributes starting with t-.

The most basic directive is t-name, used to declare new templates in a template file:

t-name takes the name of the template being defined, and declares that it can be called using QWeb.render(). It can only be used at the top-level of a template file.

The t-esc directive can be used to output text:

It takes a Javascript expression which is evaluated, the result of the expression is then HTML-escaped and inserted in the document. Since it’s an expression it’s possible to provide just a variable name as above, or a more complex expression like a computation:

To inject HTML in the page being rendered, use t-raw. Like t-esc it takes an arbitrary Javascript expression as parameter, but it does not perform an HTML-escape step.

t-raw must not be used on any data which may contain non-escaped user-provided content as this leads to cross-site scripting vulnerabilities

QWeb can have conditional blocks using t-if. The directive takes an arbitrary expression, if the expression is falsy (false, null, 0 or an empty string) the whole block is suppressed, otherwise it is displayed.

QWeb doesn’t have an “else” structure, use a second t-if with the original condition inverted. You may want to store the condition in a local variable if it’s a complex or expensive expression.

To iterate on a list, use t-foreach and t-as. t-foreach takes an expression returning a list to iterate on t-as takes a variable name to bind to each item during iteration.

t-foreach can also be used with numbers and objects (dictionaries)

QWeb provides two related directives to define computed attributes: t-att-name and t-attf-name. In either case, name is the name of the attribute to create (e.g. t-att-id defines the attribute id after rendering).

t-att- takes a javascript expression whose result is set as the attribute’s value, it is most useful if all of the attribute’s value is computed:

t-attf- takes a format string. A format string is literal text with interpolation blocks inside, an interpolation block is a javascript expression between {{ and }}, which will be replaced by the result of the expression. It is most useful for attributes which are partially literal and partially computed such as a class:

Templates can be split into sub-templates (for simplicity, maintainability, reusability or to avoid excessive markup nesting).

This is done using the t-call directive, which takes the name of the template to render:

rendering the A template will result in:

Sub-templates inherit the rendering context of their caller.

For a QWeb reference, see QWeb Templates.

Usage of QWeb in Widgets

Create a widget whose constructor takes two parameters aside from parent: product_names and color.

product_names should an array of strings, each one the name of a product

color is a string containing a color in CSS color format (ie: #000000 for black).

The widget should display the given product names one under the other, each one in a separate box with a background color with the value of color and a border. You should use QWeb to render the HTML. Any necessary CSS should be in oepetstore/static/src/css/petstore.css.

Use the widget in HomePage with half a dozen products.

Selecting DOM elements within a widget can be performed by calling the find() method on the widget’s DOM root:

But because it’s a common operation, Widget() provides an equivalent shortcut through the $() method:

The global jQuery function $() should never be used unless it is absolutely necessary: selection on a widget’s root are scoped to the widget and local to it, but selections with $() are global to the page/application and may match parts of other widgets and views, leading to odd or dangerous side-effects. Since a widget should generally act only on the DOM section it owns, there is no cause for global selection.

We have previously bound DOM events using normal jQuery event handlers (e.g. .click() or .change()) on widget elements:

While this works it has a few issues:

it does not support replacing the widget’s root element at runtime as the binding is only performed when start() is run (during widget initialization)

it requires dealing with this-binding issues

Widgets thus provide a shortcut to DOM event binding via events:

events is an object (mapping) of an event to the function or method to call when the event is triggered:

the key is an event name, possibly refined with a CSS selector in which case only if the event happens on a selected sub-element will the function or method run: click will handle all clicks within the widget, but click .my_button will only handle clicks in elements bearing the my_button class

the value is the action to perform when the event is triggered

It can be either a function:

or the name of a method on the object (see example above).

In either case, the this is the widget instance and the handler is given a single parameter, the jQuery event object for the event.

Widgets provide an event system (separate from the DOM/jQuery event system described above): a widget can fire events on itself, and other widgets (or itself) can bind themselves and listen for these events:

This widget acts as a facade, transforming user input (through DOM events) into a documentable internal event to which parent widgets can bind themselves.

trigger() takes the name of the event to trigger as its first (mandatory) argument, any further arguments are treated as event data and passed directly to listeners.

We can then set up a parent event instantiating our generic widget and listening to the user_chose event using on():

on() binds a function to be called when the event identified by event_name is. The func argument is the function to call and object is the object to which that function is related if it is a method. The bound function will be called with the additional arguments of trigger() if it has any. Example:

Triggering events on an other widget is generally a bad idea. The main exception to that rule is odoo.web.bus which exists specifically to broadcasts evens in which any widget could be interested throughout the Odoo web application.

Properties are very similar to normal object attributes in that they allow storing data on a widget instance, however they have the additional feature that they trigger events when set:

set() sets the value of a property and triggers change:propname (where propname is the property name passed as first parameter to set()) and change

get() retrieves the value of a property.

Widget Properties and Events

Create a widget ColorInputWidget that will display 3 <input type="text">. Each of these <input> is dedicated to type a hexadecimal number from 00 to FF. When any of these <input> is modified by the user the widget must query the content of the three <input>, concatenate their values to have a complete CSS color code (ie: #00FF00) and put the result in a property named color. Please note the jQuery change() event that you can bind on any HTML <input> element and the val() method that can query the current value of that <input> could be useful to you for this exercise.

Then, modify the HomePage widget to instantiate ColorInputWidget and display it. The HomePage widget should also display an empty rectangle. That rectangle must always, at any moment, have the same background color as the color in the color property of the ColorInputWidget instance.

Use QWeb to generate all HTML.

The class system of the Odoo web framework allows direct modification of existing classes using the include() method:

This system is similar to the inheritance mechanism, except it will alter the target class in-place instead of creating a new class.

In that case, this._super() will call the original implementation of a method being replaced/redefined. If the class already had sub-classes, all calls to this._super() in sub-classes will call the new implementations defined in the call to include(). This will also work if some instances of the class (or of any of its sub-classes) were created prior to the call to include().

The process to translate text in Python and JavaScript code is very similar. You could have noticed these lines at the beginning of the petstore.js file:

These lines are simply used to import the translation functions in the current JavaScript module. They are used thus:

In Odoo, translations files are automatically generated by scanning the source code. All piece of code that calls a certain function are detected and their content is added to a translation file that will then be sent to the translators. In Python, the function is _(). In JavaScript the function is _t() (and also _lt()).

_t() will return the translation defined for the text it is given. If no translation is defined for that text, it will return the original text as-is.

To inject user-provided values in translatable strings, it is recommended to use _.str.sprintf with named arguments after the translation:

This makes translatable strings more readable to translators, and gives them more flexibility to reorder or ignore parameters.

_lt() (“lazy translate”) is similar but somewhat more complex: instead of translating its parameter immediately, it returns an object which, when converted to a string, will perform the translation.

It is used to define translatable terms before the translations system is initialized, for class attributes for instance (as modules are loaded before the user’s language is configured and translations are downloaded).

Most operations with Odoo involve communicating with models implementing business concern, these models will then (potentially) interact with some storage engine (usually PostgreSQL).

Although jQuery provides a $.ajax function for network interactions, communicating with Odoo requires additional metadata whose setup before every call would be verbose and error-prone. As a result, Odoo web provides higher-level communication primitives.

To demonstrate this, the file petstore.py already contains a small model with a sample method:

This declares a model with two fields, and a method my_method() which returns a literal dictionary.

Here is a sample widget that calls my_method() and displays the result:

The class used to call Odoo models is odoo.Model(). It is instantiated with the Odoo model’s name as first parameter (oepetstore.message_of_the_day here).

call() can be used to call any (public) method of an Odoo model. It takes the following positional arguments:

The name of the method to call, my_method here

an array of positional arguments to provide to the method. Because the example has no positional argument to provide, the args parameter is not provided.

Here is an other example with positional arguments:

a mapping of keyword arguments to pass. The example provides a single named argument context.

call() returns a deferred resolved with the value returned by the model’s method as first argument.

The previous section used a context argument which was not explained in the method call:

The context is like a “magic” argument that the web client will always give to the server when calling a method. The context is a dictionary containing multiple keys. One of the most important key is the language of the user, used by the server to translate all the messages of the application. Another one is the time zone of the user, used to compute correctly dates and times if Odoo is used by people in different countries.

The argument is necessary in all methods, otherwise bad things could happen (such as the application not being translated correctly). That’s why, when you call a model’s method, you should always provide that argument. The solution to achieve that is to use odoo.web.CompoundContext().

CompoundContext() is a class used to pass the user’s context (with language, time zone, etc…) to the server as well as adding new keys to the context (some models’ methods use arbitrary keys added to the context). It is created by giving to its constructor any number of dictionaries or other CompoundContext() instances. It will merge all those contexts before sending them to the server.

You can see the dictionary in the argument context contains some keys that are related to the configuration of the current user in Odoo plus the new_key key that was added when instantiating CompoundContext().

While call() is sufficient for any interaction with Odoo models, Odoo Web provides a helper for simpler and clearer querying of models (fetching of records based on various conditions): query() which acts as a shortcut for the common combination of search() and :read(). It provides a clearer syntax to search and read models:

query() takes an optional list of fields as parameter (if no field is provided, all fields of the model are fetched). It returns a odoo.web.Query() which can be further customized before being executed

Query() represents the query being built. It is immutable, methods to customize the query actually return a modified copy, so it’s possible to use the original and the new version side-by-side. See Query() for its customization options.

When the query is set up as desired, simply call all() to execute it and return a deferred to its result. The result is the same as read()’s, an array of dictionaries where each dictionary is a requested record, with each requested field a dictionary key.

Create a MessageOfTheDay widget displaying the last record of the oepetstore.message_of_the_day model. The widget should fetch its record as soon as it is displayed.

Display the widget in the Pet Store home page.

Create a PetToysList widget displaying 5 toys (using their name and their images).

The pet toys are not stored in a new model, instead they’re stored in product.product using a special category Pet Toys. You can see the pre-generated toys and add new ones by going to Pet Store ‣ Pet Store ‣ Pet Toys. You will probably need to explore product.product to create the right domain to select just pet toys.

In Odoo, images are generally stored in regular fields encoded as base64, HTML supports displaying images straight from base64 with <img src="data:mime_type;base64,base64_image_data"/>

The PetToysList widget should be displayed on the home page on the right of the MessageOfTheDay widget. You will need to make some layout with CSS to achieve this.

In Odoo, many operations start from an action: opening a menu item (to a view), printing a report, …

Actions are pieces of data describing how a client should react to the activation of a piece of content. Actions can be stored (and read through a model) or they can be generated on-the fly (locally to the client by javascript code, or remotely by a method of a model).

In Odoo Web, the component responsible for handling and reacting to these actions is the Action Manager.

The action manager can be invoked explicitly from javascript code by creating a dictionary describing an action of the right type, and calling an action manager instance with it.

do_action() is a shortcut of Widget() looking up the “current” action manager and executing the action:

The most common action type is ir.actions.act_window which provides views to a model (displays a model in various manners), its most common attributes are:

The model to display in views

For form views, a preselected record in res_model

Lists the views available through the action. A list of [view_id, view_type], view_id can either be the database identifier of a view of the right type, or false to use the view by default for the specified type. View types can not be present multiple times. The action will open the first view of the list by default.

Either current (the default) which replaces the “content” section of the web client by the action, or new to open the action in a dialog box.

Additional context data to use within the action.

Modify the PetToysList component so clicking on a toy replaces the homepage by the toy’s form view.

Throughout this guide, we used a simple HomePage widget which the web client automatically starts when we select the right menu item. But how did the Odoo web know to start this widget? Because the widget is registered as a client action.

A client action is (as its name implies) an action type defined almost entirely in the client, in javascript for Odoo web. The server simply sends an action tag (an arbitrary name), and optionally adds a few parameters, but beyond that everything is handled by custom client code.

Our widget is registered as the handler for the client action through this:

instance.web.client_actions is a Registry() in which the action manager looks up client action handlers when it needs to execute one. The first parameter of add() is the name (tag) of the client action, and the second parameter is the path to the widget from the Odoo web client root.

When a client action must be executed, the action manager looks up its tag in the registry, walks the specified path and displays the widget it finds at the end.

a client action handler can also be a regular function, in which case it’ll be called and its result (if any) will be interpreted as the next action to execute.

On the server side, we had simply defined an ir.actions.client action:

and a menu opening the action:

Much of Odoo web’s usefulness (and complexity) resides in views. Each view type is a way of displaying a model in the client.

When an ActionManager instance receive an action of type ir.actions.act_window, it delegates the synchronization and handling of the views themselves to a view manager, which will then set up one or multiple views depending on the original action’s requirements:

Most Odoo views are implemented through a subclass of odoo.web.View() which provides a bit of generic basic structure for handling events and displaying model information.

The search view is considered a view type by the main Odoo framework, but handled separately by the web client (as it’s a more permanent fixture and can interact with other views, which regular views don’t do).

A view is responsible for loading its own description XML (using fields_view_get) and any other data source it needs. To that purpose, views are provided with an optional view identifier set as the view_id attribute.

Views are also provided with a DataSet() instance which holds most necessary model information (the model name and possibly various record ids).

Views may also want to handle search queries by overriding do_search(), and updating their DataSet() as necessary.

A common need is the extension of the web form view to add new ways of displaying fields.

All built-in fields have a default display implementation, a new form widget may be necessary to correctly interact with a new field type (e.g. a GIS field) or to provide new representations and ways to interact with existing field types (e.g. validate Char fields which should contain email addresses and display them as email links).

To explicitly specify which form widget should be used to display a field, simply use the widget attribute in the view’s XML description:

the same widget is used in both “view” (read-only) and “edit” modes of a form view, it’s not possible to use a widget in one and an other widget in the other

and a given field (name) can not be used multiple times in the same form

a widget may ignore the current mode of the form view and remain the same in both view and edit modes

Fields are instantiated by the form view after it has read its XML description and constructed the corresponding HTML representing that description. After that, the form view will communicate with the field objects using some methods. These methods are defined by the FieldInterface interface. Almost all fields inherit the AbstractField abstract class. That class defines some default mechanisms that need to be implemented by most fields.

Here are some of the responsibilities of a field class:

The field class must display and allow the user to edit the value of the field.

It must correctly implement the 3 field attributes available in all fields of Odoo. The AbstractField class already implements an algorithm that dynamically calculates the value of these attributes (they can change at any moment because their value change according to the value of other fields). Their values are stored in Widget Properties (the widget properties were explained earlier in this guide). It is the responsibility of each field class to check these widget properties and dynamically adapt depending of their values. Here is a description of each of these attributes:

required: The field must have a value before saving. If required is true and the field doesn’t have a value, the method is_valid() of the field must return false.

invisible: When this is true, the field must be invisible. The AbstractField class already has a basic implementation of this behavior that fits most fields.

readonly: When true, the field must not be editable by the user. Most fields in Odoo have a completely different behavior depending on the value of readonly. As example, the FieldChar displays an HTML <input> when it is editable and simply displays the text when it is read-only. This also means it has much more code it would need to implement only one behavior, but this is necessary to ensure a good user experience.

Fields have two methods, set_value() and get_value(), which are called by the form view to give it the value to display and get back the new value entered by the user. These methods must be able to handle the value as given by the Odoo server when a read() is performed on a model and give back a valid value for a write(). Remember that the JavaScript/Python data types used to represent the values given by read() and given to write() is not necessarily the same in Odoo. As example, when you read a many2one, it is always a tuple whose first value is the id of the pointed record and the second one is the name get (ie: (15, "Agrolait")). But when you write a many2one it must be a single integer, not a tuple anymore. AbstractField has a default implementation of these methods that works well for simple data type and set a widget property named value.

Please note that, to better understand how to implement fields, you are strongly encouraged to look at the definition of the FieldInterface interface and the AbstractField class directly in the code of the Odoo web client.

In this part we will explain how to create a new type of field. The example here will be to re-implement the FieldChar class and progressively explain each part.

Here is a first implementation that will only display text. The user will not be able to modify the content of the field.

In this example, we declare a class named FieldChar2 inheriting from AbstractField. We also register this class in the registry instance.web.form.widgets under the key char2. That will allow us to use this new field in any form view by specifying widget="char2" in the <field/> tag in the XML declaration of the view.

In this example, we define a single method: render_value(). All it does is display the widget property value. Those are two tools defined by the AbstractField class. As explained before, the form view will call the method set_value() of the field to set the value to display. This method already has a default implementation in AbstractField which simply sets the widget property value. AbstractField also watch the change:value event on itself and calls the render_value() when it occurs. So, render_value() is a convenience method to implement in child classes to perform some operation each time the value of the field changes.

In the init() method, we also define the default value of the field if none is specified by the form view (here we assume the default value of a char field should be an empty string).

Read-only fields, which only display content and don’t allow the user to modify it can be useful, but most fields in Odoo also allow editing. This makes the field classes more complicated, mostly because fields are supposed to handle both editable and non-editable mode, those modes are often completely different (for design and usability purpose) and the fields must be able to switch between modes at any moment.

To know in which mode the current field should be, the AbstractField class sets a widget property named effective_readonly. The field should watch for changes in that widget property and display the correct mode accordingly. Example:

In the start() method (which is called immediately after a widget has been appended to the DOM), we bind on the event change:effective_readonly. That allows us to redisplay the field each time the widget property effective_readonly changes. This event handler will call display_field(), which is also called directly in start(). This display_field() was created specifically for this field, it’s not a method defined in AbstractField or any other class. We can use this method to display the content of the field depending on the current mode.

From now on the conception of this field is typical, except there is a lot of verifications to know the state of the effective_readonly property:

In the QWeb template used to display the content of the widget, it displays an <input type="text" /> if we are in read-write mode and nothing in particular in read-only mode.

In the display_field() method, we have to bind on the change event of the <input type="text" /> to know when the user has changed the value. When it happens, we call the internal_set_value() method with the new value of the field. This is a convenience method provided by the AbstractField class. That method will set a new value in the value property but will not trigger a call to render_value() (which is not necessary since the <input type="text" /> already contains the correct value).

In render_value(), we use a completely different code to display the value of the field depending if we are in read-only or in read-write mode.

Create a FieldColor class. The value of this field should be a string containing a color code like those used in CSS (example: #FF0000 for red). In read-only mode, this color field should display a little block whose color corresponds to the value of the field. In read-write mode, you should display an <input type="color" />. That type of <input /> is an HTML5 component that doesn’t work in all browsers but works well in Google Chrome. So it’s OK to use as an exercise.

You can use that widget in the form view of the message_of_the_day model for its field named color. As a bonus, you can change the MessageOfTheDay widget created in the previous part of this guide to display the message of the day with the background color indicated in the color field.

Form fields are used to edit a single field, and are intrinsically linked to a field. Because this may be limiting, it is also possible to create form widgets which are not so restricted and have less ties to a specific lifecycle.

Custom form widgets can be added to a form view through the widget tag:

This type of widget will simply be created by the form view during the creation of the HTML according to the XML definition. They have properties in common with the fields (like the effective_readonly property) but they are not assigned a precise field. And so they don’t have methods like get_value() and set_value(). They must inherit from the FormWidget abstract class.

Form widgets can interact with form fields by listening for their changes and fetching or altering their values. They can access form fields through their field_manager attribute:

FormWidget is generally the FormView() itself, but features used from it should be limited to those defined by FieldManagerMixin(), the most useful being:

get_field_value(field_name)() which returns the value of a field.

set_values(values)() sets multiple field values, takes a mapping of {field_name: value_to_set}

An event field_changed:field_name is triggered any time the value of the field called field_name is changed

Show Coordinates on Google Map

Add two fields to product.product storing a latitude and a longitude, then create a new form widget to display the latitude and longitude of a product’s origin on a map

To display the map, use Google Map’s embedding:

where XXX should be replaced by the latitude and YYY by the longitude.

Display the two position fields and a map widget using them in a new notebook page of the product’s form view.

Get the Current Coordinate

Add a button resetting the product’s coordinates to the location of the user, you can get these coordinates using the javascript geolocation API.

Now we would like to display an additional button to automatically set the coordinates to the location of the current user.

To get the coordinates of the user, an easy way is to use the geolocation JavaScript API. See the online documentation to know how to use it.

Please also note that the user should not be able to click on that button when the form view is in read-only mode. So, this custom widget should handle correctly the effective_readonly property just like any field. One way to do this would be to make the button disappear when effective_readonly is true.

as a separate concept from instances. In many languages classes are full-fledged objects and themselves instance (of metaclasses) but there remains two fairly separate hierarchies between classes and instances

as well as papering over cross-browser differences, although this has become less necessary over time

**Examples:**

Example 1 (unknown):
```unknown
$ git clone http://github.com/odoo/petstore
```

Example 2 (unknown):
```unknown
oepetstore
|-- images
|   |-- alligator.jpg
|   |-- ball.jpg
|   |-- crazy_circle.jpg
|   |-- fish.jpg
|   `-- mice.jpg
|-- __init__.py
|-- oepetstore.message_of_the_day.csv
|-- __manifest__.py
|-- petstore_data.xml
|-- petstore.py
|-- petstore.xml
`-- static
    `-- src
        |-- css
        |   `-- petstore.css
        |-- js
        |   `-- petstore.js
        `-- xml
            `-- petstore.xml
```

Example 3 (javascript):
```javascript
odoo.oepetstore = function(instance, local) {
    var _t = instance.web._t,
        _lt = instance.web._lt;
    var QWeb = instance.web.qweb;

    local.HomePage = instance.Widget.extend({
        start: function() {
            console.log("pet store home page loaded");
        },
    });

    instance.web.client_actions.add(
        'petstore.homepage', 'instance.oepetstore.HomePage');
}
```

Example 4 (r):
```r
odoo.oepetstore = function(instance, local) {
    local.xxx = ...;
}
```

---

## Define module data¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/define_module_data.html

**Contents:**
- Define module data¶
- Data Types¶
  - Master Data¶
  - Demo Data¶
- Data Declaration¶
  - Manifest¶
  - CSV¶
  - XML¶
    - Data Extension¶
    - ref¶

This tutorial is an extension of the Server framework 101 tutorial. Make sure you have completed it and use the estate module you have built as a base for the exercises in this tutorial.

Master data is usually part of the technical or business requirements for the module. In other words, such data is often necessary for the module to work properly. This data will always be installed when installing the module.

We already met technical data previously since we have defined views and actions. Those are one kind of master data.

On top of technical data, business data can be defined, e.g. countries, currencies, units of measure, as well as complete country localization (legal reports, tax definitions, chart of account), and much more…

In addition to master data, which is required for a module to work properly, we can also provide data for demonstration purposes:

Help the sales representatives make their demos quickly.

Have a set of working data for developers to test new features and see how these new features look with data they might not have added themselves.

Test that the data is loaded correctly, without raising an error.

Setup most of the features to be used quickly when creating a new database.

The database manager let you create databases with demo data. The same can be achieved via the command line with --with-demo.

Reference: the documentation related to this topic can be found in Module Manifests.

Data is declared either in CSV or in XML. Each file containing data must be added in the manifest for them to be loaded.

The keys to use in the manifest to add new data are data for the master data and demo for the demo data. Both values should be a list of strings representing the relative paths to the files declaring the data.

Usually, demo data is in a demo folder, views and actions are in a views folder, security related data is in a security folder, and other data is in a data folder.

If your work tree looks like this:

Your manifest should look like this:

Reference: the documentation related to this topic can be found in CSV data files.

The easiest way to declare simple data is by using the CSV format. This is however limited in terms of features: use it for long lists of simple models, but prefer XML otherwise.

Your IDE has probably an extension to have a syntax highlighting of the CSV files

Add some standard Real Estate Property Types for the estate module: Residential, Commercial, Industrial and Land. These should always be installed.

Reference: the documentation related to this topic can be found in Data Files.

When the data to create is more complex it can be useful, or even necessary, to do it in XML.

Create some demo data for the estate module.

Home in a trailer park

During the Core Training, we saw in the Chapter 12: Inheritance chapter we could inherit (extend) an existing view. This was a special case of data extension: any data can be extended in a module.

When you are adding new fields to an existing model in a new module, you might want to populate those fields on the records created in the modules you are depending on. This is done by giving the xml_id of the record you want to extend. It won’t replace it, in this case we will set the field_c to the given value for both records.

Related fields can be set using the ref key. The value of that key is the xml_id of the record you want to link. Remember the xml_id is composed of the name of the module where the data is first declared, followed by a dot, followed by the id of the record (just the id works too if you are in the module declaring it).

Create some demo data offers for the properties you created.

Create offers using the partners defined in base

Ensure both of your demo properties are created with their Property Type set to Residential.

The value to assign to a field is not always a simple string and you might need to compute it. It can also be used to optimize the insertion of related values, or because a constraint forces you to add the related values in batch. See :Add X2many fields.

The offers you added should always be in a date relative to the installation of the module.

Sometimes, you need to call the ORM to do a search. This is not feasible with the CSV format.

In this code snippet, it is needed because the master data depends on the localization installed.

You might also need to execute python code when loading data.

Validate one of the demo data offers by using the “Accept Offer” button. Refuse the others.

Reference: the documentation related to this topic can be found in Command.

If you need to add related data in a One2many or a Many2many field, you can do so by using the Command methods.

Create one new Property, but this time with some offers created directly inside the One2many field linked to the Offers.

You should never access demo data outside of the demo data declaration, not even in tests.

There are multiple ways to access the master/demo data.

In python code, you can use the env.ref(self, xml_id, raise_if_not_found=True) method. It returns the recordset linked to the xml_id you specify.

In XML, you can use the ref key like this

It will call the ref method, and store the id of the record returned on the field related_id of the record of type tutorial.example with id id1.

In CSV, the title of the column must be suffixed with :id or /id.

In SQL, it is more complicated, see the advanced section.

Data can always be deleted by the user. Always code defensively, taking this into account.

Because we don’t want a column xml_id in every single SQL table of the database, we need a mechanism to store it. This is done with the ir.model.data model.

It contains the name of the record (the xml_id) along with the module in which it is defined, the model defining it, and the id of it.

The records created with the noupdate flag won’t be updated when upgrading the module that created them, but it will be created if it didn’t exist yet.

odoo-bin -i module will bypass this setting and always load the data. But normally one shouldn’t do this on a production database.

In some cases, it makes sense to do the import directly in SQL. This is however discouraged as it bypasses all the features of the ORM, computed fields (including metadata) and python constraints.

Generally using raw SQL also bypasses ACLs and increases the risks of injections.

Reference: Security in Odoo

It can help to speed the import time by a lot with huge files.

For more complex imports like for the translations.

It can be necessary to initialize the database.

**Examples:**

Example 1 (json):
```json
$ ./odoo-bin -h
Usage: odoo-bin [options]

Options:
--version             show program's version number and exit
-h, --help            show this help message and exit

Common options:
  [...]
  --with-demo         install demo data in new databases
[...]

$ ./odoo-bin --addons-path=... -d new-db -i base --with-demo
```

Example 2 (unknown):
```unknown
estate
├── data
│   └── master_data.xml
├── demo
│   └── demo_data.xml
├── models
│   ├── *.py
│   └── __init__.py
├── security
│   └── ir.model.access.csv
├── views
│   └── estate_property_offer_views.xml
├── __init__.py
└── __manifest__.py
```

Example 3 (json):
```json
# -*- coding: utf-8 -*-

{
    "name": "Real Estate",
    "depends": [
        ...
    ],
    "data": [
        "security/ir.model.access.csv",  # CSV and XML files are loaded at the same place
        "views/estate_property_offer_views.xml",  # Views are data too
        "data/master_data.xml",  # Split the data in multiple files depending on the model
    ],
    "demo": [
        "demo/demo_data.xml",
    ]
    "application": True,
}
```

Example 4 (unknown):
```unknown
id,field_a,field_b,related_id:id
id1,valueA1,valueB1,module.relatedid
id2,valueA2,valueB2,module.relatedid
```

---

## Chapter 6 - Going live¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/website_theme/06_going_live.html

**Contents:**
- Chapter 6 - Going live¶
- Translations¶
- Module import¶
- Conclusion¶

Congratulations! Your client has a beautifully designed homepage and contact page, and the eCommerce is fully adapted to the Airproof design. Amazing!

Now, the client wants the website translated into French. To do so:

Add French to the website in the settings and enable the language switcher in the header via presets.

Then for the translation itself, you have two options. We shall therefore test both:

Translate the content of the homepage carousel through the backend.

But for the menu, make the translations through the frontend.

Export the French .po file for your Airproof module and place it in the /i18n translations folder.

If you would like, you can add more translations directly by editing the .po file. (Using Poedit software, your code editor, or another translation tool.)

See reference documentation on Backend and Frontend translations, and how to Export them.

Be careful when using Poedit, as it doesn’t handle tags with styles well and generates an .mo file.

To see the changes made directly via the .po file, you will need to manually import the file.

Take a look at what the file i18n/fr_BE.po of our Airproof example looks like.

Great job! The website is now completely finished and your module is ready for installation in the client’s SaaS database.

Just before that, test the import process on a new database.

See reference documentation on how to deploy a module on an Odoo SaaS database.

Ensure the base_import_module is installed on the database before the module installation.

Verify all required applications are installed.

Skip the theme installation steps and start from scratch.

Manually import translations after module installation, as they won’t apply automatically.

Congratulations on completing the Build a module for a website theme tutorial! You’ve successfully navigated through every stage, from setting up your development environment to launching a fully customized Odoo website theme.

Throughout this journey, you’ve mastered:

---

## Create a project¶

**URL:** https://www.odoo.com/documentation/19.0/administration/odoo_sh/getting_started/create.html

**Contents:**
- Create a project¶
- Deploy a platform¶
- Import a database¶
  - Push modules in production¶
  - Download a backup¶
  - Upload the backup¶
  - Check outgoing email servers¶
  - Check scheduled actions¶
  - Register the subscription¶

Visit Odoo.sh and click Deploy your platform.

Sign in with a GitHub account.

Authorize Odoo.sh by clicking Authorize odoo twice.

Odoo.sh requests GitHub to:

Access your GitHub login and email.

Create a new repository, in case you start from scratch.

Access existing repositories, including organization ones, in case you start from an existing repository.

Create a webhook to notify you each time changes are pushed.

Commit changes for easier deployment.

Fill in the Deploy your platform form and click Deploy:

Github repository: to create a new repository, choose New repository and enter a name; to use an existing one, choose Existing repository and select it.

Odoo Version: select the major version of Odoo you want to use.

Use the latest major version of Odoo version when creating a new repository. If you are planning to import an existing database or applications, it might be required that their versions match.

If you are starting on Odoo Online and plan to migrate to Odoo.sh later, create your database using odoo.com/start-lts to ensure compatibility, as minor versions are not supported by Odoo.sh.

Subscription Code: enter your Odoo Enterprise subscription code that includes Odoo.sh. It is also sometimes called the subscription referral, contract number, or activation code.

Partners can use their partnership codes to initiate a trial (trial builds are limited to 1 GB storage and two staging). If a client proceeds to start a project, they must subscribe to an Odoo Enterprise plan that includes Odoo.sh hosting and use their subscription code.

Hosting location: select the region where your platform will be hosted.

Once your platform is deployed, you can import a database into your Odoo.sh project, provided it uses a supported version of Odoo.

Due to Odoo’s backup policy, the import process requires four times the size of your database dump in available storage. For example, a 10 GB dump file will require at least 40 GB of available space. We recommend allocating more than four times× the dump size temporarily, then reducing storage after the import is complete.

If your project is a trial created with a partnership code, you can only import database dumps up to 1 GB in size.

If you are using community or custom modules, add them to a branch in your GitHub repository.

Databases hosted on Odoo Online do not support custom modules.

Odoo.sh automatically detects folders containing Odoo modules. You can organize them however you prefer. For example, you can place them directly in the root directory of your repository or group them by category (e.g., accounting, project, etc.).

For publicly available community modules, you may also consider using submodules.

Go to /web/database/manager on your on-premise Odoo instance and click Backup.

Select zip (includes filestore) as the Backup Format.

You will need the Master Password of your Odoo server. If you do not have it, contact your system administrator.

If you cannot access the database manager, it may have been disabled by your system administrator. Refer to the database manager security documentation.

Log in to your portal account and navigate to the My Databases page, and download a backup by clicking the (gear) icon, then Download.

Only major versions of Odoo are compatible with Odoo.sh.

In your Odoo.sh project, navigate to the Backups tab of your Production branch, and click Import Database to upload the backup you previously downloaded.

Once the import is complete, you can access the database using the Connect button in the branch’s History tab.

Importing a backup overwrites all data currently in the branch. Consider downloading a manual backup beforehand if you want to preserve the existing data.

Odoo.sh provides a default email server. To use it, ensure that no outgoing mail server is enabled in your database by enabling developer mode and navigating to Settings ‣ Technical ‣ Email: Outgoing Mail Servers

After importing your database, all configured outgoing mail servers are disabled, and the default Odoo.sh server is used.

Port 25 is and will remain closed. If connecting to an external SMTP server, use port 465 or 587.

Scheduled actions are disabled by default after importing your database. This prevents your newly imported database from performing potentially disruptive operations such as:

sending queued emails,

triggering mass mailings, or

syncing with third-party services (e.g., calendars, cloud storage).

If you intend to use this imported database in production, re-enable only the scheduled actions you need by enabling developer mode and going to Settings ‣ Technical ‣ Automation: Scheduled Actions.

After import, the database is considered a duplicate and will be unlinked from your enterprise subscription.

You are allowed only one active database per subscription.

If you intend to make the imported database your production environment:

Unlink your previous database from the subscription.

Register the new one.

Refer to the database registration documentation for step-by-step instructions.

---

## Build PDF Reports¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/pdf_reports.html

**Contents:**
- Build PDF Reports¶
- File Structure¶
- Basic Report¶
  - Report Data¶
  - Minimal Template¶
  - Report Action¶
  - Make a Report¶
- Sub-templates¶
- Report Inheritance¶
- Additional Features¶

This tutorial is an extension of the Server framework 101 tutorial. Make sure you have completed it and use the estate module you have built as a base for the exercises in this tutorial.

We were previously introduced to QWeb where it was used to build a kanban view. Now we will expand on one of QWeb’s other main uses: creating PDF reports. A common business requirement is the ability to create documents to send to customers and to use internally. These reports can be used to summarize and display information in an organized template to support the business in different ways. Odoo can additionally add our company’s header and footer to our reports with minimal extra effort.

The documentation related to this topic can be found in QWeb Templates, QWeb Reports, and the Report Actions (ir.actions.report) section of the Actions reference.

The bulk of a PDF report is its QWeb template. It also typically needs a corresponding ir.actions.report to include the report within a module’s business logic. There is no strict rule for the file names or where they are located, but these two parts are typically stored in 2 separate files within a report folder at the top level of your module’s directory. If a module has many or multiple long report templates, then they are often organized logically across different files named after the report(s) they contain. All actions for the reports are usually stored in the same file ending with _reports.xml, regardless of the number of reports it contains.

Therefore, it is expected that your work tree will look something like this:

Don’t forget to add whatever files your template and action view will be into to your __manifest__.py. In this case, you will want to add the files to the data list and remember that the files listed in a manifest are loaded sequentially!

Goal: at the end of this section, we will be able to print a report that displays all offers for a property.

In our real estate example there are many useful reports that we could create. One simple report we can create is one that displays all of a property’s offers.

Before we do anything we first need some data to populate our reports or else this tutorial won’t be very interesting. When creating reports, you will need some data to test your report code and check that the resulting look is as expected. It is a good idea to test with data that will cover most or all of your expected use cases. A good representation set for our simple report is:

At least 3 properties where 1 is “sold”, 1 is “offer received” and 1 is “new”.

At least 2-3 offers for our “sold” and “offer received” properties

If you don’t have a set of data like this already, you can either:

Complete the Define module data tutorial (if you haven’t done so already) and add the extra cases to your demo data (you may need to create a new database to load in the demo data).

Manually create the data in your database.

Copy this data file into a new directory (data) in your estate module and copy these lines into your __manifest__.py file (you may need to create a new database to load in the demo data).

Before continuing, click through your data in your database and make sure your data is as expected. Of course you can add the data after you write your report code, but then you will not be able to incrementally test portions of your code as you write it. This can make checking for mistakes and debugging your code more difficult in the long run for complicated reports.

A minimal viable template is viewable under the “Minimal viable template” section of the Report template documentation. We can modify this example to build our minimal property offers template file:

Most of the Odoo specific (i.e. non-HTML) items in our file are explained in the minimal viable template section. Some additional features in our template are:

The use of the class="table" attribute, so our table has some nice formatting. Twitter Bootstrap (we’re using its table class in this case), and Font Awesome (useful for adding icons) classes can be used in your report template.

The use of t-set, t-value, t-foreach, and t-as so that we can loop over all the offer_ids.

If you are already familiar with website templating engines, then the QWeb directives (i.e. the t- commands) probably don’t need much explanation and you can just look at its documentation and skip ahead to the next subsection.

Otherwise you are encouraged to read more about them ( Wikipedia has a good high level description), but the general idea is that QWeb provides the ability to dynamically generate web code based on Odoo data and simple commands. I.e. QWeb can access recordset data (and methods) and process simple programming operations such as setting and accessing temporary variables. For example, in the above example:

t-set creates a temporary variable called “offers” that has its value set by t-value to the current estate.property recordset’s offer_ids.

The t-foreach and t-as usage is the equivalent to the Python:

Now that we have a template, we need to make it accessible in our app via a ir.actions.report. A practical example of ir.actions.report is here corresponding to this template. Its contents are all explained in the documentation.

An ir.actions.report is primarily used via the Print menu of a model’s view. In the practical example, the binding_model_id specifies which model’s views the report should show, and Odoo will auto-magically add it for you. Another common use case of the report action is to link it to a button as we learned in Chapter 9: Ready For Some Action?. This is handy for reports that only make sense under specific conditions. For example, if we wanted to make a “Final Sale” report, then we can link it to a “Print Sale Info” button that appears in the form view only when the property is “Sold”.

You may have noticed or are wondered why our report template loops through a recordset. When our template is passed more than one record, it can produce one PDF report for all the records. Using the Print menu in the list view with multiple records selected will demonstrate this.

Finally, you now know where to create your files and how the content of the files should look. Happy report making!

Add the property offers report from the minimal template subsection to the Print menu of the Property views.

Improve the report by adding more data. Refer to the Goal of this section to see what additional data you can add and feel free to add even more.

Bonus: Make an extra flexible report by adding in some logic so that when there are no offers on a property then we don’t create a table and instead write something about how there are no offers yet. Hint: you will need to use t-if and t-else.

Remember to check that your PDF reports match your data as expected.

Goal: at the end of this section, we will have a sub-template that we use in 2 reports.

There are two main reasons for using sub-templates. One is to make the code easier to read when working with extra-long or complicated templates. The other is to reuse code where possible. Our simple property offers report is useful, but listing property offers information can be useful for more than just one report template. One example is a report that lists all of a salesman’s properties’ offers.

See if you can understand how to call a sub-template by reading the documentation on it and/or by looking at an example (remember QWeb uses the same control flows regardless if it is for a report or a view in Odoo.)

Create and use a sub-template.

Split the table portion of the offers into its own template. Remember to check that your original report still prints correctly afterwards.

Add a new report for res.users that allows you to print all of the Real Estate Properties that are visible in their form view (i.e. in the “Settings” app). Include the offers for each of those saleman’s properties in the same report. Hint: since the binding_model_id in this case will not be within the estate module, you will need to use ref="base.model_res_users".

Your end result should look similar to the image in the Goal of this section.

Remember to check that your reports match your data as expected!

Goal: at the end of this section, we will inherit the property report in the estate_account module.

Inheritance in QWeb uses the same xpath elements as views inheritance. A QWeb template refers to its parent template in a different way though. It is even easier to do by just adding the inherit_id attribute to the template element and setting it equal to the module.parent_template_id.

We didn’t add any new fields to any of the estate models in estate_account, but we can still add information to our existing property report. For example, we know that any “Sold” properties will already have an invoice created for them, so we can add this information to our report.

Extend the property report to include some information about the invoice. You can look at the Goal of this section for inspiration (i.e. print a line when the property is Done, otherwise print nothing).

Again, remember to check that your reports match your data as expected!

All the following extra features are described further in the QWeb Reports documentation, including how to implement each of them.

We all know Odoo is used in multiple languages thanks to automated and manual translating. QWeb reports are no exception! Note that sometimes the translations do not work properly if there are unnecessary spaces in your template’s text content, so try to avoid them when possible (especially leading spaces).

You probably are tired of hearing that QWeb creates HTML, but we’re saying it again! One of the neat features of reports being written in QWeb is they can be viewed within the web browser. This can be useful if you want to embed a hyperlink that leads to a specific report. Note that the usual security checks will still apply to prevent unauthorized users from accessing the reports.

Odoo has a built-in barcode image creator that allows for barcodes to be embedded in your reports. Check out the corresponding code to see all the supported barcode types.

**Examples:**

Example 1 (unknown):
```unknown
estate
├── models
│   ├── *.py
│   └── __init__.py
├── report
│   ├── estate_property_templates.xml
│   └── estate_property_reports.xml
├── security
│   └── ir.model.access.csv
├── views
│   └── *.xml
├── __init__.py
└── __manifest__.py
```

Example 2 (xml):
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<odoo>
    <template id="report_property_offers">
        <t t-foreach="docs" t-as="property">
            <t t-call="web.html_container">
                <t t-call="web.external_layout">
                    <div class="page">
                        <h2>
                            <span t-field="property.name"/>
                        </h2>
                        <div>
                            <strong>Expected Price: </strong>
                            <span t-field="property.expected_price"/>
                        </div>
                        <table class="table">
                            <thead>
                                <tr>
                                    <th>Price</th>
                                </tr>
                            </thead>
                            <tbody>
                                <t t-set="offers" t-value="property.mapped('offer_ids')"/>
                                <tr t-foreach="offers" t-as="offer">
                                    <td>
                                        <span t-field="offer.price"/>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </t>
            </t>
        </t>
    </template>
</odoo>
```

Example 3 (bash):
```bash
for offer in offers:
```

---

## Online editor¶

**URL:** https://www.odoo.com/documentation/19.0/administration/odoo_sh/getting_started/online_editor.html

**Contents:**
- Online editor¶
- Editing the source code¶
- Committing and pushing changes¶
- Consoles¶

The Online Editor view allows editing the source code of your builds from a web browser. It also gives you the possibility to open terminals, Python consoles, Odoo shell consoles, and Jupyter Notebooks.

You can access the editor of a build through the branches tab, the builds dropdown menu, or by adding /odoo-sh/editor to the build’s URL (e.g., https://odoo-addons-master-1.dev.odoo.com/odoo-sh/editor).

The working directory is composed of the following:

You can edit the source code (files under /src) of development and staging builds. For production builds, the source code is read-only, because applying local changes on a production server is not a good practice.

Your changes won’t be propagated to new builds. It is necessary to commit them to the source code if you want them to persist.

The source code of your GitHub repository is located under /src/user.

The source code of Odoo is located under:

/src/odoo (https://github.com/odoo/odoo)

/src/enterprise (https://github.com/odoo/enterprise)

/src/themes (https://github.com/odoo/design-themes)

To open a file in the editor, double-click it in the file browser panel. You can then edit the file. To save your changes, go to File ‣ Save or use the Ctrl+S keyboard shortcut.

If you save a Python file in your Odoo server’s addons path, Odoo will detect it and reload automatically, meaning your changes are immediately visible.

However, if your changes are stored in the database, such as a field’s label or a view, it is necessary to update the related module to apply the changes. To update the module of the currently1 open file, go to Odoo ‣ Update current module.

You can also execute the following command in a terminal to update a module:

To commit and push changes to your GitHub repository:

Open a terminal by going to File ‣ New ‣ Terminal.

Change the directory to ~/src/user.

https is the name of your HTTPS GitHub remote repository (e.g., https://github.com/username/repository.git).

HEAD is the reference to the latest revision you committed.

<branch> must be replaced by the name of the branch to which you want to push the changes, most likely the current branch if you work on a development build.

You will be prompted to input your GitHub username and password. After inputting your credentials, press enter.

If you activate two-factor authentication for your GitHub account, you can create a personal access token and use it as a password. Granting the repo permission suffices.

It is not possible to authenticate yourself using SSH, as your private SSH key is not hosted in your build containers for security reasons, nor forwarded through an SSH agent, as you access the editor through a web browser.

The source folder ~/src/user is not checked out on a branch but rather on a detached revision. This is because builds work on specific revisions rather than branches, meaning you can have multiple builds on the same branch, but on different revisions.

Once your changes are pushed, according to your branch push behavior, a new build may be created. You can continue to work in the editor you pushed from, as it will have the same revision as the new build that was created. However, always make sure to be in the editor of a build using the latest revision of your branch.

You can open Python consoles, which are IPython interactive shells. Using these Python consoles (rather than IPython shells within a terminal) allows you to utilize their rich display capabilities to display objects in HTML.

The Pretty class displays lists in a legible way.

Using pandas you can display:

You can open Odoo shell consoles to experiment with the Odoo registry and model methods of your database. You can also read or write directly on your records.

In an Odoo shell console, transactions are automatically committed. This means that changes made to records are applied to the database. For example, if you change a user’s name, it will be updated in your database as well. Therefore, use Odoo shell consoles carefully on production databases.

You can use env to invoke models of your database registry, e.g., env['res.users'].

**Examples:**

Example 1 (typescript):
```typescript
.
├── home
│    └── odoo
│         ├── src
│         │    ├── odoo                Odoo Community source code
│         │    │    └── odoo-bin       Odoo server executable
│         │    ├── enterprise          Odoo Enterprise source code
│         │    ├── themes              Odoo Themes source code
│         │    └── user                Your repository branch source code
│         ├── data
│         │    ├── filestore           Database attachments, as well as the files of binary fields
│         │    └── sessions            Visitors and users sessions
│         └── logs
│              ├── install.log         Database installation logs
│              ├── odoo.log            Running server logs
│              ├── update.log          Database updates logs
│              └── pip.log             Python packages installation logs
```

Example 2 (julia):
```julia
odoo-bin -u <comma-separated module names> --stop-after-init
```

Example 3 (unknown):
```unknown
cd ~/src/user
```

Example 4 (python):
```python
git config --global user.email "you@example.com" && git config --global user.name "Your Name"
```

---

## Chapter 13: Interact With Other Modules¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/server_framework_101/13_other_module.html

**Contents:**
- Chapter 13: Interact With Other Modules¶
- Concrete Example: Account Move¶
  - Link Module¶
  - Invoice Creation¶

In the previous chapter, we used inheritance to modify the behavior of a module. In our real estate scenario, we would like to go a step further and be able to generate invoices for our customers. Odoo provides an Invoicing module, so it would be neat to create an invoice directly from our real estate module, i.e. once a property is set to ‘Sold’, an invoice is created in the Invoicing application.

Goal: at the end of this section:

A new module estate_account should be created

When a property is sold, an invoice should be issued for the buyer

Any time we interact with another module, we need to keep in mind the modularity. If we intend to sell our application to real estate agencies, some may want the invoicing feature but others may not want it.

The common approach for such use cases is to create a ‘link’ module. In our case, the module would depend on estate and account and would include the invoice creation logic of the estate property. This way the real estate and the accounting modules can be installed independently. When both are installed, the link module provides the new feature.

Create a link module.

Create the estate_account module, which depends on the estate and account modules. For now, it will be an empty shell.

Tip: you already did this at the beginning of the tutorial. The process is very similar.

When the estate_account module appears in the list, go ahead and install it! You’ll notice that the Invoicing application is installed as well. This is expected since your module depends on it. If you uninstall the Invoicing application, your module will be uninstalled as well.

It’s now time to generate the invoice. We want to add functionality to the estate.property model, i.e. we want to add some extra logic for when a property is sold. Does that sound familiar? If not, it’s a good idea to go back to the previous chapter since you might have missed something ;-)

As a first step, we need to extend the action called when pressing the ‘Sold’ button on a property. To do so, we need to create a model inheritance in the estate_account module for the estate.property model. For now, the overridden action will simply return the super call. Maybe an example will make things clearer:

A practical example can be found here.

Add the first step of invoice creation.

Create a estate_property.py file in the correct folder of the estate_account module.

_inherit the estate.property model.

Override the action_sold method (you might have named it differently) to return the super call.

Tip: to make sure it works, add a print or a debugger breakpoint in the overridden method.

Is it working? If not, maybe check that all Python files are correctly imported.

If the override is working, we can move forward and create the invoice. Unfortunately, there is no easy way to know how to create any given object in Odoo. Most of the time, it is necessary to have a look at its model to find the required fields and provide appropriate values.

A good way to learn is to look at how other modules already do what you want to do. For example, one of the basic flows of Sales is the creation of an invoice from a sales order. This looks like a good starting point since it does exactly what we want to do. Take some time to read and understand the _create_invoices method. When you are done crying because this simple task looks awfully complex, we can move forward in the tutorial.

To create an invoice, we need the following information:

a partner_id: the customer

a move_type: it has several possible values

a journal_id: the accounting journal

This is enough to create an empty invoice.

Add the second step of invoice creation.

Create an empty account.move in the override of the action_sold method:

the partner_id is taken from the current estate.property

the move_type should correspond to a ‘Customer Invoice’

to create an object, use self.env[model_name].create(values), where values is a dict.

the create method doesn’t accept recordsets as field values.

When a property is set to ‘Sold’, you should now have a new customer invoice created in Invoicing / Customers / Invoices.

Obviously we don’t have any invoice lines so far. To create an invoice line, we need the following information:

name: a description of the line

Moreover, an invoice line needs to be linked to an invoice. The easiest and most efficient way to link a line to an invoice is to include all lines at invoice creation. To do this, the invoice_line_ids field is included in the account.move creation, which is a One2many. One2many and Many2many use special ‘commands’ which have been made human readable with the Command namespace. This namespace represents a triplet command to execute on a set of records. The triplet was originally the only option to do these commands, but it is now standard to use the namespace instead. The format is to place them in a list which is executed sequentially. Here is a simple example to include a One2many field line_ids at creation of a test_model:

Add the third step of invoice creation.

Add two invoice lines during the creation of the account.move. Each property sold will be invoiced following these conditions:

6% of the selling price

an additional 100.00 from administrative fees

Tip: Add the invoice_line_ids at creation following the example above. For each line, we need a name, quantity and price_unit.

This chapter might be one of the most difficult that has been covered so far, but it is the closest to what Odoo development will be in practice. In the next chapter, we will introduce the templating mechanism used in Odoo.

**Examples:**

Example 1 (python):
```python
from odoo import models

class InheritedModel(models.Model):
    _inherit = ["inherited.model"]

    def inherited_action(self):
        return super().inherited_action()
```

Example 2 (python):
```python
from odoo import Command

def inherited_action(self):
    self.env["test_model"].create(
        {
            "name": "Test",
            "line_ids": [
                Command.create({
                    "field_1": "value_1",
                    "field_2": "value_2",
                })
            ],
        }
    )
    return super().inherited_action()
```

---

## Chapter 1: Build a Clicker game¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/master_odoo_web_framework/01_build_clicker_game.html

**Contents:**
- Chapter 1: Build a Clicker game¶
- 1. Create a systray item¶
- 2. Count external clicks¶
- 3. Create a client action¶
- 4. Move the state to a service¶
- 5. Use a custom hook¶
- 6. Humanize the displayed value¶
- 7. Add a tooltip in ClickValue component¶
- 8. Buy ClickBots¶
- 9. Refactor to a class model¶

For this project, we will build together a clicker game, completely integrated with Odoo. In this game, the goal is to accumulate a large number of clicks, and to automate the system. The interesting part is that we will use the Odoo user interface as our playground. For example, we will hide bonuses in some random parts of the web client.

To get started, you need a running Odoo server and a development environment. Before getting into the exercises, make sure you have followed all the steps described in this tutorial introduction.

The solutions for each exercise of the chapter are hosted on the official Odoo tutorials repository.

To get started, we want to display a counter in the systray.

Create a clicker_systray_item.js (and xml) file with a hello world Owl component.

Register it to the systray registry, and make sure it is visible.

Update the content of the item so that it displays the following string: Clicks: 0, and add a button on the right to increment the value.

And voila, we have a completely working clicker game!

Documentation on the systray registry

Example: adding a systray item to the registry

Well, to be honest, it is not much fun yet. So let us add a new feature: we want all clicks in the user interface to count, so the user is incentivized to use Odoo as much as possible! But obviously, the intentional clicks on the main counter should still count more.

Use useExternalListener to listen on all clicks on document.body.

Each of these clicks should increase the counter value by 1.

Modify the code so that each click on the counter increased the value by 10

Make sure that a click on the counter does not increase the value by 11!

Also additional challenge: make sure the external listener capture the events, so we don’t miss any clicks.

Owl documentation on useExternalListener

MDN page on event capture

Currently, the current user interface is quite small: it is just a systray item. We certainly need more room to display more of our game. To do that, let us create a client action. A client action is a main action, managed by the web client, that displays a component.

Create a client_action.js (and xml) file, with a hello world component.

Register that client action in the action registry under the name awesome_clicker.client_action

Add a button on the systray item with the text Open. Clicking on it should open the client action awesome_clicker.client_action (use the action service to do that).

To avoid disrupting employees’ workflow, we prefer the client action to open within a popover rather than in fullscreen mode. Modify the doAction call to open it in a popover.

You can use target: "new" in the doAction to open the action in a popover:

How to create a client action

For now, our client action is just a hello world component. We want it to display our game state, but that state is currently only available in the systray item. So it means that we need to change the location of our state to make it available for all our components. This is a perfect use case for services.

Create a clicker_service.js file with the corresponding service.

This service should export a reactive value (the number of clicks) and a few functions to update it:

Access the state in both the systray item and the client action (don’t forget to useState it). Modify the systray item to remove its own local state and use it. Also, you can remove the +10 clicks button.

Display the state in the client action, and add a +10 clicks button in it.

Short explanation on services

Right now, every part of the code that will need to use our clicker service will have to import useService and useState. Since it is quite common, let us use a custom hook. It is also useful to put more emphasis on the clicker part, and less emphasis on the service part.

Export a useClicker hook.

Update all current uses of the clicker service to the new hook:

Documentation on hooks:

We will in the future display large numbers, so let us get ready for that. There is a humanNumber function that format numbers in a easier to comprehend way: for example, 1234 could be formatted as 1.2k

Use it to display our counters (both in the systray item and the client action).

Create a ClickValue component that display the value.

Owl allows component that contains just text nodes!

definition of humanNumber function

With the humanNumber function, we actually lost some precision on our interface. Let us display the real number as a tooltip.

Tooltip needs an html element. Change the ClickValue to wrap the value in a <span/> element

Add a dynamic data-tooltip attribute to display the exact value.

Documentation in the tooltip service

Let us make our game even more interesting: once a player get to 1000 clicks for the first time, the game should unlock a new feature: the player can buy robots for 1000 clicks. These robots will generate 10 clicks every 10 seconds.

Add a level number to our state. This is a number that will be incremented at some milestones, and open new features

Add a clickBots number to our state. It represents the number of robots that have been purchased.

Modify the client action to display the number of click bots (only if level >= 1), with a Buy button that is enabled if clicks >= 1000. The Buy button should increment the number of clickbots by 1.

Set a 10s interval in the service that will increment the number of clicks by 10*clickBots.

Make sure the Buy button is disabled if the player does not have enough clicks.

The current code is written in a somewhat functional style. But to do so, we have to export the state and all its update functions in our clicker object. As this project grows, this may become more and more complex. To make it simpler, let us split our business logic out of our service and into a class.

Create a clicker_model file that exports a reactive class. Move all the state and update functions from the service into the model.

You can extends the ClickerModel with the Reactive class from @web/core/utils/reactive. The Reactive class wrap the model into a reactive proxy.

Rewrite the clicker service to instantiate and export the clicker model class.

Example of subclassing Reactive

There is not much feedback that something changed when we reached 1k clicks. Let us use the effect service to communicate that information clearly. The problem is that our click model does not have access to services. Also, we want to keep as much as possible the UI concern out of the model. So, we can explore a new strategy for communication: event buses.

Update the clicker model to instantiate a bus, and to trigger a MILESTONE_1k event when we reach 1000 clicks for the first time.

Change the clicker service to listen to the same event on the model bus.

When that happens, use the effect service to display a rainbow man.

Add some text to explain that the user can now buy clickbots.

Owl documentation on event bus

Documentation on effect service

Clearly, we need a way to provide the player with more choices. Let us add a new type of clickbot: BigBots, which are just more powerful: they provide with 100 clicks each 10s, but they cost 5000 clicks

increment level when it gets to 5k (so it should be 2)

Update the state to keep track of bigbots

bigbots should be available at level >=2

Display the corresponding information in the client action

If you need to use < or > in a template as a javascript expression, be careful since it might class with the xml parser. To solve that, you can use one of the special aliases: gt, gte, lt or lte. See the Owl documentation page on template expressions.

Now, to add another scaling point, let us add a new type of resource: a power multiplier. This is a number that can be increased at level >= 3, and multiplies the action of the bots (so, instead of providing one click, clickbots now provide us with multiplier clicks).

increment level when it gets to 100k (so it should be 3).

update the state to keep track of the power (initial value is 1).

change bots to use that number as a multiplier.

Update the user interface to display and let the user purchase a new power level (costs: 50k).

We want the user to obtain sometimes bonuses, to reward using Odoo.

Define a list of rewards in click_rewards.js. A reward is an object with: - a description string. - a apply function that take the game state in argument and can modify it. - a minLevel number (optional) that describes at which unlock level the bonus is available. - a maxLevel number (optional) that describes at which unlock level a bonus is no longer available.

You can add whatever you want to that list!

Define a function getReward that will select a random reward from the list of rewards that matches the current unlock level.

Extract the code that choose randomly in an array in a function choose that you can move to another utils.js file.

Patch the form controller. Each time a form controller is created, it should randomly decides (1% chance) if a reward should be given.

If the answer is yes, call a method getReward on the model.

That method should choose a reward, send a sticky notification, with a button Collect that will then apply the reward, and finally, it should open the clicker client action.

Documentation on patching a class

Definition of patch function

Example of patching a class

Add a command Open Clicker Game to the command palette.

Add another command: Buy 1 click bot.

Example of use of command provider registry

It is now time to introduce a completely new type of resources. Here is one that should not be too controversial: trees. We will now allow the user to plant (collect?) fruit trees. A tree costs 1 million clicks, but it will provide us with fruits (either pears or cherries).

Update the state to keep track of various types of trees: pear/cherries, and their fruits.

Add a function that computes the total number of trees and fruits.

Define a new unlock level at clicks >= 1 000 000.

Update the client user interface to display the number of trees and fruits, and also, to buy trees.

Increment the fruit number by 1 for each tree every 30s.

Our game starts to become interesting. But for now, the systray only displays the total number of clicks. We want to see more information: the total number of trees and fruits. Also, it would be useful to have a quick access to some commands and some more information. Let us use a dropdown menu!

Replace the systray item by a dropdown menu.

It should display the numbers of clicks, trees, and fruits, each with a nice icon.

Clicking on it should open a dropdown menu that displays more detailed information: each types of trees and fruits.

Also, a few dropdown items with some commands: open the clicker game, buy a clickbot, …

We now keep track of a lot more information. Let us improve our client interface by organizing the information and features in various tabs, with the Notebook component:

Use the Notebook component.

All click content should be displayed in one tab.

All tree/fruits content should be displayed in another tab.

Odoo: Documentation on Notebook component

Owl: Documentation on slots

Tests of Notebook component

You certainly noticed a big flaw in our game: it is transient. The game state is lost each time the user closes the browser tab. Let us fix that. We will use the local storage to persist the state.

Import browser from @web/core/browser/browser to access the localstorage.

Serialize the state every 10s (in the same interval code) and store it on the local storage.

When the clicker service is started, it should load the state from the local storage (if any), or initialize itself otherwise.

Once you persist state somewhere, a new problem arises: what happens when you update your code, so the shape of the state changes, and the user opens its browser with a state that was created with an old version? Welcome to the world of migration issues!

It is probably wise to tackle the problem early. What we will do here is add a version number to the state, and introduce a system to automatically update the states if it is not up to date.

Add a version number to the state.

Define an (empty) list of migrations. A migration is an object with a fromVersion number, a toVersion number, and a apply function.

Whenever the code loads the state from the local storage, it should check the version number. If the state is not uptodate, it should apply all necessary migrations.

To test our migration system, let us add a new type of trees: peaches.

Increment the state version number.

**Examples:**

Example 1 (json):
```json
{
   type: "ir.actions.client",
   tag: "awesome_clicker.client_action",
   target: "new",
   name: "Clicker"
}
```

Example 2 (vue):
```vue
const state = reactive({ clicks: 0 });
...
return {
   state,
   increment(inc) {
      state.clicks += inc
   }
};
```

Example 3 (unknown):
```unknown
this.clicker = useClicker();
```

Example 4 (json):
```json
export const rewards = [
   {
      description: "Get 1 click bot",
      apply(clicker) {
            clicker.increment(1);
      },
      maxLevel: 3,
   },
   {
      description: "Get 10 click bot",
      apply(clicker) {
            clicker.increment(10);
      },
      minLevel: 3,
      maxLevel: 4,
   },
   {
      description: "Increase bot power!",
      apply(clicker) {
            clicker.multipler += 1;
      },
      minLevel: 3,
   },
];
```

---

## Building a Module¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/backend.html

**Contents:**
- Building a Module¶
- Start/Stop the Odoo server¶
- Build an Odoo module¶
  - Composition of a module¶
  - Module structure¶
  - Object-Relational Mapping¶
  - Model fields¶
    - Common Attributes¶
    - Simple fields¶
    - Reserved fields¶

This tutorial is outdated. We recommend reading Server framework 101 instead.

This tutorial requires having installed Odoo

Odoo uses a client/server architecture in which clients are web browsers accessing the Odoo server via RPC.

Business logic and extension is generally performed on the server side, although supporting client features (e.g. new data representation such as interactive maps) can be added to the client.

In order to start the server, simply invoke the command odoo-bin in the shell, adding the full path to the file if necessary:

The server is stopped by hitting Ctrl-C twice from the terminal, or by killing the corresponding OS process.

Both server and client extensions are packaged as modules which are optionally loaded in a database.

Odoo modules can either add brand new business logic to an Odoo system, or alter and extend existing business logic: a module can be created to add your country’s accounting rules to Odoo’s generic accounting support, while the next module adds support for real-time visualisation of a bus fleet.

Everything in Odoo thus starts and ends with modules.

An Odoo module can contain a number of elements:

Declared as Python classes, these resources are automatically persisted by Odoo based on their configuration

Definition of business objects UI display

XML or CSV files declaring the model metadata :

configuration data (modules parametrization, security rules),

Handle requests from web browsers

Images, CSS or javascript files used by the web interface or website

Each module is a directory within a module directory. Module directories are specified by using the --addons-path option.

most command-line options can also be set using a configuration file

An Odoo module is declared by its manifest.

A module is also a Python package with a __init__.py file, containing import instructions for various Python files in the module.

For instance, if the module has a single mymodule.py file __init__.py might contain:

Odoo provides a mechanism to help set up a new module, odoo-bin has a subcommand scaffold to create an empty module:

The command creates a subdirectory for your module, and automatically creates a bunch of standard files for a module. Most of them simply contain commented code or XML. The usage of most of those files will be explained along this tutorial.

Use the command line above to create an empty module Open Academy, and install it in Odoo.

A key component of Odoo is the ORM layer. This layer avoids having to write most SQL by hand and provides extensibility and security services2.

Business objects are declared as Python classes extending Model which integrates them into the automated persistence system.

Models can be configured by setting a number of attributes at their definition. The most important attribute is _name which is required and defines the name for the model in the Odoo system. Here is a minimally complete definition of a model:

Fields are used to define what the model can store and where. Fields are defined as attributes on the model class:

Much like the model itself, its fields can be configured, by passing configuration attributes as parameters:

Some attributes are available on all fields, here are the most common ones:

The label of the field in UI (visible by users).

If True, the field can not be empty, it must either have a default value or always be given a value when creating a record.

Long-form, provides a help tooltip to users in the UI.

Requests that Odoo create a database index on the column.

There are two broad categories of fields: “simple” fields which are atomic values stored directly in the model’s table and “relational” fields linking records (of the same model or of different models).

Example of simple fields are Boolean, Date, Char.

Odoo creates a few fields in all models1. These fields are managed by the system and shouldn’t be written to. They can be read if useful or necessary:

The unique identifier for a record in its model.

Creation date of the record.

User who created the record.

Last modification date of the record.

user who last modified the record.

By default, Odoo also requires a name field on all models for various display and search behaviors. The field used for these purposes can be overridden by setting _rec_name.

Define a new data model Course in the openacademy module. A course has a title and a description. Courses must have a title.

Odoo is a highly data driven system. Although behavior is customized using Python code part of a module’s value is in the data it sets up when loaded.

some modules exist solely to add data into Odoo

Module data is declared via data files, XML files with <record> elements. Each <record> element creates or updates a database record.

model is the name of the Odoo model for the record.

id is an external identifier, it allows referring to the record (without having to know its in-database identifier).

<field> elements have a name which is the name of the field in the model (e.g. description). Their body is the field’s value.

Data files have to be declared in the manifest file to be loaded, they can be declared in the 'data' list (always loaded) or in the 'demo' list (only loaded in demonstration mode).

Define demonstration data

Create demonstration data filling the Courses model with a few demonstration courses.

The content of the data files is only loaded when a module is installed or updated.

After making some changes, do not forget to use odoo-bin -u openacademy to save the changes to your database.

Actions and menus are regular records in database, usually declared through data files. Actions can be triggered in three ways:

by clicking on menu items (linked to specific actions)

by clicking on buttons in views (if these are connected to actions)

as contextual actions on object

Because menus are somewhat complex to declare there is a <menuitem> shortcut to declare an ir.ui.menu and connect it to the corresponding action more easily.

The action must be declared before its corresponding menu in the XML file.

Data files are executed sequentially, the action’s id must be present in the database before the menu can be created.

Define new menu entries

Define new menu entries to access courses under the OpenAcademy menu entry. A user should be able to:

display a list of all the courses

create/modify courses

Views define the way the records of a model are displayed. Each type of view represents a mode of visualization (a list of records, a graph of their aggregation, …). Views can either be requested generically via their type (e.g. a list of partners) or specifically via their id. For generic requests, the view with the correct type and the lowest priority will be used (so the lowest-priority view of each type is the default view for that type).

View inheritance allows altering views declared elsewhere (adding or removing content).

A view is declared as a record of the model ir.ui.view. The view type is implied by the root element of the arch field:

The view’s content is XML.

The arch field must thus be declared as type="xml" to be parsed correctly.

list views, also called list views, display records in a tabular form.

Their root element is <list>. The simplest form of the list view simply lists all the fields to display in the table (each field as a column):

Forms are used to create and edit single records.

Their root element is <form>. They are composed of high-level structure elements (groups, notebooks) and interactive elements (buttons and fields):

Customise form view using XML

Create your own form view for the Course object. Data displayed should be: the name and the description of the course.

In the Course form view, put the description field under a tab, such that it will be easier to add other tabs later, containing additional information.

Form views can also use plain HTML for more flexible layouts:

Search views customize the search field associated with the list view (and other aggregated views). Their root element is <search> and they’re composed of fields defining which fields can be searched on:

If no search view exists for the model, Odoo generates one which only allows searching on the name field.

Allow searching for courses based on their title or their description.

A record from a model may be related to a record from another model. For instance, a sale order record is related to a client record that contains the client data; it is also related to its sale order line records.

Create a session model

For the module Open Academy, we consider a model for sessions: a session is an occurrence of a course taught at a given time for a given audience.

Create a model for sessions. A session has a name, a start date, a duration and a number of seats. Add an action and a menu item to display them. Make the new model visible via a menu item.

Relational fields link records, either of the same model (hierarchies) or between different models.

Relational field types are:

A simple link to an other object:

A virtual relationship, inverse of a Many2one. A One2many behaves as a container of records, accessing it results in a (possibly empty) set of records:

Because a One2many is a virtual relationship, there must be a Many2one field in the {other_model}, and its name must be {related_field}

Bidirectional multiple relationship, any record on one side can be related to any number of records on the other side. Behaves as a container of records, accessing it also results in a possibly empty set of records:

Using a many2one, modify the Course and Session models to reflect their relation with other models:

A course has a responsible user; the value of that field is a record of the built-in model res.users.

A session has an instructor; the value of that field is a record of the built-in model res.partner.

A session is related to a course; the value of that field is a record of the model openacademy.course and is required.

Inverse one2many relations

Using the inverse relational field one2many, modify the models to reflect the relation between courses and sessions.

Multiple many2many relations

Using the relational field many2many, modify the Session model to relate every session to a set of attendees. Attendees will be represented by partner records, so we will relate to the built-in model res.partner. Adapt the views accordingly.

Odoo provides two inheritance mechanisms to extend an existing model in a modular way.

The first inheritance mechanism allows a module to modify the behavior of a model defined in another module:

add fields to a model,

override the definition of fields on a model,

add constraints to a model,

add methods to a model,

override existing methods on a model.

The second inheritance mechanism (delegation) allows to link every record of a model to a record in a parent model, and provides transparent access to the fields of the parent record.

Instead of modifying existing views in place (by overwriting them), Odoo provides view inheritance where children “extension” views are applied on top of root views, and can add or remove content from their parent.

An extension view references its parent using the inherit_id field, and instead of a single view its arch field is composed of any number of xpath elements selecting and altering the content of their parent view:

An XPath expression selecting a single element in the parent view. Raises an error if it matches no element or more than one

Operation to apply to the matched element:

appends xpath’s body at the end of the matched element

replaces the matched element with the xpath’s body, replacing any $0 node occurrence in the new body with the original element

inserts the xpath’s body as a sibling before the matched element

inserts the xpaths’s body as a sibling after the matched element

alters the attributes of the matched element using special attribute elements in the xpath’s body

When matching a single element, the position attribute can be set directly on the element to be found. Both inheritances below will give the same result.

Alter existing content

Using model inheritance, modify the existing Partner model to add an instructor boolean field, and a many2many field that corresponds to the session-partner relation

Using view inheritance, display this fields in the partner form view

In Odoo, Search domains are values that encode conditions on records. A domain is a list of criteria used to select a subset of a model’s records. Each criteria is a triple with a field name, an operator and a value.

For instance, when used on the Product model the following domain selects all services with a unit price over 1000:

By default criteria are combined with an implicit AND. The logical operators & (AND), | (OR) and ! (NOT) can be used to explicitly combine criteria. They are used in prefix position (the operator is inserted before its arguments rather than between). For instance to select products “which are services OR have a unit price which is NOT between 1000 and 2000”:

A domain parameter can be added to relational fields to limit valid records for the relation when trying to select records in the client interface.

Domains on relational fields

When selecting the instructor for a Session, only instructors (partners with instructor set to True) should be visible.

Create new partner categories Teacher / Level 1 and Teacher / Level 2. The instructor for a session can be either an instructor or a teacher (of any level).

So far fields have been stored directly in and retrieved directly from the database. Fields can also be computed. In that case, the field’s value is not retrieved from the database but computed on-the-fly by calling a method of the model.

To create a computed field, create a field and set its attribute compute to the name of a method. The computation method should simply set the value of the field to compute on every record in self.

The object self is a recordset, i.e., an ordered collection of records. It supports the standard Python operations on collections, like len(self) and iter(self), plus extra set operations like recs1 + recs2.

Iterating over self gives the records one by one, where each record is itself a collection of size 1. You can access/assign fields on single records by using the dot notation, like record.name.

The value of a computed field usually depends on the values of other fields on the computed record. The ORM expects the developer to specify those dependencies on the compute method with the decorator depends(). The given dependencies are used by the ORM to trigger the recomputation of the field whenever some of its dependencies have been modified:

Add the percentage of taken seats to the Session model

Display that field in the list and form views

Display the field as a progress bar

Any field can be given a default value. In the field definition, add the option default=X where X is either a Python literal value (boolean, integer, float, string), or a function taking a recordset and returning a value:

The object self.env gives access to request parameters and other useful things:

self.env.cr or self._cr is the database cursor object; it is used for querying the database

self.env.uid or self._uid is the current user’s database id

self.env.user is the current user’s record

self.env.context or self._context is the context dictionary

self.env.ref(xml_id) returns the record corresponding to an XML id

self.env[model_name] returns an instance of the given model

Active objects – Default values

Define the start_date default value as today (see Date).

Add a field active in the class Session, and set sessions as active by default.

The “onchange” mechanism provides a way for the client interface to update a form whenever the user has filled in a value in a field, without saving anything to the database.

For instance, suppose a model has three fields amount, unit_price and price, and you want to update the price on the form when any of the other fields is modified. To achieve this, define a method where self represents the record in the form view, and decorate it with onchange() to specify on which field it has to be triggered. Any change you make on self will be reflected on the form.

For computed fields, valued onchange behavior is built-in as can be seen by playing with the Session form: change the number of seats or participants, and the taken_seats progressbar is automatically updated.

Add an explicit onchange to warn about invalid values, like a negative number of seats, or more participants than seats.

Odoo provides two ways to set up automatically verified invariants: Python constraints and SQL constraints. In a similar way, you can add more complex SQL indexes.

A Python constraint is defined as a method decorated with constrains(), and invoked on a recordset. The decorator specifies which fields are involved in the constraint, so that the constraint is automatically evaluated when one of them is modified. The method is expected to raise an exception if its invariant is not satisfied:

Add Python constraints

Add a constraint that checks that the instructor is not present in the attendees of his/her own session.

Constraints and indexes are defined using: Constraint, Index and UniqueIndex.

With the help of PostgreSQL’s documentation , add the following constraints:

CHECK that the course description and the course title are different

Make the Course’s name UNIQUE

Exercise 6 - Add a duplicate option

Since we added a constraint for the Course name uniqueness, it is not possible to use the “duplicate” function anymore (Form ‣ Duplicate).

Re-implement your own “copy” method which allows to duplicate the Course object, changing the original name into “Copy of [original name]”.

list views can take supplementary attributes to further customize their behavior:

allow changing the style of a row’s text based on the corresponding record’s attributes.

Values are Python expressions. For each record, the expression is evaluated with the record’s attributes as context values and if true, the corresponding style is applied to the row. Here are some of the other values available in the context:

uid: the id of the current user,

today: the current local date as a string of the form YYYY-MM-DD,

now: same as today with the addition of the current time. This value is formatted as YYYY-MM-DD hh:mm:ss.

{$name} can be bf (font-weight: bold), it (font-style: italic), or any bootstrap contextual color (danger, info, muted, primary, success or warning).

Either "top" or "bottom". Makes the list view editable in-place (rather than having to go through the form view), the value is the position where new rows appear.

Modify the Session list view in such a way that sessions lasting less than 5 days are colored blue, and the ones lasting more than 15 days are colored red.

Displays records as calendar events. Their root element is <calendar> and their most common attributes are:

The name of the field used for color segmentation. Colors are automatically distributed to events, but events in the same color segment (records which have the same value for their @color field) will be given the same color.

record’s field holding the start date/time for the event

record’s field holding the end date/time for the event

record’s field to define the label for each calendar event

Add a Calendar view to the Session model enabling the user to view the events associated to the Open Academy.

Search view <field> elements can have a @filter_domain that overrides the domain generated for searching on the given field. In the given domain, self represents the value entered by the user. In the example below, it is used to search on both fields name and description.

Search views can also contain <filter> elements, which act as toggles for predefined searches. Filters must have one of the following attributes:

add the given domain to the current search

add some context to the current search; use the key group_by to group results on the given field name

To use a non-default search view in an action, it should be linked using the search_view_id field of the action record.

The action can also set default values for search fields through its context field: context keys of the form search_default_{field_name} will initialize field_name with the provided value. Search filters must have an optional @name to have a default and behave as booleans (they can only be enabled by default).

Add a button to filter the courses for which the current user is the responsible in the course search view. Make it selected by default.

Add a button to group courses by responsible user.

The gantt view requires the web_gantt module which is present in the enterprise edition version.

Horizontal bar charts typically used to show project planning and advancement, their root element is <gantt>.

Add a Gantt Chart enabling the user to view the sessions scheduling linked to the Open Academy module. The sessions should be grouped by instructor.

Graph views allow aggregated overview and analysis of models, their root element is <graph>.

Pivot views (element <pivot>) a multidimensional table, allows the selection of filers and dimensions to get the right aggregated dataset before moving to a more graphical overview. The pivot view shares the same content definition as graph views.

Graph views have 4 display modes, the default mode is selected using the @type attribute.

a bar chart, the first dimension is used to define groups on the horizontal axis, other dimensions define aggregated bars within each group.

By default bars are side-by-side, they can be stacked by using @stacked="True" on the <graph>

2-dimensional line chart

Graph views contain <field> with a mandatory @type attribute taking the values:

the field should be aggregated by default

the field should be aggregated rather than grouped on

Graph views perform aggregations on database values, they do not work with non-stored computed fields.

Add a Graph view in the Session object that displays, for each course, the number of attendees under the form of a bar chart.

Used to organize tasks, production processes, etc… their root element is <kanban>.

A kanban view shows a set of cards possibly grouped in columns. Each card represents a record, and each column the values of an aggregation field.

For instance, project tasks may be organized by stage (each column is a stage), or by responsible (each column is a user), and so on.

Kanban views define the structure of each card as a mix of form elements (including basic HTML) and QWeb Templates.

Add a Kanban view that displays sessions grouped by course (columns are thus courses).

Access control mechanisms must be configured to achieve a coherent security policy.

Groups are created as normal records on the model res.groups, and granted menu access via menu definitions. However even without a menu, objects may still be accessible indirectly, so actual object-level permissions (read, write, create, unlink) must be defined for groups. They are usually inserted via CSV files inside modules. It is also possible to restrict access to specific fields on a view or object using the field’s groups attribute.

Access rights are defined as records of the model ir.model.access. Each access right is associated to a model, a group (or no group for global access), and a set of permissions: read, write, create, unlink. Such access rights are usually created by a CSV file named after its model: ir.model.access.csv.

Add access control through the Odoo interface

Create a new user “John Smith”. Then create a group “OpenAcademy / Session Read” with read access to the Session model.

Add access control through data files in your module

Create a group OpenAcademy / Manager with full access to all OpenAcademy models

Make Session and Course readable by all users

A record rule restricts the access rights to a subset of records of the given model. A rule is a record of the model ir.rule, and is associated to a model, a number of groups (many2many field), permissions to which the restriction applies, and a domain. The domain specifies to which records the access rights are limited.

Here is an example of a rule that prevents the deletion of leads that are not in state cancel. Notice that the value of the field groups must follow the same convention as the method write() of the ORM.

Add a record rule for the model Course and the group “OpenAcademy / Manager”, that restricts write and unlink accesses to the responsible of a course. If a course has no responsible, all users of the group must be able to modify it.

Wizards describe interactive sessions with the user (or dialog boxes) through dynamic forms. A wizard is simply a model that extends the class TransientModel instead of Model. The class TransientModel extends Model and reuse all its existing mechanisms, with the following particularities:

Wizard records are not meant to be persistent; they are automatically deleted from the database after a certain time. This is why they are called transient.

Wizard records may refer to regular records or wizard records through relational fields(many2one or many2many), but regular records cannot refer to wizard records through a many2one field.

We want to create a wizard that allow users to create attendees for a particular session, or for a list of sessions at once.

Create a wizard model with a many2one relationship with the Session model and a many2many relationship with the Partner model.

Wizards are simply window actions with a target field set to the value new, which opens the view (usually a form) in a separate dialog. The action may be triggered via a menu item, but is more generally triggered by a button.

An other way to launch wizards is through the Action menu of a list or form view. This is done through the binding_model_id field of the action. Setting this field will make the action appear on the views of the model the action is “bound” to.

While wizards use regular views and buttons, normally clicking any button in a form would first save the form then close the dialog. Because this is often undesirable in wizards, a special attribute special="cancel" is available which immediately closes the wizard without saving the form.

Define a form view for the wizard.

Add the action to launch it in the context of the Session model.

Define a default value for the session field in the wizard; use the context parameter self._context to retrieve the current session.

Add buttons to the wizard, and implement the corresponding method for adding the attendees to the given session.

Register attendees to multiple sessions

Modify the wizard model so that attendees can be registered to multiple sessions.

Each module can provide its own translations within the i18n directory, by having files named LANG.po where LANG is the locale code for the language, or the language and country combination when they differ (e.g. pt.po or pt_BR.po). Translations will be loaded automatically by Odoo for all enabled languages. Developers always use English when creating a module, then export the module terms using Odoo’s gettext POT export feature (Settings ‣ Translations ‣ Import/Export ‣ Export Translation without specifying a language), to create the module template POT file, and then derive the translated PO files. Many IDE’s have plugins or modes for editing and merging PO/POT files.

The Portable Object files generated by Odoo are published on Odoo’s Translations Platform, making it easy to translate the software.

By default Odoo’s POT export only extracts labels inside XML files or inside field definitions in Python code, but any Python string can be translated this way by surrounding it with the function odoo._() (e.g. _("Label"))

Choose a second language for your Odoo installation. Translate your module using the facilities provided by Odoo.

Odoo uses a report engine based on QWeb Templates, Twitter Bootstrap and Wkhtmltopdf.

A report is a combination two elements:

an ir.actions.report which configures various basic parameters for the report (default type, whether the report should be saved to the database after generation,…)

Because it largerly a standard action, as with Wizards it is generally useful to add the report as a contextual item on the list and / or form views of the model being reported on via the binding_model_id field.

Here we are also using binding_type in order for the report to be in the report contextual menu rather than the action one. There is no technical difference but putting elements in the right place helps users.

A standard QWeb view for the actual report:

the standard rendering context provides a number of elements, the most important being:

the records for which the report is printed

the user printing the report

Because reports are standard web pages, they are available through a URL and output parameters can be manipulated through this URL, for instance the HTML version of the Invoice report is available through http://localhost:8069/report/html/account.report_invoice/1 (if account is installed) and the PDF version through http://localhost:8069/report/pdf/account.report_invoice/1.

If it appears that your PDF report is missing the styles (i.e. the text appears but the style/layout is different from the html version), probably your wkhtmltopdf process cannot reach your web server to download them.

If you check your server logs and see that the CSS styles are not being downloaded when generating a PDF report, most surely this is the problem.

The wkhtmltopdf process will use the web.base.url system parameter as the root path to all linked files, but this parameter is automatically updated each time the Administrator is logged in. If your server resides behind some kind of proxy, that could not be reachable. You can fix this by adding one of these system parameters:

report.url, pointing to an URL reachable from your server (probably http://localhost:8069 or something similar). It will be used for this particular purpose only.

web.base.url.freeze, when set to True, will stop the automatic updates to web.base.url.

Create a report for the Session model

For each session, it should display session’s name, its start and end, and list the session’s attendees.

Define a dashboard containing the graph view you created, the sessions calendar view and a list view of the courses (switchable to a form view). This dashboard should be available through a menuitem in the menu, and automatically displayed in the web client when the OpenAcademy main menu is selected.

it is possible to disable the automatic creation of some fields

writing raw SQL queries is possible, but requires care as it bypasses all Odoo authentication and security mechanisms.

**Examples:**

Example 1 (sql):
```sql
from . import mymodule
```

Example 2 (jsx):
```jsx
$ odoo-bin scaffold <module name> <where to put it>
```

Example 3 (python):
```python
from odoo import models
class MinimalModel(models.Model):
    _name = 'test.model'
```

Example 4 (python):
```python
from odoo import models, fields

class LessMinimalModel(models.Model):
    _name = 'test.model2'

    name = fields.Char()
```

---

## Chapter 2: A New Application¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/server_framework_101/02_newapp.html

**Contents:**
- Chapter 2: A New Application¶
- The Real Estate Advertisement module¶
- Prepare the addon directory¶

The purpose of this chapter is to lay the foundation for the creation of a completely new Odoo module. We will start from scratch with the minimum needed to have our module recognized by Odoo. In the upcoming chapters, we will progressively add features to build a realistic business case.

Our new module will cover a business area which is very specific and therefore not included in the standard set of modules: real estate. It is worth noting that before developing a new module, it is good practice to verify that Odoo doesn’t already provide a way to answer the specific business case.

Here is an overview of the main list view containing some advertisements:

The top area of the form view summarizes important information for the property, such as the name, the property type, the postcode and so on. The first tab contains information describing the property: bedrooms, living area, garage, garden…

The second tab lists the offers for the property. We can see here that potential buyers can make offers above or below the expected selling price. It is up to the seller to accept an offer.

Here is a quick video showing the workflow of the module.

Hopefully, this video will be recorded soon :-)

Reference: the documentation related to this topic can be found in manifest.

Goal: the goal of this section is to have Odoo recognize our new module, which will be an empty shell for now. It will be listed in the Apps:

The first step of module creation is to create its directory. In the tutorials directory, add a new directory estate.

A module must contain at least 2 files: the __manifest__.py file and a __init__.py file. The __init__.py file can remain empty for now and we’ll come back to it in the next chapter. On the other hand, the __manifest__.py file must describe our module and cannot remain empty. Its only required field is the name, but it usually contains much more information.

Take a look at the CRM file as an example. In addition to providing the description of the module (name, category, summary, website…), it lists its dependencies (depends). A dependency means that the Odoo framework will ensure that these modules are installed before our module is installed. Moreover, if one of these dependencies is uninstalled, then our module and any other that depends on it will also be uninstalled. Think about your favorite Linux distribution package manager (apt, dnf, pacman…): Odoo works in the same way.

Create the required addon files.

Create the following folders and files:

/home/$USER/src/tutorials/estate/__init__.py

/home/$USER/src/tutorials/estate/__manifest__.py

The __manifest__.py file should only define the name and the dependencies of our modules. The only necessary framework module for now is base.

Restart the Odoo server and go to Apps. Click on Update Apps List, search for estate and… tadaaa, your module appears! Did it not appear? Maybe try removing the default ‘Apps’ filter ;-)

Remember to enable the developer mode as explained in the previous chapter. You won’t see the Update Apps List button otherwise.

Make your module an ‘App’.

Add the appropriate key to your __manifest__.py so that the module appears when the ‘Apps’ filter is on.

You can even install the module! But obviously it’s an empty shell, so no menu will appear.

All good? If yes, then let’s create our first model!

---

## Chapter 2: Build a dashboard¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/discover_js_framework/02_build_a_dashboard.html

**Contents:**
- Chapter 2: Build a dashboard¶
- 1. A new Layout¶
- Theory: Services¶
- 2. Add some buttons for quick navigation¶
- 3. Add a dashboard item¶
- 4. Call the server, add some statistics¶
- 5. Cache network calls, create a service¶
- 6. Display a pie chart¶
- 7. Real life update¶
- 8. Lazy loading the dashboard¶

The first part of this tutorial introduced you to most of Owl ideas. It is now time to learn about the Odoo JavaScript framework in its entirety, as used by the web client.

To get started, you need a running Odoo server and a development environment setup. Before getting into the exercises, make sure you have followed all the steps described in this tutorial introduction. For this chapter, we will start from the empty dashboard provided by the awesome_dashboard addon. We will progressively add features to it, using the Odoo JavaScript framework.

The solutions for each exercise of the chapter are hosted on the official Odoo tutorials repository.

Most screens in the Odoo web client uses a common layout: a control panel on top, with some buttons, and a main content zone just below. This is done using the Layout component, available in @web/search/layout.

Update the AwesomeDashboard component located in awesome_dashboard/static/src/ to use the Layout component. You can use {controlPanel: {} } for the display props of the Layout component.

Add a className prop to Layout: className="'o_dashboard h-100'"

Add a dashboard.scss file in which you set the background-color of .o_dashboard to gray (or your favorite color)

Open http://localhost:8069/web, then open the Awesome Dashboard app, and see the result.

Example: use of Layout in client action and template

Example: use of Layout in kanban view

In practice, every component (except the root component) may be destroyed at any time and replaced (or not) with another component. This means that each component internal state is not persistent. This is fine in many cases, but there certainly are situations where we want to keep some data around. For example, all Discuss messages should not be reloaded every time we display a channel.

Also, it may happen that we need to write some code that is not a component. Maybe something that process all barcodes, or that manages the user configuration (context, etc.).

The Odoo framework defines the idea of a service, which is a persistent piece of code that exports state and/or functions. Each service can depend on other services, and components can import a service.

The following example registers a simple service that displays a notification every 5 seconds:

Services can be accessed by any component. Imagine that we have a service to maintain some shared state:

Then, any component can do this:

One important service provided by Odoo is the action service: it can execute all kind of standard actions defined by Odoo. For example, here is how one component could execute an action by its xml id:

Let us now add two buttons to our control panel:

A button Customers, which opens a kanban view with all customers (this action already exists, so you should use its xml id).

A button Leads, which opens a dynamic action on the crm.lead model with a list and a form view. Follow the example of this use of the action service.

Let us now improve our content.

Create a generic DashboardItem component that display its default slot in a nice card layout. It should take an optional size number props, that default to 1. The width should be hardcoded to (18*size)rem.

Add two cards to the dashboard. One with no size, and the other with a size of 2.

Let’s improve the dashboard by adding a few dashboard items to display real business data. The awesome_dashboard addon provides a /awesome_dashboard/statistics route that is meant to return some interesting information.

To call a specific controller, we need to use the rpc function. It only exports a single function that perform the request: rpc(route, params, settings). A basic request could look like this:

Update Dashboard so that it uses the rpc function and call the statistics route /awesome_dashboard/statistics.

Display a few cards in the dashboard containing:

Number of new orders this month

Total amount of new orders this month

Average amount of t-shirt by order this month

Number of cancelled orders this month

Average time for an order to go from ‘new’ to ‘sent’ or ‘cancelled’

If you open the Network tab of your browser’s dev tools, you will see that the call to /awesome_dashboard/statistics is done every time the client action is displayed. This is because the onWillStart hook is called each time the Dashboard component is mounted. But in this case, we would prefer to do it only the first time, so we actually need to maintain some state outside of the Dashboard component. This is a nice use case for a service!

Register and import a new awesome_dashboard.statistics service.

It should provide a function loadStatistics that, once called, performs the actual rpc, and always return the same information.

Use the memoize utility function from @web/core/utils/functions that allows caching the statistics.

Use this service in the Dashboard component.

Check that it works as expected.

Example: simple service

Example: service with a dependency

Everyone likes charts (!), so let us add a pie chart in our dashboard. It will display the proportions of t-shirts sold for each size: S/M/L/XL/XXL.

For this exercise, we will use Chart.js. It is the chart library used by the graph view. However, it is not loaded by default, so we will need to either add it to our assets bundle, or lazy load it. Lazy loading is usually better since our users will not have to load the chartjs code every time if they don’t need it.

Create a PieChart component.

In its onWillStart method, load chartjs, you can use the loadJs function to load /web/static/lib/Chart/Chart.js.

Use the PieChart component in a DashboardItem to display a pie chart that shows the quantity for each sold t-shirts in each size (that information is available in the /statistics route). Note that you can use the size property to make it look larger.

The PieChart component will need to render a canvas, and draw on it using chart.js.

Example: lazy loading a js file

Example: rendering a chart in a component

Since we moved the data loading in a cache, it never updates. But let us say that we are looking at fast moving data, so we want to periodically (for example, every 10min) reload fresh data.

This is quite simple to implement, with a setTimeout or setInterval in the statistics service. However, here is the tricky part: if the dashboard is currently being displayed, it should be updated immediately.

To do that, one can use a reactive object: it is just like the proxy returned by useState, but not linked to any component. A component can then do a useState on it to subscribe to its changes.

Update the statistics service to reload data every 10 minutes (to test it, use 10s instead!)

Modify it to return a reactive object. Reloading data should update the reactive object in place.

The Dashboard component can now use it with a useState

Documentation on reactivity

Example: Use of reactive in a service

Let us imagine that our dashboard is getting quite big, and is only of interest to some of our users. In that case, it could make sense to lazy load our dashboard, and all related assets, so we only pay the cost of loading the code when we actually want to look at it.

One way to do this is to use LazyComponent (from @web/core/assets) as an intermediate that will load an asset bundle before displaying our component.

Move all dashboard assets into a sub folder /dashboard to make it easier to add to a bundle.

Create a awesome_dashboard.dashboard assets bundle containing all content of the /dashboard folder.

Modify dashboard.js to register itself to the lazy_components registry instead of actions.

In src/dashboard_action.js, create an intermediate component that uses LazyComponent and register it to the actions registry.

So far, we have a nice working dashboard. But it is currently hardcoded in the dashboard template. What if we want to customize our dashboard? Maybe some users have different needs and want to see other data.

So, the next step is to make our dashboard generic: instead of hard-coding its content in the template, it can just iterate over a list of dashboard items. But then, many questions come up: how to represent a dashboard item, how to register it, what data should it receive, and so on. There are many different ways to design such a system, with different trade-offs.

For this tutorial, we will say that a dashboard item is an object with the following structure:

The description value will be useful in a later exercise to show the name of items that the user can add to their dashboard. The size number is optional, and simply describes the size of the dashboard item that will be displayed. Finally, the props function is optional. If not given, we will simply give the statistics object as data. But if it is defined, it will be used to compute specific props for the component.

The goal is to replace the content of the dashboard with the following snippet:

Note that the above example features two advanced features of Owl: dynamic components and dynamic props.

We currently have two kinds of item components: number cards with a title and a number, and pie cards with some label and a pie chart.

Create and implement two components: NumberCard and PieChartCard, with the corresponding props.

Create a file dashboard_items.js in which you define and export a list of items, using NumberCard and PieChartCard to recreate our current dashboard.

Import that list of items in our Dashboard component, add it to the component, and update the template to use a t-foreach like shown above.

And now, our dashboard template is generic!

However, the content of our item list is still hardcoded. Let us fix that by using a registry:

Instead of exporting a list, register all dashboard items in a awesome_dashboard registry

Import all the items of the awesome_dashboard registry in the Dashboard component

The dashboard is now easily extensible. Any other Odoo addon that wants to register a new item to the dashboard can just add it to the registry.

Let us see how we can make our dashboard customizable. To make it simple, we will save the user dashboard configuration in the local storage so that it is persistent, but we don’t have to deal with the server for now.

The dashboard configuration will be saved as a list of removed item ids.

Add a button in the control panel with a gear icon to indicate that it is a settings button.

Clicking on that button should open a dialog.

In that dialog, we want to see a list of all existing dashboard items, each with a checkbox.

There should be a Apply button in the footer. Clicking on it will build a list of all item ids that are unchecked.

We want to store that value in the local storage.

And modify the Dashboard component to filter the current items by removing the ids of items from the configuration.

Here is a list of some small improvements you could try to do if you have the time:

Make sure your application can be translated (with env._t).

Clicking on a section of the pie chart should open a list view of all orders that have the corresponding size.

Save the content of the dashboard in a user setting on the server!

Make it responsive: in mobile mode, each card should take 100% of the width.

Example: use of env._t function

Code: translation code in web/

**Examples:**

Example 1 (javascript):
```javascript
import { registry } from "@web/core/registry";

const myService = {
    dependencies: ["notification"],
    start(env, { notification }) {
        let counter = 1;
        setInterval(() => {
            notification.add(`Tick Tock ${counter++}`);
        }, 5000);
    },
};

registry.category("services").add("myService", myService);
```

Example 2 (javascript):
```javascript
import { registry } from "@web/core/registry";

const sharedStateService = {
    start(env) {
        let state = {};
        return {
            getValue(key) {
                return state[key];
            },
            setValue(key, value) {
                state[key] = value;
            },
        };
    },
};

registry.category("services").add("shared_state", sharedStateService);
```

Example 3 (vue):
```vue
import { useService } from "@web/core/utils/hooks";

setup() {
   this.sharedState = useService("shared_state");
   const value = this.sharedState.getValue("somekey");
   // do something with value
}
```

Example 4 (vue):
```vue
import { useService } from "@web/core/utils/hooks";
...
setup() {
      this.action = useService("action");
}
openSettings() {
      this.action.doAction("base_setup.action_general_configuration");
}
...
```

---

## Chapter 10: Constraints¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/server_framework_101/10_constraints.html

**Contents:**
- Chapter 10: Constraints¶
- SQL¶
- Python¶

The previous chapter introduced the ability to add some business logic to our model. We can now link buttons to business code, but how can we prevent users from entering incorrect data? For example, in our real estate module nothing prevents users from setting a negative expected price.

Odoo provides two ways to set up automatically verified invariants: Python constraints and SQL constraints.

Reference: the documentation related to this topic can be found in Models and in the PostgreSQL’s documentation.

Goal: at the end of this section:

Amounts should be (strictly) positive

Property types and tags should have a unique name

SQL objects are defined through Constraint attributes.

Add the following constraints to their corresponding models:

A property expected price must be strictly positive

A property selling price must be positive

An offer price must be strictly positive

A property tag name and property type name must be unique

Tip: search for the unique keyword in the Odoo codebase for examples of unique names.

Restart the server with the -u estate option to see the result. Note that you might have data that prevents a SQL constraint from being set. An error message similar to the following might pop up:

For example, if some offers have a price of zero, then the constraint can’t be applied. You can delete the problematic data in order to apply the new constraints.

Reference: the documentation related to this topic can be found in constrains().

Goal: at the end of this section, it will not be possible to accept an offer lower than 90% of the expected price.

SQL constraints are an efficient way of ensuring data consistency. However it may be necessary to make more complex checks which require Python code. In this case we need a Python constraint.

A Python constraint is defined as a method decorated with constrains() and is invoked on a recordset. The decorator specifies which fields are involved in the constraint. The constraint is automatically evaluated when any of these fields are modified . The method is expected to raise an exception if its invariant is not satisfied:

A simple example can be found here.

Add Python constraints.

Add a constraint so that the selling price cannot be lower than 90% of the expected price.

Tip: the selling price is zero until an offer is validated. You will need to fine tune your check to take this into account.

Always use the float_compare() and float_is_zero() methods from odoo.tools.float_utils when working with floats!

Ensure the constraint is triggered every time the selling price or the expected price is changed!

SQL constraints are usually more efficient than Python constraints. When performance matters, always prefer SQL over Python constraints.

Our real estate module is starting to look good. We added some business logic, and now we make sure the data is consistent. However, the user interface is still a bit rough. Let’s see how we can improve it in the next chapter.

**Examples:**

Example 1 (unknown):
```unknown
_check_percentage = models.Constraint(
    'CHECK(percentage >= 0 AND percentage <= 100)',
    'The percentage of an analytic distribution should be between 0 and 100.',
)
```

Example 2 (typescript):
```typescript
ERROR rd-demo odoo.schema: Table 'estate_property_offer': unable to add constraint 'estate_property_offer_check_price' as CHECK(price > 0)
```

Example 3 (python):
```python
from odoo.exceptions import ValidationError

...

@api.constrains('date_end')
def _check_date_end(self):
    for record in self:
        if record.date_end < fields.Date.today():
            raise ValidationError("The end date cannot be set in the past")
    # all records passed the test, don't return anything
```

---

## Master the web framework¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/master_odoo_web_framework.html

**Contents:**
- Master the web framework¶
- Setup¶
- Content¶

This tutorial is designed for those who have completed the Discover the web framework tutorial and are looking to deepen their knowledge of the web framework. It is organized in four independent projects, each focusing on different features of Odoo.

Each of these chapters can be done independantly, in any order. Also, be aware that some of them cover a lot of material, so they may be quite long.

The first project is about building a clicker game. While working on it, you will learn various aspects of the web framework: systray, command palette, dialogs, notifications, customizing existing components and much more.

The second project is focused on an important category of components: fields. Field components represent the value of a field for a record, they appear in many places in the web client: in form views, obviously, but also in kanban and list views, and may even be used alone, without a view. Due to their importance, it makes sense to learn how to create and manipulate such components.

In the context of the web framework, views usually refers to the javascript implementation of a component that represents one or many records, depending on a description (ir.ui.view). Such components are actually quite complicated and usually requires various sub systems (a renderer, a model, a controller, a arch parser, …). In chapter 3, we create a new view from scratch to represent a list of images.

Finally, the last project in chapter 4 is about customizing an existing view (a kanban view) by adding a search panel on its left. It is interesting to see how one can take existing code, and modify it to suit our needs. Also, it is a realistic project, that will feature many common issues that arises while working on Odoo.

Clone the official Odoo tutorials repository and switch to the branch 19.0.

Add the cloned repository to your --addons-path.

Start a new Odoo database and install the modules for each chapter that you want to work on: awesome_clicker (for chapter 1), awesome_fields (for chapter 2), awesome_gallery (for chapter 3) or awesome_kanban (for chapter 4).

Chapter 1: Build a Clicker game

Chapter 2: Create a Gallery View

Chapter 3: Customize a kanban view

---

## Chapter 2: Create a Gallery View¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/master_odoo_web_framework/02_create_gallery_view.html

**Contents:**
- Chapter 2: Create a Gallery View¶
- 1. Make a hello world view¶
- 2. Use the Layout component¶
- 3. Parse the arch¶
- 4. Load some data¶
- 5. Solve the concurrency problem¶
- 6. Reorganize code¶
- 7. Make the view extensible¶
- 8. Display images¶
- 9. Switch to form view on click¶

Let us see how one can create a new view, completely from scratch. In a way, it is not very difficult to do, but there are no really good resources on how to do it. Note that most situations should be solved by either customizing an existing view, or with a client action.

For this exercise, let’s assume that we want to create a gallery view, which is a view that lets us represent a set of records with an image field.

The problem could certainly be solved with a kanban view, but this means that it is not possible to have our normal kanban view and the gallery view in the same action.

Let us make a gallery view. Each gallery view will be defined by an image_field attribute in its arch:

To complete the tasks in this chapter, you will need to install the awesome_gallery addon. This addon includes the necessary server files to add a new view.

The solutions for each exercise of the chapter are hosted on the official Odoo tutorials repository.

First step is to create a JavaScript implementation with a simple component.

Create the gallery_view.js , gallery_controller.js and gallery_controller.xml files in static/src.

Implement a simple hello world component in gallery_controller.js.

In gallery_view.js, import the controller, create a view object, and register it in the view registry under the name gallery.

Here is an example on how to define a view object:

Add gallery as one of the view type in the contacts.action_contacts action.

Make sure that you can see your hello world component when switching to the gallery view.

So far, our gallery view does not look like a standard view. Let’s use the Layout component to have the standard features like other views.

Import the Layout component and add it to the components of GalleryController.

Update the template to use Layout. It needs a display prop, which can be found in props.display.

For now, our gallery view does not do much. Let’s start by reading the information contained in the arch of the view.

The process of parsing an arch is usually done with a ArchParser, specific to each view. It inherits from a generic XMLParser class.

Here is an example of what an ArchParser might look like:

Create the ArchParser class in its own file.

Use it to read the image_field information.

Update the gallery view code to add it to the props received by the controller.

It is probably a little overkill to do it like that, since we basically only need to read one attribute from the arch, but it is a design that is used by every other odoo views, since it lets us extract some upfront processing out of the controller.

Example: The graph arch parser

Let us now get some real data from the server. For that we must use webSearchRead from the orm service.

Here is an example of a webSearchRead to get the records from a model:

Add a loadImages(domain) {...} method to the GalleryController. It should perform a webSearchRead call from the orm service to fetch records corresponding to the domain, and use imageField received in props.

If you didn’t include bin_size in the context of the call, you will receive the image field encoded in base64. Make sure to put bin_size in the context to receive the size of the image field. We will display the image later.

Modify the setup code to call that method in the onWillStart and onWillUpdateProps hooks.

Modify the template to display the id and the size of each image inside the default slot of the Layout component.

The loading data code will be moved into a proper model in a next exercise.

For now, our code is not concurrency proof. If one changes the domain twice, it will trigger the loadImages(domain) twice. We have thus two requests that can arrive at different time depending on different factors. Receiving the response for the first request after receiving the response for the second request will lead to an inconsistent state.

The KeepLast primitive from Odoo solves this problem, it manages a list of tasks, and only keeps the last task active.

Import KeepLast from @web/core/utils/concurrency.

Instanciate a KeepLast object in the model.

Add the webSearchRead call in the KeepLast so that only the last call is resolved.

Example: usage of KeepLast

Real views are a little bit more organized. This may be overkill in this example, but it is intended to learn how to structure code in Odoo. Also, this will scale better with changing requirements.

Move all the model code in its own GalleryModel class.

Move all the rendering code in a GalleryRenderer component.

Import GalleryModel and GalleryRenderer in GalleryController to make it work.

To extends the view, one could import the gallery view object to modify it to their taste. The problem is that for the moment, it is not possible to define a custom model or renderer because it is hardcoded in the controller.

Import GalleryModel and GalleryRenderer in the gallery view file.

Add a Model and Renderer key to the gallery view object and assign them to GalleryModel and GalleryRenderer. Pass Model and Renderer as props to the controller.

Remove the hardcoded import in the controller and get them from the props.

Use t-component to have dynamic sub component.

This is how someone could now extend the gallery view by modifying the renderer:

Update the renderer to display images in a nice way, if the field is set. If image_field is empty, display an empty box instead.

There is a controller that allows to retrieve an image from a record. You can use this snippet to construct the link:

Update the renderer to react to a click on an image and switch to a form view. You can use the switchView function from the action service.

Code: The switchView function

It is useful to have some additional information on mouse hover.

Update the code to allow an optional additional attribute on the arch:

On mouse hover, display the content of the tooltip field. It should work if the field is a char field, a number field or a many2one field. To put a tooltip to an html element, you can put the string in the data-tooltip attribute of the element.

Update the customer gallery view arch to add the customer as tooltip field.

Example: usage of t-att-data-tooltip

Let’s add a pager on the control panel and manage all the pagination like in a normal Odoo view.

Code: The usePager hook

Example: usePager in list controller

We have a nice and useful view so far. But in real life, we may have issue with users incorrectly encoding the arch of their Gallery view: it is currently only an unstructured piece of XML.

Let us add some validation! In Odoo, XML documents can be described with an RN file (Relax NG file), and then validated.

Add an RNG file that describes the current grammar:

A mandatory attribute image_field.

An optional attribute: tooltip_field.

Add some code to make sure all views are validated against this RNG file.

While we are at it, let us make sure that image_field and tooltip_field are fields from the current model.

Since validating an RNG file is not trivial, here is a snippet to help:

Example: The RNG file of the graph view

Our gallery view does not allow users to upload images. Let us implement that.

Add a button on each image by using the FileUploader component.

The FileUploader component accepts the onUploaded props, which is called when the user uploads an image. Make sure to call webSave from the orm service to upload the new image.

You maybe noticed that the image is uploaded but it is not re-rendered by the browser. This is because the image link did not change so the browser do not re-fetch them. Include the write_date from the record to the image url.

Make sure that clicking on the upload button does not trigger the switchView.

Example: usage of FileUploader

Odoo: webSave definition

For now we can only specify a tooltip field. But what if we want to allow to write a specific template for it ?

This is an example of a gallery arch view that should work after this exercise.

Replace the res.partner gallery arch view in awesome_gallery/views/views.xml with the arch in example above. Don’t worry if it does not pass the rng validation.

Modify the gallery rng validator to accept the new arch structure.

You can use this rng snippet to validate the tooltip-template tag

The arch parser should parse the fields and the tooltip template. Import visitXML from @web/core/utils/xml and use it to parse field names and the tooltip template.

Make sure that the model call the webSearchRead by including the parsed field names in the specification.

The renderer (or any sub-component you created for it) should receive the parsed tooltip template. Manipulate this template to replace the <field> element into a <t t-esc="x"> element.

The template is an Element object so it can be manipulated like a HTML element.

Register the template to Owl thanks to the xml function from @odoo/owl.

Use the useTooltip hook from @web/core/tooltip/tooltip_hook to display the tooltips. This hooks take as argument the Owl template and the variable needed by the template.

Example: useTooltip used in Kaban

Example: visitXML usage

Owl: Inline templates with xml helper function

**Examples:**

Example 1 (jsx):
```jsx
<gallery image_field="some_field"/>
```

Example 2 (javascript):
```javascript
import { registry } from "@web/core/registry";
import { MyController } from "./my_controller";

export const myView = {
      type: "my_view",
      display_name: "MyView",
      icon: "oi oi-view-list",
      multiRecord: true,
      Controller: MyController,
};

registry.category("views").add("my_controller", myView);
```

Example 3 (php):
```php
export class MyCustomArchParser {
    parse(xmlDoc) {
       const myAttribute = xmlDoc.getAttribute("my_attribute")
       return {
           myAttribute,
       }
    }
}
```

Example 4 (json):
```json
const { length, records } = this.orm.webSearchRead(this.resModel, domain, {
   specification: {
        [this.fieldToFetch]: {},
        [this.secondFieldToFetch]: {},
    },
    context: {
        bin_size: true,
    }
})
```

---

## Tutorials¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials.html

**Contents:**
- Tutorials¶
- Learn the server and web frameworks¶
    - Server framework 101
    - Discover the web framework
    - Master the web framework
- Expand your knowledge on the server framework¶
    - Define module data
    - Restrict access to data
    - Safeguard your code with unit tests
    - Write importable modules

If you are new to Odoo development, we recommend starting with the setup guide.

This introductory tutorial is designed for complete beginners seeking to get started in Odoo development. It covers the essential aspects and key concepts of the server framework. Learn to create a simple module from scratch with step-by-step instructions and practical insights.

This tutorial will teach the basics of the web framework and how to work with Owl components by customizing the web client.

Become an expert in the web framework. A large variety of features are covered such as fields, views, and even the kitten mode.

Define master and demo data for an Odoo module, leveraging the strengths of the CSV and XML file formats to accommodate specific data requirements.

Implement security measures to restrict access to sensitive data with the help of groups, access rights, and record rules.

Write effective unit tests in Python to ensure the resilience of your code and safeguard it against unexpected behaviors and regressions.

Write modules that define new models, fields and logic using only data files.

Create mixins to code features once and reuse them in multiple models.

Use QWeb, Odoo’s powerful templating engine, to create custom PDF reports for your documents.

Create a tailored website from scratch fully integrated with Odoo and editable via the Website Builder.

---

## Chapter 1 - Theming¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/website_theme/01_theming.html

**Contents:**
- Chapter 1 - Theming¶
- Setup¶
- Build your module structure¶
- Declare Odoo variables¶
- Declare Bootstrap variables¶
- Define presets¶

Now that you have Odoo installed and your server is running locally, it’s time to create your own theme module for your website.

See reference documentation on how to run Odoo.

Now that we know everything is working properly, let’s start building our module.

Based on the following structure, start creating your module that will be used as a theme. This is where you are going to add your XML pages, SCSS, JS, …

See reference documentation on how to structure your Theme module.

In your __manifest__.py file, you can declare your module with the following information:

In the primary_variables.scss file, you can override the default Odoo SCSS variables to match your design.

Based on the Airproof design, create your primary_variables.scss file and define the following elements:

Headings font family : Space Grotesk

Content font family : Lato. For this, use these 2 woff files.

The color palette name and the 5 main colors that compose it: #000000, #BBE1FA, #CEF8A1, #FFFFFF, #0B8EE6

Header & Footer : Use one of the default templates for the moment, we will create a custom header later.

See reference documentation on how to use primary variables, as well as a list of all primary variables available.

To ensure your changes are applied correctly, log in to your website and check that your color palette includes your specified colors.

You will need to override more variables to replicate the Airproof design. Remember to add them throughout the creation of your website.

For font families you can either use Google fonts (via URL) or add your own fonts.

To complete this exercise, you need to:

Create your primary_variables.scss file. You can find all the necessary information in the primary_variables.scss file from our example module.

Load your custom font in a font.scss file.

Declare the 2 files you just created in __manifest__.py as indicated in the documentation.

Install your module via your script. In our example, it looks like this:

On top of the default Odoo variables, you can also redefine the Bootstrap variables. Bootstrap is a front-end framework which is included by default in Odoo.

Based on the Airproof design, define the following elements:

Headings font sizes :

Display 1 : 6.25rem (Who will be useful for the main title of your homepage)

Inputs border radius : 10px

Inputs border color : black

Inputs border width : 1px

Large buttons border radius : 0px 10px 10px 10px

See reference documentation on how to use Bootstrap variables.

A list of all Bootstrap variables used by Odoo.

And Bootstrap framework official documentation.

You will need to override more variables to replicate the Airproof design. Remember to add them throughout the creation of your website.

Make it a habit to regularly check locally that your changes have been successfully applied and have not caused any errors.

To complete this exercise, you need to:

Create your bootstrap_overridden.scss file. You can find all the necessary information in the bootstrap_overridden.scss file from our example module.

Declare your file in the __manifest__.py as indicated in the documentation.

In addition to the variables we have just covered, you can also activate specific views to modify the design.

Add your presets.xml file and based on the Airproof design, activate the appropriate views to meet the following client requests:

Deactivate the Call-to-action in the header.

Deactivate the wishlist feature in the shop but activate it on the product page.

On the shop page, activate the filtering by categories only on the left side.

To complete the exercise, you need to install the eCommerce (website_sale) and wishlist (website_sale_whishlist) applications. Be careful! Referencing an application in your code that hasn’t been installed will result in an error.

To see the effect of your presets, add some products (Airproof Mini, Airproof Robin, Warranty, Charger cable) and create eCommerce categories (Warranties, Accessories, and Drones with Camera drones and Waterproof drones) in the database. You will find the product images here.

You will need to activate more views to replicate the Airproof design. Remember to add them throughout the creation of your website.

To deactivate the Call-to-action:

The view you have to find is in odoo/addons/website/views/website_templates.xml l:2113

Create your presets.xml file with the right records

In the manifest, add the 2 apps and declare your file.

**Examples:**

Example 1 (unknown):
```unknown
./odoo-bin --addons-path=../enterprise,addons,../myprojects --db-filter=theming -d theming
--without-demo=all -i website_airproof --dev=xml
```

Example 2 (xml):
```xml
<?xml version="1.0" encoding="utf-8"?>
<odoo>
   <!-- Disable Call-to-action in header -->
   <record id="website.header_call_to_action" model="ir.ui.view">
      <field name="active" eval="False"/>
   </record>
</odoo>
```

Example 3 (json):
```json
'depends': ['website_sale', 'website_sale_wishlist'],
'data': [
   # Options
   'data/presets.xml',
]
```

---

## Chapter 15: The final word¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/server_framework_101/15_final_word.html

**Contents:**
- Chapter 15: The final word¶
- Coding guidelines¶
- Test on the runbot¶

We will start refactoring the code to match to the Odoo coding guidelines. The guidelines aim to improve the quality of the Odoo Apps code.

Reference: you will find the Odoo coding guidelines in Coding guidelines.

Refactor your code to respect the coding guidelines. Don’t forget to run your linter and respect the module structure, the variable names, the method name convention, the model attribute order and the xml ids.

Odoo has its own CI server named runbot. All commits, branches and PR will be tested to avoid regressions or breaking of the stable versions. All the runs that pass the tests are deployed on their own server with demo data.

Play with the runbot.

Feel free to go to the runbot website and open the last stable version of Odoo to check out all the available applications and functionalities.

---

## Chapter 6: Basic Views¶

**URL:** https://www.odoo.com/documentation/19.0/developer/tutorials/server_framework_101/06_basicviews.html

**Contents:**
- Chapter 6: Basic Views¶
- List¶
- Form¶
- Search¶
  - Domains¶

We have seen in the previous chapter that Odoo is able to generate default views for a given model. In practice, the default view is never acceptable for a business application. Instead, we should at least organize the various fields in a logical manner.

Views are defined in XML files with actions and menus. They are instances of the ir.ui.view model.

In our real estate module, we need to organize the fields in a logical way:

in the list view, we want to display more than just the name.

in the form view, the fields should be grouped.

in the search view, we must be able to search on more than just the name. Specifically, we want a filter for the ‘Available’ properties and a shortcut to group by postcode.

Reference: the documentation related to this topic can be found in List.

Goal: at the end of this section, the list view should look like this:

List views, also called list views, display records in a tabular form.

Their root element is <list>. The most basic version of this view simply lists all the fields to display in the table (where each field is a column):

A simple example can be found here.

Add a custom list view.

Define a list view for the estate.property model in the appropriate XML file. Check the Goal of this section for the fields to display.

do not add the editable="bottom" attribute that you can find in the example above. We’ll come back to it later.

some field labels may need to be adapted to match the reference.

As always, you need to restart the server (do not forget the -u option) and refresh the browser to see the result.

You will probably use some copy-paste in this chapter, therefore always make sure that the id remains unique for each view!

Remember to set the correct access rights to the user as explained in the security intro!

The Create button is not shown if the user has only read permission.

Reference: the documentation related to this topic can be found in Form.

Goal: at the end of this section, the form view should look like this:

Forms are used to create and edit single records.

Their root element is <form>. They are composed of high-level structure elements (groups and notebooks) and interactive elements (buttons and fields):

It is possible to use regular HTML tags such as div and h1 as well as the the class attribute (Odoo provides some built-in classes) to fine-tune the look.

A simple example can be found here.

Add a custom form view.

Define a form view for the estate.property model in the appropriate XML file. Check the Goal of this section for the expected final design of the page.

This might require some trial and error before you get to the expected result ;-) It is advised that you add the fields and the tags one at a time to help understand how it works.

In order to avoid relaunching the server every time you do a modification to the view, it can be convenient to use the --dev xml parameter when launching the server:

This parameter allows you to just refresh the page to view your view modifications.

Reference: the documentation related to this topic can be found in Search.

Goal: at the end of this section, the search view should look like this:

Search views are slightly different from the list and form views since they don’t display content. Although they apply to a specific model, they are used to filter other views’ content (generally aggregated views such as List). Beyond the difference in use case, they are defined the same way.

Their root element is <search>. The most basic version of this view simply lists all the fields for which a shortcut is desired:

The default search view generated by Odoo provides a shortcut to filter by name. It is very common to add the fields which the user is likely to filter on in a customized search view.

Add a custom search view.

Define a search view for the estate.property model in the appropriate XML file. Check the first image of this section’s Goal for the list of fields.

After restarting the server, it should be possible to filter on the given fields.

Search views can also contain <filter> elements, which act as toggles for predefined searches. Filters must have one of the following attributes:

domain: adds the given domain to the current search

context: adds some context to the current search; uses the key group_by to group results on the given field name

A simple example can be found here.

Before going further in the exercise, it is necessary to introduce the ‘domain’ concept.

Reference: the documentation related to this topic can be found in Search domains.

In Odoo, a domain encodes conditions on records: a domain is a list of criteria used to select a subset of a model’s records. Each criterion is a triplet with a field name, an operator and a value. A record satisfies a criterion if the specified field meets the condition of the operator applied to the value.

For instance, when used on the Product model the following domain selects all services with a unit price greater than 1000:

By default criteria are combined with an implicit AND, meaning every criterion needs to be satisfied for a record to match a domain. The logical operators & (AND), | (OR) and ! (NOT) can be used to explicitly combine criteria. They are used in prefix position (the operator is inserted before its arguments rather than between). For instance, to select products ‘which are services OR have a unit price which is NOT between 1000 and 2000’:

XML does not allow < and & to be used inside XML elements. To avoid parsing errors, entity references should be used: &lt; for < and &amp; for &. Other entity references (&gt;, &apos; & &quot;) are optional.

Add filter and Group By.

The following should be added to the previously created search view:

a filter which displays available properties, i.e. the state should be ‘New’ or ‘Offer Received’.

the ability to group results by postcode.

Looking good? At this point we are already able to create models and design a user interface which makes sense business-wise. However, a key component is still missing: the link between models.

**Examples:**

Example 1 (jsx):
```jsx
<list string="Tests">
    <field name="name"/>
    <field name="last_seen"/>
</list>
```

Example 2 (jsx):
```jsx
<form string="Test">
    <sheet>
        <group>
            <group>
                <field name="name"/>
            </group>
            <group>
                <field name="last_seen"/>
            </group>
        </group>
        <notebook>
            <page string="Description">
                <field name="description"/>
            </page>
        </notebook>
    </sheet>
</form>
```

Example 3 (unknown):
```unknown
$ ./odoo-bin --addons-path=addons,../enterprise/,../tutorials/ -d rd-demo -u estate --dev xml
```

Example 4 (jsx):
```jsx
<search string="Tests">
    <field name="name"/>
    <field name="last_seen"/>
</search>
```

---

## Getting started¶

**URL:** https://www.odoo.com/documentation/19.0/administration/odoo_sh/getting_started.html

**Contents:**
- Getting started¶
- Main components¶
- User types¶

When working with Odoo.sh, it is important to understand the main components involved. While they are all interconnected, each one plays a distinct role in the development and deployment of Odoo applications:

GitHub repository: a version-controlled space where the Odoo applications’ source code is stored. It tracks every change, supports collaboration, and can be either public or private.

Odoo.sh project: a Platform as a Service (PaaS) that integrates with GitHub and enables streamlined development, testing, and deployment of Odoo applications. It includes tools such as automated backups, staging environments, and continuous integration pipelines.

Odoo database: a database stores all the operational data used and generated by Odoo applications, such as business records, configurations, and user data.

Together, they form a cohesive pipeline from code development to a live business use.

Odoo.sh involves different types of users, each with a specific role in the project lifecycle:

GitHub users: developers with access to the GitHub repository linked to the Odoo.sh project. Access to the repository does not automatically make someone a collaborator on the Odoo.sh project.

Odoo.sh collaborators: individuals managing the Odoo.sh project. Each collaborator must be linked to a GitHub user. However, collaborators are not the same as database users.

Database users: end-users of the deployed Odoo database. They interact with the live system but are not involved in development or project management.

---
