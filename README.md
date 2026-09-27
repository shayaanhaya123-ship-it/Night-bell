# Night-bell
jeu d horeur ouvert open world Dan's une foret
<!doctype html>
<html lang="fr">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no,viewport-fit=cover">
<title>Night Bell — Ultra 3D</title>
<style>
html,body{margin:0;width:100%;height:100%;overflow:hidden;background:#020509;color:#fff;font-family:Arial,Helvetica,sans-serif}
canvas{display:block}
#menu,#pause,#loading,#error{position:fixed;inset:0;display:flex;align-items:center;justify-content:center;z-index:20;background:radial-gradient(circle at 50% 35%,#17202d 0,#070b11 45%,#010204 100%)}
.panel{width:min(760px,88vw);text-align:center;padding:42px 30px;border:1px solid #445060;background:#080d14e8;box-shadow:0 25px 90px #000;border-radius:18px}
h1{font-size:clamp(48px,9vw,100px);letter-spacing:10px;margin:0 0 4px}
h2{font-size:28px;margin:0 0 22px}
p{color:#b9c2cc;font-size:18px;line-height:1.55}
button{background:#101820;color:#fff;border:1px solid #9da9b5;border-radius:10px;padding:16px 38px;font-size:21px;cursor:pointer;margin-top:15px}
button:hover{background:#1d2a37}
.small{font-size:14px;color:#87929e;margin-top:22px}
#loading .panel{max-width:560px}
#bar{height:8px;background:#222;border-radius:8px;overflow:hidden;margin-top:22px}
#fill{height:100%;width:5%;background:#c9d4df}
#error{display:none}
#errorText{white-space:pre-wrap;color:#ff9d9d;text-align:left;background:#12090b;padding:14px;border-radius:8px}
#hud{position:fixed;z-index:10;left:18px;top:16px;pointer-events:none;text-shadow:0 2px 5px #000}
#night{font-size:26px;font-weight:bold}
#objective{font-size:15px;color:#d0d7de;margin-top:5px}
#stam{margin-top:10px;width:190px;height:6px;background:#20252b;border-radius:5px}
#stam>i{display:block;height:100%;width:100%;background:#d9e2e8;border-radius:5px}
#cross{position:fixed;z-index:9;left:50%;top:50%;width:10px;height:10px;margin:-5px;border:1px solid #ffffffaa;border-radius:50%;pointer-events:none}
#hint{position:fixed;z-index:10;right:18px;bottom:15px;color:#aab4bd;font-size:13px;text-align:right;text-shadow:0 2px 4px #000}
.hidden{display:none!important}
.warn{color:#ff7272!important}

#touch{display:none;position:fixed;inset:0;z-index:15;pointer-events:none;touch-action:none}
.touch-btn{position:absolute;width:62px;height:62px;border-radius:50%;border:1px solid #ffffff66;background:#0a111bcc;color:#fff;font-size:24px;display:flex;align-items:center;justify-content:center;pointer-events:auto;user-select:none;-webkit-user-select:none;touch-action:none;box-shadow:0 5px 20px #0008}
#joy{position:absolute;left:24px;bottom:28px;width:150px;height:150px;border-radius:50%;border:1px solid #ffffff33;background:#08101888;pointer-events:auto;touch-action:none}
#stick{position:absolute;left:50%;top:50%;width:64px;height:64px;margin:-32px;border-radius:50%;background:#dbe5ed44;border:1px solid #ffffff77;pointer-events:none}
#look{position:absolute;right:0;top:0;width:55%;height:100%;pointer-events:auto;touch-action:none}
#runBtn{right:24px;bottom:28px}
#flashBtn{right:98px;bottom:28px}
#hideBtn{right:24px;bottom:102px}
#pauseBtn{right:98px;bottom:102px}
#touchTip{position:absolute;left:50%;top:18px;transform:translateX(-50%);font-size:12px;color:#ffffff99;text-shadow:0 2px 4px #000;white-space:nowrap}
@media (pointer:coarse),(max-width:800px){#touch{display:block}#hint{display:none}.panel{padding:30px 18px}.panel p{font-size:15px}#hud{left:12px;top:10px}#night{font-size:21px}#objective{max-width:60vw;font-size:13px}}
</style>
</head>
<body>
<div id="menu">
 <div class="panel">
  <h1>NIGHT BELL</h1>
  <h2>Chapitre 1 — La forêt</h2>
  <p>Une vraie scène 3D avec personnages animés, grande forêt ouverte, maisons, lumière, brouillard et poursuite.</p>
  <button id="play">JOUER</button>
  <div class="small">WASD déplacer • souris regarder • SHIFT courir • F lampe • E interaction • P pause</div>
 </div>
</div>
<div id="loading" class="hidden">
 <div class="panel"><h2>Chargement de la forêt…</h2><p id="loadText">Préparation du monde 3D</p><div id="bar"><div id="fill"></div></div></div>
</div>
<div id="error">
 <div class="panel"><h2>Le jeu n'a pas pu charger</h2><div id="errorText"></div><p>Vérifie que tu as Internet puis recharge le fichier.</p><button id="retry">RÉESSAYER</button></div>
</div>
<div id="pause" class="hidden"><div class="panel"><h2>PAUSE</h2><p>Appuie sur P pour reprendre.</p><button id="resume">REPRENDRE</button></div></div>
<div id="hud"><div id="night">NUIT 1</div><div id="objective">Rejoins le camp avec tes amis.</div><div id="stam"><i></i></div></div>
<div id="cross"></div>
<div id="hint">WASD • SHIFT • F • E • P • ÉCHAP</div>
<div id="touch">
 <div id="touchTip">Gauche : déplacer • Droite : regarder</div>
 <div id="joy"><div id="stick"></div></div>
 <div id="look"></div>
 <div id="runBtn" class="touch-btn">🏃</div>
 <div id="flashBtn" class="touch-btn">🔦</div>
 <div id="hideBtn" class="touch-btn">E</div>
 <div id="pauseBtn" class="touch-btn">Ⅱ</div>
</div>

<script type="importmap">
{
 "imports":{
  "three":"https://cdn.jsdelivr.net/npm/three@0.180.0/build/three.module.js",
  "three/addons/":"https://cdn.jsdelivr.net/npm/three@0.180.0/examples/jsm/"
 }
}
</script>

<script type="module">
import * as THREE from 'three';
import {GLTFLoader} from 'three/addons/loaders/GLTFLoader.js';
import * as SkeletonUtils from 'three/addons/utils/SkeletonUtils.js';

const $=id=>document.getElementById(id);
const menu=$('menu'), loading=$('loading'), pause=$('pause'), errorBox=$('error');
const play=$('play'), retry=$('retry'), resume=$('resume');
const loadText=$('loadText'), fill=$('fill');

let renderer,scene,camera,clock;
let player=null, playerMixer=null, playerActions={};
let friends=[], killer=null, killerMixer=null, killerActions={};
let running=false, paused=false, flashlight=true;
let night=1, nightTimer=0, stamina=100;
let yaw=0,pitch=-0.08;
const keys={};
const colliders=[];
const mixers=[];
const worldObjects=[];
const tempV=new THREE.Vector3();
const UP=new THREE.Vector3(0,1,0);

const playerPos=new THREE.Vector3(0,0,12);
const speedWalk=3.8, speedRun=7.2;

function showError(e){
 console.error(e);
 errorBox.style.display='flex';
 $('errorText').textContent=String(e?.stack||e);
 loading.classList.add('hidden');
}
window.addEventListener('error',e=>{if(running)showError(e.error||e.message)});
window.addEventListener('unhandledrejection',e=>{if(running)showError(e.reason)});

function setLoading(t,p){
 loadText.textContent=t; fill.style.width=Math.max(5,Math.min(100,p))+'%';
}

function makeCanvasTexture(base, speck, count=900){
 const c=document.createElement('canvas');c.width=c.height=256;
 const x=c.getContext('2d'); x.fillStyle=base;x.fillRect(0,0,256,256);
 for(let i=0;i<count;i++){
  x.fillStyle=speck[Math.floor(Math.random()*speck.length)];
  x.globalAlpha=.15+Math.random()*.35;
  x.fillRect(Math.random()*256,Math.random()*256,1+Math.random()*4,1+Math.random()*4);
 }
 x.globalAlpha=1;
 const t=new THREE.CanvasTexture(c); t.wrapS=t.wrapT=THREE.RepeatWrapping; t.repeat.set(20,20);
 return t;
}

function addTree(x,z,s=1){
 const g=new THREE.Group(); g.position.set(x,0,z); g.scale.setScalar(s);
 const trunkMat=new THREE.MeshStandardMaterial({color:0x3a2417,roughness:.95});
 const leafMat=new THREE.MeshStandardMaterial({color:0x102b18,roughness:.9});
 const leaf2=new THREE.MeshStandardMaterial({color:0x163b20,roughness:.9});
 const trunk=new THREE.Mesh(new THREE.CylinderGeometry(.22,.34,3.4,8),trunkMat); trunk.position.y=1.7; trunk.castShadow=true; g.add(trunk);
 for(let i=0;i<5;i++){
   const h=1.7-i*.23, r=1.9-i*.24;
   const crown=new THREE.Mesh(new THREE.ConeGeometry(r,h,9),i%2?leafMat:leaf2);
   crown.position.y=3.2+i*.55; crown.castShadow=true; g.add(crown);
 }
 // branches
 for(let i=0;i<3;i++){
  const b=new THREE.Mesh(new THREE.CylinderGeometry(.08,.13,1.8,7),trunkMat);
  b.position.set((i-1)*.28,2.3+i*.18,0); b.rotation.z=(i-1)*.5; b.rotation.x=.15; b.castShadow=true; g.add(b);
 }
 scene.add(g); worldObjects.push(g); colliders.push({x,z,r:1.25*s});
}

function addRock(x,z,s=1){
 const mat=new THREE.MeshStandardMaterial({color:0x4d5658,roughness:1});
 const r=new THREE.Mesh(new THREE.DodecahedronGeometry(.8*s,1),mat);
 r.position.set(x,.45*s,z); r.scale.y=.65; r.rotation.set(Math.random(),Math.random(),Math.random());
 r.castShadow=true;r.receiveShadow=true;scene.add(r);worldObjects.push(r);
}

function addCabin(x,z,rot=0){
 const g=new THREE.Group();g.position.set(x,0,z);g.rotation.y=rot;
 const wood=new THREE.MeshStandardMaterial({color:0x5a3823,roughness:.85});
 const dark=new THREE.MeshStandardMaterial({color:0x1b2024,roughness:.95});
 const glass=new THREE.MeshStandardMaterial({color:0x8ba4ad,emissive:0x283c42,emissiveIntensity:.35,roughness:.25});
 const body=new THREE.Mesh(new THREE.BoxGeometry(8,3.3,6),wood);body.position.y=1.65;body.castShadow=true;body.receiveShadow=true;g.add(body);
 const roof=new THREE.Mesh(new THREE.ConeGeometry(5.5,2.5,4),dark);roof.rotation.y=Math.PI/4;roof.position.y=4.55;roof.scale.z=.72;roof.castShadow=true;g.add(roof);
 const door=new THREE.Mesh(new THREE.BoxGeometry(1.25,2.25,.16),dark);door.position.set(0,1.15,3.08);g.add(door);
 for(const sx of [-2.2,2.2]){
  const w=new THREE.Mesh(new THREE.BoxGeometry(1.35,1.05,.12),glass);w.position.set(sx,1.9,3.09);g.add(w);
 }
 // chimney
 const ch=new THREE.Mesh(new THREE.BoxGeometry(.7,1.6,.7),new THREE.MeshStandardMaterial({color:0x343338}));ch.position.set(2.4,4.8,-1);g.add(ch);
 g.userData.hideSpot=true;
 scene.add(g);worldObjects.push(g);colliders.push({x,z,r:5.1});
 return g;
}

function addCampfire(x,z){
 const g=new THREE.Group();g.position.set(x,0,z);
 const wood=new THREE.MeshStandardMaterial({color:0x5c321c});
 for(let i=0;i<3;i++){const log=new THREE.Mesh(new THREE.CylinderGeometry(.11,.14,2,8),wood);log.rotation.z=Math.PI/2;log.rotation.y=i*Math.PI/3;log.position.y=.18;log.castShadow=true;g.add(log)}
 const fire=new THREE.Mesh(new THREE.ConeGeometry(.6,1.7,12),new THREE.MeshStandardMaterial({color:0xff7a22,emissive:0xff3b00,emissiveIntensity:2}));
 fire.position.y=1;g.add(fire);
 const light=new THREE.PointLight(0xff7b2a,4,15,2);light.position.y=1.3;light.castShadow=true;g.add(light);
 scene.add(g);worldObjects.push(g);return g;
}

function setupWorld(){
 scene=new THREE.Scene();
 scene.background=new THREE.Color(0x03070b);
 scene.fog=new THREE.FogExp2(0x07100f,.012);
 camera=new THREE.PerspectiveCamera(68,innerWidth/innerHeight,.05,300);
 camera.position.set(0,2.2,16);
 renderer=new THREE.WebGLRenderer({antialias:true,powerPreference:'high-performance'});
 renderer.setPixelRatio(Math.min(devicePixelRatio,1.75));
 renderer.setSize(innerWidth,innerHeight);
 renderer.shadowMap.enabled=true;
 renderer.shadowMap.type=THREE.PCFSoftShadowMap;
 renderer.outputColorSpace=THREE.SRGBColorSpace;
 renderer.toneMapping=THREE.ACESFilmicToneMapping;
 renderer.toneMappingExposure=.85;
 document.body.appendChild(renderer.domElement);

 const hemi=new THREE.HemisphereLight(0x7890ad,0x11150f,.42);scene.add(hemi);
 const moon=new THREE.DirectionalLight(0x9bb6d8,1.25);moon.position.set(-30,50,-25);moon.castShadow=true;
 moon.shadow.mapSize.set(2048,2048);moon.shadow.camera.left=-70;moon.shadow.camera.right=70;moon.shadow.camera.top=70;moon.shadow.camera.bottom=-70;
 scene.add(moon);

 const groundTex=makeCanvasTexture('#1b291d',['#152419','#223522','#2a3a25','#101a13']);
 const groundMat=new THREE.MeshStandardMaterial({map:groundTex,roughness:1});
 const ground=new THREE.Mesh(new THREE.PlaneGeometry(180,180,1,1),groundMat);
 ground.rotation.x=-Math.PI/2;ground.receiveShadow=true;scene.add(ground);

 // subtle paths
 const pathMat=new THREE.MeshStandardMaterial({color:0x2b2b22,roughness:1});
 for(let i=0;i<8;i++){
  const p=new THREE.Mesh(new THREE.CircleGeometry(3.5,24),pathMat);p.rotation.x=-Math.PI/2;
  p.position.set((i-4)*3,0.012,8-i*8);p.scale.set(1.8,.8,1);scene.add(p);
 }

 // forest
 for(let i=0;i<320;i++){
   let x=(Math.random()*160)-80,z=(Math.random()*160)-80;
   if(Math.hypot(x,z)<16 || (Math.abs(x)<12&&Math.abs(z-12)<14)){i--;continue}
   addTree(x,z,.75+Math.random()*.85);
 }
 for(let i=0;i<80;i++){
   let x=Math.random()*150-75,z=Math.random()*150-75;
   if(Math.hypot(x,z)<12){i--;continue} addRock(x,z,.25+Math.random()*.7);
 }
 addCabin(-16,-7,-.15);addCabin(24,-22,.5);addCabin(-31,28,-.7);
 addCampfire(0,8);

 // stars
 const starGeo=new THREE.BufferGeometry(), arr=[];
 for(let i=0;i<700;i++){arr.push((Math.random()-.5)*250,35+Math.random()*90,(Math.random()-.5)*250)}
 starGeo.setAttribute('position',new THREE.Float32BufferAttribute(arr,3));
 const stars=new THREE.Points(starGeo,new THREE.PointsMaterial({color:0xc8d8e8,size:.12,sizeAttenuation:true}));
 scene.add(stars);

 const flashlight=new THREE.SpotLight(0xcfe7ff,18,28,Math.PI/8,.55,1.5);
 flashlight.position.set(0,2.2,0);flashlight.target.position.set(0,1,-10);
 camera.add(flashlight);camera.add(flashlight.target);scene.add(camera);
 camera.userData.flash=flashlight;

 addEventListener('resize',onResize);
}

function onResize(){if(!camera||!renderer)return;camera.aspect=innerWidth/innerHeight;camera.updateProjectionMatrix();renderer.setSize(innerWidth,innerHeight)}

function setupModel(gltf,kind){
 const model=gltf.scene;
 model.traverse(o=>{if(o.isMesh){o.castShadow=true;o.receiveShadow=true}});
 const mixer=new THREE.AnimationMixer(model);
 const actions={};
 for(const clip of gltf.animations) actions[clip.name]=mixer.clipAction(clip);
 return {model,mixer,actions,clips:gltf.animations};
}

function playNamed(actions,clips,names){
 let clip=null;
 for(const n of names){
   clip=clips.find(c=>String(c.name).toLowerCase().includes(n.toLowerCase()));
   if(clip)break;
 }
 if(!clip)clip=clips[0];
 if(!clip)return null;
 const a=actions[clip.name];
 if(!a)return null;
 const owner=actions;
 if(owner.__current===clip.name)return a;
 if(owner.__current && actions[owner.__current]){
   actions[owner.__current].fadeOut(.18);
 }
 a.reset().fadeIn(.18).play();
 owner.__current=clip.name;
 return a;
}

async function loadAssets(){
 const loader=new GLTFLoader();
 setLoading('Chargement du personnage 3D…',25);
 const base=await new Promise((resolve,reject)=>loader.load('https://threejs.org/examples/models/gltf/Soldier.glb',resolve,undefined,reject));
 setLoading('Création du personnage animé…',45);

 const p=setupModel(base,'player');player=p.model;player.scale.setScalar(1.15);player.position.copy(playerPos);scene.add(player);
 playerMixer=p.mixer;playerActions=p.actions;
 playNamed(playerActions,p.clips,['idle']);
 mixers.push(playerMixer);

 // Friends
 const spots=[[-5,0,8],[5,0,7],[-3,0,16]];
 for(let i=0;i<spots.length;i++){
   const c=SkeletonUtils.clone(base.scene);
   c.scale.setScalar(.98);c.position.set(spots[i][0],0,spots[i][2]);c.rotation.y=Math.PI;
   c.traverse(o=>{if(o.isMesh){o.castShadow=true;o.receiveShadow=true}});
   const m=new THREE.AnimationMixer(c);
   const acts={};base.animations.forEach(a=>acts[a.name]=m.clipAction(a));
   playNamed(acts,base.animations,['idle']);
   scene.add(c);friends.push({model:c,mixer:m,actions:acts,clips:base.animations,phase:i*2});mixers.push(m);
 }
 setLoading('Création du tueur…',70);
 const k=SkeletonUtils.clone(base.scene);k.scale.setScalar(1.3);k.position.set(35,0,-35);
 k.traverse(o=>{
   if(o.isMesh){
     o.castShadow=true;o.receiveShadow=true;
     if(o.material?.color)o.material=o.material.clone();
     if(o.material?.color)o.material.color.set(0x111216);
   }
 });
 killer=k;killerMixer=new THREE.AnimationMixer(k);
 killerActions={};base.animations.forEach(a=>killerActions[a.name]=killerMixer.clipAction(a));
 playNamed(killerActions,base.animations,['idle']);scene.add(killer);mixers.push(killerMixer);

 setLoading('Mise en place de la lumière…',88);
 await new Promise(r=>setTimeout(r,150));
 setLoading('Prêt !',100);
}

let touchMoveX=0,touchMoveZ=0,touchRun=false;
let joyId=null,lookId=null,lookX=0,lookY=0;
function setKey(code,v){keys[code]=v}
function applyJoy(dx,dy){
 const max=62, len=Math.hypot(dx,dy); if(len>max){dx=dx/len*max;dy=dy/len*max}
 $('stick').style.transform=`translate(${dx}px,${dy}px)`;
 const nx=dx/max, ny=dy/max;
 touchMoveX=nx; touchMoveZ=ny;
 setKey('KeyA',nx<-.18);setKey('KeyD',nx>.18);setKey('KeyW',ny<-.18);setKey('KeyS',ny>.18);
}
function resetJoy(){applyJoy(0,0);$('stick').style.transform='translate(0,0)'}
function bindTouchControls(){
 const joy=$('joy'),look=$('look');
 joy.addEventListener('pointerdown',e=>{if(joyId!==null)return;joyId=e.pointerId;joy.setPointerCapture(e.pointerId);const r=joy.getBoundingClientRect();applyJoy(e.clientX-(r.left+r.width/2),e.clientY-(r.top+r.height/2));e.preventDefault()});
 joy.addEventListener('pointermove',e=>{if(e.pointerId!==joyId)return;const r=joy.getBoundingClientRect();applyJoy(e.clientX-(r.left+r.width/2),e.clientY-(r.top+r.height/2));e.preventDefault()});
 const endJoy=e=>{if(e.pointerId===joyId){joyId=null;resetJoy();}};joy.addEventListener('pointerup',endJoy);joy.addEventListener('pointercancel',endJoy);
 look.addEventListener('pointerdown',e=>{if(lookId!==null)return;lookId=e.pointerId;look.setPointerCapture(e.pointerId);lookX=e.clientX;lookY=e.clientY;e.preventDefault()});
 look.addEventListener('pointermove',e=>{if(e.pointerId!==lookId||!running||paused)return;const dx=e.clientX-lookX,dy=e.clientY-lookY;lookX=e.clientX;lookY=e.clientY;yaw-=dx*.006;pitch-=dy*.004;pitch=Math.max(-.5,Math.min(.3,pitch));e.preventDefault()});
 const endLook=e=>{if(e.pointerId===lookId)lookId=null};look.addEventListener('pointerup',endLook);look.addEventListener('pointercancel',endLook);
 $('runBtn').addEventListener('pointerdown',e=>{touchRun=true;setKey('ShiftLeft',true);e.preventDefault()});
 const endRun=e=>{touchRun=false;setKey('ShiftLeft',false)};$('runBtn').addEventListener('pointerup',endRun);$('runBtn').addEventListener('pointercancel',endRun);
 $('flashBtn').addEventListener('pointerdown',e=>{if(running&&!paused)flashlight=!flashlight;e.preventDefault()});
 $('hideBtn').addEventListener('pointerdown',e=>{if(running&&!paused)interact();e.preventDefault()});
 $('pauseBtn').addEventListener('pointerdown',e=>{if(running)togglePause();e.preventDefault()});
}
bindTouchControls();

function start(){
 if(running){paused=false;pause.classList.add('hidden');return}
 running=true;paused=false;menu.classList.add('hidden');errorBox.style.display='none';
 setupWorld();
 clock=new THREE.Clock();
 loadAssets().then(()=>{
   running=true;
   requestAnimationFrame(loop);
   document.body.requestPointerLock?.();
 }).catch(showError);
}

function togglePause(){
 if(!running)return;
 paused=!paused;pause.classList.toggle('hidden',!paused);
 if(paused)document.exitPointerLock?.();else document.body.requestPointerLock?.();
}

function updatePlayer(dt){
 let x=0,z=0;if(keys.KeyW)z-=1;if(keys.KeyS)z+=1;if(keys.KeyA)x-=1;if(keys.KeyD)x+=1;
 const moving=x||z;
 let sprint=(keys.ShiftLeft||keys.ShiftRight)&&stamina>1&&moving;
 if(sprint){stamina=Math.max(0,stamina-dt*24)}else stamina=Math.min(100,stamina+dt*15);
 $('stam').firstElementChild.style.width=stamina+'%';
 if(moving){
  const len=Math.hypot(x,z);x/=len;z/=len;
  const ca=Math.cos(yaw),sa=Math.sin(yaw);
  const dx=(ca*x-sa*z)*(sprint?speedRun:speedWalk)*dt;
  const dz=(sa*x+ca*z)*(sprint?speedRun:speedWalk)*dt;
  const nx=player.position.x+dx,nz=player.position.z+dz;
  let blocked=false;
  for(const c of colliders){if(Math.hypot(nx-c.x,nz-c.z)<c.r+.8){blocked=true;break}}
  if(!blocked){player.position.x=nx;player.position.z=nz}
  player.rotation.y=Math.atan2(dx,dz);
  const clip= sprint?'run':'walk';
  const act=playNamed(playerActions,playerMixer?Object.keys(playerActions).map(n=>({name:n})):[],[clip]);
 }
 const camTarget=player.position.clone();camTarget.y=1.5;
 const dist=6.5, h=2.6;
 const off=new THREE.Vector3(Math.sin(yaw)*dist,h,Math.cos(yaw)*dist);
 off.y += pitch*2.0;
 camera.position.lerp(camTarget.clone().add(off),1-Math.pow(.001,dt));
 camera.lookAt(camTarget);
}

function updateFriends(dt){
 friends.forEach((f,i)=>{
   const a=performance.now()*.00035+f.phase;
   const tx=player.position.x+Math.cos(a)*4, tz=player.position.z+Math.sin(a)*4;
   const dx=tx-f.model.position.x,dz=tz-f.model.position.z,d=Math.hypot(dx,dz);
   if(d>.7){f.model.position.x+=dx/d*1.5*dt;f.model.position.z+=dz/d*1.5*dt;f.model.rotation.y=Math.atan2(dx,dz);playNamed(f.actions,f.clips,['walk'])}
   else playNamed(f.actions,f.clips,['idle']);
});

}

function updateKiller(dt){
 if(night<5){killer.visible=false;return}
 killer.visible=true;
 const dx=player.position.x-killer.position.x,dz=player.position.z-killer.position.z,d=Math.hypot(dx,dz);
 if(d<45){
   killer.rotation.y=Math.atan2(dx,dz);
   const sp=2.7+(night-5)*.13;
   killer.position.x+=dx/d*sp*dt;killer.position.z+=dz/d*sp*dt;
   playNamed(killerActions,Object.keys(killerActions).map(n=>({name:n})),['run']);
   if(d<4.5){$('objective').textContent='LE TUEUR EST JUSTE DERRIÈRE TOI !';$('objective').classList.add('warn')}
   else $('objective').classList.remove('warn');
 }else playNamed(killerActions,Object.keys(killerActions).map(n=>({name:n})),['idle']);
}


function interact(){
 if(!player)return;
 let nearest=null, best=7;
 for(const o of worldObjects){
   if(!o.userData?.hideSpot)continue;
   const d=o.position.distanceTo(player.position);
   if(d<best){best=d;nearest=o}
 }
 if(nearest){
   player.visible=!player.visible;
   $('objective').textContent=player.visible?'Tu es sorti de la cachette.':'Tu es caché dans la maison.';
 }
}

function updateNight(dt){
 nightTimer+=dt;
 if(nightTimer>=20 && night<29){
   nightTimer=0;night++;
   $('night').textContent='NUIT '+night;
   $('objective').textContent=night<5?'Explore la forêt et reste avec tes amis.':night<29?'Le tueur te cherche. Trouve une maison et survis.':'Atteins la sortie de la forêt !';
 }
}

function loop(){
 requestAnimationFrame(loop);
 if(!running||paused)return;
 const dt=Math.min(clock.getDelta(),.05);
 updateNight(dt);updatePlayer(dt);updateFriends(dt);updateKiller(dt);
 const bob=(keys.KeyW||keys.KeyA||keys.KeyS||keys.KeyD)?Math.sin(performance.now()*.012)*.025:0;
 camera.position.y+=bob;
 const f=camera.userData.flash; if(f)f.intensity=flashlight?(13+Math.sin(performance.now()*.006)*.5):0;
 renderer.render(scene,camera);
}

play.addEventListener('click',start);
retry.addEventListener('click',()=>{location.reload()});
resume.addEventListener('click',togglePause);
window.addEventListener('keydown',e=>{
 keys[e.code]=true;
 if(e.code==='KeyP'&&!e.repeat){if(!running)start();else togglePause()}
 if(e.code==='Escape'&&!e.repeat)togglePause();
 if(e.code==='KeyF'&&!e.repeat&&running){flashlight=!flashlight}
 if(e.code==='KeyE'&&!e.repeat&&running){interact()}
});
window.addEventListener('keyup',e=>keys[e.code]=false);

let dragging=false,lastX=0,lastY=0;
document.addEventListener('pointerlockchange',()=>{
 if(document.pointerLockElement===document.body)dragging=true;else dragging=false;
});
document.addEventListener('mousemove',e=>{
 if(!running||paused)return;
 if(document.pointerLockElement!==document.body)return;
 yaw-=e.movementX*.0025;pitch-=e.movementY*.0018;pitch=Math.max(-.5,Math.min(.3,pitch));
});

document.addEventListener('click',()=>{if(running&&!paused&&!matchMedia('(pointer:coarse)').matches)document.body.requestPointerLock?.()});

// Le jeu attend JOUER ou la touche P.
</script>
</body>
</html>
