# My personal thoughts on FullStack!
The thoughts and the flow here comes from my extensive readings and work experience. I try to follow best practices and conventions all the time, but I am only in the beginning of the road and technology/stack changes all the time. Nothing here is final, but rather it's a roadmap to get started on. Everyone has his/her own way of doing things and no one is right or wrong, what's more important is to follow and stick with guideline you and your team pick, and be consistent throughout. 

The only thing I would like to add/improve on here is integrating TypeScript (which I kind put my foot on already) and Testing. I do try my best to use JSDoc to the fullest and take advantage of the existing libraries' typedefinitions when writing JSDoc documentation but I would still like to completely move on TS. As for testing, although I believe that it's a very important step, you need some time and effort to create proper tests and for a beginner, it's too much time consuming (mainly concerned with asnyc/redux testing). I am sure that this will change once I properly get into testing but I am already very busy so this is not a priority right now. 
<br />
<br />
 
## DATABASE
---
### Firebase: (Serverless/Backendless - SQL)
- "NoSQL"
- Very easy to get started on
- Can manage your data on cloud
- Can skip building a backend
- Has lots of features, a few to note are:
	- realtime-database
	- storage
	- 1-click auth
	- cloud functions
### Atlas - Mongo DB: (Cloud database)
- "NoSQL"
- Can manage your data on cloud
- Advanced querying
- Recommended for scale
### Postgress: (Can integrate into heroku)
- Relational
- Will be on cloud as long as you host it
- Advanced querying
- Recommended for scale  

## BACKEND
---
### "Cloud" vs Server (My preference):
- If you have simple api, mainly for CRUD, Cloud functions are perfect and easy
	- Cold starts should not be a huge problem at lower scale
- Server if you have rather more complex system and want it running all the time
	- Eg: I would prefer using sockets on a server
		- Cloud is capable of real-time through other means as well, i.e. SWR
	
### Free-tier providers (My preferences):
- Heroku (server)
- Vercel (cloud)
<br />
<br />
 
## FRONTEND
---
### CORE: React
- Adopt hooks
  - If you are repeating yourself, extract is as custom hook
  - If it doesn't make sense as a hook, extract as utility
- Create as small components as possible
  - If it's a directly related component, keep it in the same folder
  - Else create a new component under /components
- Take advantage of React.memo (not to be confused with useMemo)
  - Not super important in the beginning, just something to keep in mind
### STATE MANAGEMENT (Global/Providers): Redux
- We want to avoid using redux as much as possible
- Want to limit it to providers only
- Only things that need to be shared across the app
### UI: Material-ui (Mui)
- Build core components based on Mui
- Avoid using default html unless it's for simple cases
  - This is to ensure the theme applies all across the app
### FOLDER/APP STRUCTURE
```
/internals
	/generators
		/component
			index.js
			{...templateFiles}.{templatingExtension}
		/component
			index.js
			{...templateFiles}.{templatingExtension}
		/component
			index.js
			{...templateFiles}.{templatingExtension}
		/utils
			index.js
			{someGeneratorUtils}.js
		index.js
	/scripts
		{someUsefulBuildOrLintScript}.js
/src
	/api
		rest.js
		{someEntity}Api.js
	/components
		/SomeComponent
			index.js
			Lazy.js
			SomeComponent.js
			{SomeComponentButton}.js
			{Wrapper}.js
			types.js
		types.js
	/containers
		/SomeContainer
			index.js
			Lazy.js
			SomeContainer.js
			types.js
	/providers
		/SomeProvider
			actions.js
			constants.js
			index.js
			SomeProvider.js
			reducer.js
			selectors.js
	/routes
		/pages
			{SomePage}.js
		index.js
		RouteWrapper.js
	/hooks
		/redux
			{useSomeReduxHook}.js
		{useSomeHook}.js
	/store
		index.js
		reducers.js
	/utils
		index.js
		{specificUtilGroup}.js
		...
	index.js
	theme.js
	icons.js
{...configs}.{js/json}
``` 

### FOLDER/APP STRUCTURE (Explained)
```
/internals
	/generators
		==> Templating engine configuration files
		==> Allows you to create the boilerplate code instantly
		==> i.e. npm run generate component MyComponent
	/scripts
		==> Special scripts you want to run your code agains
		==> Could be generator scripts, linter, and bundle analyzers
/src
	/api ==> API endpoints
		rest.js	==> Holds the utilities and core methods for the REST operations
		{someEntity}Api.js	==> Contains entity specific REST calls 
	/components  ==> "Dumb" components, components with simple/no logic
		/{SomeComponent}
			index.js ==> 1-liner, exports the {SomeComponent} as default
			Lazy.js	==> 2-liner, exports a lazy-loadable component
			{SomeComponent}.js ==> Core component file
			{SomeComponentButton}.js ==> Smaller directly-related components
			{Wrapper}.js ==> Maybe some wrapper to hold smaller components
			types.js ==> propTypes and defaultProps exported from the file (for JS project)
		types.js  ==> shared types (among the components)
	/containers ==> "Smart" components that need specific logic and state and generally contain multiple components
		/App

		/{SomeContainer} ==> Same as above
			index.js
			Lazy.js
			{SomeContainer}.js
			types.js
	/providers ==> "Components" that can be access from any component in the code
		/{SomeProvider} ==> Non-commented are same as above
			actions.js ==> Redux actions (actions to be dispatched and sent to the reducer)
			constants.js ==> Redux constants (holds keys for reducer to understand the action)
			index.js
			{SomeProvider}.js
			reducer.js ==> Redux reducer (holds and manages state based on the actions)
			selectors.js ==> Redux selector (extracts the values from the state and "listens" to changes)
	/routes ==> The core routing logic lives here
		/pages ==> Contains all the pages 
			{SomePage}.js ==> Some page component 
		index.js ==> Wrapped and extracted components to be passed to the app
		RouteWrapper.js ==> Wraps the pages with roles/security/etc.
	/hooks ==> Custom hooks to prevent duplicate code
		/redux ==> Redux hooks to prevent using "connect" in all over the places and handle things through hooks
			{useSomeReduxHook}.js ==> Some redux hook that uses "useDispatch" and/or "useSelector"
		{useSomeHook}.js ==> Some generic hook
	/store ==> Redux store
		index.js ==> Has the store logic, middleware and other tools
		reducers.js ==> Base/core reducers that are to be combined and passed to the store creation
	/utils ==> Utilities shared by the entire app
		index.js ==> Core / generic utilities
		{specificUtilGroup}.js ==> More specific utilities, i.e. "date"
		...
	index.js ==> Boilerplate, creates and and renders the app & store
	theme.js ==> Contains base/default theme, preferably generated through Mui and modified to taste
	icons.js ==> Icons exported with easier-to-remember names so we can just "import SomeIcon from 'icons';"
{...configs}.{js/json} ==> All the configuration files, i.e. package.json, .gitignore, eslint, prettier, jsconfig etc.
```
