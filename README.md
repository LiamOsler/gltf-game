# breakreality

Development repository for itch.io Mini Jame Gam #10

## Collaborators:
- [Liam Osler](http://github.com/liamosler)
- [Brian Wo](http://github.com/brainwo)

## Description:

It's a game?

## Libraries and Assets:

### Graphics:

[three.js](http://threejs.org):
- GLTFLoader

### Physics:
[cannon.js](https://schteppe.github.io/cannon.js/)


### 3D Assets used:

3D Models and Textures:

Retro3DGraphicsCollection (Miziziziz):
https://github.com/Miziziziz/Retro3DGraphicsCollection

PSX Style Cars (GGBotNet):
https://ggbot.itch.io/psx-style-cars

Retro Urban Kit (Kenney):
https://kenney.nl/assets/retro-urban-kit

Retro PSX Style Tree Pack (Elegant Crow):
https://elegantcrow.itch.io/psx-retro-style-tree-pack

## Notes / Requirements:
Viewing the game locally using the index.html file requires either CORS being allowed by the browser (alternatively, host the repo locally with http)

## Details:

### Model/Mesh/Object naming scheme:
![Model naming scheme](README/naming_slide1.PNG)
![Mesh naming scheme](README/naming_slide2.PNG)
![Movement Mesh naming scheme](README/naming_slide3.PNG)

### Basic ```.gltf``` Three Scene setup with low resolution rendering and sharp pixelation:


Setup a three.js scene:
```js
    const threeDOM = document.getElementById("three-area");
    const scene = new THREE.Scene();
```

Create a GLTFLoader use the load function to load the file, and add it to the scene: 
```js
    const loader = new THREE.GLTFLoader();
    loader.load( 'assets/world.gltf', function ( gltf ) {
        const model = gltf.scene;
        scene.add(model);
    } );
```
- The .gltf contains the models, shaders (principled BSDF in Blender), textures and lights in one single file.

Basic three.js renderer setup:
```js
    const resolution = {x: 160, y: 144};
    const renderer = new THREE.WebGLRenderer();
    renderer.setSize( resolution.x, resolution.y );
    renderer.physicallyCorrectLights = true; //Ensures lights match Blender renders
    renderer.setClearColor( 0xffffff, 0);
    threeDOM.appendChild( renderer.domElement );
```

Basic Camera setup:
```js
    const camera = new THREE.PerspectiveCamera( 45, resolution.x/resolution.y, 1, 200 );
    camera.up.set( 0, 0, 1 );
    camera.position.set( -25,-25, 25 );
    camera.lookAt( 0, 0, 0 );
```

Basic animation function setup:
```js
    function animate() {
        requestAnimationFrame( animate );
        renderer.render( scene, camera );
    }

    animate();
```

Barebones GLTFLoader with pixellated styling ```.html``` file:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GLTFLoader</title>
    <style>
        body { 
            margin: 0;
        }
        
        #three-area {
            image-rendering: pixelated;
            zoom: 4;
        }
    </style>
</head>

<body>
    <div id="three-area"></div>
</body>

<script src="scripts/three.js"></script>
<script src="scripts/GLTFLoader.js"></script>
<script>
    const threeDOM = document.getElementById( "three-area" );
    const scene = new THREE.Scene();

    const loader = new THREE.GLTFLoader();
    loader.load( 'assets/world.gltf', function ( gltf ) {
        const model = gltf.scene;
        scene.add( model );
    } );

    const resolution = {x: 160, y: 144};
    const renderer = new THREE.WebGLRenderer();
    renderer.setSize( resolution.x, resolution.y );
    renderer.physicallyCorrectLights = true; //Ensures lights match Blender renders
    renderer.setClearColor( 0xffffff, 0);
    threeDOM.appendChild( renderer.domElement );

    const camera = new THREE.PerspectiveCamera( 45, resolution.x/resolution.y, 1, 200 );
    camera.up.set( 0, 0, 1 );
    camera.position.set( -25,-25, 25 );
    camera.lookAt( 0, 0, 0 );

    function animate() {
        requestAnimationFrame( animate );
        renderer.render( scene, camera );
    }

    animate();
</script>
</html>
```
Results:
![Movement Mesh naming scheme](README/screenshot3.PNG)

## Enhancing the scene:

### Adding shadows:

Add ```castShadow = true``` and ```receiveShadow = true``` to the all of the meshes in the GLTF file:
```js
    loader.load( 'assets/world.gltf', function ( gltf ) {
        const model = gltf.scene;

        model.traverse( function( node ) {
            node.castShadow = true;
            node.receiveShadow = true;
        } );

        scene.add( model );
    } );
```

Ensure  ```renderer.shadowMap.enabled = true```

Results:
![Movement Mesh naming scheme](README/screenshot4.PNG)



