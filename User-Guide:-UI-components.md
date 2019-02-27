We have a series of UI components available to swiftly create your own front end applications. We use material design.... We'll introduce all the components.

In the demo environment we have a Realm called '[[Smart Building|Demo-Smart-Building]]'. There is a front-end example which illustrates how the Basic web components work.

# Basic UI components

## Button

## Radio Button

## Switch

## Checkbox

## Slider

## Text sensor

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

See [here](../../tree/master/ui/component/or-thermostat).

## Asset tree

## Map

## Header

Will be available in Material Design in the future. This is a more project specific example which we don't want to withhold.

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
- Using Mixin
- Keycloak templates (working on the UI)
- [Demo Smart Building](Demo-Smart-Building)
- [Get Started](https://openremote.io/get-started-manager/)