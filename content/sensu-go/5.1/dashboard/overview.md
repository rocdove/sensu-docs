---
title: "Dashboard Overview"
linkTitle: "Overview"
weight: 120
version: "5.1"
product: "Sensu Go"
platformContent: false
menu:
  sensu-go-5.1:
    parent: dashboard
---

The Sensu backend includes the **Sensu dashboard**:
a unified view of your events, entities, and checks with user-friendly tools to reduce alert fatigue.

![Sensu dashboard events view](/images/dashboard-events.png)

### Accessing the Dashboard
After [starting the Sensu backend][1], you can access the dashboard in your browser
by visiting http://localhost:3000. You may need to replace `localhost` with the
hostname or IP address where the Sensu backend is running.

### Signing In
Sign in to the dashboard with your [sensuctl][2] username and password.
See the [role-based access control reference][3] for [default user credentials][4] and instructions for [creating new users][5].

### Themes
Use the preferences menu to change the theme or switch to the dark theme.

[1]: ../../getting-started/installation-and-configuration/#starting-the-services
[2]: ../../sensuctl/reference/
[3]: ../../reference/rbac
[4]: ../../reference/rbac#default-user
[5]: ../../reference/rbac#creating-a-user
