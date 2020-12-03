 <!-- <script>
     let component;
     let data_json;
     let latitude;
     let currently;
     let latlng



     async function getfetch(){
        let response =await fetch("https://cors-anywhere.herokuapp.com/https://api.darksky.net/forecast/22383e0d5e0700370ed7629b7aad7b9f/35.6804,139.7690?lang=ja");
        data_json = await response.json();
        console.log(data_json);
    }

	  
		  //]]>
</script>


<button on:click={getfetch}>
    取得
</button>
{#if data_json}
    <p>緯度 {data_json.latitude}</p>
    <p>緯度 {data_json.longitude}</p>
    <p>timezone {data_json.timezone}</p>
    <p>現在の天気 {data_json.currently.summary}</p>
    <p>1日おきの天気 {data_json.daily.summary}</p>
    <p>1時間おきの天気 {data_json.hourly.summary}</p> -->
<!-- {/if}     -->


<script>
    import {apikey} from './dark_api.js';
	import {
		LeafletMap,
		LeafletMarker,
		LeafletPopup,
		LeafletTileLayer,
    } from "svelte-leaflet";

	import { toLatLng } from "leaflet/src/geo/LatLng.js";
    let apikeystr = new apikey();

	let markerVisible = true;
	let markerName = "Null Island";
    let markerLatLng = toLatLng(35.68141918995945, 139.7671194333653,1);
    let data_json;
    async function getfetch(){
        let response =await fetch("https://cors-anywhere.herokuapp.com/https://api.darksky.net/forecast/" + apikeystr.private_key + "/" + markerLatLng.lat + "," + markerLatLng.lng + "?lang=ja");
        data_json = await response.json();
        console.log(data_json)
    }
</script>

<style>
	fieldset {
		position:absolute;
		width: 90vw;
	}
	label {
		display: block;
	}
</style>

<button on:click={getfetch}>
    天気情報取得
</button>
{#if data_json}
    <p>緯度 {data_json.latitude}</p>
    <p>緯度 {data_json.longitude}</p>
    <p>timezone {data_json.timezone}</p>
    <p>現在の天気 {data_json.currently.summary}</p>
{:else }
    <p>緯度 </p>
    <p>緯度 </p>
    <p>timezone </p>
    <p>現在の天気 </p>
{/if}    

<fieldset>
	<label>
		<input type="checkbox" bind:checked={markerVisible} />
		display marker
	</label>
	<label>
		Popup content:
		<input type="text" bind:value={markerName} />
	</label>
	<label>
		Latitude:
		<input type="number" bind:value={markerLatLng.lat} />
	</label>
	<label>
		Longitude:
		<input type="number" bind:value={markerLatLng.lng} />
	</label>
</fieldset>
<LeafletMap style="height:50vh; width: 80vw; position:absolute; left:20vw;">
	<LeafletTileLayer url={"https://a.tile.openstreetmap.org/{z}/{x}/{y}.png"} options={{
		attribution:
			'Map data &copy; <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors, <a href="https://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>',
    }} />
	{#if markerVisible}
        <LeafletMarker bind:latLng={markerLatLng} options={{ draggable: true }}>
			<LeafletPopup options={{ closeButton: false, closeOnClick: false }}>
				{markerName}
			</LeafletPopup>
        </LeafletMarker>
	{/if}
</LeafletMap>


