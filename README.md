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

### Building UI

You start with the New Project.
Give the app name, and enter your company domain name.
Choose the environment the app would run on (I select API 22 - Android 5.1 Lollipop).
I will add an empty activity.
I would keep the defaults for the activity.
Wait till gradle builds the environment.
When it is done select [res] and [layout] and double click on [activity_main.xml]
When the UI designer appears you might want to select the TextView widget on the simulator - the selected text shows "Hello World!".
Let's add PlainText field to enter text to be analyzed by Watson Sentiment Analysis service.
Double click on the widget to add a value (some test text).
And also let's add the Button to fire an action.
Double click on the widget to add a value (a text caption on the button ).

### Wiring UI with the code

Get back to the code by selecting [java], your appName (with your company domain name) and [MainActivity] - you need to double click the activity

Add necessary parameters just after the import section (first thing in the class definition): textField, editField, button. It should generate the adequate imports:

```android
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity {

    TextView textView;
    EditText editText;
    Button button;
    String sentiment;
```

Then you need to initialize the view and the UI parameters.

```android

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        //initialize UI parameters
        textView = (TextView) findViewById(R.id.textView);
        editText = (EditText) findViewById(R.id.editText);
        button = (Button) findViewById(R.id.button);
```

Then fire an action (through onClickListener) when the button is pressed.
```android
 @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        //initialize UI parameters
        textView = (TextView) findViewById(R.id.textView);
        editText = (EditText) findViewById(R.id.editText);
        button = (Button) findViewById(R.id.button);

        //fire action when button is pressed
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                System.out.println("Logging to the console that the button pressed for the text : " + editText.getText());
                textView.setText("Displaying at UI the sentiment to be checked for : " + editText.getText());

                AskWatsonTask task = new AskWatsonTask();
                task.execute(new String[]{});
            }

        });


    }
```

Now we need to add the fake AskWatson task that manages the thread for listener.
The text needs to be added just before the onCreate method.

```android

private class AskWatsonTask extends AsyncTask<String, Void, String> {
        @Override
        protected String doInBackground(String... textsToAnalyse) {

            System.out.println(editText.getText());

            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    textView.setText("what is happening inside a thread - we are running Watson AlchemyAPI");
                }
            });
            sentiment = "Test sentiment";
            System.out.println(sentiment);

            //passing the result to be displayed at UI in the main tread
            return sentiment;
        }

        //setting the value of UI outside of the thread
        @Override
        protected void onPostExecute(String result) {
            textView.setText("The message's sentiment is: " + result);
        }
    }
 ````

Now we are ready to run the simulator - press the green "PLAY" arrow next to the "APP"
If you click [Analyze!] button you should be able to see the result at the [TextView] field.

## Step 2: Add the Watson SDK to your project

Check the detaily described steps here at (Watson Developer Cloud SDK for JAVA)[https://github.com/watson-developer-cloud/java-sdk]

Alternatively (download the java library for the SDK from here)[https://github.com/watson-developer-cloud/java-sdk/releases/download/java-sdk-3.3.1/java-sdk-3.3.1-jar-with-dependencies.jar] and place it in the lib folder to get in included for the environment.

In order to make it work place the downloaded jar file in the libs folder under the app root directory in apps/libs:

You should be able to see the app in the libs folder in the projects view in the Android Studio.
Then double click on the jar to add it to the [app] as the library:

Finally we are able to import Watson libraries:

Now we need to allow to call Watson service from our app.
Double click /app/manifest/AndroidManifest.xml in the view [Android]:

Add the following under <manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.ibm.sentimentsensitiveapp">
```xml
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

The entire look of the xml file:


### Step 2a. Create a Watson service and get the key token for it
Now, you create the Watson AlchemyAPI service. 
From the IBM Bluemix catalog, click Watson > AlchemyAPI > Create. Be sure to use a static API key as shown in the following image.
Find the Service Credentials.

Bluemix provides your credentials in JSON format. The JSON snippet lists credentials, such as the API key and secret, and connection information for the service. 



