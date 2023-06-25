---
title: "Fetching Client IP in WebLogic Server Behind Load Balancer"
weight: 4
date: 2022-12-24T20:37:21+05:00
draft: false
images: []
resources:
- name: "featured-image"
  src: "featured-image.png"
lightgallery: true
tags: ["weblogic", "ip","loadbalancer"]
categories: ["Technology"]

---
### Load Balancers and Client IP Addresses
load balancer often hides client Ip from the application. When a load balancer is used, application and WebLogic logs load balancer’s IP instead of the user’s IP. Most of the load balancers support HTTP Header X-Forwarded-For. This header often stores the user’s IP. Weblogic can be configured to read IP addresses from this HTTP header. If IP is not being stored in X-Forwarded-For we have to check on which HTTP Header Load balancer is storing IP for the client. Then instead of X-Forwarded-For use that HTTP Header.

### X-Forwarded-For
Steps to Configure.
1. Open http://server:port/console and login.

2. Go to “Environment > Servers > {Servers_Config} > Logging > HTTP”.

3. Click “Lock & Edit”.

4. Select the checkbox for “HTTP access log file enabled”.

5. Save the changes.

6. Expand the “Advanced” section.

7. Change the Format to Extended.

8. Add cs(X-Forwarded-For) to the Extended Logging Format Fields.

9. Set the Log File Buffer to 0. (This will write entires immediately to the log file.)

10. Save the changes.

11. Click the “Release Configuration” button.

12. Restart the web server.

### WebLogic plugin Enabled
Requests to a WebLogic Server (WLS) usually go through a web server or a load balancer which serves as a proxy for the client requests. When the WLS requests are “front-ended” by either a web server or a load-balancer, the requests are handled via a plugin. It is important for WLS to be aware of the proxy so as to handle the request correctly. Informing the Weblogic Server of the proxy, and therefore the presence of the plugin is achieved using the WLS setting “WebLogic plugin Enabled.”

Secondly, we must enable we “WebLogic plugin Enabled.” There are different ways to enable WebLogic plugin. We can use any one way depending on the architecture of these levels. The levels are:

1. The domain level

2. The cluster level

3. The individual managed server level

To configure this you need to login to WLS Administration Console as an Administrator. Within the console, first, click on “Lock and Edit” to acquire a domain edit lock. This step is required if you are running WLS in production mode.

#### To configure this setting at the domain level, perform the following steps:

1. In the “Domain Structure” pane on the left side, click on the name of the domain — In this case IDMDomain

2. Within the “Settings for <DomainName>” page, navigate to the “Web Applications” sub-tab under the “Configuration” main tab

3. Scroll down until you see a check box titled “WebLogic Plugin Enabled”

4. Make sure the checkbox is checked and click “Save”

#### To configure this setting at the cluster level, perform the following steps:

1. In the “Domain Structure” pane on the left side, click on “+” icon against “Environment” and then click on “Clusters”

2. In the “Summary of Clusters” page, click on the cluster you want to enable this setting for, e.g., oam_cluster

3. In the “Settings for <cluster_name>” page, expand “Advanced” and make sure the box against “WebLogic plugin Enabled” is checked and click “Save”

#### To configure this setting at the managed server level, perform the following steps:

1. In the “Domain Structure” pane on the left side, click on “+” icon against “Environment” and then click on “Servers”

2. In the “Summary of Servers” page, click on the server you want to enable this setting for, e.g., wls_oam1

3. In the “Settings for <server_name>” page, expand “Advanced” and make sure the box against “WebLogic plugin Enabled” is checked and click “Save”

Once the property has been configured to the desired value, and at the desired scope (domain, cluster, or server), click on “Activate Changes” to commit the configuration change.

Restart the required servers.

#### Client IP Sample Application:

Code :

```private static String getClientIp(HttpServletRequest request) {
String remoteAddr = “”;
if (request != null) {
remoteAddr = request.getHeader(“X-FORWARDED-FOR”);
if (remoteAddr == null || “”.equals(remoteAddr)) {
remoteAddr = request.getRemoteAddr();
}
System.out.println(remoteAddr);
}
return remoteAddr;
}
```
Note : There is the possibility that load balancer or reverse proxy server doesn't forward Ip in X-Forwarded-For Header , then you have to find that header key that holds the IP of the client. If you find that please configure it on WebLogic the samely as you have done in X-Forwarded-For Header.

#### To find headers details use the following code :

```private Map<String, String> getRequestHeadersInMap(HttpServletRequest request) { 

Map<String, String> result = new HashMap<>();
Enumeration headerNames = request.getHeaderNames();
while (headerNames.hasMoreElements()) {
String key = (String) headerNames.nextElement();
String value = request.getHeader(key);
result.put(key, value);
}
return result;
}
```