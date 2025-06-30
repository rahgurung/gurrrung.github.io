---
title: Introduction to React Native Navigation
readtime: 25
thumbnail: /Introduction-to-React-Native-Navigation/header.png
date: 2022-09-04 12:27:50
description: An easy to understand explanation to React Native Navigation with visual diagrams, code and output animations.
tags:
  - ReactJS
  - React Native
  - JavaScript
  - Mobile App Development
---

In this article, we are going to learn how we can implement navigation in React Native. We are going to use [<u>React Navigation</u>](https://reactnavigation.org/) which is the official library for adding routing and navigation to React Native applications. We are going to discuss the following types of Navigation and how to implement them:

* Stack Navigation
* Tab Navigation
* Drawer Navigation

### **Prerequisites**

* Basics of React

* [<u>React Navigation Setup</u>](https://reactnavigation.org/docs/getting-started)

{% asset_img stack_navigation_illustration.png "Stack Navigation Illustration'Stack Navigation Illustration'" %}

***Stack Navigation*** In this kind of navigation, a stack of screens is maintained. Each time we visit a screen, it is pushed to the stack, and pressing back will pop the item from the stack and show the last screen. A good example of this type of navigation is web pages where we navigate between different pages of a site and press back to move to the last page. Before we proceed, we need to add the library needed for Stack navigation.

{% codeblock lang:js %}
    npm install @react-navigation/native-stack
{% endcodeblock %}

### **How we can create screens?**

To add screens available to be navigated through in the application, we need to create a basic structure as given in the code example below which includes initializing the Stack navigator with `createNativeStackNavigator()` and using it inside the `NavigationContainer`. 

{% codeblock lang:js %}
  import * as React from 'react';
  import { Text } from 'react-native';

  // Imports related to navigation
  import {NavigationContainer} from '@react-navigation/native';
  import {createNativeStackNavigator} from '@react-navigation/native-stack';

  // Initialize the Stack navigator
  const Stack = createNativeStackNavigator();

  // We always surround navigator with NavigationContainer
  function StackNavigation() {
    return (
      <NavigationContainer>
        <Stack.Navigator>
          {/* Add your screens here */}
        </Stack.Navigator>
      </NavigationContainer>
    );
  }

  export default StackNavigation;
{% endcodeblock %}

Inside `<Stack.Navigator>` is the place where we need to place all our screens. Each screen goes with `<Stack.Screen>` tag and we will pass two props `name` and `component`, where name is the name of the component, make sure that it is unique and avoid using special characters and spaces and component is the component we want to add as the screen. Below given is a code with two screens added. By default, the screen which comes first inside `<Stack.Navigator>` will be visible first to the user or in other words it will be the screen that will be pushed there in the stack already, for example in the below code Screen1 will be visible.
>  **Tip:** We can also use `initialRouteName` prop on `Stack.Navigator` to specify the default screen too.

{% codeblock lang:js %}
import * as React from 'react';
import {Text} from 'react-native';
import {NavigationContainer} from '@react-navigation/native';
import {createNativeStackNavigator} from '@react-navigation/native-stack';

// Our Screens
function Screen1() {
  return <Text>Screen 1</Text>;
}

function Screen2() {
  return <Text>Screen 2</Text>;
}

// Initialize the Stack navigator
const Stack = createNativeStackNavigator();

function StackNavigation() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name="Screen1" component={Screen1} />
        <Stack.Screen name="Screen2" component={Screen2} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}

export default StackNavigation;
{% endcodeblock %}

The application right now looks like the image given below where we see our default screen (Screen1) and currently there is no way to navigate to Screen2. We will talk about how to do that in the below section.

{% asset_img screen1-visible-as-default-screen.png "Screen1 is visible as the default screen'Screen1 is visible as the default screen'" %}

### How we can travel between screens?

Now that we have two screens in our application. Let’s see how we can move between these screens or we can say how we can push new screens to the stack. React Navigation comes with a method to achieve this called `navigate()` which is available in the navigation prop, this prop is added automatically for each screen. In `navigate()` method, we just need to pass the name of the screen we want to navigate to. This name is the same name that we set in the previous section. Let’s see a code example, where we have a button in Screen1 to navigate to Screen2 and a button in the latter to navigate to the former.

{% codeblock lang:js %}

import * as React from 'react';
import {Text, Button} from 'react-native';
import {NavigationContainer} from '@react-navigation/native';
import {createNativeStackNavigator} from '@react-navigation/native-stack';

// Our Screens
function Screen1({navigation}) {
  return (
    <>
      <Text>Screen 1</Text>
      <Button
        title="Go to Screen 2"
        onPress={() => navigation.navigate('Screen2')}
      />
    </>
  );
}

function Screen2({navigation}) {
  return (
    <>
      <Text>Screen 2</Text>
      <Button
        title="Go to Screen 1"
        onPress={() => navigation.navigate('Screen1')}
      />
    </>
  );
}

// Initialize the Stack navigator
const Stack = createNativeStackNavigator();

function StackNavigation() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name="Screen1" component={Screen1} />
        <Stack.Screen name="Screen2" component={Screen2} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}

