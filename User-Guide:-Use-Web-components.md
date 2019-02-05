# OR Header
## Install

```bash
npm i -S @openremote/or-header
```

## Usage
`
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
`