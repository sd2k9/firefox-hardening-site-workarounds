Firefox Hardening Site Workarounds
==================================


### Overview

Hardening firefox for privacy breaks some ill-behaving sites.  
Temporary setting changes to workaround these failures are collected here.

Please contribute updates or new sites via issue, merge request or mail.

Hardening Sources (to just name a few)
- https://www.privacy-handbuch.de/handbuch_21.htm
- https://github.com/ghacksuserjs/ghacks-user.js
- https://www.kuketz-blog.de/firefox-aboutconfig-user-js-firefox-kompendium-teil10/
- https://github.com/pyllyukko/user.js

Hardening Add-ons
- uMatrix
- NoScript
- CanvasBlocker
- Location Guard

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
    security.ssl.require_safe_negotiation = true

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


### General Behaviour

Downloading/Saving file only outputs "401 NOT AUTHORIZED"
  - File content can be viewed in Firefox, but not saved
  - Solution  
    privacy.firstparty.isolate = false


### Websites

kraken.com
  - No login possible
  - Solution  
    network.http.referer.trimmingPolicy = 1     (secure default: 2)  
    network.http.sendRefererHeader      = 2     (secure default: 0)

openrouteservice.org
  - No routing possible
  - Error messages in Firefox console  
    Cross-Origin Request Blocked: The Same Origin Policy disallows reading the remote resource at https://api.openrouteservice.org/pdirections?api_key=58d904a497c67e00015b45fc75e30fe544834c7a97ebc3ce784d6e4f&attributes=detourfactor%7Cpercentage&coordinates=10.245566,51.726045%7C10.416713,51.799539&elevation=true&extra_info=steepness%7Cwaytype%7Csurface&geometry=true&geometry_format=geojson&instructions=true&instructions_format=html&options=%7B%22avoid_borders%22:%22%22,%22avoid_countries%22:%22%22%7D&preference=fastest&profile=foot-walking&units=m. (Reason: CORS header ‘Access-Control-Allow-Origin’ missing).  
    Cross-Origin Request Blocked: The Same Origin Policy disallows reading the remote resource at https://api.openrouteservice.org/pgeocoding?api_key=58d904a497c67e00015b45fc75e30fe544834c7a97ebc3ce784d6e4f&lang=en&limit=1&location=10.245566,+51.726045. (Reason: CORS header ‘Access-Control-Allow-Origin’ missing).  
    Cross-Origin Request Blocked: The Same Origin Policy disallows reading the remote resource at https://api.openrouteservice.org/pgeocoding?api_key=58d904a497c67e00015b45fc75e30fe544834c7a97ebc3ce784d6e4f&lang=en&limit=1&location=10.416713,+51.799539. (Reason: CORS header ‘Access-Control-Allow-Origin’ missing).  
    Cross-Origin Request Blocked: The Same Origin Policy disallows reading the remote resource at https://api.openrouteservice.org/pplaces?api_key=58d904a497c67e00015b45fc75e30fe544834c7a97ebc3ce784d6e4f&request=category_list. (Reason: CORS header ‘Access-Control-Allow-Origin’ missing).
  - Known bug, will not fix  
    https://github.com/GIScience/openrouteservice-app/issues/193
  - Solution  
    network.http.referer.XOriginPolicy = 1 (secure default: 2)  
    network.http.sendRefererHeader = 2     (secure default: 0)

https://addons.thunderbird.net, https://accounts.firefox.com
  - No login possible
  - Solution  
    network.http.sendRefererHeader = 1     (secure default: 0)

https://www.windy.com, https://www.windfinder.com, etc.
  - "Snowstorm"-like display
  - Solution  
    privacy.resistFingerprinting = false  (secure default: true)

https://www.xilinx.com/registration/sign-in.html
  - Login redirects to Maintenance page
  - Solution  
    network.http.sendRefererHeader = 2    (secure default: 0)

https://share.garmin.com, https://inreach.garmin.com
  - Left-Bar does not come up, no tracks visible
  - Solution  
    network.http.referer.trimmingPolicy=1     (secure default: 2)  
    network.http.sendRefererHeader=2          (secure default: 0)

https://inreach.garmin.com
  - Login not possible (unknown error)
  - Solution  
    network.http.referer.trimmingPolicy=1     (secure default: 2)  
    network.http.sendRefererHeader=2          (secure default: 0)
  - Login does not continue (spinnng wheel)
  - Solution  
    Reload page (F5)

https://www.kickstarter.com Payment
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

https://www.ebay-kleinanzeigen.de
  - Pictures added to announce are all blank
  - Solution  
    privacy.resistFingerprinting = false (secure default: true)
