<template>
  <section class="container">
    <!--<div>
      <logo/>
      <h1 class="title">
        pawdium
      </h1>
      <h2 class="subtitle">
        My glorious Nuxt.js project
      </h2>
      <div class="links">
        <a
          href="https://nuxtjs.org/"
          target="_blank"
          class="button--green">Documentation</a>
        <a
          href="https://github.com/nuxt/nuxt.js"
          target="_blank"
          class="button--grey">GitHub</a>
      </div>
    </div>-->
  </section>
</template>

<script>
import Logo from '~/components/Logo.vue'

export default {
  components: {
    Logo
  },
  mounted () {
	var THREE = require('three');
  
	var scene = new THREE.Scene();
	var camera = new THREE.PerspectiveCamera( 45, window.innerWidth/window.innerHeight, 1, 1000 ); 
	camera.position.z = 350;
	camera.position.y = 50;

	var renderer = new THREE.WebGLRenderer({ alpha: true,  antialias: true});
	renderer.setSize( window.innerWidth, window.innerHeight ); 
	renderer.shadowMap.enabled = true;
	renderer.shadowMap.type = THREE.PCFSoftShadowMap;
	document.body.appendChild( renderer.domElement );
	
	
	/* fog */
	scene.fog = new THREE.Fog( 0xffffff, 300, 500 ); 
	
	/* lights */
	var ambientLight = new THREE.AmbientLight( 0xffffff, 0.5 );
	scene.add( ambientLight );
	
	var light = new THREE.PointLight(0xffffff, 1, 11110, 2);
	light.position.set(-100,200,0); 
	light.castShadow = true;
	scene.add( light );
	
	
	/* ground */
	var planeGeometry = new THREE.PlaneGeometry( 2000, 2000 );
	planeGeometry.rotateX( - Math.PI / 2 );
	
	var material = new THREE.MeshPhongMaterial( {color: 0xffffff} );
	
	var plane = new THREE.Mesh( planeGeometry, material );
	plane.position.y = -1;
	plane.receiveShadow = true;
	scene.add( plane );
	
	/* forest */
	var forest = new THREE.Object3D();
	var objectLoader = new THREE.ObjectLoader();		
	objectLoader.load( "http://localhost:3000/models/trees/tree-1-fixed-3.json", function(obj) {
		for (var i=0; i<8; i++) {
			var tree = obj.clone();
			tree.castShadow = true;
			tree.rotation.x = -90;
			tree.position.x = -160 + 40 * i;
			tree.position.z = -100 + 200 * Math.random();
			forest.add(tree);
		}
	},
	function ( xhr ) {
		console.log( ( xhr.loaded / xhr.total * 100 ) + '% loaded' );
	},
	function ( error ) {
		console.log( 'An error happened');
	});
	scene.add(forest);

	var animate = function () {
		requestAnimationFrame( animate );

		forest.position.x += 0.2;
		if ( forest.position.x >= 60 ) forest.position.x = 0

		renderer.render( scene, camera );
	};

	animate();
  }
}
</script>

<style>

/* Sample `apply` at-rules with Tailwind CSS
.container {
  @apply min-h-screen flex justify-center items-center text-center mx-auto;
}
*/

canvas { position:fixed; top:0; left:0; bottom:0; right:0; z-index:0; }

.container {
  /*z-index:1;
  position:relative;*/
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
