# Fabric8 v2 demo

## Pre-demo setup
- Run quickstart script
- Add jadvisor
- Add API registry
- Configure grafana dashboard to show Java memory usage
  - Series: /.*\.java.lang:type=Memory/
  - select: HeapMemoryUsage.used
  - Left Y axis format: bytes
- Build Spring boot webmvc ip quickstart
  - Edit application to use local docker image

## Fabric8 Console
- Diagram
  - Highlight links
  - Clickable elements
- Pods
  - Label filtering
  - Details page
  - Jolokia connection for JVM
- Controllers
  - Highlight scaling but don't do it yet
  - Details view
- Services
  - Details view
  - Address link - disabled until active
  - Pods link
- Apps
  - Grouping of kubernetes items
  - Pods link
  - Controller link
  - Services link
  - App/pod view

- Create application
  - Karaf Quickstart : Camel Rest using SQL
  - Show karaf ssh console
    - Slimline - few bundles
  - Show code that builds it
    - Highlight fabric8 maven plugin properties
  - Wiki app editing

- Show nav bar links
  - Logs
    - Single/multiple pod
  - Metrics
    - Multiple JVMs?
  - API registry
    - Swagger UI for pods

