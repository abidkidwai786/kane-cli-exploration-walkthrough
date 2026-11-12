---
assurance:
  id: t-12
  base: sha256:03f492a41626104ad8b6427c4f4183a78a5831996fa49d141c241cea50d9353c
---
# Appointment Confirmation exposes History link and documented target

> Reach the observed Appointment Confirmation screen via its verified submission flow and inspect the History navigation link and documented history.php target; do not infer the result of activating it.

## Step 1

At https://katalon-demo-cura.herokuapp.com/, use {{setup_credentials}} to follow the CURA Healthcare Service Setup flow and reach Make Appointment.

## Step 2

On the CURA Healthcare Service Make Appointment screen, use {{appointment_details}} to complete the documented Submit appointment flow and reach Appointment Confirmation.

## Step 3 @verifies ac-29

On the CURA Healthcare Service Appointment Confirmation screen, open its navigation and locate History, then assert a History link is present in that navigation.

## Step 4 @verifies ac-30

On the Appointment Confirmation screen, inspect the navigation History link's browser DOM destination without following it, then assert its href equals https://katalon-demo-cura.herokuapp.com/history.php.
