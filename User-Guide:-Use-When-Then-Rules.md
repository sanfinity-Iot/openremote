When-Then rules are used in combination with the or-rules UI component. They are meant to be used to allow application users to define 'When this, then this' statements. For example "turn on a light when it's clouded" or "send push notification to anybody who reaches the stadium".
![Manager Rules Editor](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20When-Then%20Editor.png)

# Guide to setting up your first When-Then rule
To get familiar with the When-Then interface we will be creating a rule that turns on lights when the temperature drops below a certain threshold or when the humidity is higher than 75 (makes sense right?). We only want this rule to be active on friday evening.
This guide assumes you have followed the [Quickstart](https://github.com/openremote/openremote/blob/master/README.md) and have the platform currently running.

1. Switch from the `master` realm to the `Smart city` realm in the top. You will see the assets we use a demo setup. 
2. Navigate to the `Rules` page, here you will find some demo rules that are running at the moment. We will add our own.
3. Add a new rule:
   * Click the `+`
   * Select 'When-Then'
   * Name it `Weather: control lights`
4. Create the When side of the rule:
   * Click the `+`, a list of asset types will appear
   * Select `Weather Asset`, the next will appear showing `Any of this type`
   * Switch to the asset named `Weather`
   * In the next field select the attribute of interest: `temperature`
   * In the next field select the operator: `less than`
   * Finally set a value of `10`. You have now finished the first condition. This can be combined through `AND` or `OR` with other conditions.
5. Create a second condition to monitor the humidity:
   * Click the `+` in the section below where it says `Or when` and select `Weather`
   * Switch to `Weather`
   * Select: `humidity`
   * Select: `greater than`
   * Set a value of `75`. You have finished the OR condition. If this or the previous condition is met, the `Then` actions will trigger.
6. Create an action on the `Then` side of the rule:
   * Click the `+` on the right in the `Then` section
   * Select the `Light Asset` asset type.
   * Select the lights that you want to control. In this case we want to turn on all lights at once using the `Lighting Noordereiland`.
   * Select the attribute you want to control: `On Off`
   * Toggle the switch on. The Actions are done.
7. Set the schedule for this rule:
   * Open up the scheduler by clicking `Always active` next to the rules name.
   * Select 'Plan a repeating occurrence' as we want the rule to work every friday evening
   * Deselect `all day` and use the calendar picker of the `Start date` to select the upcoming friday, and set the time to `18:00`
   * For the end date make sure you have the same date selected and set the time to `23:00`. Your first friday evening is planned.
   * At `Repeat occurrence every` click `Friday`
   * Leave the `Repetition ends` at `Never`
   * Click `Apply` and save the rule in the top right.

Your rule is finished! Every Friday evening the rule will check if one of the two conditions is met and if so, turns on the lights of Noordereiland. 

# See Also

- [[Create Rules|User-Guide:-Create Rules]]
- [[Create Groovy Rules|User-Guide:-Create Rules with Groovy Editor]]
- [[Use Flow Rules|User-Guide:-Use Flow Rules]]
- [[Manager UI Guide|User-Guide:-Manager-UI]]