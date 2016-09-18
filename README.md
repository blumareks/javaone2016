# javaone2016
Android and IBM Watson  - a short lab on making your app cognitive with sentiment analysis

## Overview

This lab shows you how to build a mobile application that will analyze tone, or sentiment, in text. You’ll build the application in Java/Android and use the IBM Watson AlchemyAPI service to analyze sentiment conveyed in text. 

To create the application in this lab, follow these main steps:
- Create a typical Android application in Java.
- Install the Watson SDK for Java.
- Create the Bluemix Watson service and get the key token to it.
- Add some code in your Android app to invoke the cognitive service.

## Prerequisites 

You need the following software:
- Android studio

## Step 1. Create a typical application in Android
You’ll build a simple application with a submit button, an editable text field, and an output field.
Step 1a. Create the GUI
First, create a simple single view application with a graphical user interface (GUI) that includes a text field, a label, and a button. When a user presses the button, the text in the text field is to be sent to Watson (that would be implemented in the next steps), which analyzes it and returns an opinion that is shown in the output field.

For now you would take the text in the text field and simply echo it in the label field when the button is pressed.

