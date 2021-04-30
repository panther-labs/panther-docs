# Unit Test Mocking

{% hint style="info" %}
This feature is available in Panther version 1.17
{% endhint %}

Panther features a simple framework to allow basic mocking for unit tests. 

When writing a detection that requires an external API call, mocks can be utilized to mimic the server response in the unit tests without having to actually execute an API call. 

Mocks are defined by a given `Mock Name` and `Return Value` which respectively denote the name of the object to patch and the `string` to be returned when the patched object is invoked.

Added mocks are defined on the unit test level allowing users to define different mocks for each unit test.  
The section to add mocks to a unit test are located directly underneath the unit test resource definition

![](../.gitbook/assets/image%20%282%29.png)

Note: Mocks are allowed on the **global** and **built-in** python namespaces. This includes things such as:

1. Imported Modules and Functions
2. Global Variables
3. Built-in Python Functions
4. User-Defined Functions

