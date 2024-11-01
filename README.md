# streamMail

Uses stream device protocol to send email notifications from IOC. Based on this Tech Talk discussion: https://epics.anl.gov/tech-talk/2013/msg02050.php

## Requirements

streamDevice: IOC interfaces with sendmail using stream. More info here: https://paulscherrerinstitute.github.io/StreamDevice/
std module: Screens use timer screens from std module. More info here: https://github.com/epics-modules/std

## Adding to an existing IOC

Add the following to the IOC's RELEASE
```
SMAIL = /local/copy/of/streamMail
```
Since the RELEASE is updated, will need to run make. 

Add the following to IOC's st.cmd:
```
iocshLoad("$(SMAIL)/iocsh/streamMail.iocsh", "PREFIX=$(PREFIX),MSERVER=email.server.com,ADDRFROM=sender@address.com,ADDRTO=destination@address.com")
```

Just replace email.server.com, sender@address.com (doesn't need to be a real account), and
desitination@address.com (should be real address).