export default StackNavigation;
{% endcodeblock %}

{% asset_img navigation-animation.gif "Animation of navigation between screens'Animation of navigation between screens'" %}

You can see that clicking on Go to Screen 2 takes us to `Screen2` and further, there is a back button available in the upper left section in `Screen2` to go back. Go to Screen 1 button in `Screen2` takes us back to Screen1 and behaves the same as pressing the back button, this is being handled by React Navigation Library. Instead of pushing new `Screen1` to stack, it is looking back in the history of the stack to navigate to the required page if found. Further in a similar fashion, if the page to navigate to is in the stack already, it will keep popping the stack until it reaches that page. If your requirement is to create new screens and push to stack instead of looking back in the history stack, use `navigation.push("name")` instead of `navigation.navigate("name")` .

>  **Note:** React Navigation provides two types of Stack Navigator API, we are using [<u>Native Stack</u>](https://reactnavigation.org/docs/native-stack-navigator) instead of [<u>Stack</u>](https://reactnavigation.org/docs/stack-navigator/), because former uses native navigation primitives to provide better performance. If you want more customizability go with the latter.
<hr>

{% asset_img Tab-Navigation-Illustration.png "Tab Navigation Illustration'Tab Navigation Illustration'" %}

Another commonly used navigation style is having tabs on the page and navigating to and fro. Tabs can be either on the top of the screen or bottom. React navigation comes with three different APIs for tabs:

* [<u>Bottom Tabs</u>](https://reactnavigation.org/docs/bottom-tab-navigator) A simple tab bar on the bottom of the screen.

* [<u>Material Bottom Tabs</u>](https://reactnavigation.org/docs/material-bottom-tab-navigator) A material-design-themed tab bar on the bottom of the screen.

* [<u>Material Top Tabs</u>](https://reactnavigation.org/docs/material-top-tab-navigator) A material-design-themed tab bar on the top of the screen.

In this article, we are only going to look at Bottom Tabs to get an idea of how to use a tab navigator. Start with installing the required package for Bottom Tabs:

{% codeblock lang:js %}
    npm install @react-navigation/bottom-tabs
{% endcodeblock %}

Just like Stack Navigation, we have to follow a structure as given in the sample code below. We have to initialize the navigator using `createBottomTabNavigator()` and then use it inside `NavigationContainer` as given in the example, all the tab screens go inside `Tab.Navigator`.

{% codeblock lang:js %}

  import * as React from 'react';
  import { Text } from 'react-native';

  // Imports related to navigation
  import {NavigationContainer} from '@react-navigation/native';
  import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';

  // Initialize the Tab navigator
  const Tab = createBottomTabNavigator();

  // We always surround navigator with NavigationContainer
  function TabsNavigation() {
    return (
      <NavigationContainer>
        <Tab.Navigator>
          {/* Add your tab components here */}
        </Tab.Navigator>
      </NavigationContainer>
    );
  }

  export default TabsNavigation;
{% endcodeblock %}

Let’s create simple components for our tabs, `Tab1` and `Tab2` each of which needs to be inside `<Tab.Navigator>` element. We need to create each tab with Tab.Screen and pass name and component in a similar fashion as done in the Stack example.
>  You can also use `navigation.navigate()` method in a similar fashion as done in Stack Navigator.

{% codeblock lang:js %}

  import * as React from 'react';
  import {Text} from 'react-native';
  import {NavigationContainer} from '@react-navigation/native';
  import {createBottomTabNavigator} from '@react-navigation/bottom-tabs';

  // Our Tabs
  function Tab1() {
    return <Text>Tab 1</Text>;
  }

  function Tab2() {
    return <Text>Tab 2</Text>;
  }

  // Initialize the Tab navigator
  const Tab = createBottomTabNavigator();

  // We always surround navigator with NavigationContainer
  function TabsNavigation() {
    return (
      <NavigationContainer>
        <Tab.Navigator>
          <Tab.Screen name="Tab1" component={Tab1} />
          <Tab.Screen name="Tab2" component={Tab2} />
        </Tab.Navigator>
      </NavigationContainer>
    );
  }

  export default TabsNavigation;
{% endcodeblock %}

Let’s see an animation of how our project looks with current code changes. Clicking on tab names on the button should change the rendered component as given in the animation below.

{% asset_img tab-change-animation.gif "Animation of tab change'Animation of tab change'" %}

>  You can also customize the icon of each Tab using `tabBarIcon` key on the options prop on each `Tab.Screen`,or by passing `tabBarIcon` to screenOptions on `Tab.Navigator`, to keep code example simple I leave that exploration up to you.

{% asset_img drawer-navigation-illustation.png "Drawer Navigation'Drawer Navigation'" %}

**Drawer** can be said to be a special type of Tab where the buttons to change the tab live in a drawer on the left side (or right). To use a drawer in the project, start with installing the dependency.
{% codeblock lang:js %}
    npm install @react-navigation/drawer
{% endcodeblock %}
Further, to complete the setup follow the instructions given [here](https://reactnavigation.org/docs/drawer-navigator/#installation). It is advised to run pod install inside ios folder after doing all the dependencies installation.

Just like other navigators already discussed previously, Drawer also has a similar structure to be followed. We use `createDrawerNavigator()` to initialize the navigator and use it inside `<NavigationContainer>` as given in the code example below.

{% codeblock lang:js %}
  import * as React from 'react';
  import { Text } from 'react-native';

  // Imports related to navigation
  import {NavigationContainer} from '@react-navigation/native';
  import {createDrawerNavigator} from '@react-navigation/drawer';

  // Initialize the Drawer navigator
  const Drawer = createDrawerNavigator();

  // We always surround navigator with NavigationContainer
  function DrawerNavigation() {
    return (
      <NavigationContainer>
        <Drawer.Navigator>
          {/* Add your Drawer components here */}
        </Drawer.Navigator>
      </NavigationContainer>
    );
  }

  export default DrawerNavigation;
{% endcodeblock %}

In a similar fashion, we will use Drawer.Navigator to place all our different screen/Drawer components. Now let's see a full-fledged drawer with three screens. Each screen will go with `Drawer.Screen` with `name` and `component` props. On each screen, we also have a link to navigate between them, or alternatively, we can use the drawer to change screens. Below given are the code and the output.

{% codeblock lang:js %}

  import 'react-native-gesture-handler';
  import * as React from 'react';
  import {Text, Button} from 'react-native';
  import {NavigationContainer} from '@react-navigation/native';
  import {createDrawerNavigator} from '@react-navigation/drawer';

  // Our Screens
  function Screen1({navigation}) {
    return (
      <>
        <Text>Screen 1</Text>
        <Button
          title="Go to Screen 2"
          onPress={() => navigation.navigate('Screen2')}
        />
      </>
    );
  }

  function Screen2({navigation}) {
    return (
      <>
        <Text>Screen 2</Text>
        <Button
          title="Go to Screen 3"
          onPress={() => navigation.navigate('Screen3')}
        />
      </>
    );
  }

  function Screen3({navigation}) {
    return (
      <>
        <Text>Screen 3</Text>
        <Button
          title="Go to Screen 1"
          onPress={() => navigation.navigate('Screen1')}
        />
      </>
    );
  }

  // Initialize the Drawer navigator
  const Drawer = createDrawerNavigator();

  function DrawerNavigation() {
    return (
      <>
        <NavigationContainer>
          <Drawer.Navigator>
            <Drawer.Screen name="Screen1" component={Screen1} />
            <Drawer.Screen name="Screen2" component={Screen2} />
            <Drawer.Screen name="Screen3" component={Screen3} />
          </Drawer.Navigator>
        </NavigationContainer>
        <NavigationContainer />
      </>
    );
  }

  export default DrawerNavigation;
{% endcodeblock %}

{% asset_img drawer-navigation-animation.gif "Drawer Navigation animation'Drawer Navigation animation'" %}
