#Basic Webcomponents

##OR Console##

##Button##

##Switch##

##Slider##

##Sensor##

##List select#

##Form##

#Advanced Web components#

We are building a series of more advanced webcomponents which still have a general purpose.

##Header##
###Install###

```bash
npm i -S @openremote/or-header
```

###Usage###
```
import '@openremote/or-header';

<or-header logo="${logoImage}">
    <div slot="desktop-left">
        <a href="/map">Map</a>
        <a href="/overview">Overview</a>
    </div>
    <div slot="desktop-right">
        <a>Logout</a>
    </div>
    <div slot="mobile-top">
        <a href="/map">Map</a>
        <a>Logout</a>
    </div>
    <div slot="mobile-bottom">
    </div>
</or-header>
```

##Map##

##Scheduler##

