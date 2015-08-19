helloworld-mdb: Helloworld Using an MDB (Message-Driven Bean)
============================================================
Author: Serge Pagop, Andy Taylor, Jeff Mesnil  
Level: Intermediate  
Technologies: JMS, EJB, MDB  
Summary: The `helloworld-mdb` quickstart uses *JMS* and *EJB Message-Driven Bean* (MDB) to create and deploy JMS topic and queue resources in JBoss EAP.  
Target Product: JBoss EAP  
Source: <https://github.com/jboss-developer/jboss-eap-quickstarts/>  

What is it?
-----------

The `helloworld-mdb` quickstart demonstrates the use of *JMS* and *EJB Message-Driven Bean* in Red Hat JBoss Enterprise Application Platform.

This project creates two JMS resources:

* A queue named `HELLOWORLDMDBQueue` bound in JNDI as `java:/queue/HELLOWORLDMDBQueue`
* A topic named `HELLOWORLDMDBTopic` bound in JNDI as `java:/topic/HELLOWORLDMDBTopic`


System requirements
-------------------

The application this project produces is designed to be run on Red Hat JBoss Enterprise Application Platform 7 or later. 

All you need to build this project is Java 8.0 (Java SDK 1.8) or later and Maven 3.1.1 or later. See [Configure Maven for JBoss EAP 7](https://github.com/jboss-developer/jboss-developer-shared-resources/blob/master/guides/CONFIGURE_MAVEN_JBOSS_EAP7.md#configure-maven-to-build-and-deploy-the-quickstarts) to make sure you are configured correctly for testing the quickstarts.


Use of EAP_HOME
---------------

In the following instructions, replace `EAP_HOME` with the actual path to your JBoss EAP installation. The installation path is described in detail here: [Use of EAP_HOME and JBOSS_HOME Variables](https://github.com/jboss-developer/jboss-developer-shared-resources/blob/master/guides/USE_OF_EAP_HOME.md#use-of-eap_home-and-jboss_home-variables).


Start the JBoss EAP Server with the Full Profile
---------------

1. Open a command prompt and navigate to the root of the JBoss EAP directory.
2. The following shows the command line to start the server with the full profile:

        For Linux:   EAP_HOME/bin/standalone.sh -c standalone-full.xml
        For Windows: EAP_HOME\bin\standalone.bat -c standalone-full.xml

Configure the JBoss EAP Server
---------------------------

You configure the JMS `HELLOWORLDMDBQueue` queue and `HELLOWORLDMDBTopic` topic by running JBoss CLI commands. For your convenience, this quickstart batches the commands into a `configure-jms.cli` script provided in the root directory of this quickstart. 

1. Before you begin, back up your server configuration file
    * If it is running, stop the JBoss EAP server.
    * Backup the file: `EAP_HOME/standalone/configuration/standalone-full.xml`
    * After you have completed testing this quickstart, you can replace this file to restore the server to its original configuration.
2. Start the JBoss EAP server by typing the following: 

        For Linux:  EAP_HOME/bin/standalone.sh -c standalone-full.xml
        For Windows:  EAP_HOME\bin\standalone.bat -c standalone-full.xml
3. Review the `configure-jms.cli` file in the root of this quickstart directory. This script adds the `test` queue to the `messaging` subsystem in the server configuration file.

4. Open a new command prompt, navigate to the root directory of this quickstart, and run the following command, replacing EAP_HOME with the path to your server:

        For Linux: EAP_HOME/bin/jboss-cli.sh --connect --file=configure-jms.cli 
        For Windows: EAP_HOME\bin\jboss-cli.bat --connect --file=configure-jms.cli 
   You should see the following result when you run the script:

        The batch executed successfully.
        {"outcome" => "success"}
5. Stop the JBoss EAP server.


Review the Modified Server Configuration
-----------------------------------

After stopping the server, open the `EAP_HOME/standalone/configuration/standalone-full.xml` file and review the changes.

The following `HELLOWORLDMDBQueue` jms-queue `HELLOWORLDMDBTopic` jms-topic and were configured in <server name="default"> element under the <subsystem xmlns="urn:jboss:domain:messaging-activemq:1.0"> section of the `messaging` subsystem.

      <jms-queue name="HELLOWORLDMDBQueue" entries="queue/HELLOWORLDMDBQueue java:jboss/exported/jms/queue/HELLOWORLDMDBQueue"/>
      <jms-topic name="HELLOWORLDMDBTopic" entries="topic/HELLOWORLDMDBTopic java:jboss/exported/jms/topic/HELLOWORLDMDBTopic"/>



Build and Deploy the Quickstart
-------------------------

1. Make sure you have started the JBoss EAP server as described above.
2. Open a command prompt and navigate to the root directory of this quickstart.
3. Type this command to build and deploy the archive:

        mvn clean install wildfly:deploy

4. This will deploy `target/jboss-helloworld-mdb.war` to the running instance of the server. Look at the JBoss EAP console or Server log and you should see log messages corresponding to the deployment of the message-driven beans and the JMS destinations:

9:19:42,246 INFO  [org.jboss.as.server.deployment] (MSC service thread 1-8) WFLYSRV0027: Starting deployment of "jboss-helloworld-mdb.war" (runtime-name: "jboss-helloworld-mdb.war")
19:19:42,378 INFO  [org.jboss.as.ejb3] (MSC service thread 1-4) WFLYEJB0042: Started message driven bean 'HelloWorldQTopicMDB' with 'activemq-ra.rar' resource adapter
19:19:42,391 INFO  [org.jboss.as.ejb3] (MSC service thread 1-7) WFLYEJB0042: Started message driven bean 'HelloWorldQueueMDB' with 'activemq-ra.rar' resource adapter



Access the application 
---------------------

The application will be running at the following URL: <http://localhost:8080/jboss-helloworld-mdb/> and will send some messages to the queue.

To send messages to the topic, use the following URL: <http://localhost:8080/jboss-helloworld-mdb/HelloWorldMDBServletClient?topic>

Investigate the Server Console Output
-------------------------

Look at the JBoss EAP console or Server log and you should see log messages like the following:

19:19:48,994 INFO  [class org.jboss.as.quickstarts.mdb.HelloWorldQueueMDB] (Thread-3 (ActiveMQ-client-global-threads-399527411)) Received Message from queue: This is message 2
19:19:48,995 INFO  [class org.jboss.as.quickstarts.mdb.HelloWorldQueueMDB] (Thread-42 (ActiveMQ-client-global-threads-399527411)) Received Message from queue: This is message 5
19:19:48,995 INFO  [class org.jboss.as.quickstarts.mdb.HelloWorldQueueMDB] (Thread-45 (ActiveMQ-client-global-threads-399527411)) Received Message from queue: This is message 4
19:19:49,007 INFO  [class org.jboss.as.quickstarts.mdb.HelloWorldQueueMDB] (Thread-43 (ActiveMQ-client-global-threads-399527411)) Received Message from queue: This is message 3
19:19:49,008 INFO  [class org.jboss.as.quickstarts.mdb.HelloWorldQueueMDB] (Thread-37 (ActiveMQ-client-global-threads-399527411)) Received Message from queue: This is message 1
19:19:51,979 INFO  [class org.jboss.as.quickstarts.mdb.HelloWorldTopicMDB] (Thread-43 (ActiveMQ-client-global-threads-399527411)) Received Message from topic: This is message 2
19:19:51,979 INFO  [class org.jboss.as.quickstarts.mdb.HelloWorldTopicMDB] (Thread-3 (ActiveMQ-client-global-threads-399527411)) Received Message from topic: This is message 1
19:19:51,982 INFO  [class org.jboss.as.quickstarts.mdb.HelloWorldTopicMDB] (Thread-45 (ActiveMQ-client-global-threads-399527411)) Received Message from topic: This is message 4
19:19:51,983 INFO  [class org.jboss.as.quickstarts.mdb.HelloWorldTopicMDB] (Thread-42 (ActiveMQ-client-global-threads-399527411)) Received Message from topic: This is message 5
19:19:51,983 INFO  [class org.jboss.as.quickstarts.mdb.HelloWorldTopicMDB] (Thread-37 (ActiveMQ-client-global-threads-399527411)) Received Message from topic: This is message 3



Undeploy the Archive
--------------------

1. Make sure you have started the JBoss EAP server as described above.
2. Open a command prompt and navigate to the root directory of this quickstart.
3. When you are finished testing, type this command to undeploy the archive:

        mvn wildfly:undeploy


Remove the JMS Configuration
----------------------------

You can remove the JMS configuration by running the  `remove-jms.cli` script provided in the root directory of this quickstart or by manually restoring the back-up copy the configuration file. 

### Remove the JMS Configuration by Running the JBoss CLI Script

1. Start the JBoss EAP server by typing the following: 

        For Linux:  EAP_HOME/bin/standalone.sh -c standalone-full.xml
        For Windows:  EAP_HOME\bin\standalone.bat -c standalone-full.xml
2. Open a new command prompt, navigate to the root directory of this quickstart, and run the following command, replacing EAP_HOME with the path to your server:

        For Linux: EAP_HOME/bin/jboss-cli.sh --connect --file=remove-jms.cli 
        For Windows: EAP_HOME\bin\jboss-cli.bat --connect --file=remove-jms.cli 
   This script removes `HELLOWORLDMDBQueue` queue and `HELLOWORLDMDBTopic` topic from the `messaging` subsystem in the server configuration. You should see the following result when you run the script:

        The batch executed successfully.
        {"outcome" => "success"}


### Remove the JMS Configuration Manually
1. If it is running, stop the JBoss EAP server.
2. Replace the `EAP_HOME/standalone/configuration/standalone-full.xml` file with the back-up copy of the file.


Run the Quickstart in Red Hat JBoss Developer Studio or Eclipse
-------------------------------------
You can also start the server and deploy the quickstarts or run the Arquillian tests from Eclipse using JBoss tools. For general information about how to import a quickstart, add a JBoss EAP server, and build and deploy a quickstart, see [Use JBoss Developer Studio or Eclipse to Run the Quickstarts](https://github.com/jboss-developer/jboss-developer-shared-resources/blob/master/guides/USE_JBDS.md#use-jboss-developer-studio-or-eclipse-to-run-the-quickstarts) 

_NOTE:_ Within JBoss Developer Studio, be sure to define a server runtime environment that uses the `standalone-full.xml` configuration file.


Debug the Application
------------------------------------

If you want to debug the source code of any library in the project, run the following command to pull the source into your local repository. The IDE should then detect it.

    mvn dependency:sources
   


<!-- Build and Deploy the Quickstart to OpenShift - Coming soon! -->
