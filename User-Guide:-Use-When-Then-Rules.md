When-Then rules are used in combination with the or-rules UI component. They are meant to be used to allow application users to define WHEN THIS, THAN THAT statements. For example "turn on a light when it's clouded" or "send push notification to anybody who reaches the stadium".
![Manager Rules Editor](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20When-Then%20Editor.png)

# Guide to setting up your first When-Then rule
To get familiar with the When-Then interface we will be creating a rule that turns on lights when the temperature drops below a certain threshold or when the humidity is higher than 75 (makes sense right?).
This guide assumes you have followed the [Quickstart](https://github.com/openremote/openremote/blob/master/README.md)and have the platform currently running.

1. Switch from the `master` realm to the `Smart city` realm in the top. You will see the assets we use a demo setup. 
2. Navigate to the `Rules` page, here you will find some demo rules that are running at the moment. We will add our own.
3. Add a new rule:
   * Click the `+`
   * Select 'When-Then'
   * Name it `Weather: control lights`
4. Create the When side of the rule:
   *


# See Also

- [[Create Rules|User-Guide:-Create Rules]]
- [[Create Groovy Rules|User-Guide:-Create Rules with Groovy Editor]]
- [[Create Javascript Rules|User-Guide:-Create Rules with Javascript Editor]]
- [[Use Flow Rules|User-Guide:-Use Flow Rules]]
- [Demo Smart City](Demo-Smart-City)
- [Get Started](https://openremote.io/get-started-manager/)