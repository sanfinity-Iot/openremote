Groovy is a programming language that can seamlessly work together with Java. Groovy rules are written in Groovy, and so the most freedom compared to other rule types. Groovy rules can interact with a number of objects that are exposed to the scripting engine.

These rules are capable of everything OpenRemote allows rules to do and are intended for rules that are too complex to be defined in other editors. Users who are technically literate may prefer to write rules in Groovy regardless of rule complexity.

Groovy rules can be created in the technical manager and need to be written manually.

# How do Groovy rules work?

Groovy rules work using a Groovy scripting engine. Rules are parsed and various objects are bound:

```LOG``` provides basic logging functionality using ```java.util.logging.Logger``` .

```rules``` provides access to the ruleset. This is how individual rules are added to the ruleset. [Relevant class in source.](https://github.com/openremote/openremote/blob/master/manager/src/main/java/org/openremote/manager/rules/RulesBuilder.java)

```assets``` provides access to relevant assets. [Relevant class in source.](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/rules/Assets.java)

```users``` provides access to relevant users. [Relevant class in source.](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/rules/Users.java)

```notifications``` provides the ability to send notifications. [Relevant class in source.](https://github.com/openremote/openremote/blob/master/manager/src/main/java/org/openremote/manager/rules/facade/NotificationsFacade.java)

The rules are then registered, ordering rules from top to bottom within a ruleset.

# See Also

- [[Create Rules|User-Guide:-Create Rules]]
- [[Create Javascript Rules|User-Guide:-Create Rules with Javascript Editor]]
- [[Use JSON Rules|User-Guide:-Use JSON Rules]]
- [[Use Flow Rules|User-Guide:-Use Flow Rules]]
- [Demo Smart City](Demo-Smart-City)
- [Demo Smart Building](Demo-Smart-Building)
- [Get Started](https://openremote.io/get-started-manager/)