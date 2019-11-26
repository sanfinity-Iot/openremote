Flow rules are mainly intended for attribute linking and defining virtual attributes, though they can also function as regular rules. They're useful for users who don't have the time to learn a programming language, but still want to define rules more complex than is possible in the JSON rule editor.

The Flow Editor is currently a standalone application that interacts with the Manager. It has its own [repository](../../../floweditor).

# The Flow rules model

## Node collections

A Flow rule is an object that is stored in JSON and interpreted by the Manager. Flow rules are referred to as node collections in the back end. These are collections of connected nodes that (generally) receive, process, and then output information. A node collection can be named and given a description, just like other rule types.

## Nodes and connections

A node is an entity that receives, manipulates, or outputs data via its sockets. Nodes are split into three categories.

**Input** nodes receive information from somewhere else to send it to other nodes in the collection. An example of an input node is a text node, that just outputs text that is put in by the user. A node that outputs asset attribute data is also an input node.

**Processor**s receive information from other nodes, process it, then output it to other nodes. These nodes generally don't interact with anything outside the Flow editor. An example of a processor is a math node that outputs the sum of its inputs.

**Output** nodes receive information from other nodes and send it somewhere else. Generally, output nodes send information to the Manager. An example of this is a node that sets an asset attribute to a specific value.

A node collection needs connections for it to do anything. Connections are laid by the user from socket to socket, allowing data to flow through. Connections define the interactions between each individual node, and so define the behaviour of the entire rule.

How is this processed?
Describe the structure of the Flow rules.
Refer to separate Wiki 'Use Flows' for describing the front-end application.

# See Also

- Use the Flow Editor
- [[Create Rules|User-Guide:-Create Rules]]
- [[Create Groovy Rules|User-Guide:-Create Rules with Groovy Editor]]
- [[Create Javascript Rules|User-Guide:-Create Rules with Javascript Editor]]
- [[Use JSON Rules|User-Guide:-Use JSON Rules]]
- [Demo Smart City](Demo-Smart-City)
- [Demo Smart Building](Demo-Smart-Building)
- [Get Started](https://openremote.io/get-started-manager/)