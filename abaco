ESTE ES EL CODIGO DE LA PLATAFORMA ABACO


<!doctype html>
<html lang="es" class="h-full">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>ABACO - Sistema de Administración Ciudadana Geoespacial</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="/_sdk/data_sdk.js"></script>
  <script src="/_sdk/element_sdk.js"></script>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css">
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
  <style>
    body {
      box-sizing: border-box;
    }
    
    @import url('https://fonts.googleapis.com/css2?family=Exo+2:wght@400;500;600;700;800&display=swap');
    
    * {
      font-family: 'Exo 2', sans-serif;
    }
    
    .status-badge {
      display: inline-block;
      padding: 0.25rem 0.75rem;
      border-radius: 9999px;
      font-size: 0.75rem;
      font-weight: 600;
      text-transform: uppercase;
      letter-spacing: 0.025em;
    }
    
    .card-hover {
      transition: all 0.3s ease;
    }
    
    .card-hover:hover {
      transform: translateY(-2px);
      box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1);
    }
    
    .stat-card {
      background: linear-gradient(135deg, var(--color-1) 0%, var(--color-2) 100%);
      border-radius: 1rem;
      padding: 1.5rem;
      color: white;
      position: relative;
      overflow: hidden;
    }
    
    .stat-card::before {
      content: '';
      position: absolute;
      top: 0;
      right: 0;
      width: 100px;
      height: 100px;
      background: rgba(255, 255, 255, 0.1);
      border-radius: 50%;
      transform: translate(30%, -30%);
    }
    
    .loading-spinner {
      border: 3px solid rgba(0, 0, 0, 0.1);
      border-left-color: #1e40af;
      border-radius: 50%;
      width: 24px;
      height: 24px;
      animation: spin 1s linear infinite;
    }
    
    @keyframes spin {
      to { transform: rotate(360deg); }
    }
    
    .tab-button {
      padding: 0.75rem 1.5rem;
      border-bottom: 3px solid transparent;
      font-weight: 600;
      transition: all 0.2s;
      cursor: pointer;
    }
    
    .tab-button:hover {
      background: rgba(0, 0, 0, 0.05);
    }
    
    .tab-button.active {
      color: #1e40af;
      border-bottom-color: #1e40af;
    }

    #map {
      height: 500px;
      border-radius: 0.75rem;
      border: 2px solid #e5e7eb;
    }

    .export-btn {
      background: linear-gradient(135deg, #059669 0%, #10b981 100%);
      transition: all 0.3s ease;
    }

    .export-btn:hover {
      transform: translateY(-2px);
      box-shadow: 0 8px 20px -5px rgba(5, 150, 105, 0.4);
    }

    .geo-icon {
      display: inline-block;
      animation: pulse 2s ease-in-out infinite;
    }

    @keyframes pulse {
      0%, 100% { opacity: 1; }
      50% { opacity: 0.6; }
    }
  </style>
  <style>@view-transition { navigation: auto; }</style>
 </head>
 <body class="h-full bg-gray-50">
  <div id="app" class="h-full w-full flex flex-col overflow-auto"><!-- Header -->
   <header class="bg-gradient-to-r from-blue-900 via-blue-800 to-blue-700 text-white shadow-lg">
    <div class="max-w-7xl mx-auto px-6 py-4">
     <div class="flex items-center justify-between">
      <div>
       <h1 id="systemTitle" class="text-3xl font-bold tracking-tight">🗺️ ABACO GIS</h1>
       <p id="campaignName" class="text-blue-200 text-sm mt-1">Arquitectura Blindada de Administración Ciudadana Operativa</p>
      </div>
      <div class="flex items-center gap-4">
       <div class="bg-blue-800 rounded-lg px-4 py-2 border border-blue-600">
        <p class="text-xs text-blue-200">Sistema Geoespacial</p>
        <p class="text-sm font-bold">QGIS Ready</p>
       </div>
       <div class="text-right">
        <p class="text-sm font-semibold" id="userName">Admin de Campaña</p>
        <p class="text-xs text-blue-200" id="userRole">admin_geoespacial</p>
       </div><button onclick="logout()" class="bg-blue-800 hover:bg-blue-900 px-4 py-2 rounded-lg text-sm font-medium transition"> Cerrar Sesión </button>
      </div>
     </div>
    </div>
   </header><!-- Navigation Tabs -->
   <nav class="bg-white border-b border-gray-200 shadow-sm">
    <div class="max-w-7xl mx-auto px-6">
     <div class="flex gap-1"><button class="tab-button active" onclick="switchTab('dashboard')">📊 Dashboard</button> <button class="tab-button" onclick="switchTab('votantes')">👥 Votantes</button> <button class="tab-button" onclick="switchTab('mapa')">🗺️ Mapa GIS</button> <button class="tab-button" onclick="switchTab('territorio')">📍 Territorio</button> <button class="tab-button" onclick="switchTab('lideres')">⭐ Líderes</button> <button class="tab-button" onclick="switchTab('exportar')">💾 Exportar</button>
     </div>
    </div>
   </nav><!-- Main Content -->
   <main class="flex-1 max-w-7xl w-full mx-auto px-6 py-8"><!-- Dashboard Tab -->
    <div id="dashboardTab" class="tab-content"><!-- Stats Grid -->
     <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
      <div class="stat-card" style="--color-1: #1e40af; --color-2: #3b82f6;">
       <div class="relative z-10">
        <p class="text-blue-100 text-sm font-medium mb-1">Total Votantes</p>
        <p id="statTotal" class="text-4xl font-bold">0</p>
       </div>
      </div>
      <div class="stat-card" style="--color-1: #16a34a; --color-2: #22c55e;">
       <div class="relative z-10">
        <p class="text-green-100 text-sm font-medium mb-1">Con Geolocalización</p>
        <p id="statGeolocalizados" class="text-4xl font-bold">0</p>
       </div>
      </div>
      <div class="stat-card" style="--color-1: #ea580c; --color-2: #f97316;">
       <div class="relative z-10">
        <p class="text-orange-100 text-sm font-medium mb-1">Pendientes</p>
        <p id="statPendientes" class="text-4xl font-bold">0</p>
       </div>
      </div>
      <div class="stat-card" style="--color-1: #7c3aed; --color-2: #a78bfa;">
       <div class="relative z-10">
        <p class="text-purple-100 text-sm font-medium mb-1">Zonas Activas</p>
        <p id="statZonas" class="text-4xl font-bold">0</p>
       </div>
      </div>
     </div><!-- Charts Section -->
     <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
      <div class="bg-white rounded-xl shadow-md p-6">
       <h3 class="text-lg font-bold text-gray-800 mb-4">Por Estado de Voto</h3>
       <div id="chartEstados" class="space-y-3"></div>
      </div>
      <div class="bg-white rounded-xl shadow-md p-6">
       <h3 class="text-lg font-bold text-gray-800 mb-4">Por Territorio</h3>
       <div id="chartTerritorios" class="space-y-3"></div>
      </div>
     </div>
    </div><!-- Votantes Tab -->
    <div id="votantesTab" class="tab-content hidden">
     <div class="bg-white rounded-xl shadow-md p-6 mb-6">
      <div class="flex items-center justify-between mb-6">
       <h2 class="text-2xl font-bold text-gray-800">Registro de Votantes</h2><button onclick="showAddVotanteForm()" class="bg-blue-600 hover:bg-blue-700 text-white px-6 py-2.5 rounded-lg font-semibold transition"> ➕ Nuevo Votante </button>
      </div><!-- Add Votante Form -->
      <form id="addVotanteForm" class="hidden mb-8 p-6 bg-gray-50 rounded-lg border-2 border-blue-200" onsubmit="handleAddVotante(event)">
       <h3 class="text-lg font-bold text-gray-800 mb-4">Agregar Nuevo Votante con Geolocalización</h3>
       <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4"><!-- Personal Info -->
        <div class="col-span-full">
         <h4 class="text-md font-semibold text-gray-700 mb-3 border-b pb-2">👤 Información Personal</h4>
        </div>
        <div><label class="block text-sm font-medium text-gray-700 mb-2" for="nombre">Nombre *</label> <input type="text" id="nombre" required class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500">
        </div>
        <div><label class="block text-sm font-medium text-gray-700 mb-2" for="apellido">Apellido *</label> <input type="text" id="apellido" required class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500">
        </div>
        <div><label class="block text-sm font-medium text-gray-700 mb-2" for="cedula">Cédula *</label> <input type="text" id="cedula" required class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500">
        </div>
        <div><label class="block text-sm font-medium text-gray-700 mb-2" for="telefono">Teléfono</label> <input type="tel" id="telefono" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500">
        </div>
        <div><label class="block text-sm font-medium text-gray-700 mb-2" for="email">Email</label> <input type="email" id="email" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500">
        </div><!-- Geographic Info -->
        <div class="col-span-full mt-4">
         <h4 class="text-md font-semibold text-gray-700 mb-3 border-b pb-2">🗺️ Información Geográfica</h4>
        </div>
        <div><label class="block text-sm font-medium text-gray-700 mb-2" for="ciudad">Ciudad *</label> <input type="text" id="ciudad" required class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500">
        </div>
        <div><label class="block text-sm font-medium text-gray-700 mb-2" for="comuna">Comuna *</label> <input type="text" id="comuna" required class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500">
        </div>
        <div><label class="block text-sm font-medium text-gray-700 mb-2" for="barrio">Barrio *</label> <input type="text" id="barrio" required class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500">
        </div>
        <div><label class="block text-sm font-medium text-gray-700 mb-2" for="zona">Zona *</label> <input type="text" id="zona" required class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500">
        </div>
        <div class="col-span-full"><label class="block text-sm font-medium text-gray-700 mb-2" for="direccion_completa">Dirección Completa</label> <input type="text" id="direccion_completa" placeholder="Calle, número, referencias" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500">
        </div><!-- Coordinates -->
        <div class="col-span-full mt-2">
         <h4 class="text-md font-semibold text-gray-700 mb-3 border-b pb-2">📍 Coordenadas GPS (Para QGIS)</h4>
        </div>
        <div><label class="block text-sm font-medium text-gray-700 mb-2" for="latitud">Latitud <span class="geo-icon">🌐</span></label> <input type="text" id="latitud" placeholder="-34.6037" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500">
         <p class="text-xs text-gray-500 mt-1">Ejemplo: -34.6037</p>
        </div>
        <div><label class="block text-sm font-medium text-gray-700 mb-2" for="longitud">Longitud <span class="geo-icon">🌐</span></label> <input type="text" id="longitud" placeholder="-58.3816" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500">
         <p class="text-xs text-gray-500 mt-1">Ejemplo: -58.3816</p>
        </div><!-- Electoral Info -->
        <div class="col-span-full mt-4">
         <h4 class="text-md font-semibold text-gray-700 mb-3 border-b pb-2">🗳️ Información Electoral</h4>
        </div>
        <div><label class="block text-sm font-medium text-gray-700 mb-2" for="estado_voto">Estado de Voto</label> <select id="estado_voto" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500"> <option value="pendiente">Pendiente</option> <option value="contactado">Contactado</option> <option value="confirmado">Confirmado</option> <option value="votó">Votó</option> <option value="no_votó">No Votó</option> </select>
        </div>
        <div><label class="block text-sm font-medium text-gray-700 mb-2" for="lider_asignado">Líder Asignado</label> <input type="text" id="lider_asignado" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500">
        </div>
        <div><label class="block text-sm font-medium text-gray-700 mb-2" for="puesto_votacion">Puesto de Votación</label> <input type="text" id="puesto_votacion" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500">
        </div>
        <div><label class="block text-sm font-medium text-gray-700 mb-2" for="puesto_latitud">Puesto Latitud</label> <input type="text" id="puesto_latitud" placeholder="-34.6037" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500">
        </div>
        <div><label class="block text-sm font-medium text-gray-700 mb-2" for="puesto_longitud">Puesto Longitud</label> <input type="text" id="puesto_longitud" placeholder="-58.3816" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500">
        </div>
        <div><label class="block text-sm font-medium text-gray-700 mb-2" for="mesa">Mesa</label> <input type="text" id="mesa" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500">
        </div>
       </div>
       <div class="flex gap-3 mt-6"><button type="submit" id="submitBtn" class="bg-green-600 hover:bg-green-700 text-white px-6 py-2.5 rounded-lg font-semibold transition flex items-center gap-2"> <span>💾 Guardar Votante</span> </button> <button type="button" onclick="hideAddVotanteForm()" class="bg-gray-400 hover:bg-gray-500 text-white px-6 py-2.5 rounded-lg font-semibold transition"> Cancelar </button>
       </div>
      </form><!-- Votantes List -->
      <div id="votantesList"></div>
     </div>
    </div><!-- Mapa Tab -->
    <div id="mapaTab" class="tab-content hidden">
     <div class="bg-white rounded-xl shadow-md p-6 mb-6">
      <div class="flex items-center justify-between mb-6">
       <div>
        <h2 class="text-2xl font-bold text-gray-800">Mapa Geoespacial Interactivo</h2>
        <p class="text-sm text-gray-600 mt-1">Visualización de votantes geolocalizados compatible con QGIS</p>
       </div>
       <div class="flex gap-3"><button onclick="centerMap()" class="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded-lg text-sm font-semibold transition"> 🎯 Centrar Mapa </button> <button onclick="refreshMap()" class="bg-green-600 hover:bg-green-700 text-white px-4 py-2 rounded-lg text-sm font-semibold transition"> 🔄 Actualizar </button>
       </div>
      </div>
      <div id="map" class="mb-4"></div>
      <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mt-6">
       <div class="bg-blue-50 border border-blue-200 rounded-lg p-4">
        <div class="flex items-center gap-3">
         <div class="w-4 h-4 bg-blue-600 rounded-full"></div>
         <div>
          <p class="text-sm font-semibold text-gray-700">Votantes</p>
          <p class="text-xs text-gray-600">Ubicación del votante</p>
         </div>
        </div>
       </div>
       <div class="bg-green-50 border border-green-200 rounded-lg p-4">
        <div class="flex items-center gap-3">
         <div class="w-4 h-4 bg-green-600 rounded-full"></div>
         <div>
          <p class="text-sm font-semibold text-gray-700">Confirmados</p>
          <p class="text-xs text-gray-600">Voto confirmado</p>
         </div>
        </div>
       </div>
       <div class="bg-orange-50 border border-orange-200 rounded-lg p-4">
        <div class="flex items-center gap-3">
         <div class="w-4 h-4 bg-orange-600 rounded-full"></div>
         <div>
          <p class="text-sm font-semibold text-gray-700">Pendientes</p>
          <p class="text-xs text-gray-600">Por contactar</p>
         </div>
        </div>
       </div>
      </div>
     </div>
    </div><!-- Territorio Tab -->
    <div id="territorioTab" class="tab-content hidden">
     <div class="bg-white rounded-xl shadow-md p-6">
      <h2 class="text-2xl font-bold text-gray-800 mb-6">Estructura Territorial</h2>
      <div id="territorioContent" class="space-y-4"></div>
     </div>
    </div><!-- Líderes Tab -->
    <div id="lideresTab" class="tab-content hidden">
     <div class="bg-white rounded-xl shadow-md p-6">
      <h2 class="text-2xl font-bold text-gray-800 mb-6">Gestión de Líderes</h2>
      <div id="lideresContent" class="space-y-4"></div>
     </div>
    </div><!-- Exportar Tab -->
    <div id="exportarTab" class="tab-content hidden">
     <div class="bg-white rounded-xl shadow-md p-6">
      <h2 class="text-2xl font-bold text-gray-800 mb-4">Exportar Datos para QGIS</h2>
      <p class="text-gray-600 mb-8">Descarga tus datos en formatos compatibles con sistemas GIS profesionales</p>
      <div class="grid grid-cols-1 md:grid-cols-2 gap-6"><!-- GeoJSON Export -->
       <div class="border-2 border-green-200 rounded-xl p-6 card-hover bg-gradient-to-br from-green-50 to-white">
        <div class="flex items-start gap-4">
         <div class="text-4xl">
          🗺️
         </div>
         <div class="flex-1">
          <h3 class="text-xl font-bold text-gray-800 mb-2">GeoJSON</h3>
          <p class="text-sm text-gray-600 mb-4">Formato estándar para datos geoespaciales. Compatible con QGIS, Google Earth y la mayoría de herramientas GIS.</p>
          <ul class="text-xs text-gray-600 mb-4 space-y-1">
           <li>✅ Geometría de puntos (votantes)</li>
           <li>✅ Atributos completos</li>
           <li>✅ Sistema WGS84 (EPSG:4326)</li>
          </ul><button onclick="exportGeoJSON()" class="export-btn text-white px-6 py-2.5 rounded-lg font-semibold w-full"> 📥 Descargar GeoJSON </button>
         </div>
        </div>
       </div><!-- CSV Export -->
       <div class="border-2 border-blue-200 rounded-xl p-6 card-hover bg-gradient-to-br from-blue-50 to-white">
        <div class="flex items-start gap-4">
         <div class="text-4xl">
          📊
         </div>
         <div class="flex-1">
          <h3 class="text-xl font-bold text-gray-800 mb-2">CSV con Coordenadas</h3>
          <p class="text-sm text-gray-600 mb-4">Archivo de texto delimitado. Importable en QGIS como capa de puntos XY.</p>
          <ul class="text-xs text-gray-600 mb-4 space-y-1">
           <li>✅ Columnas Latitud/Longitud</li>
           <li>✅ Compatible con Excel</li>
           <li>✅ Fácil de editar</li>
          </ul><button onclick="exportCSV()" class="bg-gradient-to-r from-blue-600 to-blue-700 hover:from-blue-700 hover:to-blue-800 text-white px-6 py-2.5 rounded-lg font-semibold w-full transition"> 📥 Descargar CSV </button>
         </div>
        </div>
       </div><!-- KML Export -->
       <div class="border-2 border-red-200 rounded-xl p-6 card-hover bg-gradient-to-br from-red-50 to-white">
        <div class="flex items-start gap-4">
         <div class="text-4xl">
          🌍
         </div>
         <div class="flex-1">
          <h3 class="text-xl font-bold text-gray-800 mb-2">KML (Google Earth)</h3>
          <p class="text-sm text-gray-600 mb-4">Formato de Google Earth. Visualización 3D con marcadores personalizados.</p>
          <ul class="text-xs text-gray-600 mb-4 space-y-1">
           <li>✅ Compatible con Google Earth</li>
           <li>✅ Marcadores con información</li>
           <li>✅ Importable en QGIS</li>
          </ul><button onclick="exportKML()" class="bg-gradient-to-r from-red-600 to-red-700 hover:from-red-700 hover:to-red-800 text-white px-6 py-2.5 rounded-lg font-semibold w-full transition"> 📥 Descargar KML </button>
         </div>
        </div>
       </div><!-- Shapefile Info -->
       <div class="border-2 border-purple-200 rounded-xl p-6 card-hover bg-gradient-to-br from-purple-50 to-white">
        <div class="flex items-start gap-4">
         <div class="text-4xl">
          📦
         </div>
         <div class="flex-1">
          <h3 class="text-xl font-bold text-gray-800 mb-2">Instrucciones QGIS</h3>
          <p class="text-sm text-gray-600 mb-4">Cómo importar datos en QGIS Desktop</p>
          <ul class="text-xs text-gray-600 space-y-2">
           <li><strong>GeoJSON:</strong> Arrastrar al canvas de QGIS</li>
           <li><strong>CSV:</strong> Capa → Añadir capa → Texto delimitado</li>
           <li><strong>KML:</strong> Capa → Añadir capa → Vector</li>
          </ul>
          <div class="mt-4 p-3 bg-purple-100 rounded-lg">
           <p class="text-xs font-semibold text-purple-800">💡 Sistema de coordenadas: WGS84 (EPSG:4326)</p>
          </div>
         </div>
        </div>
       </div>
      </div><!-- Export Stats -->
      <div class="mt-8 p-6 bg-gradient-to-r from-gray-100 to-gray-50 rounded-xl border border-gray-200">
       <h3 class="text-lg font-bold text-gray-800 mb-4">📈 Estadísticas de Exportación</h3>
       <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
        <div class="text-center">
         <p class="text-3xl font-bold text-blue-600" id="exportTotal">0</p>
         <p class="text-sm text-gray-600">Total Registros</p>
        </div>
        <div class="text-center">
         <p class="text-3xl font-bold text-green-600" id="exportGeo">0</p>
         <p class="text-sm text-gray-600">Con Coordenadas</p>
        </div>
        <div class="text-center">
         <p class="text-3xl font-bold text-orange-600" id="exportSinGeo">0</p>
         <p class="text-sm text-gray-600">Sin Coordenadas</p>
        </div>
        <div class="text-center">
         <p class="text-3xl font-bold text-purple-600" id="exportPuestos">0</p>
         <p class="text-sm text-gray-600">Puestos de Votación</p>
        </div>
       </div>
      </div>
     </div>
    </div>
   </main><!-- Footer -->
   <footer class="bg-white border-t border-gray-200 mt-auto">
    <div class="max-w-7xl mx-auto px-6 py-4">
     <p class="text-center text-sm text-gray-600"><strong>ABACO GIS</strong> - Arquitectura Blindada de Administración Ciudadana Operativa | Sistema Geoespacial Compatible con QGIS</p>
    </div>
   </footer>
  </div>
  <script>
    let votantesData = [];
    let map = null;
    let markers = [];
    
    const defaultConfig = {
      system_title: "ABACO GIS",
      campaign_name: "Arquitectura Blindada de Administración Ciudadana Operativa",
      primary_color: "#1e40af",
      secondary_color: "#3b82f6",
      success_color: "#16a34a",
      warning_color: "#ea580c",
      text_color: "#1f2937"
    };

    const dataHandler = {
      onDataChanged(data) {
        votantesData = data.filter(v => v.activo !== false);
        updateDashboard();
        renderVotantesList();
        renderTerritorioView();
        renderLideresView();
        updateExportStats();
        if (map) {
          updateMapMarkers();
        }
      }
    };

    async function onConfigChange(config) {
      const systemTitle = config.system_title || defaultConfig.system_title;
      const campaignName = config.campaign_name || defaultConfig.campaign_name;
      
      document.getElementById('systemTitle').textContent = systemTitle;
      document.getElementById('campaignName').textContent = campaignName;
    }

    function mapToCapabilities(config) {
      return {
        recolorables: [
          {
            get: () => config.primary_color || defaultConfig.primary_color,
            set: (value) => {
              window.elementSdk.config.primary_color = value;
              window.elementSdk.setConfig({ primary_color: value });
            }
          },
          {
            get: () => config.secondary_color || defaultConfig.secondary_color,
            set: (value) => {
              window.elementSdk.config.secondary_color = value;
              window.elementSdk.setConfig({ secondary_color: value });
            }
          },
          {
            get: () => config.success_color || defaultConfig.success_color,
            set: (value) => {
              window.elementSdk.config.success_color = value;
              window.elementSdk.setConfig({ success_color: value });
            }
          },
          {
            get: () => config.warning_color || defaultConfig.warning_color,
            set: (value) => {
              window.elementSdk.config.warning_color = value;
              window.elementSdk.setConfig({ warning_color: value });
            }
          },
          {
            get: () => config.text_color || defaultConfig.text_color,
            set: (value) => {
              window.elementSdk.config.text_color = value;
              window.elementSdk.setConfig({ text_color: value });
            }
          }
        ],
        borderables: [],
        fontEditable: undefined,
        fontSizeable: undefined
      };
    }

    function mapToEditPanelValues(config) {
      return new Map([
        ["system_title", config.system_title || defaultConfig.system_title],
        ["campaign_name", config.campaign_name || defaultConfig.campaign_name]
      ]);
    }

    async function initApp() {
      if (window.elementSdk) {
        await window.elementSdk.init({
          defaultConfig,
          onConfigChange,
          mapToCapabilities,
          mapToEditPanelValues
        });
      }

      const initResult = await window.dataSdk.init(dataHandler);
      if (!initResult.isOk) {
        showToast('Error al inicializar el sistema', 'error');
      }

      initMap();
    }

    function initMap() {
      map = L.map('map').setView([-34.6037, -58.3816], 12);
      
      L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '© OpenStreetMap contributors',
        maxZoom: 19
      }).addTo(map);
    }

    function updateMapMarkers() {
      markers.forEach(marker => map.removeLayer(marker));
      markers = [];

      const geolocalizados = votantesData.filter(v => v.latitud && v.longitud);
      
      geolocalizados.forEach(votante => {
        const lat = parseFloat(votante.latitud);
        const lng = parseFloat(votante.longitud);
        
        if (!isNaN(lat) && !isNaN(lng)) {
          const color = getMarkerColor(votante.estado_voto);
          
          const marker = L.circleMarker([lat, lng], {
            radius: 8,
            fillColor: color,
            color: '#fff',
            weight: 2,
            opacity: 1,
            fillOpacity: 0.8
          }).addTo(map);

          marker.bindPopup(`
            <div class="p-2">
              <h3 class="font-bold text-gray-800">${votante.nombre} ${votante.apellido}</h3>
              <p class="text-sm text-gray-600 mt-1"><strong>Cédula:</strong> ${votante.cedula}</p>
              <p class="text-sm text-gray-600"><strong>Estado:</strong> ${votante.estado_voto}</p>
              <p class="text-sm text-gray-600"><strong>Zona:</strong> ${votante.zona}</p>
              <p class="text-sm text-gray-600"><strong>Barrio:</strong> ${votante.barrio}</p>
              ${votante.lider_asignado ? `<p class="text-sm text-gray-600"><strong>Líder:</strong> ${votante.lider_asignado}</p>` : ''}
              <p class="text-xs text-gray-500 mt-2">📍 ${lat.toFixed(6)}, ${lng.toFixed(6)}</p>
            </div>
          `);

          markers.push(marker);
        }
      });

      if (markers.length > 0) {
        const group = L.featureGroup(markers);
        map.fitBounds(group.getBounds().pad(0.1));
      }
    }

    function getMarkerColor(estado) {
      const colors = {
        'pendiente': '#ea580c',
        'contactado': '#3b82f6',
        'confirmado': '#16a34a',
        'votó': '#059669',
        'no_votó': '#dc2626'
      };
      return colors[estado] || '#6b7280';
    }

    function centerMap() {
      if (markers.length > 0) {
        const group = L.featureGroup(markers);
        map.fitBounds(group.getBounds().pad(0.1));
      } else {
        map.setView([-34.6037, -58.3816], 12);
      }
    }

    function refreshMap() {
      updateMapMarkers();
      showToast('Mapa actualizado', 'success');
    }

    function switchTab(tabName) {
      document.querySelectorAll('.tab-content').forEach(tab => tab.classList.add('hidden'));
      document.querySelectorAll('.tab-button').forEach(btn => btn.classList.remove('active'));
      
      document.getElementById(tabName + 'Tab').classList.remove('hidden');
      event.target.classList.add('active');

      if (tabName === 'mapa' && map) {
        setTimeout(() => {
          map.invalidateSize();
          updateMapMarkers();
        }, 100);
      }
    }

    function updateDashboard() {
      const total = votantesData.length;
      const geolocalizados = votantesData.filter(v => v.latitud && v.longitud).length;
      const confirmados = votantesData.filter(v => v.estado_voto === 'confirmado' || v.estado_voto === 'votó').length;
      const pendientes = votantesData.filter(v => v.estado_voto === 'pendiente').length;
      const zonas = new Set(votantesData.map(v => v.zona)).size;

      document.getElementById('statTotal').textContent = total;
      document.getElementById('statGeolocalizados').textContent = geolocalizados;
      document.getElementById('statPendientes').textContent = pendientes;
      document.getElementById('statZonas').textContent = zonas;

      renderChartEstados();
      renderChartTerritorios();
    }

    function renderChartEstados() {
      const estados = {};
      votantesData.forEach(v => {
        estados[v.estado_voto] = (estados[v.estado_voto] || 0) + 1;
      });

      const total = votantesData.length || 1;
      const colores = {
        'pendiente': '#ea580c',
        'contactado': '#3b82f6',
        'confirmado': '#16a34a',
        'votó': '#059669',
        'no_votó': '#dc2626'
      };

      const html = Object.entries(estados).map(([estado, count]) => {
        const porcentaje = ((count / total) * 100).toFixed(1);
        const color = colores[estado] || '#6b7280';
        return `
          <div class="mb-2">
            <div class="flex justify-between items-center mb-1">
              <span class="text-sm font-medium text-gray-700 capitalize">${estado}</span>
              <span class="text-sm font-semibold text-gray-900">${count} (${porcentaje}%)</span>
            </div>
            <div class="w-full bg-gray-200 rounded-full h-3">
              <div class="h-3 rounded-full transition-all duration-500" style="width: ${porcentaje}%; background-color: ${color};"></div>
            </div>
          </div>
        `;
      }).join('');

      document.getElementById('chartEstados').innerHTML = html || '<p class="text-gray-500 text-center py-4">Sin datos</p>';
    }

    function renderChartTerritorios() {
      const territorios = {};
      votantesData.forEach(v => {
        const key = `${v.ciudad} - ${v.comuna}`;
        territorios[key] = (territorios[key] || 0) + 1;
      });

      const total = votantesData.length || 1;
      const entries = Object.entries(territorios).sort((a, b) => b[1] - a[1]).slice(0, 5);

      const html = entries.map(([territorio, count]) => {
        const porcentaje = ((count / total) * 100).toFixed(1);
        return `
          <div class="mb-2">
            <div class="flex justify-between items-center mb-1">
              <span class="text-sm font-medium text-gray-700">${territorio}</span>
              <span class="text-sm font-semibold text-gray-900">${count} (${porcentaje}%)</span>
            </div>
            <div class="w-full bg-gray-200 rounded-full h-3">
              <div class="bg-blue-600 h-3 rounded-full transition-all duration-500" style="width: ${porcentaje}%;"></div>
            </div>
          </div>
        `;
      }).join('');

      document.getElementById('chartTerritorios').innerHTML = html || '<p class="text-gray-500 text-center py-4">Sin datos</p>';
    }

    function showAddVotanteForm() {
      document.getElementById('addVotanteForm').classList.remove('hidden');
      document.getElementById('nombre').focus();
    }

    function hideAddVotanteForm() {
      document.getElementById('addVotanteForm').classList.add('hidden');
      document.getElementById('addVotanteForm').reset();
    }

    async function handleAddVotante(event) {
      event.preventDefault();
      
      if (votantesData.length >= 999) {
        showToast('Límite máximo de 999 votantes alcanzado', 'error');
        return;
      }

      const submitBtn = document.getElementById('submitBtn');
      const originalText = submitBtn.innerHTML;
      submitBtn.innerHTML = '<div class="loading-spinner mx-auto"></div>';
      submitBtn.disabled = true;

      const formData = {
        id: 'VOT-' + Date.now(),
        nombre: document.getElementById('nombre').value,
        apellido: document.getElementById('apellido').value,
        cedula: document.getElementById('cedula').value,
        telefono: document.getElementById('telefono').value,
        email: document.getElementById('email').value,
        ciudad: document.getElementById('ciudad').value,
        comuna: document.getElementById('comuna').value,
        barrio: document.getElementById('barrio').value,
        zona: document.getElementById('zona').value,
        latitud: document.getElementById('latitud').value,
        longitud: document.getElementById('longitud').value,
        direccion_completa: document.getElementById('direccion_completa').value,
        estado_voto: document.getElementById('estado_voto').value,
        lider_asignado: document.getElementById('lider_asignado').value,
        puesto_votacion: document.getElementById('puesto_votacion').value,
        puesto_latitud: document.getElementById('puesto_latitud').value,
        puesto_longitud: document.getElementById('puesto_longitud').value,
        mesa: document.getElementById('mesa').value,
        fecha_registro: new Date().toISOString(),
        ultimo_contacto: new Date().toISOString(),
        activo: true
      };

      const result = await window.dataSdk.create(formData);
      
      submitBtn.innerHTML = originalText;
      submitBtn.disabled = false;

      if (result.isOk) {
        showToast('Votante registrado exitosamente', 'success');
        hideAddVotanteForm();
      } else {
        showToast('Error al registrar votante', 'error');
      }
    }

    function renderVotantesList() {
      const container = document.getElementById('votantesList');
      
      if (votantesData.length === 0) {
        container.innerHTML = `
          <div class="text-center py-16">
            <div class="text-6xl mb-4">📋</div>
            <p class="text-xl font-semibold text-gray-700 mb-2">No hay votantes registrados</p>
            <p class="text-gray-500">Comienza agregando el primer votante al sistema</p>
          </div>
        `;
        return;
      }

      const html = votantesData.map(votante => {
        const estadoColors = {
          'pendiente': 'bg-orange-100 text-orange-800',
          'contactado': 'bg-blue-100 text-blue-800',
          'confirmado': 'bg-green-100 text-green-800',
          'votó': 'bg-emerald-100 text-emerald-800',
          'no_votó': 'bg-red-100 text-red-800'
        };

        const hasGeo = votante.latitud && votante.longitud;
        
        return `
          <div class="bg-white border border-gray-200 rounded-lg p-5 mb-3 card-hover">
            <div class="flex items-start justify-between">
              <div class="flex-1">
                <div class="flex items-center gap-3 mb-3">
                  <h3 class="text-lg font-bold text-gray-900">${votante.nombre} ${votante.apellido}</h3>
                  <span class="status-badge ${estadoColors[votante.estado_voto] || 'bg-gray-100 text-gray-800'}">${votante.estado_voto}</span>
                  ${hasGeo ? '<span class="text-green-600 text-sm font-semibold">🌐 Geolocalizado</span>' : '<span class="text-gray-400 text-sm">📍 Sin coordenadas</span>'}
                </div>
                <div class="grid grid-cols-2 lg:grid-cols-4 gap-3 text-sm">
                  <div>
                    <p class="text-gray-500 font-medium">Cédula</p>
                    <p class="text-gray-900 font-semibold">${votante.cedula}</p>
                  </div>
                  <div>
                    <p class="text-gray-500 font-medium">Teléfono</p>
                    <p class="text-gray-900">${votante.telefono || '-'}</p>
                  </div>
                  <div>
                    <p class="text-gray-500 font-medium">Zona</p>
                    <p class="text-gray-900">${votante.zona}</p>
                  </div>
                  <div>
                    <p class="text-gray-500 font-medium">Líder</p>
                    <p class="text-gray-900">${votante.lider_asignado || 'Sin asignar'}</p>
                  </div>
                </div>
                <div class="mt-3 text-sm text-gray-600">
                  <span class="font-medium">📍 ${votante.ciudad} → ${votante.comuna} → ${votante.barrio}</span>
                  ${hasGeo ? `<br><span class="text-xs text-green-600">🌐 Coords: ${parseFloat(votante.latitud).toFixed(4)}, ${parseFloat(votante.longitud).toFixed(4)}</span>` : ''}
                </div>
              </div>
              <div class="flex gap-2 ml-4">
                <button onclick="editVotante('${votante.__backendId}')" class="text-blue-600 hover:text-blue-800 px-3 py-1.5 rounded hover:bg-blue-50 transition font-medium text-sm">
                  ✏️ Editar
                </button>
                <button onclick="deleteVotante('${votante.__backendId}')" class="text-red-600 hover:text-red-800 px-3 py-1.5 rounded hover:bg-red-50 transition font-medium text-sm">
                  🗑️ Eliminar
                </button>
              </div>
            </div>
          </div>
        `;
      }).join('');

      container.innerHTML = html;
    }

    function renderTerritorioView() {
      const territorios = {};
      
      votantesData.forEach(v => {
        if (!territorios[v.ciudad]) {
          territorios[v.ciudad] = {};
        }
        if (!territorios[v.ciudad][v.comuna]) {
          territorios[v.ciudad][v.comuna] = {};
        }
        if (!territorios[v.ciudad][v.comuna][v.barrio]) {
          territorios[v.ciudad][v.comuna][v.barrio] = [];
        }
        territorios[v.ciudad][v.comuna][v.barrio].push(v);
      });

      const html = Object.entries(territorios).map(([ciudad, comunas]) => `
        <div class="border border-gray-200 rounded-lg overflow-hidden">
          <div class="bg-blue-600 text-white px-5 py-3">
            <h3 class="text-xl font-bold">🏙️ ${ciudad}</h3>
            <p class="text-sm text-blue-100 mt-1">${Object.values(comunas).flat().reduce((sum, b) => sum + Object.values(b).flat().length, 0)} votantes</p>
          </div>
          <div class="p-5 space-y-4">
            ${Object.entries(comunas).map(([comuna, barrios]) => `
              <div class="bg-gray-50 rounded-lg p-4">
                <h4 class="font-bold text-gray-800 mb-3">📍 Comuna: ${comuna}</h4>
                <div class="space-y-2 pl-4">
                  ${Object.entries(barrios).map(([barrio, votantes]) => `
                    <div class="flex items-center justify-between py-2 border-b border-gray-200 last:border-0">
                      <span class="text-gray-700 font-medium">🏘️ ${barrio}</span>
                      <span class="bg-blue-100 text-blue-800 px-3 py-1 rounded-full text-sm font-semibold">${votantes.length} votantes</span>
                    </div>
                  `).join('')}
                </div>
              </div>
            `).join('')}
          </div>
        </div>
      `).join('');

      document.getElementById('territorioContent').innerHTML = html || '<p class="text-gray-500 text-center py-8">No hay datos territoriales disponibles</p>';
    }

    function renderLideresView() {
      const lideres = {};
      
      votantesData.forEach(v => {
        if (v.lider_asignado) {
          if (!lideres[v.lider_asignado]) {
            lideres[v.lider_asignado] = {
              total: 0,
              confirmados: 0,
              zonas: new Set()
            };
          }
          lideres[v.lider_asignado].total++;
          if (v.estado_voto === 'confirmado' || v.estado_voto === 'votó') {
            lideres[v.lider_asignado].confirmados++;
          }
          lideres[v.lider_asignado].zonas.add(v.zona);
        }
      });

      const html = Object.entries(lideres).map(([nombre, stats]) => {
        const efectividad = stats.total > 0 ? ((stats.confirmados / stats.total) * 100).toFixed(1) : 0;
        return `
          <div class="bg-white border border-gray-200 rounded-lg p-5 card-hover">
            <div class="flex items-center justify-between mb-4">
              <div class="flex items-center gap-3">
                <div class="w-12 h-12 bg-gradient-to-br from-purple-500 to-purple-700 rounded-full flex items-center justify-center text-white text-xl font-bold">
                  ${nombre.charAt(0).toUpperCase()}
                </div>
                <div>
                  <h3 class="text-lg font-bold text-gray-900">${nombre}</h3>
                  <p class="text-sm text-gray-500">${stats.zonas.size} zona(s) asignada(s)</p>
                </div>
              </div>
              <div class="text-right">
                <p class="text-3xl font-bold text-purple-600">${efectividad}%</p>
                <p class="text-xs text-gray-500">Efectividad</p>
              </div>
            </div>
            <div class="grid grid-cols-3 gap-4 pt-4 border-t border-gray-200">
              <div class="text-center">
                <p class="text-2xl font-bold text-gray-900">${stats.total}</p>
                <p class="text-xs text-gray-500 mt-1">Total votantes</p>
              </div>
              <div class="text-center">
                <p class="text-2xl font-bold text-green-600">${stats.confirmados}</p>
                <p class="text-xs text-gray-500 mt-1">Confirmados</p>
              </div>
              <div class="text-center">
                <p class="text-2xl font-bold text-orange-600">${stats.total - stats.confirmados}</p>
                <p class="text-xs text-gray-500 mt-1">Pendientes</p>
              </div>
            </div>
          </div>
        `;
      }).join('');

      document.getElementById('lideresContent').innerHTML = html || '<p class="text-gray-500 text-center py-8">No hay líderes asignados aún</p>';
    }

    function updateExportStats() {
      const total = votantesData.length;
      const conGeo = votantesData.filter(v => v.latitud && v.longitud).length;
      const sinGeo = total - conGeo;
      const puestos = new Set(votantesData.filter(v => v.puesto_votacion).map(v => v.puesto_votacion)).size;

      document.getElementById('exportTotal').textContent = total;
      document.getElementById('exportGeo').textContent = conGeo;
      document.getElementById('exportSinGeo').textContent = sinGeo;
      document.getElementById('exportPuestos').textContent = puestos;
    }

    function exportGeoJSON() {
      const features = votantesData
        .filter(v => v.latitud && v.longitud)
        .map(v => ({
          type: 'Feature',
          geometry: {
            type: 'Point',
            coordinates: [parseFloat(v.longitud), parseFloat(v.latitud)]
          },
          properties: {
            id: v.id,
            nombre: v.nombre,
            apellido: v.apellido,
            cedula: v.cedula,
            telefono: v.telefono,
            email: v.email,
            ciudad: v.ciudad,
            comuna: v.comuna,
            barrio: v.barrio,
            zona: v.zona,
            direccion: v.direccion_completa,
            estado_voto: v.estado_voto,
            lider: v.lider_asignado,
            puesto: v.puesto_votacion,
            mesa: v.mesa,
            fecha_registro: v.fecha_registro
          }
        }));

      const geojson = {
        type: 'FeatureCollection',
        crs: {
          type: 'name',
          properties: {
            name: 'EPSG:4326'
          }
        },
        features: features
      };

      downloadFile(JSON.stringify(geojson, null, 2), 'abaco_votantes.geojson', 'application/geo+json');
      showToast(`✅ Exportados ${features.length} votantes en formato GeoJSON`, 'success');
    }

    function exportCSV() {
      const headers = [
        'id', 'nombre', 'apellido', 'cedula', 'telefono', 'email',
        'ciudad', 'comuna', 'barrio', 'zona', 'latitud', 'longitud',
        'direccion_completa', 'estado_voto', 'lider_asignado',
        'puesto_votacion', 'puesto_latitud', 'puesto_longitud', 'mesa',
        'fecha_registro'
      ];

      const rows = votantesData.map(v => [
        v.id, v.nombre, v.apellido, v.cedula, v.telefono, v.email,
        v.ciudad, v.comuna, v.barrio, v.zona, v.latitud, v.longitud,
        v.direccion_completa, v.estado_voto, v.lider_asignado,
        v.puesto_votacion, v.puesto_latitud, v.puesto_longitud, v.mesa,
        v.fecha_registro
      ].map(field => `"${field || ''}"`).join(','));

      const csv = [headers.join(','), ...rows].join('\n');
      
      downloadFile(csv, 'abaco_votantes.csv', 'text/csv');
      showToast(`✅ Exportados ${votantesData.length} registros en formato CSV`, 'success');
    }

    function exportKML() {
      const placemarks = votantesData
        .filter(v => v.latitud && v.longitud)
        .map(v => `
    <Placemark>
      <name>${v.nombre} ${v.apellido}</name>
      <description><![CDATA[
        <b>Cédula:</b> ${v.cedula}<br/>
        <b>Estado:</b> ${v.estado_voto}<br/>
        <b>Zona:</b> ${v.zona}<br/>
        <b>Barrio:</b> ${v.barrio}<br/>
        <b>Líder:</b> ${v.lider_asignado || 'Sin asignar'}
      ]]></description>
      <Point>
        <coordinates>${v.longitud},${v.latitud},0</coordinates>
      </Point>
    </Placemark>`).join('\n');

      const kml = `<?xml version="1.0" encoding="UTF-8"?>
<kml xmlns="http://www.opengis.net/kml/2.2">
  <Document>
    <name>ABACO - Votantes Geolocalizados</name>
    <description>Sistema de Administración Ciudadana Operativa</description>
${placemarks}
  </Document>
</kml>`;

      downloadFile(kml, 'abaco_votantes.kml', 'application/vnd.google-earth.kml+xml');
      showToast(`✅ Exportados ${votantesData.filter(v => v.latitud && v.longitud).length} votantes en formato KML`, 'success');
    }

    function downloadFile(content, filename, mimeType) {
      const blob = new Blob([content], { type: mimeType });
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = filename;
      document.body.appendChild(a);
      a.click();
      document.body.removeChild(a);
      URL.revokeObjectURL(url);
    }

    function editVotante(backendId) {
      showToast('Función de edición en desarrollo', 'info');
    }

    async function deleteVotante(backendId) {
      const votante = votantesData.find(v => v.__backendId === backendId);
      if (!votante) return;

      const confirmDiv = document.createElement('div');
      confirmDiv.className = 'fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50';
      confirmDiv.innerHTML = `
        <div class="bg-white rounded-xl p-6 max-w-md shadow-2xl">
          <h3 class="text-xl font-bold text-gray-900 mb-3">¿Confirmar eliminación?</h3>
          <p class="text-gray-700 mb-6">¿Está seguro de eliminar a <strong>${votante.nombre} ${votante.apellido}</strong>?</p>
          <div class="flex gap-3 justify-end">
            <button onclick="this.closest('.fixed').remove()" class="px-5 py-2.5 bg-gray-300 hover:bg-gray-400 rounded-lg font-semibold transition">
              Cancelar
            </button>
            <button onclick="confirmDelete('${backendId}')" class="px-5 py-2.5 bg-red-600 hover:bg-red-700 text-white rounded-lg font-semibold transition">
              Eliminar
            </button>
          </div>
        </div>
      `;
      document.body.appendChild(confirmDiv);
    }

    async function confirmDelete(backendId) {
      document.querySelector('.fixed.inset-0').remove();
      
      const votante = votantesData.find(v => v.__backendId === backendId);
      if (!votante) return;

      const result = await window.dataSdk.delete(votante);
      
      if (result.isOk) {
        showToast('Votante eliminado exitosamente', 'success');
      } else {
        showToast('Error al eliminar votante', 'error');
      }
    }

    function logout() {
      showToast('Cerrando sesión...', 'info');
    }

    function showToast(message, type = 'info') {
      const colors = {
        success: 'bg-green-600',
        error: 'bg-red-600',
        info: 'bg-blue-600'
      };
      
      const toast = document.createElement('div');
      toast.className = `fixed bottom-6 right-6 ${colors[type]} text-white px-6 py-3 rounded-lg shadow-lg z-50 font-semibold`;
      toast.textContent = message;
      document.body.appendChild(toast);
      
      setTimeout(() => toast.remove(), 3000);
    }

    window.addEventListener('load', initApp);
  </script>
 <script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9bbf259d93f4b6ac',t:'MTc2ODA3OTI3OC4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
