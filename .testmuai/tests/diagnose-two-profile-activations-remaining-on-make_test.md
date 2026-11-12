---
assurance:
  id: t-25
  base: sha256:290b1af4b7341063458008be386f35432be1fce8f792b16d4169a701b6d09cc0
---
# Diagnose two Profile activations remaining on Make Appointment

> Reproduce the reported Make Appointment flow-break with two Profile activations; inspect whether the appointment screen stays unchanged, as a diagnostic observation rather than a guaranteed service contract.

## Step 1 @verifies ac-39

At https://katalon-demo-cura.herokuapp.com/, follow the observed Setup flow to reach the CURA Healthcare Service Make Appointment screen, then assert the heading reads Make Appointment.

## Step 2 @verifies ac-19, ac-34, ac-13

On the CURA Healthcare Service Make Appointment screen, expose navigation and inspect its Profile link, then assert the Profile action is available and its link destination is https://katalon-demo-cura.herokuapp.com/profile.php.

## Step 3 @verifies ac-20

From Make Appointment, activate Profile twice, inspecting the displayed screen after each activation and returning to Make Appointment between attempts if navigation occurs, then assert exactly 2 of the 2 Profile activations left the Make Appointment screen unchanged.
