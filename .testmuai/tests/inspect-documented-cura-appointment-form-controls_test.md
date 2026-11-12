---
assurance:
  id: t-2
  base: sha256:cd75cb2b988746cedf38478786b901d8f4f72ce2ef7cd0f0ed095c3a0e3dac68
---
# Inspect documented CURA appointment form controls

> From the explored web entry point, reach Make Appointment and observe the documented form fields, coverage choices, facility option and booking control without assuming an unobserved booking outcome.

## Step 1 @verifies ac-3, ac-4

At https://katalon-demo-cura.herokuapp.com/, reach the CURA Healthcare Service Make Appointment form through its documented setup flow, using {{login_username}} and {{login_password}} if sign-in is needed; then assert the form shows the Visit Date textbox labeled "Visit Date (Required)" and a Book Appointment button.

## Step 2 @verifies ac-5

On the CURA Healthcare Service Make Appointment form, inspect the Facility selector; then assert it offers Tokyo CURA Healthcare Center.

## Step 3 @verifies ac-6

On the CURA Healthcare Service Make Appointment form, inspect the readmission control; then assert an Apply for hospital readmission checkbox is present.

## Step 4 @verifies ac-7, ac-8, ac-9

On the CURA Healthcare Service Make Appointment form, inspect the coverage radios; then assert the None, Medicaid, and Medicare radio choices are present.

## Step 5 @verifies ac-10

On the CURA Healthcare Service Make Appointment form, inspect the comment field; then assert the Comment textbox is present.
