OpenRemote has its own rules engine which can be used for a wide range of functions, from simple linking of attributes, to complex rules based algorithms for eg. predictive models. There are 4 different types of rules scripting languages, all with their own purpose. We'll describe them here and refer to a dedicated Wiki for each type.

Rules can be build within global rulesets, allowing them to use any attribute across the different realms. Alternatively, if you select a specific realm or asset, the ruleset will only be able to use rules with attributes within the asset, or any of it child assets.

Rules can be enabled or disabled. In addition, there is an option 'Continue on error' which prevents the rules engine stopping in case one of the rules in the rule set is creating an error.

# Groovy rules

Groovy rules scripting language is available within the (Technical) manager and can be used for complex rules. They have the most flexibility but also need clear understanding of the Groovy language especially to prevent errors.

# Java script rules

t.b.d.

# JSON rules

The JSON rules object model is intended for application users, to create workflow rules, using a front-end UI. It works with the or-rules UI component which handles the connection between front-end and the JSOn rules. An example of the JSON rules working with the front-end is part of the [Demo Smart Building](https://github.com/openremote/openremote/wiki/Demo-Smart-Building) and [Demo Smart City](https://github.com/openremote/openremote/wiki/Demo-Smart-City).

# Flow rules

The Flow rules model is intended for application users which want to link attributes together, using a conversion. Its main purpose is

# See Also

- [[Create Groovy Rules|User-Guide:-Create Rules with Groovy Editor]]
- [[Create Javascript Rules|User-Guide:-Create Rules with Javascript Editor]]
- [[Use JSON Rules|User-Guide:-Use JSON Rules]]
- [[Use Flow Rules|User-Guide:-Use Flow Rules]]
- [Demo Smart City](Demo-Smart-City)
- [Demo Smart Building](Demo-Smart-Building)
- [Get Started](https://openremote.io/get-started-manager/)