# Main [Doc](http://google.com/)

This is my main documentation

## Table of Contents

This is my TOC [content](http://google.com/)

#### Lightbox Demo

The following images source will render in image gallery:

![GitHub Logo](assets/GitHub%20Logo.jpg)

![GH Wallpaper](assets/GH%20Wallpaper.jpg)

![AI](assets/AI.jpg)

# Test PlantUML diagram

```plantuml
@startuml Login flow
autonumber 1
actor user as usr
participant WorkList as wl
participant browser as brw
box Reporting
    participant ApplicationService as aps
    participant FrontEndService as fes
    participant LaunchCapabilityService as lcs
end box

== Initialization application service ==
loop for all configured applications
aps -> lcs++: get /<configured-url>/.well-known/launchcapability
activate aps
return launchcapability
aps -> aps: add launchcapability to launchcapabilities
aps -> lcs++: get /configured-url/.well-known/jwks
return jwks
aps -> aps: store jwks
end loop
deactivate aps

== Initialization external service ==
wl -> aps++: get /Applications
activate wl
return launchcapabilities
deactivate wl


== Launch from a worklist ==
usr -> wl: open reporting for procedure P
activate wl
wl -> aps++: get reporting/jwks
return jwks
wl -> wl: Z1=encrypt(procedure identifier with jwks)
wl -> brw: open URL /reporting?jwe=Z1
deactivate wl
activate brw
brw -> fes++: open /reporting?jwe=Z1
fes -> lcs++: decode Z1
return procedure.identifier=P
return reporting procedure P
brw -> aps++: /Applications
return launchcapabilities
loop for all other applications
    brw -> aps++: get /Applications/Token?applicationName=Logging
    return jwks
end loop
deactivate brw

== Launch from reporting ==
usr -> brw: open logging
activate brw
brw -> brw: Z2=encrypt(procedure.identifier with jwks[logging])
brw -> fes++: open url /logging?jwe=Z2
fes -> lcs++: decode Z2
return procedure.identifier=P
return logging procedure P

@enduml
```
