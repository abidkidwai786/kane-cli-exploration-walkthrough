---
assurance:
  id: t-28
  base: sha256:e0ee4bded2371c6e456a4e1285d8a908a308607a4f3803c70718c37da9ddded8
---
# Profile navigation Logout has the documented href

> Reach Profile using verified Setup and Profile flows and inspect its navigation Logout target without assuming a post-click session result.

## Step 1

Open https://katalon-demo-cura.herokuapp.com/ in the browser and reach the CURA Healthcare Service Make Appointment screen through the observed Setup journey, using {{login_username}} and {{login_password}} if sign-in is requested.

## Step 2

From the Make Appointment navigation, follow Profile to reach the Profile screen.

## Step 3 @verifies ac-15, ac-33, ac-40

On the Profile screen, expose its navigation and inspect the navigation Logout link without following it, then assert the navigation contains a Logout link whose href equals https://katalon-demo-cura.herokuapp.com/authenticate.php?logout.
