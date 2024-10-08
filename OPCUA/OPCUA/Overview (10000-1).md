*OPCUA* (Open Platform Communication Unified Architecture) specifies the following things:
- The **information model** which represents structure, behavior and semantics of systems. 
- The **message model** to interact between applications.
- The **communication model** to transfer data between end-points.
- The **conformance model** to guarantee interoperability between systems. 

## General 
*OPCUA* is a platform independent standard that allows many kinds of systems and devices to communicate via request/response **messages** between **clients** and **servers** or **Network-Messages** between **publishers** and **subscribers** over various types of networks. It supports robust, secure communication that assures the identity of *OPCUA* **Applications** to resist attacks. 


### Client Server Model
*OPCUA* defines a set of **services** that **servers** may provide. It is up the individual servers to specify to **clients** what service sets they support. 

Information is communicated using OPCUA-Defined and vendor-defined data types, and servers define **object models** that clients can dynamically discover. Servers can provide access to both current as well as historical data, in addition to **alarms** and **events** to notify clients of important changes. 

In addition to the Client Server model there also exists a **PubSub** model in which **Publishers** can push information to associated **Subscribers**. 

### OPCUA DESIGN GOALS
*OPCUA* provides a consistent, integrated **address space** and service model. This allows a single **server** to integrate data, **alarms**, **events** and history into it's **address space** and then provide access to them via its set of **services**. These **services** also include integrated **security model**. 

*OPCUA* allows **servers** to provide **clients** with type definitions of **objects** access from the **address space**. This allows information models to be used to describe the content of the **address space**. This data can be exposed in many different formats including binary structures and XML/JSON. 

Through the **address space** **clients** can query from the **server** for the metadata that describes the format of the data. In many cases, even if the **clients** have not been pre-programmed with knowledge of the data formats they will still be able to determine the formats at run time and properly utilise the data. 

*OPCUA* allows many **relationships** between **nodes** instead of being limited to just a single hierarchy. Thus, **servers** can present data in a variety of hierarchies tailored to the way a set of **clients** would typical want to view the data. 