### Intro to RHEL8

issue#1 - subscription-manager

So, the first problem I ran into was dealing with `subscription-manager`, which of course made me feel like it was time to retire from a life of Linux & open a pet shop or something, vecause if you can't register a machine...

I digress.

Anyway, registering didn't seem to be working for Workstation, so after a tiny bit of Google-fu I found that this _might_ be caused due to my RHEL beta entitlement not including Workstation. NBD, let's just wipe & retry.

After reinstalling server editon (which I confirmed matched my subscription beforehand), I got the same error, _again_, 

````
Service level "Self Support" is not available to units of organization [redacted].
````

at which point I start to feel a bit anxious. More searching led me to a [RHEL KNowledgebase article][subman] with a workaround that worked like a charm. Apparently, there's a [bug][buggo] that does a thing that isn't nice & is breaking stuff, but as a workaround, those attempting to register their machines with RHEL8 can. do so by first unsetting the preselected `service-level`, like so:

````
# subscription-manager service-level --unset
````

Once the service-level is unset, you should be able to register normally using `subscription-manager register...`.








[buggo]: https://bugzilla.redhat.com/show_bug.cgi?id=1650941
[subman]: https://access.redhat.com/solutions/3702301