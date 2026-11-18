index.html

<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="theme-color" content="#101014">
<title>Alejandro Box - Films & Séries</title>
<style>
*{box-sizing:border-box}
body{margin:0;background:#101014;color:white;font-family:Arial,sans-serif}
header{padding:20px 5%;display:flex;justify-content:space-between;align-items:center;background:#17171e}
.logo{font-size:23px;font-weight:900;color:#e63946}
.logo span{color:white}
.hero{padding:55px 6%;background:linear-gradient(90deg,#191923,#29202b,#15151c)}
.hero h1{font-size:38px;margin:0 0 12px}
.hero p{color:#ccc;max-width:420px;line-height:1.6}
.hero button,.watch{background:#e63946;color:white;border:0;padding:13px 22px;border-radius:7px;font-weight:bold;cursor:pointer}
main{padding:25px 5%}
.search{width:100%;padding:15px;border:1px solid #353541;background:#20202a;color:white;border-radius:8px;font-size:16px}
.categories{display:flex;gap:10px;overflow-x:auto;padding:20px 0}
.categories button{background:#24242d;color:white;border:0;padding:10px 17px;border-radius:20px;white-space:nowrap;cursor:pointer}
.categories button.active{background:#e63946}
h2{font-size:21px}
.grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(145px,1fr));gap:17px}
.card{background:#1c1c25;border-radius:10px;overflow:hidden;cursor:pointer}
.poster{height:190px;display:flex;align-items:center;justify-content:center;font-size:55px;background:linear-gradient(145deg,#402c4b,#171722)}
.card:nth-child(3n+2) .poster{background:linear-gradient(145deg,#244a55,#171722)}
.card:nth-child(3n) .poster{background:linear-gradient(145deg,#67412e,#171722)}
.info{padding:11px}
.info h3{font-size:15px;margin:0 0 6px}
.info p{font-size:12px;color:#aaa;margin:0}
footer{text-align:center;padding:30px;color:#888;font-size:12px}
.modal{display:none;position:fixed;inset:0;background:#000d;align-items:center;justify-content:center;padding:20px}
.modal-content{background:#22222c;padding:25px;border-radius:12px;max-width:400px;width:100%}
.close{float:right;background:none;color:white;border:0;font-size:25px;cursor:pointer}
@media(max-width:500px){.hero h1{font-size:29px}.poster{height:165px}}
</style>
</head>
<body>
<header>
  <div class="logo">ALEJANDRO <span>BOX</span></div>
  <div>🎬</div>
</header>

<section class="hero">
  <h1>Votre cinéma,<br>partout avec vous.</h1>
  <p>Découvrez des films et des séries. Explorez votre prochain coup de cœur.</p>
  <button onclick="document.getElementById('catalogue').scrollIntoView()">Explorer le catalogue</button>
</section>

<main id="catalogue">
  <input class="search" id="search" placeholder="🔍 Rechercher un film ou une série...">

  <div class="categories" id="categories">
    <button class="active" data-category="Tous">Tous</button>
    <button data-category="Action">Action</button>
    <button data-category="Aventure">Aventure</button>
    <button data-category="Comédie">Comédie</button>
    <button data-category="Drame">Drame</button>
    <button data-category="Série">Séries</button>
  </div>

  <h2>🍿 Notre catalogue</h2>
  <div class="grid" id="movies"></div>
</main>

<div class="modal" id="modal">
  <div class="modal-content">
    <button class="close" onclick="closeModal()">×</button>
    <div id="details"></div>
  </div>
</div>

<footer>© 2026 ALEJANDRO BOX · Votre cinéma, partout avec vous.</footer>

<script>
const films=[
 {title:"Le Dernier Voyage",category:"Aventure",year:2025,emoji:"🚀",description:"Une aventure extraordinaire au-delà des frontières."},
 {title:"Mission Secrète",category:"Action",year:2025,emoji:"🕵️",description:"Une mission qui va tout changer."},
 {title:"Rires en Famille",category:"Comédie",year:2024,emoji:"😂",description:"Une histoire pleine de surprises et de rires."},
 {title:"Le Grand Défi",category:"Drame",year:2025,emoji:"🏆",description:"Un rêve, du courage et une nouvelle chance."},
 {title:"Les Aventuriers",category:"Aventure",year:2024,emoji:"🌴",description:"Un voyage mystérieux vers l'inconnu."},
 {title:"Nouvelle Génération",category:"Série",year:2025,emoji:"🎭",description:"Une série sur les défis de la vie."}
];

let selected="Tous";
let favorites=[];

function render(){
 const query=document.getElementById("search").value.toLowerCase();
 const list=films.filter(f=>
 (selected==="Tous"||f.category===selected)&&
 f.title.toLowerCase().includes(query)
 );
 document.getElementById("movies").innerHTML=list.map(f=>`
 <article class="card" onclick="openModal('${f.title}')">
 <div class="poster">${f.emoji}</div>
 <div class="info">
 <h3>${f.title}</h3>
 <p>${f.category} · ${f.year}</p>
 </div></article>`).join("")||"<p>Aucun résultat trouvé.</p>";
}

document.getElementById("search").addEventListener("input",render);

document.getElementById("categories").addEventListener("click",e=>{
 if(e.target.tagName!=="BUTTON")return;
 selected=e.target.dataset.category;
 document.querySelectorAll(".categories button").forEach(b=>b.classList.remove("active"));
 e.target.classList.add("active");
 render();
});

function openModal(title){
 const f=films.find(x=>x.title===title);
 document.getElementById("details").innerHTML=`
 <h2>${f.emoji} ${f.title}</h2>
 <p>${f.category} · ${f.year}</p>
 <p>${f.description}</p>
 <p>Contenu de démonstration : la vidéo n'est pas encore intégrée.</p>
 <button class="watch" onclick="alert('Le lecteur vidéo sera ajouté dans une prochaine étape.')">▶ Regarder</button>
 <button class="watch" onclick="toggleFavorite('${f.title}')">♡ Favoris</button>`;
 document.getElementById("modal").style.display="flex";
}

function closeModal(){
 document.getElementById("modal").style.display="none";
}

function toggleFavorite(title){
 if(favorites.includes(title)){
 favorites=favorites.filter(x=>x!==title);
 }else{
 favorites.push(title);
 }
 alert(favorites.includes(title)?"Ajouté aux favoris !":"Retiré des favoris.");
}

render();
</script>
</body>
</html>