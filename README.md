Aplicación EcoRide para Android 🛴
Proyecto Final Android — Jetpack Compose + MVVM

Cómo importar en Android Studio
Abre Android Studio → "Abrir" → selecciona la carpetaEcoRideApp/
Espera a que Gradle sincronice (puede tardar 1-2 minutos la primera vez)
Conecta un emulador o dispositivo físico
Configurar la URL de la API
El archivo RetrofitInstance.ktusa http://10.0.2.2:5000/que equivale al localhost:5000ordenador cuando se ejecuta en el emulador de Android .

Emulador → no toques nada, ya funciona
Dispositivo físico → cambia 10.0.2.2por la IP local de tu ordenador (la que aparece al arrancar el servidor Flask, ej: http://192.168.1.X:5000/)
Arrancar el servidor Python antes de usar la aplicación
cd server
python application.py
El servidor debe estar corriendo en el puerto 5000.

Estructura del proyecto
com.ecoride.app/
├── data/
│   ├── api/
│   │   ├── ApiService.kt          ← Interfaz Retrofit con todos los endpoints
│   │   ├── RetrofitInstance.kt    ← Singleton OkHttp + Retrofit + AuthInterceptor
│   │   └── models/
│   │       ├── AuthModels.kt      ← DTOs de Login/Register
│   │       ├── VehicleModels.kt   ← DTO de patinete
│   │       └── RentalModels.kt    ← DTOs de alquiler
│   ├── local/
│   │   ├── VehicleLocal.kt        ← @Entity VehicleEntity + @Dao VehicleDao
│   │   └── AppDatabase.kt         ← @Database Room (caché local)
│   ├── datastore/
│   │   └── TokenDataStore.kt      ← JWT token persistido en DataStore
│   └── repository/
│       ├── AuthRepository.kt      ← Login, register, logout
│       ├── VehicleRepository.kt   ← Red + caché Room (offline-first)
│       └── RentalRepository.kt    ← Iniciar/finalizar alquiler, historial
├── navigation/
│   ├── Routes.kt                  ← Rutas selladas
│   └── NavGraph.kt                ← NavHost con todas las pantallas
├── ui/
│   ├── theme/Theme.kt             ← Material3 verde EcoRide
│   ├── components/Components.kt   ← EcoRideTextField, EcoRideButton, StatusChip…
│   ├── login/                     ← LoginScreen + LoginViewModel
│   ├── register/                  ← RegisterScreen + RegisterViewModel
│   ├── vehicles/                  ← VehicleListScreen + ViewModel (LazyColumn)
│   ├── detail/                    ← VehicleDetailScreen + ViewModel
│   └── history/                   ← RentalHistoryScreen + ViewModel
└── MainActivity.kt                ← Punto de entrada, DI manual
Flujo de la app
Login ──────► Lista de patinetes ──────► Detalle + Iniciar alquiler
  │                 │
  │            Botón historial
  │                 │
  └── Registro  Historial de alquileres
Criterios del proyecto cubiertos
Criterio	Implementación
ViewModel	Uno por pantalla, sobrevive rotación
Izado estatal	StateFlowen ViewModel, collectAsStateWithLifecycleen UI
Ciclo de vida	viewModelScopeen todas las corrutinas
Estructura de paquetes	data/(api, local, almacén de datos, repositorio) + ui/por característica
Adaptación	ApiServicecon GET/POST/PUT, manejo de errores HTTP
Corrutinas	Dispatchers.IOen repositorios, hilo principal libre
Habitación	Caché de patinetes, la aplicación funciona sin red
Almacén de datos	El token JWT persiste entre sesiones.
Columna perezosa	Lista de patinetes e historial
Componibles reutilizables	EcoRideButton, StatusChip, InfoRow,EcoRideTextField
Navegación	NavGraph con paso de vehicleIdcomo argumento
Indicadores de carga	CircularProgressIndicator+LinearProgressIndicator
Credenciales de prueba
Admin: admin@ecoride.com / Admin1234!
O regístrate como usuario nuevo desde la aplicación.
