---
# This page was generated from the add-on.
title: Header Based Session Management
type: userguide
weight: 2
---

# Header Based Session Management

This [add-on](/docs/desktop/addons/authentication-helper/) adds a new session management type which supports an arbitrary number of headers.

The header values can include the following tokens:

|   |                         |                                   |
|---|-------------------------|-----------------------------------|
|   | `{%json:path.to.data%}` | JSON authentication response data |
|   | `{%env:env_var%}`       | Environmental variable            |
|   | `{%script:glob_var%}`   | Global script variable            |
|   | `{%header:env_var%}`    | Authentication response header    |
|   | `{%url:key%}`           | Authentication URL param          |

When adding Header Based Session Management via the API the `headers` parameter is a string of `header:value` pairs separated by newline characters: `\n`.

Note that due to restrictions in the core:

* Existing contexts are not updated in the GUI if you add or remove this add-on
* Header Based Session Management cannot be added to a context via the API

These restrictions will be addressed in a future release.

Latest code: [HeaderBasedSessionManagementMethodType.java](https://github.com/zaproxy/zap-extensions/blob/main/addOns/authhelper/src/main/java/org/zaproxy/addon/authhelper/HeaderBasedSessionManagementMethodType.java)