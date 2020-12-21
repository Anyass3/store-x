# svelte-storeX
This is inspired by Vuex

It's for now a minimal implementation of the vuex

It uses the svelte store

It makes working with svelte stores somewhat clean and organised

Also the compiled version can be used in any other js library

# installation
`npm install store-x`

Also you can try cdn
```html
<script src="https://cdn.jsdelivr.net/npm/store-x/dist/index.min.js"><script>
```

for old browsers
```html
<script src="https://cdn.jsdelivr.net/npm/store-x/dist/old.index.min.js"><script>
```


# Usage 
example in svelte
> it will be similar in other js frameworks

store1.js
```js
export default {
	state: { // each state value is going to a writable svelte store
		camera: 'off',
	},
	getters: { //
		getCameraState(state){
			return state.camera
		},
	},
	mutations: {
		setCameraState(state, val){
			state.camera.set(val)
		},
	},
	actions: {
		cameraState({commit}, val){
			commit('setCameraState', val)
		},
	}
}
```
you may can add other state values, getters etc


store2.js
```js
export default {
	state: {
		startedVideoStream: false,
	},
	getters: {
		videoIsStreaming(state){
			return state.startedVideoStream
		},
	},
	mutations: {
		setStreamingVideo(state, val){
			state.startedVideoStream.set(val)
		},
	},
	actions: {
		streamingVideo({commit}, val){
			commit('setStreamingVideo', val)
		},
	}
}
```
you may can add other state values, getters etc


now in the main-store-flie

stores.js
```js
import svelteX from 'svelteX';

import store1 from './store1';
import store2 from './store2';

export default sveltex([store1,store2])
```

now in your svelte components

com1.svelte
```svelte
<script>
	import stores from './stores.js'
	const {getCameraState, videoIsStreaming} = stores.getters
	const {streamingVideo, cameraState} = stores.actions
  
	$: cameraState=getCameraState()
	$: streaming=videoIsStreaming()
</script>

<p>camera is {cameraState}</p>
<p>video has {videoIsStreaming?'':'NOT'} started streaming</p>


<button on:click={()=cameraState('on')}>on camera</button>
<button on:click={()=cameraState('off')}>off camera</button>

<button on:click={()=streamingVideo(true)}>start streaming</button>
<button on:click={()=streamingVideo(false)}>start streaming</button>
```
