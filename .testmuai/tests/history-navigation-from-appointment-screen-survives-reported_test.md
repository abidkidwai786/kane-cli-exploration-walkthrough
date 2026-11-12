---
assurance:
  id: t-11
  base: sha256:a19b14fa730001306349adad02a6f313e9b5ca960f3d7be121e75264632ddae5
---
# History navigation from appointment screen survives reported unchanged activation

> In the appointment-screen context of the reported failed walk, activate History twice if the first activation leaves the screen unchanged; check the documented History destination rather than treating the observed no-op as expected.

## Step 1

At https://katalon-demo-cura.herokuapp.com/, use {{setup_credentials}} to follow the CURA Healthcare Service Setup flow and reach the Make Appointment screen.

## Step 2 @verifies ac-27

On the CURA Healthcare Service Make Appointment screen at https://katalon-demo-cura.herokuapp.com/#appointment, activate History in the navigation; if the screen remains unchanged, activate History a second time, then assert the browser URL equals https://katalon-demo-cura.herokuapp.com/history.php.

## Step 3 @verifies ac-28

On the reached CURA Healthcare Service History page, inspect its main heading, then assert it reads History.
