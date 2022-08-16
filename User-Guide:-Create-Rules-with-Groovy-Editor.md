Groovy is a programming language that can seamlessly work together with Java. Groovy rules are written in Groovy, and so offer the most freedom compared to other rule types. Groovy rules can interact with a number of objects that are exposed to the scripting engine.

These rules are capable of everything OpenRemote allows rules to do and are intended for rules that are too complex to be defined in other editors. Users who have experience in programming may prefer to write rules in Groovy regardless of rule complexity.

Groovy rules can be created in the technical manager and need to be written manually.

# How do Groovy rules work?

Groovy rules work using a Groovy scripting engine. Rules are parsed and various objects are bound:

```LOG``` provides basic logging functionality using ```java.util.logging.Logger``` .

```rules``` provides access to the ruleset. This is how individual rules are added to the ruleset. [Relevant class in source.](https://github.com/openremote/openremote/blob/master/manager/src/main/java/org/openremote/manager/rules/RulesBuilder.java)

```assets``` provides access to relevant assets. [Relevant class in source.](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/rules/Assets.java)

```users``` provides access to relevant users. [Relevant class in source.](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/rules/Users.java)

```notifications``` provides the ability to send notifications. [Relevant class in source.](https://github.com/openremote/openremote/blob/master/manager/src/main/java/org/openremote/manager/rules/facade/NotificationsFacade.java)

The rules are then registered, ordered from top to bottom within a ruleset.

# Example Groovy rules

Here are a few examples for your reference:
## Control a Group
By adding this [Group Control Groovy Rule ](https://github.com/openremote/openremote/blob/master/test/src/test/resources/org/openremote/test/rules/ChildAssetControl.groovy) for each group asset, you can control all these child assets at the same time, just by creating the corresponding attributes of the children (same name and type) in the parent asset. Take an example of controlling all lights in a building: by creating an 'onOff' (boolean) attribute in the building asset you can control all lights, which are children of that building asset, at the same time. Note that you create a [Control Groovy rule](https://github.com/openremote/openremote/blob/master/test/src/test/resources/org/openremote/test/rules/ChildAssetControl.groovy) for each parent asset and adjust the parentAssetID to the ID of the parent asset. You can find the ID in the URL of the asset page. Additionally, don't forget to add the configuration item 'Rule state' to the attribute of both the parent and child assets.

# See Also

- [[Create Rules|User-Guide:-Create Rules]]
- [[Use WHEN-THEN Rules|User-Guide:-Use WHEN-THEN Rules]]
- [[Use Flow Rules|User-Guide:-Use Flow Rules]]
- [[Manager UI Guide|User-Guide:-Manager-UI]]