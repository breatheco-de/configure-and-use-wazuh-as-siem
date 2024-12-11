<!-- hide -->
# Configure and use Wazuh as a SIEM

> By [@rosinni](https://github.com/rosinni) and [other contributors](https://github.com/breatheco-de/configure-and-use-wazuh-as-siem/graphs/contributors) at [4Geeks Academy](https://4geeksacademy.co/)

[![build by developers](https://img.shields.io/badge/build_by-Developers-blue)](https://4geeks.com)
[![Twitter Follow](https://img.shields.io/twitter/follow/4geeksacademy?style=social&logo=twitter)](https://twitter.com/4geeksacademy)

*These instructions are also [available in Spanish](https://github.com/breatheco-de/configure-and-use-wazuh-as-siem/blob/main/README.es.md)*

### Before you start...

> We need you! These exercises are created and maintained in collaboration with people like you. If you find any errors or typos, please contribute and/or report them.
<!-- endhide -->

<onlyfor saas="false" withBanner="false">

## 🌱 How to Start This Project

Through this exercise, we will collect and analyze security events from a Linux endpoint [a Debian machine with WordPress](https://4geeks.com/interactive-coding-tutorial/deploying-wordpress-site-debian), monitoring access, file changes, and simulating potential attacks. We will use Wazuh's capabilities as a SIEM to manage these events.

> ⚠ In case you don't have the **Debian machine with WordPress** available, you can download this [image .ova](https://storage.googleapis.com/breathecode/virtualbox/debian-with-wordpress.ova) with a configured Debian machine with WordPress.

```bash
username: debian
password: 123456
```

</onlyfor>

## 📝 Instructions

### Adding Multiple Data Sources to Wazuh

As a SIEM, Wazuh collects and analyzes data from various sources, such as servers, applications, firewalls, routers, and more. The first step is to ensure that Wazuh is configured to receive logs from these various sources.

- [ ] **Source 1: Wazuh Agents (Endpoints)**
If you already have Wazuh agents installed on your endpoints [as in the EDR exercise](#), these data are one of the sources. Ensure that the agents are active and sending data to the Wazuh Manager. If not, install the agents on the `Debian machine with WordPress`.

- [ ] **Source 2: Application Logs**
Configure Wazuh agents to monitor specific application logs on the endpoints. For example, you can configure an agent to collect logs from a web server or a database server. Edit the agent's configuration file on each endpoint to include the log locations.

Example, to monitor Web Server Logs. In the agent configuration file (/var/ossec/etc/ossec.conf), add the configuration to monitor them.

```xml
<localfile>
  <log_format>apache</log_format>
  <location>/var/log/apache2/access.log</location>
</localfile> 
<localfile>
  <log_format>apache</log_format>
  <location>/var/log/apache2/error.log</location>
</localfile>
```

Additionally, since WordPress does not generate logs by default, enable log generation to record PHP errors and warnings. To enable WordPress error logs, edit the wp-config.php file and add the following lines:

```php
define( 'WP_DEBUG', true );
define( 'WP_DEBUG_LOG', true );
define( 'WP_DEBUG_DISPLAY', false );
```

> 💡 This will generate a log file at /wp-content/debug.log where all PHP errors and warnings related to WordPress will be recorded.

- [ ] **Source 3:** Firewall or Router Logs If you have access to a firewall or router, ensure that the relevant logs are sent to Wazuh via agents. This is typically done by installing a Wazuh agent on the machine handling the firewall logs or configuring the device to send logs via syslog to a machine with a Wazuh agent installed.

> **NOTE:** We will practice with sources 1 and 2.


### Multi-Source Attack Simulation

Now we are going to simulate a distributed attack across different systems.

1. Attempt to log in with incorrect credentials on the endpoint (Debian machine) several times.

2. Modify sensitive files on the endpoint so that Wazuh detects the change.

```bash
sudo echo "Simulating malicious change" >> /etc/passwd
```

3. Simulate Apache access and error logs. Generate activity by accessing the WordPress website and attempt to access a non-existent page to generate a 404 error and verify that it is logged in Apache's log.

### Monitor in the Dashboard

1. Go to the dashboard and observe the events in `Threat Hunting`. Look for event correlation involving failed access attempts, port scans from another machine, and file modifications on the endpoints. Wazuh should generate alerts as it correlates events from these multiple sources.

The results obtained should be something similar to this, where you will be able to see all the simulations performed. Example: `T1078`, which refers to the misuse of credentials, `T1548.003`, which indicates a successful sudo to ROOT, and multiple Apache 400 errors, which may result from failed attempts to access web server resources.

![imagen 1](https://github.com/breatheco-de/configure-and-use-wazuh-as-siem/blob/main/assets/wazuh-siem-results.png?raw=true)

2. Go to the Reports section in Wazuh and generate a report that shows all recent events and alerts. Filter the events by type and source to see the correlations and anomalies detected.

> ⚠ If the events do not appear immediately, review the log collection configuration on the agents and ensure that all sources (such as application and device logs) are properly connected and sending data to the Wazuh Manager.

<!-- hide -->

## Contributors

Thanks to these amazing people ([emoji key](https://github.com/kentcdodds/all-contributors#emoji-key)):

1. [Rosinni Rodriguez (rosinni)](https://github.com/rosinni) contribution: (build-tutorial) ✅, (documentation) 📖
  
2. [Alejandro Sanchez (alesanchezr)](https://github.com/alesanchezr), contribution: (bug reports) 🐛

This project follows the [all-contributors](https://github.com/kentcdodds/all-contributors) specification. Contributions of any kind are welcome!

This and other exercises are used to [learn to code](https://4geeksacademy.com/us/learn-to-code) by students at 4Geeks Academy [Coding Bootcamp](https://4geeksacademy.com/us/coding-bootcamp) led by [Alejandro Sánchez](https://twitter.com/alesanchezr) and many other contributors. Learn more about our [Programming Courses](https://4geeksacademy.com/us/programming-courses) to become a [Full Stack Developer](https://4geeksacademy.com/us/coding-bootcamps/full-stack-developer), or our [Data Science Bootcamp](https://4geeksacademy.com/us/coding-bootcamps/data-science-machine-learning-bootcamp). You can also dive into cybersecurity with our [Cybersecurity Bootcamp](https://4geeksacademy.com/us/coding-bootcamps/cybersecurity-bootcamp).

<!-- endhide -->

