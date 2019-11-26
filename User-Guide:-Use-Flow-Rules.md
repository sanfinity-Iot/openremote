Flow rules are mainly intended for attribute linking and defining virtual attributes, though they can also function as regular rules. They're useful for users who don't have the time to learn a programming language, but still want to define rules more complex than is possible in the JSON rule editor.

The Flow Editor UI is currently a standalone application that interacts with the Manager. It has its own [repository](../../../floweditor).

# The Flow rules model

## Node collections

A Flow rule is an object that is stored in JSON and interpreted by the Manager. Flow rules are referred to as node collections in the back end. These are collections of connected nodes that (generally) receive, process, and then output information. A node collection can be named and given a description, just like other rule types.

## Nodes and connections

A node is an entity that receives, manipulates, or outputs data via its sockets. Nodes are split into three categories.

**Input** nodes receive information from somewhere else to send it to other nodes in the collection. An example of an input node is a text node, that just outputs text that is put in by the user. A node that outputs asset attribute data is also an input node.

**Processor**s receive information from other nodes, process it, then output it to other nodes. These nodes generally don't interact with anything outside the Flow editor. An example of a processor is a math node that outputs the sum of its inputs.

**Output** nodes receive information from other nodes and send it somewhere else. Generally, output nodes send information to the Manager. An example of this is a node that sets an asset attribute to a specific value.

Connections are laid by the user from socket to socket, allowing data to flow through. Connections define the interactions between each individual node, and so define the behaviour of the entire rule.

## Execution

Node collections are sent to the manager and converted into a rule. All rules have a condition and an action. The action is executed when the condition is met. A flow rule executes only when the value of an asset attribute on the LHS of the rule has been changed. However, not all input nodes are included in the LHS of a flow rule.

A flow rule is generated backwards. The system goes through every output node and traverses down the node tree, marking every asset attribute input node it comes across. For every output node, it constructs a single rule. Input nodes without attached output nodes aren't considered, neither are output nodes that aren't connected to input nodes. Detached processor nodes are ignored as well.

## User interface

The user interface is a separate project that isn't tied into the back end and only communicates with it. See [the repository](../../../floweditor/blob/master/README.md) for more information.

# See Also

- [Use the Flow Editor](../../../floweditor/blob/master/README.md)
- [[Create Rules|User-Guide:-Create Rules]]
- [[Create Groovy Rules|User-Guide:-Create Rules with Groovy Editor]]
- [[Create Javascript Rules|User-Guide:-Create Rules with Javascript Editor]]
- [[Use JSON Rules|User-Guide:-Use JSON Rules]]
- [Demo Smart City](Demo-Smart-City)
- [Demo Smart Building](Demo-Smart-Building)
- [Get Started](https://openremote.io/get-started-manager/)