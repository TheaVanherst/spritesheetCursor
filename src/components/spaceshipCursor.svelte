<script>
    import mousePosition from "$lib/mousePosition.js";
    import { screenSize } from "$lib/writables.js";
    import { fly } from "svelte/transition";

    import { spring } from 'svelte/motion';

    // mouse cursor elastic settings
    let position = spring(
        { x: -($screenSize / 2), y: -100 },
        { stiffness: 0.1, damping: 0.8 }
    );

    $: $mousePosition.x !== position.x ? position.set({ x: $mousePosition.x, y: $mousePosition.y }) : false;

    let shoot = {}
</script>

<svelte:window on:mousedown={() => shoot = {}}/>

<div id="spaceShipController">
	<div id="spaceShip" style="left: {$position.x}px; top: {$position.y}px">
		<img id="ship" src="/shipCursorYRaw.png" style="object-position: -{$mousePosition.xTilt}px -{$mousePosition.yTilt}px">
		<div id="boosterController" style="top: {-$mousePosition.yMulti * 3}px;">
			<img id="booster1" src="/animatedBooster.gif" style="left: {$mousePosition.xMulti}px;">
			<img id="booster2" src="/animatedBooster.gif">
		</div>
	</div>
	<div id="laserController">
		{#key shoot}
			<div id="laser"
				 out:fly={{y: -400, duration: 1200 }}
				 style="left: {$position.x}px; top: {$position.y}px">
				<img src="/shipCursorLaser.webp">
			</div>
		{/key}
	</div>
</div>

<style>
	#spaceShipController {
		pointer-events: none;
		-webkit-user-select: none; /* Safari */
		-ms-user-select: none; /* IE 10 and IE 11 */
		user-select: none; /* Standard syntax */

		position: 	fixed;
		width: 		max-content;
		height: 	max-content;
		z-index: 	99;
		animation:
				yMovement 25s infinite ease-in-out,
				xMovement 20s infinite ease-in-out;}

	#laserController #laser {
		position: 	absolute;}
	#laserController #laser img {
		margin-top:	 	47px;
		margin-left: 	-9px;
		position: 		absolute;}
	#laserController #laser:last-of-type {
		display: none;}

	#boosterController {
		position: 	absolute;
		z-index: 	2;
		margin-top: 5px;}
	#booster1 {
		top: 	69px;
		margin-left: -9px;
		z-index: 2;
		animation: flicker 12s infinite ease-in-out;}
	#booster2 {
		top: 	73px;
		left: 	-7px;
		z-index: 1;
		animation: flicker 22s infinite ease-in-out;}

	#spaceShip {
		position: 	absolute;
		z-index: 	1;}
	#spaceShip img {
		object-fit: none;
		position: 	absolute;}

	#spaceShip #ship {
		top: 	32px;
		left: 	-24px;
		width: 	48px;
		height: 49px;
		object-position: calc(-48px * 2) 0;}

	@keyframes xMovement {
		0% {	margin-left: -10px;}
		50% {	margin-left: 10px;}
		70% {	margin-left: 0;}
		80% {	margin-left: 5px;}
		100% {	margin-left: -10px;}}
	@keyframes yMovement {
		0% {	margin-top: 15px;}
		20% {	margin-top: 10px;}
		70% {	margin-top: 0;}
		100% {	margin-top: 15px;}}
	@keyframes zMovement {
		0% {	transform: scale(1.02);}
		20% {	transform: scale(1)}
		50% {	transform: scale(0.99)}
		100% {	transform: scale(1.02);}}

	@keyframes flicker {
		0% {	opacity:0.1; 	transform: scale(1, 1);}
		2% {	opacity:1; 		transform: scale(1.1, 1);}
		8% {	opacity:0.1; 	transform: scale(0.9, 1);}
		9% {	opacity:1; 		transform: scale(1, 1);}
		12% {	opacity:0.1; 	transform: scale(0.8, 1);}
		20% {	opacity:1; 		transform: scale(1, 1);}
		25% {	opacity:0.3; 	transform: scale(0.8, 1);}
		30% {	opacity:1; 		transform: scale(1, 1);}
		70% {	opacity:0.7; 	transform: scale(0.8, 1);}
		72% {	opacity:0.2; 	transform: scale(0.7, 1);}
		77% {	opacity:.9; 	transform: scale(1.1, 1);}
		100% {	opacity:.9; 	transform: scale(1, 1);}}

</style>