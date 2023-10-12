
# Project Title

OPC-UA-.NETStandard

OPC UA, which stands for "OPC Unified Architecture," is a widely used industrial communication protocol and framework designed for secure and reliable data exchange in industrial automation and control systems. It was developed to address the limitations of earlier versions of the OPC (OLE for Process Control) standard. OPC UA is not just a protocol; it's a comprehensive architecture for connecting industrial devices and systems, providing a standardized way to access and exchange data in various industrial applications.

Key features and aspects of OPC UA include:

    Platform Independence: OPC UA is designed to work across different operating systems and hardware platforms, making it versatile and adaptable to various industrial environments.

    Security: OPC UA places a strong emphasis on security, providing encryption and authentication mechanisms to protect data and ensure that only authorized users and applications can access and manipulate it. This is particularly important in critical industrial systems where data integrity and security are paramount.

    Scalability: OPC UA supports a wide range of data types and structures, allowing it to handle complex data models and large data sets. This scalability is essential for accommodating the diverse data requirements of industrial automation and control systems.

    Reliability: It is known for its reliability in data exchange, ensuring that data is delivered consistently and without loss, even in challenging industrial environments.

    Information Modeling: OPC UA employs an information modeling approach, enabling users to define custom data structures and hierarchies. This flexibility makes it suitable for representing a wide variety of data and system configurations.

    Standardized Services: OPC UA defines a set of standardized services that enable operations such as reading, writing, subscribing to data changes, and invoking methods on remote devices and servers. These services facilitate interoperability between different devices and software applications.

    Historical Data Access: OPC UA provides mechanisms for accessing historical data, which is crucial for tasks such as trend analysis and process optimization.

    Discovery and Redundancy: OPC UA includes mechanisms for device discovery and supports redundancy configurations, enhancing system availability and fault tolerance.

OPC UA has gained widespread adoption in industries like manufacturing, energy, automotive, and more, where robust and secure communication between devices and systems is essential. It has become a cornerstone technology in the development of the Industrial Internet of Things (IIoT) and Industry 4.0 initiatives, enabling seamless integration and data exchange between machines, sensors, and control systems in modern industrial environments.


## Table of Contents

**Installation:**

**Getting started:**

**OPC UA IIoT StarterKit – Setup Ubuntu Environment**
Overview

    Install and Configure MQTT Broker
    Install and Configure .NET Core
    Build StarterKit Code

Install and Configure MQTT Broker

These steps explain how to set up an insecure broker for testing. A broker used in production applications would need to have TLS enabled and some sort of client authentication.

Install Mosquitto Broker with these commands:

sudo apt update
sudo apt install mosquitto

Download Java 8 from here. For Ubuntu 64-bit the download is the "Linux x64 Compressed Archive".

cd ~/
tar -xvf jre-8u291-linux-x64.tar.gz 

Download mqtt-spy from here.

mkdir ~/mqtt-spy
cd ~/mqtt-spy
~/jre1.8.0_291/bin/java -jar mqtt-spy-1.0.0.jar 

