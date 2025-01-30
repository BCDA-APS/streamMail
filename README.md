# streamMail

Uses stream device protocol to send email notifications from IOC.

This support creates "Notifiers".  Each notifier is capable of watching one PV and emailing a recipient when configured condition is met.

The watched PV is compared to a user-designated trigger value using a user-designated comparator ('less than', 'less than or equal to', 'equal to', 'more than or equal to', or 'more than')

The user can set the rate of email notifications and how notifications are stopped.  Notifications can be sent:

- just once after the condition is met and then its disables.
- repeatably after the condition is met until the user clears the notifier. Notifications are sent at the user-selected rate regardless if the condition is still met
- after first time condition is met and then only if the condition is still met.  It's rechecked at the user-selected rate

There are "user notifiers" that can be configured at run-time whose settings are auto-saved for futures use.

The user can also "pre-define" notifiers using the example_streamMail.substitutions file as an example.

Based on this Tech Talk discussion: https://epics.anl.gov/tech-talk/2013/msg02050.php

## Requirements

- streamDevice: IOC interfaces with sendmail using stream. More info here: https://paulscherrerinstitute.github.io/StreamDevice/

- std module: Screens use timer screens from std module. More info here: https://github.com/epics-modules/std

## Adding to an existing IOC

Add the following to the IOC's RELEASE
```
SMAIL = /local/copy/of/streamMail
```
Since the RELEASE is updated, will need to run make. 

### Adding user notifiers

If only user notifiers are wanted (i.e. 'run-time configured' not 'pre-defined' notifiers) Add the following to IOC's st.cmd:
```
iocshLoad("$(SMAIL)/iocsh/streamMail.iocsh", "PREFIX=$(PREFIX),MSERVER=email.server.com,ADDRFROM=sender@address.com,ADDRTO=destination@address.com")
```
Just replace email.server.com, sender@address.com (doesn't need to be a real account), and
desitination@address.com (should be real address).

### Adding pre-defined notifiers

If pre-defined notifiers are to be used the iocshLoad command loads **streamMail_pd_subs.iocsh** instead needs a substitutions file:
```
iocshLoad("$(SMAIL)/iocsh/streamMail_pd_subs.iocsh", "SUBS_FILE=substitutions/notifiers.substitutions, PREFIX=$(PREFIX),MSERVER=herald.aps.anl.gov,ADDRFROM=sender@address.com,ADDRTO=destination@address.com")
```
In <top>/sMailApp/Db is an example substitutions file (example_streamMail.substitutions) to follow.  
**streamMail_pd_subs.iocsh** will also load 10 user notifiers using the ADDRFROM and ADDRTO as the sender/recipient addresses until overwritten by autosaved changes

## caQtDM screens

### Adding to existing screen
- StreamMail.ui - macros used to load the summary screen: "P=<IOC PREFIX>:,SM=strMail,N1=1,N2=2,N3=3,N4=4,N5=5,N6=6,N7=7,N8=8,N9=9,N10=10"

- StreamMail_single.ui -- macros used to load a single notifier screen (also accessible from summary screen): "P=$(P), SM=$(SM), N=$(N)"

### From command line
Running the main screen from the streamMail ui folder (/streamMail/sMailApp/op/ui) for the devault user notifiers:
```
> caQtDM -style plastique -noMsg -macro "P=25idIpTest:,SM=strMail,N1=1,N2=2,N3=3,N4=4,N5=5,N6=6,N7=7,N8=8,N9=9,N10=10" StreamMail.ui
```
If you have a couple of pre-defined notifiers, then replacs N1=1, N2=2, etc with notifiers labels from the substitution file.  For example, this IOC has two predefined notifiers C1, C2:
```
> caQtDM -style plastique -noMsg -macro "P=25idIpTest:,SM=strMail,N1=C1,N2=C2" StreamMail.ui &
```




