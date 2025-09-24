<head>
  <title>Pekerace</title>
  <style>
    body {
      margin:0;
      display:flex;
      justify-content:center;
      align-items:center;
      height:100vh;
      background:linear-gradient(135deg, #0b0d12 0%, #1a1f2b 100%);
      font-family: system-ui, sans-serif;
      color:#fff;
    }
    .menu {
      text-align:center;
    }
    h1, h2 {
      margin-bottom:1.5rem;
      color:#e11d48;
      text-shadow:0 0 10px rgba(0,0,0,.7);
    }
    .btn {
      display:block;
      margin:0.5rem auto;
      padding:1rem 2rem;
      font-size:1.1rem;
      border:none;
      border-radius:12px;
      cursor:pointer;
      font-weight:600;
      background:#12151c;
      color:#fff;
      border:2px solid #e11d48;
      transition:all .2s;
      width:260px;
    }
    .btn:hover {
      background:#e11d48;
      color:#fff;
    }
    #gameScreen, #pilotMenu, #teamMenu, #driverMenu {
      display:none;
    }
    canvas{
      width:100%;
      height:auto;
      display:block;
    }
    /* Botones con logo y texto */
    .team-btn {
      display:flex;
      align-items:center;
      justify-content:flex-start;
      gap:15px;
      margin:0.5rem auto;
      padding:0.8rem 1rem;
      font-size:1rem;
      border:none;
      border-radius:12px;
      cursor:pointer;
      font-weight:600;
      background:#12151c;
      color:#fff;
      border:2px solid #e11d48;
      transition:all .2s;
      width:300px;
      text-align:left;
    }
    .team-btn:hover {
      background:#e11d48;
      color:#fff;
    }
    .team-btn img {
      width:50px;
      height:50px;
      object-fit:contain;
    }
    /* Organizar equipos en filas */
    #teamList {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 1rem;
    }
  </style>
</head>
<body>
  <!-- Menú principal -->
  <div id="mainMenu" class="menu">
    <h1>Pekerace</h1>
    <button class="btn" id="btnPilot">Carrera de piloto</button>
    <button class="btn" id="btnManager">Carrera de mánager</button>
    <button class="btn" id="btnSettings">Ajustes</button>
  </div>

  <!-- Menú Carrera de piloto -->
  <div id="pilotMenu" class="menu">
    <h1>Carrera de piloto</h1>
    <button class="btn" id="btnContinue">Continuar</button>
    <button class="btn" id="btnNew">Nueva carrera</button>
    <button class="btn" id="btnBack">Volver</button>
  </div>

  <!-- Selección de escudería -->
  <div id="teamMenu" class="menu">
    <h2>Elige tu escudería</h2>
    <div id="teamList"></div>
    <button class="btn" id="btnBackTeam">Volver</button>
  </div>

  <!-- Selección de piloto -->
  <div id="driverMenu" class="menu">
    <h2>Elige tu piloto</h2>
    <div id="driverList"></div>
    <button class="btn" id="btnBackDriver">Volver</button>
  </div>

