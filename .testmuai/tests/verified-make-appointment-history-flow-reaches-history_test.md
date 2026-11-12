---
assurance:
  id: t-16
  base: sha256:f6052adcf78ced532c9aea06c584daacbffdb82219660face48b12a1db80f718
---
# Verified Make Appointment History flow reaches History

> Reach Make Appointment by its verified Setup flow and follow its verified History route to the observed History screen without asserting that the removed link control remains present.

## Step 1

At https://katalon-demo-cura.herokuapp.com/, use {{setup_credentials}} if sign-in is required to reach the CURA Healthcare Service Make Appointment screen through the verified Setup flow.

## Step 2 @verifies ac-27

From the CURA Healthcare Service Make Appointment screen, follow the verified History flow to its destination, then assert the browser URL equals https://katalon-demo-cura.herokuapp.com/history.php.

## Step 3 @verifies ac-28

On the reached CURA Healthcare Service History page, inspect its main heading, then assert it reads History.
