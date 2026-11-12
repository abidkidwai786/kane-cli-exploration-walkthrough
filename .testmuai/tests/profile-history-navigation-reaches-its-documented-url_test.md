---
assurance:
  id: t-17
  base: sha256:ba89a5e43fdf58d095a149fd038ea5fb66d7a78efdd31ebb35a85ddc36e4b792
---
# Profile History navigation reaches its documented URL

> Reach Profile via the verified Setup and Profile flows and follow its observed History link to the History destination.

## Step 1

At https://katalon-demo-cura.herokuapp.com/, follow the verified Setup flow to reach the CURA Healthcare Service Make Appointment screen.

## Step 2

From the Make Appointment screen, follow the verified Profile flow to the CURA Healthcare Service Profile screen.

## Step 3 @verifies ac-31

On the Profile screen, expose its navigation and inspect History, then assert its History link exists.

## Step 4 @verifies ac-32

On the Profile screen, inspect the History link's browser DOM destination, then assert its href equals https://katalon-demo-cura.herokuapp.com/history.php.

## Step 5 @verifies ac-27

From the Profile screen, follow History to its destination, then assert the browser URL equals https://katalon-demo-cura.herokuapp.com/history.php.