<script>
  const mainMenu = document.getElementById('mainMenu');
  const gameScreen = document.getElementById('gameScreen');
  const pilotMenu = document.getElementById('pilotMenu');
  const teamMenu = document.getElementById('teamMenu');
  const driverMenu = document.getElementById('driverMenu');

  const btnPilot = document.getElementById('btnPilot');
  const btnManager = document.getElementById('btnManager');
  const btnSettings = document.getElementById('btnSettings');
  const btnContinue = document.getElementById('btnContinue');
  const btnNew = document.getElementById('btnNew');
  const btnBack = document.getElementById('btnBack');
  const btnBackTeam = document.getElementById('btnBackTeam');
  const btnBackDriver = document.getElementById('btnBackDriver');

  const teamListDiv = document.getElementById('teamList');
  const driverListDiv = document.getElementById('driverList');

  const teams = [
    { name: "Cerulean Racing", logo:"Imagenes/Cerulean.png", drivers: ["Marco Bellini", "Luca Moretti"] },
    { name: "Apex GP", logo:"Imagenes/Apex.png", drivers: ["James Hunter", "Oliver Clarke"] },
    { name: "Tempst Racing", logo:"Imagenes/Temps.png", drivers: ["Carlos Mendes", "Diego Alvarez"] },
    { name: "Blizzard Motors", logo:"Imagenes/Wizzard.png", drivers: ["Erik Johansen", "Mika Virtanen"] },
    { name: "Solaris F1 Team", logo:"Imagenes/Solar.png", drivers: ["Kenji Sato", "Hiroshi Tanaka"] },
    { name: "Vortex Dynamics", logo:"Imagenes/Vortex.png", drivers: ["Alexander Weiss", "Leon Schmidt"] },
    { name: "Panthera GP", logo:"Imagenes/Panthera.png", drivers: ["Samuel Johnson", "David Carter"] },
    { name: "Orion Motors", logo:"Imagenes/Orion.png", drivers: ["Rafael Souza", "Gabriel Costa"] },
    { name: "Falcon Speedworks", logo:"Imagenes/Falcon.png", drivers: ["Liam O’Connor", "Patrick Doyle"] },
    { name: "Titan Racing", logo:"Imagenes/Titan.png", drivers: ["Nikolai Petrov", "Ivan Volkov"] }
  ];

  let selectedTeam = null;

  btnPilot.addEventListener('click', ()=>{
    mainMenu.style.display='none';
    pilotMenu.style.display='block';
  });

  btnManager.addEventListener('click', ()=>{
    alert('Carrera de mánager aún no implementada');
  });

  btnSettings.addEventListener('click', ()=>{
    alert('Ajustes aún no implementados');
  });

  btnContinue.addEventListener('click', ()=>{
    pilotMenu.style.display='none';
    window.location.href = 'Game.html';
  });

  btnNew.addEventListener('click', ()=>{
    localStorage.setItem("currentRace", 0);
    localStorage.removeItem("lapTimes");
    localStorage.removeItem("completedRaces");
    pilotMenu.style.display='none';
    teamMenu.style.display='block';
    loadTeams();
  });

  btnBack.addEventListener('click', ()=>{
    pilotMenu.style.display='none';
    mainMenu.style.display='block';
  });

  btnBackTeam.addEventListener('click', ()=>{
    teamMenu.style.display='none';
    pilotMenu.style.display='block';
  });

  btnBackDriver.addEventListener('click', ()=>{
    driverMenu.style.display='none';
    teamMenu.style.display='block';
  });

  const btnOnline = document.getElementById('btnOnline');

  btnOnline.addEventListener('click', ()=>{
  mainMenu.style.display='none';
  window.location.href = 'Online.html'; // aquí cargaremos el menú online
});

  function loadTeams(){
    teamListDiv.innerHTML = '';
    teams.forEach((team)=>{
      const btn = document.createElement('button');
      btn.className = 'team-btn';
      btn.innerHTML = `<img src="${team.logo}" alt="${team.name}"> <span>${team.name}</span>`;
      btn.addEventListener('click', ()=>{
        selectedTeam = team;
        showDrivers(team);
      });
      teamListDiv.appendChild(btn);
    });
  }

  function showDrivers(team){
    teamMenu.style.display='none';
    driverMenu.style.display='block';
    driverListDiv.innerHTML = '';
    team.drivers.forEach(driver => {
      const btn = document.createElement('button');
      btn.className = 'btn';
      btn.textContent = driver;
      btn.addEventListener('click', ()=>{
        // Guardar selección en localStorage
        localStorage.setItem('selectedTeam', JSON.stringify(selectedTeam));
        localStorage.setItem('selectedDriver', driver);

      
        window.location.href = 'Game.html';
      });
      driverListDiv.appendChild(btn);
    });
  }
</script>
</body>
