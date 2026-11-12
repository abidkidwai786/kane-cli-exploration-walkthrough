---
assurance:
  id: t-26
  base: sha256:ac5a830b68fe2c46e12a4ade10aeeb4d1db3e18750280b99084e4061f10c0411
---
# Verify Profile arrival from Make Appointment

> From Make Appointment follow the verified Profile flow, inspect the destination profile.php and its Profile heading without claiming all activations always succeed or that personal details are visible.

## Step 1 @verifies ac-39

At https://katalon-demo-cura.herokuapp.com/, follow the observed Setup flow to reach the CURA Healthcare Service Make Appointment screen, then assert the heading reads Make Appointment.

## Step 2 @verifies ac-19, ac-34, ac-13

On the CURA Healthcare Service Make Appointment screen, expose navigation and inspect its Profile link, then assert the Profile action is available and its link destination is https://katalon-demo-cura.herokuapp.com/profile.php.

## Step 3 @verifies ac-22, ac-21

From Make Appointment, follow the Profile navigation action to its destination, then assert the browser URL is https://katalon-demo-cura.herokuapp.com/profile.php and the destination heading reads Profile.
