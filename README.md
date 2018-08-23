# How-to-customize-ANTD-theme-in-create-react-app-without-EJECT

## Ejecting a creat-react-app is sort of a bomb zone eapecially for newbee like me and those who do not want to get into any trouble with wepack later on. So after googling for half a afternoon, I found a way and here to give you some thoughts.

## After create-react-app and install antd in your project, now is the time to do some customization
## Step one
#### install these three packages
- [npm install react-app-rewired --save-dev](https://github.com/timarney/react-app-rewired)
- [npm install react-app-rewire-less --save-dev](https://github.com/timarney/react-app-rewired/tree/master/packages/react-app-rewire-less)
- [npm install babel-plugin-import --save-dev](https://github.com/ant-design/babel-plugin-import)

## Step two
#### since we are going to override webpack cofig without actually editing the cofig file, so we have to create a file named (config-overrides.js) at the root direction of our project.
```javascript
const { injectBabelPlugin } = require('react-app-rewired');
const rewireLess = require('react-app-rewire-less');

module.exports = function override(config, env) {
  config = injectBabelPlugin(['import', { libraryName: 'antd', style: 'css' }], config);
  config = injectBabelPlugin(['import', { libraryName: 'antd', style: true }], config); // change importing css to less
  config = rewireLess.withLoaderOptions({
    modifyVars: { '@primary-color': '#ff0000' },
    javascriptEnabled: true
  })(config, env);
  return config;
};

```

## Step three
#### Since we want to import less file delibrately, we are going to use the bable-plugin-import module, then put these config to your .bablerc.json file(if you cannot find this file, just simply create one yourself)

```json
{
	"plugins": [
		[
			"import",
			{
				"libraryName": "antd",
				"style": true
			}
		]
	]
}
```
## Step four dont worry we are almost there
#### All work done, now we are going to edite your package.json to make sure using the overrided config to start build or test

```json
	"scripts": {
		"start": "react-app-rewired start",
		"build": "react-app-rewired build",
		"test": "react-app-rewired test --env=jsdom",
		"eject": "react-scripts eject"
	},
```

