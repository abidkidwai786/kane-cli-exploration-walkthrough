---
assurance:
  id: t-29
  base: sha256:83ce675ea2f06b609c8d0fd3967a873692518f48e9a852425c98fc799956d02f
---
# Profile body Logout has the documented href

> Reach Profile using verified Setup and Profile flows and inspect its body Logout target without assuming a post-click session result.

## Step 1

Open https://katalon-demo-cura.herokuapp.com/ in the browser and reach the CURA Healthcare Service Make Appointment screen through the observed Setup journey, using {{login_username}} and {{login_password}} if sign-in is requested.

## Step 2

From the Make Appointment navigation, follow Profile to reach the Profile screen.

## Step 3 @verifies ac-16, ac-33, ac-40

In the Profile page body, inspect the separate Logout link without following it, then assert the body contains a Logout link whose href equals https://katalon-demo-cura.herokuapp.com/authenticate.php?logout.
