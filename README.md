# User Manual

## Introduction

Anypoint Connector for Skyflow (Skyflow Connector) lets you easily connect to Skyflow’s Data Privacy Vault to securely protect sensitive information. Using this connector, you can replace sensitive information in your services with tokens, and strengthen the protection of sensitive information with a zero trust data vault. You can also use this connector to detokenize and retrieve the original data by sending tokens subject to Skyflow’s fine grained authorization policies.

## Prerequisites

This document assumes that you are familiar with Mule 4, Anypoint Connectors, and Anypoint Studio. To increase your familiarity with Studio, consider completing a Anypoint Studio Tutorial. This page requires some basic knowledge of Mule Concepts, Components in a Mule Flow, and  Global Elements.
To use this connector, you will also need a Skyflow Account. Reach out to the Skyflow team here to get started: https://www.skyflow.com/get-demo.
After signing up for an account, you will need to create a vault and an associated Skyflow Connection. This is where you map the fields in the JSON payload being sent to Skyflow from the connector that need to be tokenized in the vault. Refer to this quickstart guide to create your first connection: https://docs.skyflow.com/developer-portal/connections/connections-quickstart/

## Hardware and Software Requirements

For hardware and software requirements, please visit the Hardware and Software Requirements page.

## Mule Compatibility

| Application/Service | Version |
| ------------- | ------------- |
| Mule Runtime | 4.3.0 and later |
| Anypoint Studio | 7.10.x and later |

## Installing the Connector

You can install the connector in Anypoint Studio using the instructions in Installing a Connector from Anypoint Exchange.

## How to Configure

 1. After adding connector dependency to the Mule project, click on the Global Elements tab at the base of the canvas.
 2. In the Global Mule Configuration Elements screen for the Skyflow Connector, click Create.
 3. In the Choose Global Type wizard, collapse connector configuration and select ‘Skyflow Connector Config' and click OK.
 4. Give your configuration a unique name and select ‘Credentials file’ for the Connection field. This indicates that you will be using a credentials file stored in your local machine containing private keys to sign the jwt token needed to get a Skyflow bearer.
 5. Under the Credentials file path, enter the file path and under the Base URL enter the Skyflow connection base url as follows:
 6. Test the connection to verify that the connector was able to successfully generate an access token from Skyflow.

Alternatively, under the Connection field, you can select the JWT Connection option. In this configuration, you will be expected to enter the contents of the skyflow credential file including the client id, api key and private key. The benefit of this approach is that you can use MuleSoft’s secure config properties to encrypt the credentials  required to authenticate with Skyflow.
The Skyflow connector supports four operations: Tokenize, Bulk Tokenize, Detokenize and Bulk Detokenize. After you have configured the connector global elements, from the palette on the right, you can drag and drop the required operation.
In addition to pre configured Basic Settings, you will need to enter the connection ID and the relative path obtained from your Skyflow Connection under the Skyflow Connection Locator settings as follows:

## Required Connector Namespace and Schema

When designing your application in Studio, the act of dragging the connector from the palette into the Anypoint Studio canvas should automatically populate the XML code with the connector namespace and schema location.
```aidl
<?xml version="1.0" encoding="UTF-8"?>

<mule xmlns:http="http://www.mulesoft.org/schema/mule/http" xmlns:skyflow="http://www.mulesoft.org/schema/mule/skyflow"
      xmlns="http://www.mulesoft.org/schema/mule/core"
      xmlns:doc="http://www.mulesoft.org/schema/mule/documentation" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.mulesoft.org/schema/mule/core http://www.mulesoft.org/schema/mule/core/current/mule.xsd
                 http://www.mulesoft.org/schema/mule/skyflow http://www.mulesoft.org/schema/mule/skyflow/current/mule-skyflow.xsd
                 http://www.mulesoft.org/schema/mule/http http://www.mulesoft.org/schema/mule/http/current/mule-http.xsd">
    <skyflow:config name="Skyflow_connector_Config" doc:name="Skyflow connector Config">
        <skyflow:credentials-file-connection credentialsFilePath="/Users/ashleyjose/Downloads/cred.json" baseUrl="https://ebfc9bee4242.gateway.skyflowapis.com" />
    </skyflow:config>
    <http:listener-config name="HTTP_Listener_config" doc:name="HTTP Listener config" basePath="demo" >
        <http:listener-connection host="0.0.0.0" port="8081" />
    </http:listener-config>
    <flow name="skyflowconnectorFlow">
        <http:listener doc:name="Listener" doc:id="6ca793d8-f779-4e11-94b2-0f1be5b4c6f0" config-ref="HTTP_Listener_config" path="/tokenize"/>
        <skyflow:tokenize doc:name="Tokenize" config-ref="Skyflow_connector_Config" connectionId="j3a091f26fa84dad9f0bbe5c14539f9d" relativePath="tokenize">
            <skyflow:fields ><![CDATA[#[%dw 2.0
output application/json
---

{
"ssn": attributes.queryParams.ssn,
"email": attributes.queryParams.email
}]]]></skyflow:fields>
        </skyflow:tokenize>
    </flow>
</mule>
```

## Maven Dependency Information

Maven is backing the application, this XML snippet must be included in your pom.xml file.
 
    <dependency>
        <groupId>com.skyflow</groupId>
        <artifactId>mule-skyflow-connector</artifactId>
        <version>1.0.0</version>
        <classifier>mule-plugin</classifier>
    </dependency>


# Release notes

## 1.0.0
### March 28, 2022
Release Notes for version V1.0.0 of the Mule 4 - Skyflow Connector.
##Version 1.0.0 - Compatibility
The Skyflow connector is compatible with:

|Application/Service|Version|
|-------------------|-------|
|Mule Runtime|4.3.0|
|Magento Rest API |2.X.X|

## Version 1.0.0 - Features
 - Added support for four operations: Tokenize, Bulk Tokenize, Detokenize, and Bulk Detokenize.
 - Added support for authenticating to Skyflow using service account credentials.
