---
# This page was generated from the add-on.
title: Ajax Spider Automation Framework Support
type: userguide
weight: 1
---

# Ajax Spider Automation Framework Support

This add-on supports the Automation Framework.

## Job: spiderAjax

The spiderAjax job allows you to run the Ajax Spider - it is slower than the traditional spider but handles modern web applications well.


It is covered in the video: [ZAP Chat 10 Automation Framework Part 4 - Spidering](https://youtu.be/WivoyVerBCo).


This job supports monitor tests.

```
  - type: spiderAjax                   # The ajax spider - slower than the spider but handles modern apps well
    parameters:
      context:                         # String: Name of the context to spider, default: first context
      user:                            # String: An optional user to use for authentication, must be defined in the env
      url:                             # String: Url to start spidering from, default: first context URL
      maxDuration:                     # Int: The max time in minutes the ajax spider will be allowed to run for, default: 0 unlimited
      maxCrawlDepth:                   # Int: The max depth that the crawler can reach, default: 10, 0 is unlimited
      numberOfBrowsers:                # Int: The number of browsers the spider will use, more will be faster but will use up more memory, default: number of cores
      runOnlyIfModern:                 # Boolean: If true then the spider will only run if a "modern app" alert is raised, default: false
      inScopeOnly:                     # Boolean: If true then any URLs requested which are out of scope will be ignored, default: true
      browserId:                       # String: Browser Id to use, default: firefox-headless
      clickDefaultElems:               # Bool: When enabled only click the default element: 'a', 'button' and 'input', default: true
      clickElemsOnce:                  # Bool: When enabled only click each element once, default: true
      eventWait:                       # Int: The time in milliseconds to wait after a client side event is fired, default: 1000
      maxCrawlStates:                  # Int: The maximum number of crawl states the crawler should crawl, default: 0 unlimited
      randomInputs:                    # Bool: When enabled random values will be entered into input element, default: true
      reloadWait:                      # Int: The time in milliseconds to wait after the URL is loaded, default: 1000
      scopeCheck:                      # String: The scope check, either Flexible or Strict, default: Strict
      elements:                        # A list of HTML elements to click - will be ignored unless clickDefaultElems is false
      - "a"
      - "button"
      - "input"
      excludedElements:                 # A list of HTML elements to exclude from click.
        - description: "Logout Button"  # String: Description of the exclusion.
          element: "button"             # String: Name of the element.
          xpath:                        # String: XPath of the element, optional.
          text:                         # String: Text of the element (exact match and case sensitive), optional.
          attributeName: "aria-label"   # String: Name of the attribute, optional unless the value is provided.
          attributeValue: "Logout"      # String: Value of the attribute, optional unless the name is provided.
      
    tests:
      - name: 'At least 100 URLs found'      # String: Name of the test, default: statistic + operator + value
        type: 'stats'                        # String: Type of test, only 'stats' is supported for now
        statistic: 'spiderAjax.urls.added'   # String: Name of an integer / long statistic, currently supported: 'spiderAjax.urls.added'
        operator: '>='                       # String ['==', '!=', '>=', '>', '<', '<=']: Operator used for testing
        value: 100                           # Int: Change this to the number of URLs you expect to find
        onFail: 'info'                       # String [warn, error, info]: Change this to 'warn' or 'error' for the test to take effect
```

If the 'runOnlyIfModern' is set to 'True' then the [passiveScan-wait](/docs/desktop/addons/automation-framework/job-pscanwait/) job MUST be run before this one (as well as after it) and the [Modern Web Application](/docs/alerts/10109/) rule installed and enabled. If either of those things are not done then the ajax spider will always run and a warning output. If they are both done and no "Modern Web Application" alert is raised then the assumption is made that this is a tradition app and therefore the ajax spider is not needed.
