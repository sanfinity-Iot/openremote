We plan to have a series of UI components available to swiftly create your own front end applications. We use a combination of Polymer LIT, Material Design and our own OpenRemote elements. The UI components are [published on NPM](https://www.npmjs.com/~openremote).

_You can follow our progress at this page, and you are welcome to contribute in the mean time!_

In the main branch and online demo, we have a Realm called '[[Tenant A - Smart Building|Demo-Smart-Building]]'. There is a front-end example which illustrates how the Basic UI components will work.

# Basic UI components

We have a series of basic UI components, which you can use to create any user interface. We'll explain how you can change the 'Text sensor' and how you can link it to an attribute in the manager. The other Basic UI components work similar.

## Text sensor

_T.B.D. _

We will describe, as an example, how you can add this component to the Application for the Appartment user in the Demo Smart Building and connect it to the outdoor temperature, you have added as an attribute (see [Connect a HTTP API](User Guide: Connecting to a HTTP API). 

## Button

## Radio Button

## Switch

## Checkbox

## Slider

## Text input

- different types
- validation

## Text area

## Dropdown

## Color picker

## Date / time picker

## Toast

# Composite UI components

We are building a series of composite UI components which still have a general purpose. They can be used for end user apps or for management dashboards.

## Weekschedule

## Scene selector

## Thermostat

See [here](../tree/master/ui/component/or-thermostat).

## Map

## Asset tree

## Rules

## Header

Will be available in Material Design in the future. This is a more project specific example which we don't want to withhold. It's published on NPM.

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

## Bottom app bar

# See Also
- Keycloak templates (working on the UI)
- [Demo Smart Building](Demo-Smart-Building)
- [Get Started](https://openremote.io/get-started-manager/)