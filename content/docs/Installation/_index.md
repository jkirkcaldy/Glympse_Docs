---
title: Installation
next: /docs/Installation/database
weight: 1
---
To install Glympse, you will need a database, a redis cache and a rabbitmq queue all of which must be accessible by all of the Glympse containers you deploy. For that reason it is recommended to keep this as a separate compose file. 

{{< cards >}}
  {{< card link="/docs/installation/database/" title="Database Install" icon="database" >}}
    {{< card link="/docs/installation/services" title="Services" icon="selector" >}}
  {{< card link="/docs/installation/install_glympse" title="Glympse Install" icon="server" >}}
{{< /cards >}}