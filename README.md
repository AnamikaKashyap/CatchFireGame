# CatchFireGame

https://anamikakashyap.github.io/CatchFireGame/

<!DOCTYPE html>

<html>
    
<head>
        
<title>Catch Fire</title>
 
<style>
body {
    
background-color: rgb(20,30,40);
    
height:100%;
    
width: 100%;
    
overflow:hidden;

}

div {
    
font-family: courier;
    
color: white;
    
font-weight: bold;

}

span {
    
position: absolute;
    
user-select:none;
    
z-index:-1; 
/Wanda's note/
}
.water {
    
pointer-events: none; 
/* css3 - makes the upper element 'invisible' to mouse-interaction (so fire elements below can be clicked) */

}

#GO {
    
text-align: center;
    
margin: 0 auto;
    
font-size:32px;
    
transform:translateY(50%);
    
background-color: rgba(20,30,40,0.6);

}
</style>

<script>
alert('This is my first game made on mobile. Thanks a lot for your support & feedback! ??\nFollow me if you like this??')

// ___________

//  GLOBAL VARIABLES

// ___________

const WATER = []

const FIRES =[]

let INTERVAL
const MIN_PX = 4
const FIRE_RESIZE = 1.005

const F_MIN_PX= 24

const F_DICR = 16

let NEW_F_R = 2


const SCREEN_W = innerWidth 

const SCREEN_H = innerHeight 

const GAME_OVER_FSIZE = 150

let EVENTS_STOP = false



let animationID

let START_TIME = 0


// ___________

//  MAIN GAME FUNCTIONS

// ___________

window.onload = () =>{

    
       START_TIME = Date.now()
    
       INTERVAL = random_fires()
    
       game_loop()
    

}



function game_over(){

    
cancelAnimationFrame(animationID) 
    
clearInterval(INTERVAL)
    
    

document.body.style.pointerEvents = 'none'
    
document.removeEventListener('click', water_splash, true)
 
    
gOverText = "\t\tYou're on Fire!\nYour score is "+ Math.round(TIME_ELAPSED/10)
    
alert(gOverText)
    
goverEl = document.getElementById('GO')
    
goverEl.innerHTML = "Game Over <br> Upvote & Share your score in comments ??"

}


function game_loop(){

    
animationID = requestAnimationFrame(game_loop)
    
TIME_ELAPSED = Date.now() - START_TIME 
    document.getElementById('score').innerHTML = Math.round(TIME_ELAPSED/10)
    
    
WATER.forEach((w,i)=>{
          
if (water_resize(w)) {
              
WATER.splice(i,1)
              
w.remove()
          
}
    
})

       
    

// update & resize fires
    
FIRES.forEach((f, i)=>{

       
if(f.is_remove){
          
FIRES.splice(i, 1)
          
return f.remove()
       
}
       
       
prev_fontSize = f.px.s;
       
if (prev_fontSize>GAME_OVER_FSIZE){
           
return game_over()
       
}
       
f.px.s *= FIRE_RESIZE
       
f.style.fontSize = Math.round(f.px.s) + 'px'
       
f.px.x -= (f.px.s-prev_fontSize)/2
       
f.px.y -= (f.px.s-prev_fontSize)/2
       
f.style.top = f.px.y + 'px'
       
f.style.left = f.px.x + 'px'
    
})
    

}


// ___________

//  CSS/HTML CONTROL FUNCTIONS

// ___________

function random_fires(){

    
const rand_px = () => Math.floor(Math.random()*NEW_F_R)*8 + F_MIN_PX
    
const rand_x = (px) => Math.floor(Math.random()*(SCREEN_W-2*px)+px)
    
const rand_y = (px) => Math.floor(Math.random()*(SCREEN_H-2*px)+px)
    
NEW_F_R += 1
    return setInterval(()=>{
        
let px = rand_px()
        
let x = rand_x(px)
        
let y = rand_y(px)
        
new_fire= addTextEl('??', x, y, px, 'fire')
        
new_fire.px ={s: px, x:x, y: y}
        
new_fire.is_remove = false
        
FIRES.push(new_fire)
        

         
// fire clicks event
        
new_fire.addEventListener('click', (e) => {

          fsize = parseFloat(e.target.style.fontSize)

          if(fsize <= F_MIN_PX+F_DICR){

          return e.target.is_remove = true

          }
          e.target.px.s = Math.max(fsize-F_DICR, F_MIN_PX)

          e.target.style.fontSize = e.target.px.s + 'px'

          e.target.px.x += F_DICR/2

          e.target.px.y += F_DICR/2

          e.target.style.left = e.target.px.x+'px'

          e.target.style.top = e.target.px.y+'px'

        });
        
        
    
}, 1000)

}




function addTextEl(text, x, y, px=16, elClass){

    
    let a = document.createElement('span');

    a.innerHTML = text
    document.body.appendChild(a)

    a.style.left = x + 'px'
    a.style.top = y + 'px'

    a.style.fontSize = String(px)+'px'

    if (elClass) {a.classList.add(elClass)}

    return a
}

function water_resize(wEl){

    wEl.speed = wEl.speed*0.97 || 
(Math.random()*0.5+2)
    
wEl.px_s= wEl.px_s*0.9 || parseFloat(wEl.style.fontSize) 
    
wEl.style.fontSize =Math.round(wEl.px_s)+'px'
    
wEl.angle = wEl.angle ||Math.random()*2*Math.PI
    
wEl.style.left = parseFloat(wEl.style.left)+Math.cos(wEl.angle)*wEl.speed + 'px'
    
wEl.style.top = parseFloat(wEl.style.top)+Math.sin(wEl.angle)*wEl.speed + 'px'
    
    
return (wEl.px_s<4)
}

function water_splash(){
    for (let i=0; i<10; i++){
          
w=addTextEl('??', event.clientX, event.clientY, 24, 'water')
          
WATER.push(w)
    
}

}


// ___________

//  GAME EVENTS

// ___________


document.addEventListener('click', water_splash, true)
</script>
   
</head>
    
<body>
        
<div>Score: <span id='score'>0</span></div>
        
<div id='GO'></div>
    
</body>

</html>
  
