# Dependencies
Run this command at the root of the project directory
```
git clone https://github.com/ericjim/microsite-ui
cd ./microsite-ui
npm install
git checkout fix-use-guides
```
This will include `microsite-ui` a library with ejs partials which can be used to compose a microsite. The `fix-use-guides` branch is for sites that contain a guides section. Once all sites have this feature, we can merge this branch in. For now, only Mobile Toolkit has one.


# Development
```
> npm install -g harp
```

```
> harp server # Will host a local dev server
```

# Production

```
> harp compile # Will output a www/ folder
> firebase deploy # Will go live
```