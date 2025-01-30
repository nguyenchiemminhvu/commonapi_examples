- [Introduction](#introduction)
- [Key Features](#key-features)
- [Architecture](#architecture)
- [Installation](#installation)
- [Workflow In Programming](#workflow-in-programming)
- [Franca IDL](#franca-idl)
  - [Key Concepts](#key-concepts)
    - [Interfaces](#interfaces)
    - [Methods](#methods)
    - [Broadcasts](#broadcasts)
    - [Attributes](#attributes)
  - [FIDL Data Types](#fidl-data-types)
    - [Primitive Data Types](#primitive-data-types)
    - [Complex Data Types](#complex-data-types)
- [Sample Codes](#sample-codes)
  - [Sync and Async APIs](#sync-and-async-apis)
    - [Define the Interface (CommonAPI IDL)](#define-the-interface-commonapi-idl)
    - [Generate Code](#generate-code)
    - [Implement Server](#implement-server)
    - [Implement Client](#implement-client)
    - [Server Main Loop](#server-main-loop)
    - [Compile And Run](#compile-and-run)
    - [Sample Output](#sample-output)
  - [Broadcast API](#broadcast-api)
    - [Define the Interface (CommonAPI IDL)](#define-the-interface-commonapi-idl-1)
    - [Generate Code](#generate-code-1)
    - [Implement Server](#implement-server-1)
    - [Implement Client](#implement-client-1)
    - [Server Main Loop](#server-main-loop-1)
    - [Compile And Run](#compile-and-run-1)
    - [Sample Output](#sample-output-1)
  - [SOME/IP Binding](#someip-binding)
    - [Define the Interface (CommonAPI IDL)](#define-the-interface-commonapi-idl-2)
    - [Generate Code](#generate-code-2)
    - [Configure SOME/IP Binding](#configure-someip-binding)
    - [Implement Server](#implement-server-2)
    - [Implement Client](#implement-client-2)
    - [Server Main Loop](#server-main-loop-2)
    - [Compile And Run](#compile-and-run-2)
    - [Sample Output](#sample-output-2)
- [References](#references)


# Introduction

```CommonAPI``` is a middleware framework designed to facilitate communication between software components in distributed systems. It is particularly popular in the automotive industry, where it is used to enable communication between different Electronic Control Units (ECUs) within a vehicle. ```CommonAPI``` abstracts the underlying communication mechanisms, allowing developers to focus on the application logic rather than the intricacies of inter-process communication (```IPC```).

# Key Features

```Language Independence```: ```CommonAPI``` supports multiple programming languages, including ```C++``` and ```Java```. This allows for seamless integration of components written in different languages.

```Platform Independence```: ```CommonAPI``` is designed to be platform-agnostic, meaning it can run on various operating systems and hardware architectures.

```Interface Definition Language (IDL)```: CommonAPI uses an ```IDL``` to define interfaces and data types. This ```IDL``` is used to generate code that handles the serialization and deserialization of data, as well as the communication logic.

```Support for Multiple Communication Protocols```: ```CommonAPI``` can work with different communication protocols, such as ```SOME/IP``` (Scalable service-Oriented MiddlewarE over IP), ```D-Bus```, and others. This flexibility makes it suitable for a wide range of applications.

```Asynchronous Communication```: ```CommonAPI``` supports both synchronous and asynchronous communication patterns, allowing developers to choose the most appropriate model for their application.

# Architecture

The architecture of ```CommonAPI``` can be divided into several key components:

```Interface Definition```: Interfaces are defined using the ```CommonAPI IDL```. These interfaces specify the methods, properties, and events that a service can expose.

```Code Generation```: The CommonAPI code generator takes the IDL definitions and generates the necessary C++ (or other language) code. This includes proxy classes (for clients) and stub classes (for servers), as well as serialization/deserialization logic.

```Runtime```: The ```CommonAPI``` runtime provides the infrastructure for communication. It handles the actual transmission of messages between components, using the specified communication protocol.

```Bindings```: Bindings are implementations of the ```CommonAPI``` runtime for specific communication protocols. For example, there are bindings for ```SOME/IP```, ```D-Bus```, etc.

# Installation

**Install Dependencies**

```
sudo apt update
sudo apt install -y cmake g++ libboost-all-dev openjdk-11-jdk python3
```

**Build and install CommonAPI runtime**

```
git clone --recursive https://github.com/GENIVI/capicxx-core-runtime.git
cd capicxx-core-runtime
mkdir build && cd build
cmake ..
make
sudo make install
```

# Workflow In Programming

```Define Interfaces```: Start by defining the interfaces using the ```CommonAPI IDL```. This includes specifying methods, properties, and events.

```Generate Code```: Use the ```CommonAPI``` code generator to generate the C++ code from the IDL definitions. This will create the necessary proxy and stub classes.

```Implement Services```: Implement the server-side logic by extending the generated stub classes. Similarly, implement the client-side logic using the generated proxy classes.

```Configure Communication```: Configure the communication settings, such as the protocol to be used (e.g., ```SOME/IP```, ```D-Bus```) and any necessary parameters.

```Deploy and Run```: Deploy the components to the target environment and run them. The CommonAPI runtime will handle the communication between the components.

# Franca IDL

```Franca IDL``` is a text-based language that describes how software components should interact.

It defines interfaces, methods, attributes, and data types that components use to communicate.

```Franca IDL``` is often used with ```CommonAPI```, which is a framework for ```C++``` that helps in creating these communication interfaces.

Example:

```
struct Device {
    String id
    String name
    Boolean isOn
}

enum DeviceType {
    LIGHT,
    THERMOSTAT,
    CAMERA
}

struct TemperatureSettings {
    Float currentTemperature
    Float targetTemperature
}

interface SmartHome {
    version { major 1 minor 0 }

    method AddDevice {
        in {
            Device newDevice
            DeviceType type
        }
        out {
            Boolean success
            String message
        }
    }

    method ToggleDevice {
        in {
            String deviceId
            Boolean turnOn
        }
        out {
            Boolean success
        }
    }

    method GetTemperatureSettings {
        out {
            TemperatureSettings settings
        }
    }

    method SetTargetTemperature {
        in {
            Float targetTemperature
        }
        out {
            Boolean success
        }
    }

    attribute Array<Device> devices
    attribute Float currentTemperature

    broadcast DeviceToggled {
        String deviceId
        Boolean isOn
    }

    broadcast TemperatureChanged {
        Float newTemperature
    }
}
```

## Key Concepts

### Interfaces

An interface in ```Franca IDL``` defines a set of methods, attributes, and broadcasts that a component provides. It acts as a contract or blueprint for communication between components.

### Methods

A method defines an action that one component can ask another to perform. It specifies input parameters (```in```) and output parameters (```out```).

### Broadcasts

A broadcast is a way for a component to send notifications to other components. It is like a one-way message that doesn’t expect a response.

### Attributes

An attribute is a data value that can be read or written by other components. It represents the state of a component.

## FIDL Data Types

```Franca IDL``` (Interface Definition Language) supports a variety of data types to define the structure of data that can be passed between components.

### Primitive Data Types

**Boolean**

Represents true or false.

Example:

```
Boolean isReady = true;
```

**Integer Types**

```Int8```: 8-bit signed integer (range: -128 to 127).

```Int16```: 16-bit signed integer (range: -32,768 to 32,767).

```Int32```: 32-bit signed integer (range: -2,147,483,648 to 2,147,483,647).

```Int64```: 64-bit signed integer (very large range).

```UInt8```: 8-bit unsigned integer (range: 0 to 255).

```UInt16```: 16-bit unsigned integer (range: 0 to 65,535).

```UInt32```: 32-bit unsigned integer (range: 0 to 4,294,967,295).

```UInt64```: 64-bit unsigned integer (very large range).

**Floating-Point Types**

```Float```: 32-bit floating-point number (for decimal values).

```Double```: 64-bit floating-point number (for more precise decimal values).

**String**

Represents text data.

Example:

```
String name = "John";
```

**Byte**

Represents raw binary data (8-bit unsigned value).

Example:

```
Byte data = 0xFF;
```

### Complex Data Types

**Array**

A collection of elements of the same type.

Example:

```
Array<Int32> numbers = [1, 2, 3];
```

**Struct**

A custom data type that groups multiple fields (like a record).

Example:

```
struct Person {
  String name
  Int32 age
}
```

**Enumeration (Enum)**

A custom type that defines a set of named values.

Example:

```
enum Color {
  RED,
  GREEN,
  BLUE
}
```

**Map**

A collection of key-value pairs, where each key maps to a value.

Example:

```
Map<String, Int32> ageMap = {"John": 25, "Alice": 30};
```

**Union**

A type that can hold one of several different types at a time.

Example:

```
union MyUnion {
  Int32 number
  String text
}
```

# Sample Codes

## Sync and Async APIs

### Define the Interface (CommonAPI IDL)

The ```CommonAPI``` interface is defined using an Interface Definition Language (```IDL```) file. This file specifies the service methods and their input/output parameters.

```
interface MyService {
    version { major 1 minor 0 }

    method mySyncMethod {
        in {
            Int32 input
        }
        out {
            Int32 result
        }
    }

    method myAsyncMethod {
        in {
            Int32 input
        }
        out {
            Int32 result
        }
    }
}
```

### Generate Code

The ```IDL``` file is used to generate C++ code that provides stubs for client and server implementations.

```
commonapi-tools generate -f MyService.fidl
```

### Implement Server

The server implementation defines how the service methods work. It processes client requests and returns responses.

```
#include "MyServiceStubImpl.hpp"
#include <iostream>

MyServiceStubImpl::MyServiceStubImpl()
{
    // Constructor
}

MyServiceStubImpl::~MyServiceStubImpl()
{
    // Destructor
}

void MyServiceStubImpl::mySyncMethod(const std::shared_ptr<CommonAPI::ClientId> _client, int32_t _input, mySyncMethodReply_t _reply)
{
    std::cout << "Server: mySyncMethod called with input = " << _input << std::endl;
    int32_t result = _input * 2; // Example logic
    _reply(result); // Send the result back to the client
}

void MyServiceStubImpl::myAsyncMethod(const std::shared_ptr<CommonAPI::ClientId> _client, int32_t _input, myAsyncMethodReply_t _reply)
{
    std::cout << "Server: myAsyncMethod called with input = " << _input << std::endl;
    int32_t result = _input + 10; // Example logic
    _reply(result); // Send the result back to the client
}
```

### Implement Client

The client implementation interacts with the server using synchronous and asynchronous calls.

```
#include "MyServiceProxy.hpp"
#include <iostream>
#include <thread>
#include <chrono>

int main()
{
    // Get the CommonAPI runtime
    std::shared_ptr<CommonAPI::Runtime> runtime = CommonAPI::Runtime::get();

    // Create a proxy instance
    std::shared_ptr<MyServiceProxy<>> myServiceProxy = runtime->buildProxy<MyServiceProxy>("local", "my.service");

    // Wait for the service to become available
    std::cout << "Waiting for service to become available..." << std::endl;
    while (!myServiceProxy->isAvailable())
    {
        std::this_thread::sleep_for(std::chrono::milliseconds(10));
    }
    std::cout << "Service is available!" << std::endl;

    // Synchronous call
    {
        CommonAPI::CallStatus callStatus;
        int32_t result;
        myServiceProxy->mySyncMethod(5, callStatus, result); // Call the synchronous method
        if (callStatus == CommonAPI::CallStatus::SUCCESS)
        {
            std::cout << "Client: Synchronous call result = " << result << std::endl;
        }
        else
        {
            std::cerr << "Client: Synchronous call failed!" << std::endl;
        }
    }

    // Asynchronous call
    {
        myServiceProxy->myAsyncMethodAsync(7, [](const CommonAPI::CallStatus& callStatus, int32_t result) {
            if (callStatus == CommonAPI::CallStatus::SUCCESS)
            {
                std::cout << "Client: Asynchronous call result = " << result << std::endl;
            }
            else
            {
                std::cerr << "Client: Asynchronous call failed!" << std::endl;
            }
        });
    }

    // Wait for the asynchronous call to complete
    std::this_thread::sleep_for(std::chrono::seconds(2));

    return 0;
}
```

### Server Main Loop

The server needs to keep running to handle client requests.

```
#include "MyServiceStubImpl.hpp"
#include <iostream>
#include <memory>

int main()
{
    // Get the CommonAPI runtime
    std::shared_ptr<CommonAPI::Runtime> runtime = CommonAPI::Runtime::get();

    // Create an instance of the service implementation
    std::shared_ptr<MyServiceStubImpl> myService = std::make_shared<MyServiceStubImpl>();

    // Register the service
    runtime->registerService("local", "my.service", myService);

    std::cout << "Server is running..." << std::endl;

    // Keep the server running
    while (true)
    {
        std::this_thread::sleep_for(std::chrono::seconds(1));
    }

    return 0;
}
```

### Compile And Run

Compile the server and client and run them to see the communication in action.

```
g++ -std=c++11 server.cpp MyServiceStubImpl.cpp -o server -lCommonAPI
g++ -std=c++11 client.cpp -o client -lCommonAPI

./server
./client
```

### Sample Output

When the server and client run, the following output should be observed.

**Server**

```
Server is running...
Server: mySyncMethod called with input = 5
Server: myAsyncMethod called with input = 7
```

**Client**

```
Waiting for service to become available...
Service is available!
Client: Synchronous call result = 10
Client: Asynchronous call result = 17
```

## Broadcast API

### Define the Interface (CommonAPI IDL)

```
interface MyService {
    version { major 1 minor 0 }

    broadcast myEvent {
        out {
            String message
        }
    }
}
```

### Generate Code

```
commonapi-tools generate -f MyService.fidl
```

### Implement Server

```
#include "MyServiceStubImpl.hpp"
#include <iostream>
#include <thread>
#include <chrono>

MyServiceStubImpl::MyServiceStubImpl()
{
    // Start a thread to periodically broadcast events
    eventThread = std::thread(&MyServiceStubImpl::broadcastEvents, this);
}

MyServiceStubImpl::~MyServiceStubImpl()
{
    if (eventThread.joinable())
    {
        eventThread.join();
    }
}

void MyServiceStubImpl::broadcastEvents()
{
    int counter = 0;
    while (true)
    {
        std::this_thread::sleep_for(std::chrono::seconds(2)); // Broadcast every 2 seconds
        std::string message = "Event " + std::to_string(++counter);
        std::cout << "Server: Broadcasting event with message = " << message << std::endl;
        fireMyEvent(message); // Broadcast the event to all subscribed clients
    }
}
```

### Implement Client

```
#include "MyServiceProxy.hpp"
#include <iostream>
#include <thread>
#include <chrono>

class MyServiceClient
{
public:
    MyServiceClient()
    {
        // Get the CommonAPI runtime
        std::shared_ptr<CommonAPI::Runtime> runtime = CommonAPI::Runtime::get();

        // Create a proxy instance
        myServiceProxy = runtime->buildProxy<MyServiceProxy>("local", "my.service");

        // Wait for the service to become available
        std::cout << "Waiting for service to become available..." << std::endl;
        while (!myServiceProxy->isAvailable())
        {
            std::this_thread::sleep_for(std::chrono::milliseconds(10));
        }
        std::cout << "Service is available!" << std::endl;

        // Subscribe to the event
        myServiceProxy->getMyEventEvent().subscribe([this](const std::string& message)
        {
            this->onMyEvent(message);
        });
    }

    void onMyEvent(const std::string& message)
    {
        std::cout << "Client: Received event with message = " << message << std::endl;
    }

private:
    std::shared_ptr<MyServiceProxy<>> myServiceProxy;
};

int main()
{
    MyServiceClient client;

    // Keep the client running to receive events
    while (true)
    {
        std::this_thread::sleep_for(std::chrono::seconds(1));
    }

    return 0;
}
```

### Server Main Loop

```
#include "MyServiceStubImpl.hpp"
#include <iostream>
#include <memory>

int main()
{
    // Get the CommonAPI runtime
    std::shared_ptr<CommonAPI::Runtime> runtime = CommonAPI::Runtime::get();

    // Create an instance of the service implementation
    std::shared_ptr<MyServiceStubImpl> myService = std::make_shared<MyServiceStubImpl>();

    // Register the service
    runtime->registerService("local", "my.service", myService);

    std::cout << "Server is running..." << std::endl;

    // Keep the server running
    while (true)
    {
        std::this_thread::sleep_for(std::chrono::seconds(1));
    }

    return 0;
}
```

### Compile And Run

```
g++ -std=c++11 server.cpp MyServiceStubImpl.cpp -o server -lCommonAPI
g++ -std=c++11 client.cpp -o client -lCommonAPI

./server
./client
```

### Sample Output

```
Server is running...
Server: Broadcasting event with message = Event 1
Server: Broadcasting event with message = Event 2
Server: Broadcasting event with message = Event 3
...
```

```
Waiting for service to become available...
Service is available!
Client: Received event with message = Event 1
Client: Received event with message = Event 2
Client: Received event with message = Event 3
...
```

## SOME/IP Binding

### Define the Interface (CommonAPI IDL)

```
interface MyService {
    version { major 1 minor 0 }

    method myMethod {
        in {
            Int32 input
        }
        out {
            Int32 result
        }
    }

    broadcast myEvent {
        out {
            String message
        }
    }
}
```

### Generate Code

```
commonapi-tools generate -f MyService.fidl
```

### Configure SOME/IP Binding

```
[service]
service_id = 0x1234
instance_id = 0x5678
major_version = 1
minor_version = 0

[methods]
myMethod.method_id = 0x0001

[events]
myEvent.event_id = 0x0002
```

### Implement Server

```
#include "MyServiceStubImpl.hpp"
#include <iostream>
#include <thread>
#include <chrono>

MyServiceStubImpl::MyServiceStubImpl()
{
    // Start a thread to periodically broadcast events
    eventThread = std::thread(&MyServiceStubImpl::broadcastEvents, this);
}

MyServiceStubImpl::~MyServiceStubImpl()
{
    if (eventThread.joinable())
    {
        eventThread.join();
    }
}

void MyServiceStubImpl::myMethod(const std::shared_ptr<CommonAPI::ClientId> _client, int32_t _input, myMethodReply_t _reply)
{
    std::cout << "Server: myMethod called with input = " << _input << std::endl;
    int32_t result = _input * 2; // Example logic
    _reply(result); // Send the result back to the client
}

void MyServiceStubImpl::broadcastEvents()
{
    int counter = 0;
    while (true)
    {
        std::this_thread::sleep_for(std::chrono::seconds(2)); // Broadcast every 2 seconds
        std::string message = "Event " + std::to_string(++counter);
        std::cout << "Server: Broadcasting event with message = " << message << std::endl;
        fireMyEvent(message); // Broadcast the event to all subscribed clients
    }
}
```

### Implement Client

```
#include "MyServiceProxy.hpp"
#include <iostream>
#include <thread>
#include <chrono>

class MyServiceClient
{
public:
    MyServiceClient()
    {
        // Get the CommonAPI runtime
        std::shared_ptr<CommonAPI::Runtime> runtime = CommonAPI::Runtime::get();

        // Create a proxy instance
        myServiceProxy = runtime->buildProxy<MyServiceProxy>("local", "my.service");

        // Wait for the service to become available
        std::cout << "Waiting for service to become available..." << std::endl;
        while (!myServiceProxy->isAvailable())
        {
            std::this_thread::sleep_for(std::chrono::milliseconds(10));
        }
        std::cout << "Service is available!" << std::endl;

        // Subscribe to the event
        myServiceProxy->getMyEventEvent().subscribe([this](const std::string& message) {
            this->onMyEvent(message);
        });
    }

    void callMyMethod()
    {
        CommonAPI::CallStatus callStatus;
        int32_t result;
        myServiceProxy->myMethod(5, callStatus, result); // Call the synchronous method
        if (callStatus == CommonAPI::CallStatus::SUCCESS)
        {
            std::cout << "Client: myMethod result = " << result << std::endl;
        }
        else
        {
            std::cerr << "Client: myMethod call failed!" << std::endl;
        }
    }

    void onMyEvent(const std::string& message)
    {
        std::cout << "Client: Received event with message = " << message << std::endl;
    }

private:
    std::shared_ptr<MyServiceProxy<>> myServiceProxy;
};

int main()
{
    MyServiceClient client;

    // Call the method
    client.callMyMethod();

    // Keep the client running to receive events
    while (true)
    {
        std::this_thread::sleep_for(std::chrono::seconds(1));
    }

    return 0;
}
```

### Server Main Loop

```
#include "MyServiceStubImpl.hpp"
#include <iostream>
#include <memory>

int main()
{
    // Get the CommonAPI runtime
    std::shared_ptr<CommonAPI::Runtime> runtime = CommonAPI::Runtime::get();

    // Create an instance of the service implementation
    std::shared_ptr<MyServiceStubImpl> myService = std::make_shared<MyServiceStubImpl>();

    // Register the service
    runtime->registerService("local", "my.service", myService);

    std::cout << "Server is running..." << std::endl;

    // Keep the server running
    while (true)
    {
        std::this_thread::sleep_for(std::chrono::seconds(1));
    }

    return 0;
}
```

### Compile And Run

```
g++ -std=c++11 server.cpp MyServiceStubImpl.cpp -o server -lCommonAPI-SOMEIP
g++ -std=c++11 client.cpp -o client -lCommonAPI-SOMEIP

./server
./client
```

### Sample Output

```
Server is running...
Server: myMethod called with input = 5
Server: Broadcasting event with message = Event 1
Server: Broadcasting event with message = Event 2
...
```

```
Waiting for service to become available...
Service is available!
Client: myMethod result = 10
Client: Received event with message = Event 1
Client: Received event with message = Event 2
...
```

# References

[https://some-ip.com/](https://some-ip.com/)

[https://github.com/COVESA/vsomeip/wiki/vsomeip-in-10-minutes](https://github.com/COVESA/vsomeip/wiki/vsomeip-in-10-minutes)

[https://www.embien.com/automotive-insights/some-ip-protocol-communication-a-comprehensive-guide](https://www.embien.com/automotive-insights/some-ip-protocol-communication-a-comprehensive-guide)

[https://www.autosar.org/fileadmin/standards/R22-11/FO/AUTOSAR_PRS_SOMEIPProtocol.pdf](https://www.autosar.org/fileadmin/standards/R22-11/FO/AUTOSAR_PRS_SOMEIPProtocol.pdf)
