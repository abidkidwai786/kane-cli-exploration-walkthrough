---
assurance:
  id: t-30
  base: sha256:c3752b2c3be3d70e3eb39229aba66d5e28ee4afbadf28a39abe49abfc1e0b526
---
# Appointment Confirmation navigation Logout has the documented href

> Reach Appointment Confirmation using verified Setup and Submit appointment flows and inspect its navigation Logout target without assuming session termination.

## Step 1

Open https://katalon-demo-cura.herokuapp.com/ in the browser and reach the CURA Healthcare Service Make Appointment screen through the observed Setup journey, using {{login_username}} and {{login_password}} if sign-in is requested.

## Step 2

From Make Appointment, complete the observed Submit appointment journey using valid booking inputs from {{appointment_details}} to reach the Appointment Confirmation screen.

## Step 3 @verifies ac-25, ac-23, ac-40

On the Appointment Confirmation screen, expose the navigation and inspect its Logout link without following it, then assert the navigation contains a Logout link whose href equals https://katalon-demo-cura.herokuapp.com/authenticate.php?logout.
