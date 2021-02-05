# Exploring the Firebase Remote Config

Firebase remote config provides you handy out of the box solution for your app remote configuration.
In simple words, Firebase remote config:
- is a cloud service placed under "developing and testing" firebase product category
- provides changing behavior and appearance of your application, without requiring app build publication (or store permission) or users to download an app update
- can be managed from Firebase Console or Remote Config REST API

## Why is it required?

Being a successful app developer usually means making frequent changes to your app. Sometimes, these changes are new features or bug fixes. But sometimes, the most impactful updates are one-line changes to your code, like adjusting a line of text or color of buttons.

Although these kinds of changes are easy to make, publishing them is **still a multi-day process**.
- Make code changes (text, colors, and other behavior)
- Create app build
- Test them thropughly
- Publish app to store
- Wait for store verification
- Wait for user to update the app


## Remote Config at rescue

Wouldn’t it be nice if you could make some of these tweaks without having to go through that whole process?

- **App defaults updation**

    Change your application’s defaults without republishing new version or waiting for user to update the app.
    ![How parameter values are prioritized in the Remote Config](../docs/images/firebase_remote_config_process.png?raw=true "Title")

    - Remote Config is a server-side stored JSON file which supports the following data types:
        - String
        - Number
        - Bool

    - Provides support for Android, iOS, Web, C++, Unity and backend APIs

    - Ability to override in-app default values by defining server-side parameter values 



- **Deliver the Right Content to the Right People**

    Customize your application appearances for different user segmentation by making use of conditions, which use targeting rules to deliver specific values for different users.

    ![How parameter values are prioritized in the Remote Config](../docs/images/firebase_remote_config_segmentation.png?raw=true "Title")![How parameter values are prioritized in the Remote Config](../docs/images/firebase_remote_config_conditional.png?raw=true "Title")

    - Firebase Remote Config allows maintaining multiple remote config values for a single  parameter based on different conditions:
        - Operating System
        - App Version
        - Region
        - Language
        - Custom User/Audience

    <!----
    You can can also deliver different values based on audiences with defaults values like free/paid user or according to user properties of Google Analytics for Firebase.
    -->

    - Using Firebase Remote Config with Google Analytics enables use of Analytics audiences and user properties to customize app for segments of user base with flexibility and precision

    - Firebase Remote Config with prediction provides ability to create parameter with values depending on a user’s predicted behavior

- **Support for experimentation:**

    Ability to improve your application with A/B testings (gauge the impact of making certain changes without affecting your app's conversion rate).

    ![How parameter values are prioritized in the Remote Config](../docs/images/firebase_remote_config_testing.png?raw=true "Title")![How parameter values are prioritized in the Remote Config](../docs/images/firebase_remote_config_rollouts.png?raw=true "Title")

    - Firebase Remote Config works with A/B Testing allowing user to setup and run experiments to test changes to your app

    - Rather than rolling out a specific version in stages (like Google Play will let you) you may want to roll out certain features

    - Create a flag for a feature, implement the proper feature visibility in the app

    - Gauge the percentile as you see seamless adoption

    - Get rid of the conditional once you’re happy with the rollout


## How Does It Work?

Remote Config includes a **client library** that handles important tasks like **fetching parameter values** and **caching them**, while still giving us control over when new values are activated or fetched so they affect our app’s user experience.
<!-- This lets us safeguard our app experience by controlling the timing of any changes. -->

**Client Side or In App:**
- Remote Config client library `get` method provide a single access point for parameter values
- Our app gets server-side values using the same logic it uses to get in-app default values, so we can add the capabilities of Remote Config to our app without writing a lot of code

**Server Side or In Firebase console:**
- To override in-app default values, use the Firebase console or the Remote Config back-end APIs
    - To create parameters with the same names as the parameters used in your app
    - For each parameter set a server-side default value to override the in-app default value
    - User can also create conditional values to override the in-app default value for app instances that meet certain conditions

This image below shows how parameter values are prioritized in the Remote Config back end and in our app:

![How parameter values are prioritized in the service and your app](../docs/images/firebase_remote_config_priority.png?raw=true "Title")

From the above graph we can see two sides:

- **Server-side:** First of all, the server will look at current values if were set. Then he gets them by the highest priority rule, otherwise, the server will return the default value.
- **Client-side:** If the client gets the values back from the server will use these values, in different circumstances, will use the default values.

Basically, Firebase Remote Config’s working strategy depends on three main terms: Parameters, Conditions and Rules.

- Parameters are basic key-value pairs to control your app-defaults
- Conditions can have one or multiple rules and is used to target group of application instances
- Rules are used to trigger a condition and create different application instances.

## Understanding the usecase

### Switch for a crisis time
### Setting configuration parameters, hosts, and URLs
### Easy roll-back of a buggy feature
### Different values for different conditions for the A/B test
### Pass any JSON object like translations strings, etc

## Advantages
- Completely free out-of-the-box solution
- Supports multiple platforms (Android/iOS/WEB)
- Easy to setup
- Convenient console
- Once fetched, the config is being cached in persistent storage on the device
- Supports setting default values
- Can easily be used for A/B tests (check it out in my next article)
- Very flexible in settings different values for different conditions (by build type, country, user’s language, app version, etc.)

## Limitations
- **It’s not intended to be used in real-time**
If you change a value in the console — it will take some time for your app to update the config. We will discuss later on how to decrease it or to make it even real-time

- **It may not be available right away** (you need to set the default values and stick to certain loading strategy)

- **It may not be available at all in some countries** such as China, where Google services are very limited.

- **Is not suitable for storing confidential data** because it’s possible to decode any parameter keys or values stored.

- **You can have up to 2000 parameters**
Parameter keys can be up to 256 characters long, must start with an underscore or (A-Z, a-z), and may also include numbers. The total length of parameter value strings within a project cannot exceed 800,000 characters.

- **You may have up to 500 conditions**

<!-- DON’T use Remote Config with user authantication to make update content>
<! -- DON’T change the requirements of application’s platform.>

