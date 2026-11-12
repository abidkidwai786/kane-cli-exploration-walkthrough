---
assurance:
  id: t-27
  base: sha256:1a0f0fd2ccd8462def9fc5df17fe7e214b149d7368bee67486ff1145a4ff2433
---
# History navigation Logout has the documented href

> Reach History using verified Setup and History flows and inspect the navigation Logout target without inferring a post-click session result.

## Step 1

Open https://katalon-demo-cura.herokuapp.com/ in the browser and reach the CURA Healthcare Service Make Appointment screen through the observed Setup journey, using {{login_username}} and {{login_password}} if sign-in is requested.

## Step 2

From the Make Appointment navigation, follow History to reach the History screen.

## Step 3 @verifies ac-24, ac-23, ac-40

On the History screen, expose its navigation and inspect the navigation Logout link without following it, then assert the navigation contains a Logout link whose href equals https://katalon-demo-cura.herokuapp.com/authenticate.php?logout.
