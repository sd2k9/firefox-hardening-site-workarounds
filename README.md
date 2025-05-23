Firefox Hardening Site Workarounds
==================================

See as Github Page:  
https://sd2k9.github.io/firefox-hardening-site-workarounds/

---

### Overview

Hardening firefox for privacy breaks some ill-behaving sites.  
Temporary setting changes to workaround these failures are collected here.

Please contribute updates or new sites via issue, merge request or mail.

Hardening Sources (to just name a few)
- [https://www.privacy-handbuch.de/handbuch_21.htm](https://www.privacy-handbuch.de/handbuch_21.htm)
- [https://github.com/ghacksuserjs/ghacks-user.js](https://github.com/ghacksuserjs/ghacks-user.js)
- [https://www.kuketz-blog.de/firefox-aboutconfig-user-js-firefox-kompendium-teil10](https://www.kuketz-blog.de/firefox-aboutconfig-user-js-firefox-kompendium-teil10/)
- [https://github.com/pyllyukko/user.js](https://github.com/pyllyukko/user.js)

Hardening Add-ons
- [uMatrix](https://addons.mozilla.org/en-US/firefox/addon/umatrix/)
- [NoScript](https://addons.mozilla.org/en-US/firefox/addon/noscript/)
- [CanvasBlocker](https://addons.mozilla.org/en-US/firefox/addon/canvasblocker/)
- [Location Guard](https://addons.mozilla.org/en-US/firefox/addon/location-guard/)

License: [Creative Commons Attribution-ShareAlike 4.0](LICENSE.txt)

### Firefox Error Messages

Error code: MOZILLA_PKIX_ERROR_OCSP_RESPONSE_FOR_CERT_MISSING
  - Text: Secure Connection Failed  
    An error occurred during a connection to website. The OCSP response does not
    include a status for the certificate being verified.
  - Solution  
    security.OCSP.require = false

Error code: SSL_ERROR_UNSAFE_NEGOTIATION
  - Text: Secure Connection Failed  
    An error occurred during a connection to website.
    Peer attempted old style (potentially vulnerable) handshake.
  - Solution  
    security.ssl.require_safe_negotiation = false

Error code: SSL_ERROR_UNSUPPORTED_VERSION
  - Text: Secure Connection Failed  
    An error occurred during a connection to XXX.
    Peer using unsupported version of security protocol.
  - Solution  
    Unknown; Use a default Firefox profile instead

Error code: MOZILLA_PKIX_ERROR_ADDITIONAL_POLICY_CONSTRAINT_FAILED
  - Text: Your connection is not secure  
    The certificate does not come from a trusted source.
  - Solution  
    Add exception or have the site owner to use a valid certificate

Error code: SEC_ERROR_OCSP_SERVER_ERROR
  - Text: Secure Connection Failed
  - Solution  
    security.OCSP.require = false (secure default: true)

### General Behaviour

Downloading/Saving file only outputs "401 NOT AUTHORIZED"
  - File content can be viewed in Firefox, but not saved
  - Solution  
    privacy.firstparty.isolate = false

File Save As... fails in Firefox+uMatrix
  - References
    - [https://github.com/uBlockOrigin/uMatrix-issues/issues/92](https://github.com/uBlockOrigin/uMatrix-issues/issues/92)
    - [https://bugzilla.mozilla.org/show_bug.cgi?id=1343466](https://bugzilla.mozilla.org/show_bug.cgi?id=1343466)
  - See the failing request in the uMatrix Logger
    - Directly enable it there by right-clicking on the "--" in the blocked line
  - Solution  
    Either allow "other" for the source location
    or  
    Open Tools/Download (Ctrl-Shift-Y) and click "Reload" icon

Time displayed has some hours offset to local time
  - When resistFingerprinting is enabled the time zone is spoofed to UTC
  - Solution  
    privacy.resistFingerprinting = false  (secure default: true)
  - See [Mozilla Bug 1364261](https://bugzilla.mozilla.org/show_bug.cgi?id=1364261)


### General Web Cancer

Google ReCaptcha
  - umatrix
    - allow css,image,script,xhr,frame, www.google.com
    - allow css,script       www.gstatic.com
  - Canvasblocker: Disable

### Websites

[kraken.com](https://kraken.com)
  - No login possible
  - Solution  
    network.http.referer.trimmingPolicy = 1     (secure default: 2)  
    network.http.sendRefererHeader      = 2     (secure default: 0)

[addons.thunderbird.net](https://addons.thunderbird.net),
[accounts.firefox.com](https://accounts.firefox.com)
  - No login possible
  - Solution  
    network.http.sendRefererHeader = 1     (secure default: 0)

[www.windy.com](https://www.windy.com),
[www.windfinder.com](https://www.windfinder.com), etc.
  - "Snowstorm"-like display
  - Solution  
    privacy.resistFingerprinting = false  (secure default: true)

[www.xilinx.com/registration/sign-in.html](https://www.xilinx.com/registration/sign-in.html)
  - Login redirects to Maintenance page
  - Tool download not working
  - Solution  
    network.http.sendRefererHeader = 2    (secure default: 0)

[share.garmin.com](https://share.garmin.com),
[inreach.garmin.com](https://inreach.garmin.com)
  - Left-Bar does not come up, no tracks visible
  - Solution  
    network.http.referer.trimmingPolicy=1     (secure default: 2)  
    network.http.sendRefererHeader=2          (secure default: 0)

[inreach.garmin.com](https://inreach.garmin.com)
  - Login not possible (unknown error)
  - Solution  
    network.http.referer.trimmingPolicy=1     (secure default: 2)  
    network.http.sendRefererHeader=2          (secure default: 0)
  - Login does not continue (spinnng wheel)
  - Solution  
    Reload page (F5)

[www.kickstarter.com](https://www.kickstarter.com) Payment
  - unknown_error when paying
  - Solution  
    Disable CanvasBlocker
  - Better solution available?

Local [gpx2map](https://github.com/sd2k9/gpx2map) with Google Maps
  - No map visible
  - Solution  
    uMatrix disable local scope filtering
  - Better solution available?

Local [gpx2map](https://github.com/sd2k9/gpx2map) Openstreetmap Map
  - No map visible
  - Solution  
    uMatrix enable scripts 1st party, cloudflare, openstreetmaps,  
    images for google.com to show markers

[www.kleinanzeigen.de](https://www.kleinanzeigen.de)
  - Pictures added to announce are all blank
  - Solution  
    privacy.resistFingerprinting = false (secure default: true)

[web.familinkframe.com](https://web.familinkframe.com)
  - Pictures uploaded are all blank
  - Solution  
    privacy.resistFingerprinting = false (secure default: true)

[alternate.de](https://www.alternate.de)
  - Cannot add items to basket, cannot use filtering functions etc.
    - Spinning wheel forever
  - No login possible
    - Error message: _Das hat nicht funktioniert_
    - Redirect to https://www.alternate.de/html/help/invalidRequest.html
  - Solution  
    network.http.referer.trimmingPolicy = 1     (secure default: 2)  
    network.http.sendRefererHeader      = 2     (secure default: 0)

[bing.com/maps](https://www.bing.com/maps)
 - Map does not display
    - Firefox shows error message concerning CanvasBlocker slowing down the page
  - Solution  
    Add to CanvasBlocker Whitelist: www.bing.com/maps
  - Better: Use [Openstreetmap](https://www.openstreetmap.org/) and
    [Openrouteservice](https://maps.openrouteservice.org/) instead

[google.com/maps](https://www.google.com/maps)
  - Left bar is empty
  - Empty consent frame from "consent.google.com" blocks the screen
  - Solution  
    NoScript: Enable WebGL (trusted)  
    uMatrix: Cookies, Frame, Other
  - Better: Use [Openstreetmap](https://www.openstreetmap.org/) and
    [Openrouteservice](https://maps.openrouteservice.org/) instead

[my-hammer.de](https://www.my-hammer.de)
  - Pictures uploaded contain only color bars
  - Solution  
    privacy.resistFingerprinting = false (secure default: true)

[teams.microsoft.com](https://teams.microsoft.com/)
  - Login not possible / Get stuck in login look with error message
  - Solution  
    network.cookie.cookieBehavior = 4  (secure default: 5)
  - Same setting in GUI
    - about:preferences#privacy
    - Custom/Cookies/Cross-site cookies --> Cross-site _tracking_ cookies
  - uMatrix permissions
    ```
    teams.microsoft.com cdn.office.net script allow
    teams.microsoft.com cdn.office.net xhr allow
    teams.microsoft.com login.live.com frame allow
    teams.microsoft.com login.microsoftonline.com cookie allow
    teams.microsoft.com login.microsoftonline.com frame allow
    teams.microsoft.com login.microsoftonline.com script allow
    teams.microsoft.com login.microsoftonline.com xhr allow
    teams.microsoft.com teams.events.data.microsoft.com xhr allow
    teams.microsoft.com teams.microsoft.com cookie allow
    teams.microsoft.com teams.microsoft.com frame allow
    teams.microsoft.com teams.microsoft.com script allow
    teams.microsoft.com teams.microsoft.com xhr allow
    login.microsoftonline.com aadcdn.msauth.net script allow
    login.microsoftonline.com aadcdn.msauth.net xhr allow
    login.microsoftonline.com aadcdn.msftauth.net script allow
    login.microsoftonline.com aadcdn.msftauth.net xhr allow
    login.microsoftonline.com login.live.com frame allow
    login.microsoftonline.com login.microsoftonline.com cookie allow
    login.microsoftonline.com login.microsoftonline.com script allow
    login.microsoftonline.com login.microsoftonline.com xhr allow
    login.microsoftonline.com teams.microsoft.com cookie allow
    login.microsoftonline.com teams.microsoft.com script allow
    ```


[outlook.office.com](https://outlook.office.com)
  - Login not possible / Get stuck in login look with error message
  - uMatrix permissions
    ```
    outlook.office.com 1st-party cookie allow
    outlook.office.com 1st-party script allow
    outlook.office.com 1st-party xhr allow
    outlook.office.com aria.microsoft.com xhr allow
    outlook.office.com cdn.office.net script allow
    outlook.office.com data.microsoft.com xhr allow
    outlook.office.com YOURCOMPANY cookie allow
    outlook.office.com login.microsoftonline.com cookie allow
    outlook.office.com login.microsoftonline.com xhr allow
    outlook.office.com office.net xhr allow
    outlook.office.com static.microsoft script allow
    outlook.office.com static.microsoft xhr allow
    ```

[web.microsoftstream.com](https://web.microsoftstream.com/)
  - Login not possible / Get stuck in login look with error message
  - uMatrix permissions
    ```
    web.microsoftstream.com 1st-party cookie allow
    web.microsoftstream.com 1st-party script allow
    web.microsoftstream.com 1st-party xhr allow
    web.microsoftstream.com azureedge.net script allow
    web.microsoftstream.com azureedge.net xhr allow
    web.microsoftstream.com cdn.office.net script allow
    web.microsoftstream.com login.microsoftonline.com cookie allow
    ```

[eu.docusign.net](https://eu.docusign.net/),
[app.docusign.com](https://app.docusign.com/)
  - Signature not possible
  - Solution  
    browser.link.open_newwindow = 2 (new window) or 3 (new tab) (secure default: 1)

[web.whatsapp.com](https://web.whatsapp.com/)
  - Signin not possible
    - Solution  
      javascript.options.wasm = true (secure default: false)
    - Can disable again after loading the page
  - Signin data is lost after browser restart
    - Solution
      1. Disable cookie and site data deletion when Firefox is closed
      1. Or make a backup of the data after signin  
         In Firefox profile at `storage/default/https+++web.whatsapp.com*`
      1. Copy to this location again before starting Firefox the next time

[www.bahn.de](https://www.bahn.de/)
  - Signin not possible
    - Error message: 429 Too Many Requests (nginx)
  - Solution
    1. Enable webgl in NoScript
    1. webgl.disabled = false (secure default: true)
    1. webgl.min_capability_mode = false (secure default: true)

[couchers.org](https://couchers.org/)
  - Map search not usable
  - Solution (not complete)
    1. Enable webgl in NoScript
    1. webgl.disabled = false (secure default: true)
    1. webgl.min_capability_mode = false (secure default: true)

[register.gotowebinar.com](https://register.gotowebinar.com/)
  - Login not possible
  - uMatrix permissions
    ```
    gotowebinar.com 1st-party cookie allow
    gotowebinar.com 1st-party script allow
    gotowebinar.com 1st-party xhr allow
    gotowebinar.com cdn.recordingassets.logmeininc.com media allow
    gotowebinar.com gotomeeting.com xhr allow
    ```

[pendla.com](https://pendla.com/)
  - Map search not usable
  - Solution (not complete)
    1. Enable webgl in NoScript
    1. webgl.disabled = false (secure default: true)
    1. webgl.min\_capability\_mode = false (secure default: true)

[one.element.io](https://one.element.io/)
  - Signin not possible
    - Solution  
      javascript.options.wasm = true (secure default: false)  
      dom.webaudio.enabled = true (secure default: false)
    - Can disable again after loading the page
  - Media is not visible and cannot be downloaded
    - [Solution](https://github.com/element-hq/element-web/issues/28370)  
      dom.serviceWorkers.enabled = true (secure default: false)  
    - Can disable again after loading the page
    - Sometimes need to manually start the service worker  
      Web Developer Tools (Ctrl+Shift+I) / Application / Service Workers / one.element.io sw.js
  - Signin data is lost after browser restart
    - Solution
      1. Disable cookie and site data deletion when Firefox is closed
      1. Or make a backup of the data after signin  
         In Firefox profile at `storage/default/https+++one.element.io`
      1. Copy to this location again before starting Firefox the next time
  - Security related actions which require re-login don't complete
    - Solution  
      browser.link.open_newwindow = 2 (new window) or 3 (new tab) (secure default: 1)

[COMPANY.atlassian.net/jira/](https://COMPANY.atlassian.net/jira/)
  - Login not possible
  - Operation not possible
  - Solution (not complete)
    1. uMatrix permissions
      ```
      COMPANY.atlassian.net 1st-party cookie allow
      id.atlassian.com 1st-party cookie allow
      id.atlassian.com 1st-party script allow
      id.atlassian.com 1st-party xhr allow
      id.atlassian.com web-security-reports.services.atlassian.com other allow
      id.atlassian.com id-frontend.prod-east.frontend.public.atl-paas.net script allow
      id.atlassian.com identity-common-frontend.prod-east.frontend.public.atl-paas.net script allow
      login.microsoftonline.com atlassian.com cookie allow
      login.microsoftonline.com id.atlassian.com xhr allow
      login.microsoftonline.com csp.microsoft.com other allow
      ```