To connect to a broker using mqtt-spy:

    Create a connection to the broker;
    Subscribe to all topic (enter ‘#’ as the topic name);
    Publish to a text topic and verify the response was received.

Install and Configure .NET Core

Install Visual Studio Code:

sudo apt install code

Manually download Visual Studio Code from https://code.visualstudio.com/ if necessary.

Install the following extensions (select extensions icon on right side toolbar):

    C#

Install .NET Core SDK:

sudo snap install dotnet-sdk --classic

Test installation:

mkdir helloworld
cd helloworld/
dotnet new console
dotnet build
dotnet bin/Debug/net5.0/helloworld.dll 

If all is good the following output should be printed:

Hello World!

Build StarterKit Code

Fetch code from GitHub:

cd ~/
git clone https://github.com/OPCF-Members/UA-IIoT-StarterKit.git
cd UA-IIoT-StarterKit
git submodule update --init

Build code:

cd ~/UA-IIoT-StarterKit
dotnet build MqttAgent/MqttAgent.csproj 

Run code

cd ~/UA-IIoT-StarterKit/build/bin/Debug/net50/
dotnet MqttAgent.dll --help

The following output should be produced:

Usage: MqttAgent [options] [command]

Options:
  -?|-h|--help  Show help information

Commands:
  discover   Discovers OPC UA publishers.
  publish    Publishes I/O data to an MQTT broker.
  subscribe  Subscribes for data from OPC UA publishers.

Use "MqttAgent [command] --help" for more information about a command.

**OPC UA IIoT StarterKit – MqttAgent
Overview**

The main application is called MqttAgent.

It can act as a publisher or subscriber depending on the command line arguments.

The available commands are:

Usage: MqttAgent [options] [command]

Options:
  -?|-h|--help  Show help information

Commands:
  discover   Discovers OPC UA publishers.
  publish    Publishes I/O data to an MQTT broker.
  subscribe  Subscribes for data from OPC UA publishers.

Use "MqttAgent [command] --help" for more information about a command.

Confguration Files

The MqttAgent depends on the following configuration files stored in the ‘./config’ directory:
File 	Description
*-connection.json 	A PubSub connection configuration. It specifies the number and the location of topics which the application publishes to/subscribes to.
The contents of the file are described here.
*-nameplate.json 	A JSON file that stores metadata about the host.
It is simple self-describing set of name-value pairs.
datasets/*.json 	A folder containing JSON files that describe datasets that may be published to the MQTT broker.
The contents of a dataset file are described here.
sources/*.json 	A folder containing JSON files that describe variables .
The contents of a dataset file are described here.

The file formats are defined by the OPC UA specification and may be exchanged between applications developed by different vendors.
Getting the Code

Clone the GitHub repository into the workspace directory:

git clone https://github.com/OPCF-Members/UA-IIoT-StarterKit.git
cd UA-IIoT-StarterKit
git submodule update --init

Building the code Windows:

Open MqttAgent.sln with Visual Studio
Build solution

Building the code for the RaspberryPi:

dotnet publish -f net50 -r linux-arm -o ./build/MqttAgent ./MqttAgent/MqttAgent.csproj

Copy code to RaspberryPi (based on setup instructions):

scp -i ../.ssh/id_rsa -r ./build/MqttAgent pi@raspberrypi:~/

**For More Detalils*

https://github.com/OPCFoundation/UA-IIoT-StarterKit/tree/master



**How to build and run the samples in Visual Studio on Windows**

    Open the UA Sample Applications.sln solution file using Visual Studio 2017.
    Choose a project in the Solution Explorer and set it with a right click as Startup Project.
    Hit F5 to build and execute the sample.

**How to build and run the console samples on Windows, Linux and iOS**

This section describes how to run the NetCoreConsoleClient and NetCoreConsoleServer sample applications.

Please follow instructions in this article to setup the dotnet command line environment for your platform. As of today .Net Standard 2.0 is required.
Prerequisites

    Once the dotnet command is available, navigate to the root folder in your local copy of the repository and execute dotnet restore UA Sample Applications.sln. This command calls into NuGet to restore the tree of dependencies.

**Start the server**

    Open a command prompt.
    Navigate to the folder Samples/NetCoreConsoleServer.
    To run the server sample type dotnet run --project NetCoreConsoleServer.csproj -a.
        The server is now running and waiting for connections.
        The -a flag allows to auto accept unknown certificates and should only be used to simplify testing.

**Start the client**

    Open a command prompt
    Navigate to the folder Samples/NetCoreConsoleClient.
    To run the sample type dotnet run --project NetCoreConsoleClient.csproj -a to connect to the OPC UA console sample server running on the same host.
        The -a flag allows to auto accept unknown certificates and should only be used to simplify testing.
        To connect to another OPC UA server specify the server as first argument and type e.g. dotnet run --project NetCoreConsoleClient.csproj -a opc.tcp://myserver:51210/UA/SampleServer.
    If not using the -a auto accept option, on first connection, or after certificates were renewed, the server may have refused the client certificate. Check the server and client folder %LocalApplicationData%/OPC Foundation/CertificateStores/RejectedCertificates for rejected certificates. To approve a certificate copy it to the %LocalApplicationData%/OPC Foundation/CertificateStores/UA Applications folder.
    Retry step 3 to connect using a secure connection.

