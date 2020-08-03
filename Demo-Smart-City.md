The Smart City demo illustrates a few imaginary applications in a single realm. It is part of the [online demo](https://openremote.io/demo/) as well as the project. It illustrates how the integration of several vertical solutions in a central platform, benefits a City as well as its Citizens:
* maintaining digital assets by exposing the data as assets and attributes, in a standardised manner, including acces rights.
* creating applications requiring the combination of assets, using WHEN-THEN rules or the FLOW editor, or creating dedicated front-end applications for desktop and mobile.

There are actually two realms which are part of the demo, once you have set it up yourself (See [Get Started](https://openremote.io/get-started-manager/)): the 'smartcity' realm and the 'master' realm for system manager (for configuration the project and remote management of multiple realms). 

# Demo Smart City

To view a few example applications we have added the Smart City realm to the project https://localhost/main/?realm=smartcity/ (Username: smartcity, Password: smartcity), which gives control over all areas. It's build using the OpenRemote UI components (for Developers see [Working on the UI](https://github.com/openremote/openremote/wiki/Developer-Guide%3A-Working-on-the-UI)).

If you just want to see how it looks before actually installing OpenRemote, see the [online Demo Smart City](https://openremote.io/demo/) and select your preferred language at the top right.

If you open the application on desktop you will get four main views: Map (Figure 1), Assets (Figure 2), Rules (Figure 3), and Insights. On mobile you will only see Map, and Assets.

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Smart%20City%20-%20Demo%20Map.png)
_Figure 1. The Map view of the Demo Smart City, showing the map with different attributes across the City_

## Map and Assets

Both the Map and Asset view give you access to the respective Assets and related attributes. You can see and control them, but also have the option to look at the historical data via graphs or event lists.

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Smart%20City%20-%20Demo%20Assets.png)
_Figure 2. The Asset view of the Demo Smart City, showing an EV charger with the different attributes_

## Rules

The Rules view (only visible in the desktop version, see figure 3) allows you to build two types of rules:
* WHEN-THEN: allows you to set Left Hand Side conditions of available attributes, and trigger a Right Hand Side action for another attribute.
* FLOW: allows for processing attributes and converting them into new attributes. 

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20WHEN-THEN_2.png)
_Figure 3. The Rules view of the Demo Smart City, showing showing an imaginary example which sends an e-mail when the Ozone levels are increasing while water levels are too high._

# Application for System Manager

The application for the System Manager requires you to set up the demo environment locally first. Follow the [Get Started](https://openremote.io/get-started-manager/). Next, you can access both the Application for System Manager and the Smart City Demo via your browser (if you are using Docker Toolbox replace localhost with 192.168.99.100):

Application for System Manager: https://localhost/main/?realm=master/ (Username: admin, Password: secret)

Application for Demo Smart City: https://localhost/main/?realm=smartcity/ (Username: smartcity, Password: smartcity)

If you open the Application for the System Manager, you will see two Realms in the upper right realm-picker: 'master', and 'smartcity'. The [Demo Smart City](#demo-smart-city) refers to the 'smartcity'. If you select the 'smartcity' realm in the System Manager, you will notice that you see exactly the same attributes as visible in the [Demo Smart City](#demo-smart-city). 

## Groovy rules and Global rules

In addition to the WHEN-THEN and FLOW rules, when logged into the master with the admin privileges, there is a third type 'GROOVY' which allows you to program more complex logic.

You will also notice that you can switch between realm rules and global rules. The global rules allow you to define rules which can use attributes across the different realms, as opposed to the realm rules which will only work within the selected realm. 

See the documentation on our wiki for more details.

# See Also
- [Next 'Get Started' step: Working on the UI](Developer-Guide%3A-Working-on-the-UI)
- [Get Started](https://openremote.io/get-started-manager/)
- [OpenRemote Wiki](https://github.com/openremote/openremote/wiki)