## URI's 
URIs provide a syntax for constructing unique identifiers for resources that are used in different contexts within the OPCUA specification. There are three cases in which a URI is to be constructed:

### Namespace URI
Assigned by the creator of a **information model**. Typically of the format `tag:<authority-domain-name>,<yyyy-MM>:UA:<model-short-name>`
Where the `<authority-domain-name>` is the domain owned by the authority creating the **information model**. In many cases this will be `opcfoundation.org`. The `<yyyy-MM>` is the year and month the URI was created. 


## Object Model
The primary objective of the *OPCUA* **Address Space** is to provide a standardized way for **servers** to represent objects to **clients**. The *OPCUA* object model defines objects in terms of **variables** and **methods**. It also allows **relationships** to the other objects to be expressed. 


The elements of this model are represented as **nodes**. Each **Node** is assigned a **node-class** and each **node-class** represents a different element of the object model. 
![[OPCUA_Object_Model.png]]
## Node Model
Objects and their components are represented in the **Address Space** as a set of **Nodes** described by **attributes** and interconnected by **references**. 
![[OPCUA_AddressSpace_Node_Model.png]]
### NodeClasses
**NodeClasses** are defined in terms of the **Attributes** and **References** that shall instantiated (given values) when a **Node** is defined in the **address space**. 

> [!Warning] NodeClasses are fixed
> Servers and clients cannot define their own NodeClasses and a node may only be a class of one of the predefined types that exist within OPCUA (Aka the metadata of the addresspace itself)

### Attributes
**Attributes** are data elements that describe **nodes**. **Clients** can access **attribute** values using Read, Write, Query, and Subscription/MonitoredItem **Services**. 

**Attributes** are elementary components of **NodeClasses**. Their definitions are included as part of the **NodeClass** definition. They are not included as part of the address space. 

Each definition consists of an *attribute id*, a *name*, a *description*, a *data-type* and a *mandatory/optional indicator*. The set of **Attributes** for a **NodeClass** shall not be extended by a client or server. 

When a Node is instantiated in the **AddressSpace**, the values of the **NodeClass Attributes** are provided. The *mandatory/optional indicator* determines whether or not the attribute has to be instantiated. 

> [!Hint] Value(s) are attributes
> Some NodeClasses, such as the Variable Class can have values. Such as if there is a Variable NodeClass with a Boolean Data-Type, the value attribute can be True/False. 


### References 
**References** are used to relate nodes to each-other. They can be accesses using the **browsing** and **querying** **services**. 

Like **attributes*** they are defined as fundamental components of **Nodes**. Unlike **attributes** **references** are defined as instances of **ReferenceType Nodes** which are visible in the address space and are defined using the NodeClass of the same name. 

The node that contains the **Reference** is referred to as the *source node* and the node that is reference is referred to as the *target* node. The combination of the *source node*, *reference type* and *target node* are used in OPCUA services to uniquely identify references. Thus, each node can reference another **Node** of the same reference type only once. 

Any subtype of the reference types are considered to be equal to the base type. The *target node* of the **Reference** may be in the same or a different address space. *Target Nodes* located in other servers are identified in OPC UA services using a combination of the remote server name and the identifier assigned to the node by the remote server. 

OPCUA Does not require that the *target node* exists. 
