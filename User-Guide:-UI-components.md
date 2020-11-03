We have a series of UI components available to swiftly create your own front end applications. We use a combination of Polymer LIT, Material Design and our own OpenRemote elements. The UI components are [published on NPM](https://www.npmjs.com/~openremote).

_See the [Developer Guide for the current status](./Developer-Guide:-Working-on-the-UI)_

We have a series of basic UI components, and more advanced project specific components. The current available components can be found in [openremote / ui / components](https://github.com/openremote/openremote/tree/master/ui/component):
- or-asset-tree
- or-asset-viewer
- or-attribute-card
- or-attribute-history
- or-attribute-input
- or-bottom-navigation
- or-chart
- or-data-viewer
- or-icon
- or-input
- or-log-viewer
- or-map
- or-mwc-componenents
- or-panel
- or-rules
- or-smart-notify
- or-survey-results
- or-survey
- or-table
- or-thermostat
- or-timeline
- or-translate

## Header example

This is a mere example for a non existing component. It's published on NPM.

### Install

```bash
npm i -S @openremote/or-header
```

### Usage
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

# See Also
- [Developer Guide: Working on the UI](./Developer-Guide:-Working-on-the-UI)
- [Demo Smart City](Demo-Smart-City)
- [Get Started](https://openremote.io/get-started-manager/)