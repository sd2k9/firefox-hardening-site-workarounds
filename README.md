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
- [https://www.kuketz-blog.de/firefox-aboutconfig-user-js-firefox-kompendium-teil10/](https://www.kuketz-blog.de/firefox-aboutconfig-user-js-firefox-kompendium-teil10/)
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

[maps.openrouteservice.org](https://maps.openrouteservice.org)
  - No routing possible
  - Error messages in Firefox console  
    Cross-Origin Request Blocked: The Same Origin Policy disallows reading the remote resource at https://api.openrouteservice.org/pdirections?api_key=58d904a497c67e00015b45fc75e30fe544834c7a97ebc3ce784d6e4f&attributes=detourfactor%7Cpercentage&coordinates=10.245566,51.726045%7C10.416713,51.799539&elevation=true&extra_info=steepness%7Cwaytype%7Csurface&geometry=true&geometry_format=geojson&instructions=true&instructions_format=html&options=%7B%22avoid_borders%22:%22%22,%22avoid_countries%22:%22%22%7D&preference=fastest&profile=foot-walking&units=m. (Reason: CORS header ‘Access-Control-Allow-Origin’ missing).  
    Cross-Origin Request Blocked: The Same Origin Policy disallows reading the remote resource at https://api.openrouteservice.org/pgeocoding?api_key=58d904a497c67e00015b45fc75e30fe544834c7a97ebc3ce784d6e4f&lang=en&limit=1&location=10.245566,+51.726045. (Reason: CORS header ‘Access-Control-Allow-Origin’ missing).  
    Cross-Origin Request Blocked: The Same Origin Policy disallows reading the remote resource at https://api.openrouteservice.org/pgeocoding?api_key=58d904a497c67e00015b45fc75e30fe544834c7a97ebc3ce784d6e4f&lang=en&limit=1&location=10.416713,+51.799539. (Reason: CORS header ‘Access-Control-Allow-Origin’ missing).  
    Cross-Origin Request Blocked: The Same Origin Policy disallows reading the remote resource at https://api.openrouteservice.org/pplaces?api_key=58d904a497c67e00015b45fc75e30fe544834c7a97ebc3ce784d6e4f&request=category_list. (Reason: CORS header ‘Access-Control-Allow-Origin’ missing).
  - Known bug, will not fix  
    [https://github.com/GIScience/openrouteservice-app/issues/193](https://github.com/GIScience/openrouteservice-app/issues/193)
  - Solution  
    network.http.referer.XOriginPolicy = 1 (secure default: 2)  
    network.http.sendRefererHeader = 2     (secure default: 0)

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

[www.ebay-kleinanzeigen.de](https://www.ebay-kleinanzeigen.de)
  - Pictures added to announce are all blank
  - Solution  
    privacy.resistFingerprinting = false (secure default: true)

[web.familinkframe.com](https://web.familinkframe.com)
  - Pictures uploaded are all blank
  - Solution  
    privacy.resistFingerprinting = false (secure default: true)

[alternate.de](https://www.alternate.de)
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


[outlook.office365.com](https://outlook.office365.com)
  - Login not possible / Get stuck in login look with error message
  - uMatrix permissions
    ```
    outlook.office365.com cdn.office.net script allow
    outlook.office365.com cdn.office.net xhr allow
    outlook.office365.com microsoftonline.com cookie allow
    outlook.office365.com outlook.office365.com cookie allow
    outlook.office365.com outlook.office365.com script allow
    outlook.office365.com outlook.office365.com xhr allow
    ```


[eu.docusign.net](https://eu.docusign.net/)
  - Signature not possible
  - Solution  
    browser.link.open_newwindow = 2 (secure default: 1)

[web.whatsapp.com/](https://web.whatsapp.com/)
  - Signin not possible
  - Solution  
    javascript.options.wasm = true (secure default: false)
  - Signin data is lost after browser restart
  - Solution
    1. Disable cookie and site data deletion when Firefox is closed
    1. Or make a backup of the data after signin  
       In Firefox profile at `storage/default/https+++web.whatsapp.com*`
    1. Copy to this location again before starting Firefox the next time
