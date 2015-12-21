# Build status #

Travis: [![Build Status](https://travis-ci.org/Intelecom/smsgw-client-java.svg?branch=master)](https://travis-ci.org/Intelecom/smsgw-client-java)

# About #
This Java library contains some convenience [APIs](http://intelecom.github.io/smsgw-client-java/apidocs/index.html) that lets you connect to the Intelecom SMS Gateway. Alternatively, you could integrate directly with one of the [interfaces](https://github.com/Intelecom/sms/blob/master/Interfaces-general.md) yourself.

The library consist of two parts: [One client builder](http://intelecom.github.io/smsgw-client-java/apidocs/com/intele/chimera/client/GatewayClientBuilder.html) and [builders to create a request](http://intelecom.github.io/smsgw-client-java/apidocs/com/intele/chimera/client/request/package-summary.html).

The client implementation uses the JAX-RS 2.0 [Client interface](https://docs.oracle.com/javaee/7/api/javax/ws/rs/client/Client.html). Therefore, you need to run it in a JAX-RS 2.0 J2EE compliant container or provide a JAX-RS 2.0 implementation yourself (e.g. [Jersey](https://jersey.java.net/)).

# Installation #
## Maven ##

```XML
<dependency>
	<groupId>com.intele.chimera</groupId>
	<artifactId>smsgw-client-java</artifactId>
	<version>1.0.0</version>
</dependency>
```

## Gradle ##

```
compile "com.intele.chimera:smsgw-client-java:1.0.0"
```

# Example usage #
## Standalone ##

```Java
try(GatewayClient gatewayClient = new GatewayClientBuilder().build()) {
	GatewayRequest gatewayRequest = new GatewayRequest.Builder(100, "username", "password").build();
	gatewayRequest.addMessage(
			new Sms.Builder("+4741000000", "Test message").withPrice(0).build());
	Response response = gatewayClient.send(gatewayRequest);

	System.out.println(response.getMessageStatus().get(0).getStatusCode());
	System.out.println(response.getMessageStatus().get(0).getStatusMessage());
	System.out.println(response.getMessageStatus().get(0).getMessageId());
} catch(Exception e) {
	e.printStackTrace();
}
```