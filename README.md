<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Shopping Mall Tycoon 3D</title>

<style>
body{
    margin:0;
    overflow:hidden;
    font-family:Arial,sans-serif;
}

#ui{
    position:absolute;
    top:10px;
    left:10px;
    background:rgba(0,0,0,.7);
    color:white;
    padding:15px;
    border-radius:10px;
}

button{
    padding:10px;
    margin-top:5px;
    cursor:pointer;
}
</style>
</head>
<body>

<div id="ui">
    <h2>🏬 Shopping Mall</h2>
    <p>💰 Coins: <span id="coins">1000</span></p>
    <p>🛒 Cart: <span id="cart">0</span></p>
    <button onclick="buyItem()">Buy Donut 🍩</button>
</div>

<script type="module">

import * as THREE from "https://unpkg.com/three@0.160.0/build/three.module.js";

const scene = new THREE.Scene();
scene.background = new THREE.Color(0xa0d8ff);

const camera = new THREE.PerspectiveCamera(
75,
window.innerWidth/window.innerHeight,
0.1,
1000
);

const renderer = new THREE.WebGLRenderer({antialias:true});
renderer.setSize(window.innerWidth,window.innerHeight);
document.body.appendChild(renderer.domElement);

const light = new THREE.DirectionalLight(0xffffff,2);
light.position.set(5,10,5);
scene.add(light);

const ambient = new THREE.AmbientLight(0xffffff,1);
scene.add(ambient);

const floor = new THREE.Mesh(
new THREE.PlaneGeometry(50,50),
new THREE.MeshStandardMaterial({color:0xdddddd})
);

floor.rotation.x = -Math.PI/2;
scene.add(floor);

const player = new THREE.Mesh(
new THREE.BoxGeometry(1,2,1),
new THREE.MeshStandardMaterial({color:0x0066ff})
);

player.position.y = 1;
scene.add(player);

function createShop(x,z,color){
    const shop = new THREE.Mesh(
    new THREE.BoxGeometry(4,3,4),
    new THREE.MeshStandardMaterial({color})
    );

    shop.position.set(x,1.5,z);
    scene.add(shop);
}

createShop(-10,-10,0xff4444);
createShop(10,-10,0xffff00);
createShop(0,10,0x44ff44);

const keys={};

window.addEventListener("keydown",(e)=>{
    keys[e.key.toLowerCase()] = true;
});

window.addEventListener("keyup",(e)=>{
    keys[e.key.toLowerCase()] = false;
});

let coins = 1000;
let cart = 0;

window.buyItem = function(){

    if(coins >= 50){
        coins -= 50;
        cart++;

        document.getElementById("coins").innerText = coins;
        document.getElementById("cart").innerText = cart;
    }
}

function animate(){

    requestAnimationFrame(animate);

    const speed = 0.12;

    if(keys["w"]) player.position.z -= speed;
    if(keys["s"]) player.position.z += speed;
    if(keys["a"]) player.position.x -= speed;
    if(keys["d"]) player.position.x += speed;

    camera.position.x = player.position.x;
    camera.position.y = player.position.y + 5;
    camera.position.z = player.position.z + 8;

    camera.lookAt(player.position);

    renderer.render(scene,camera);
}

animate();

window.addEventListener("resize",()=>{

    camera.aspect =
    window.innerWidth/window.innerHeight;

    camera.updateProjectionMatrix();

    renderer.setSize(
    window.innerWidth,
    window.innerHeight
    );
});

</script>

</body>
</html>
