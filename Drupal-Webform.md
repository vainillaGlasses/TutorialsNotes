# Drupal - Webform

The [Webform module](https://www.drupal.org/project/webform) in Drupal 8 makes it easy to create complex forms in no time. While the [Contact module in Drupal 8](https://www.webwash.net/build-a-blog-in-drupal-8-custom-contact-forms/) core does allow you to create simple personal and site-wide contact forms, functionality is limited. 

Video about ir https://youtu.be/VncMRSwjVto



## Install in Humpback

```bash
ahoy site composer require drupal/webform
ahoy drush en -y webform webform_ui
```

- **Optional** ``drush en webform webform_ui``

- **Do not download webform libraries** ``drush webform-libraries-download``





From: https://www.webwash.net/getting-started-webform-drupal-8/



## Create a SignUp

1. Click on the “Add webform” button.

2. Give the form a name such as “[MyPageName] signup” and an appropriate description if you want.

3. If you plan to have a lot of forms then adding categories can be useful. We’ll add a Newsletter category by selecting the “Other…” option.

4. Click on Save to create the form.

   **On the webform**

1. Click on the “Add element” button.

2. Add an "Text Field"

3. Enter “First name” for the title.

4. We’ll leave all the other settings as they are. Click on Save to finish.

5. Add an "Email"

6. Enter a title of Email and click on Save.

   ** If you wanted to make sure the user entered their email address correctly then you can use the “Email confirm” element 

   **Customize**

1. Click on the Biuld tab.
2. To the right of “Submit button(s)”, click on Customize.
3. Enter Signup under “Submit button label”. We’ve also changed the Title to Signup.
4. Click on Save.
5. Tick Required for both the first name and email elements and then “Save elements”.



## Testing the Form and Viewing Results

1. Click in Test tab
2. Fill the fields
3. Click "Singup" button
4. Click in Results tab, and check the entry



## Emailing Results

1. Go to ``Settings > Emails/Handlers``tab
2. Click add email
3. In "Send to" you can set it as custom email, site email or current user email, etc.
4. The email that’s sent can be customized in the Message section if required but we’ll just stick with the default message.

5. Click on Save to create the email handler.



## Adding Checkboxes 

1. From the Edit tab, click on the Elements sub-tab, then “Add element”.

2. Use the filter to help find “Checkboxes other” and select “Add element”.

3. Give it a title of “What are your main interests?”.

4. With long titles, it’s often worth shortening the associated key as this will be used for CSS classes and referred to in other areas of Webform. Click on the Edit link to the right of the title and change the key to main_interests.

5. Scroll down to the “Elements options” section.

6. The Options list allows you to select from pre-defined lists such as days of the week. For our example, we want to enter values, so leave Options set to “Custom…”.

7. For each option, we need to add a value and text pair. The “Option value” is used internally and you’ll see the values in the markup and CSS IDs and classes that are added. The “Option text” will appear to the user on the form. We’ve created three options with pairs html / HTML, css / CSS and js / JavaScript, as shown below.
8. Scroll down to the “Other option settings” section. These settings apply to an additional item that will be added to the end of the list of checkboxes. The default settings create an “Other…” checkbox with an additional textfield if other is selected. The settings are customizable but for our example we’ll stick with the defaults.
9. Click on Save to complete the process.



## Adding Radio Buttons

1. Add the Radios element.

2. Give it a title of “Which JavaScript library are you most interested in?”.

3. Change the key to js_library by clicking on Edit to the right of the title.

4. Add options for jQuery, AngularJS and React as shown below.

5. Leave all the other options at their default settings and click on Save.



## Enhancing Checkboxes and Radio Buttons

1. Click Webform > Configuration > Libraries  tab

2. On the list, check el jQuery: iCheck
3. Save
4. On Settings tab.
5. Expand the “Element Settings” section
6. Within that section you’ll find “Checkbox/radio settings”. Expand this also.
7. Select an option from “Enhance checkboxes/radio buttons using iCheck” that works with your theme (it’s worth trying the different styles). In our screenshots, we’ve used the “Square: Blue” option
8. Scroll to the bottom of the screen and click on “Save configuration”.