# 📱 Plan de Implementación: App "Venta de Teléfonos" (Flutter + Firebase)

> ⚠️ **Nota sobre el entorno**: "Antigravity" no corresponde a un IDE conocido en el ecosistema Flutter. Se asumirá **VS Code** como entorno principal. Si te referías a un editor con asistente IA (Cursor, Windsurf, Zed, etc.), el flujo es idéntico; solo cambia la extensión de sugerencias.

---

## 🛠️ Herramientas y Recursos Requeridos

| Categoría | Herramientas Recomendadas |
|-----------|---------------------------|
| **IDE / Editor** | VS Code + extensiones: `Flutter`, `Dart`, `Firebase`, `Error Lens`, `Pubspec Assist` |
| **UI/UX Diseño** | Figma (recomendado) o Adobe XD. Plugins útiles: `Flutter Flow`, `Material 3 Design Kit` |
| **Backend / BaaS** | Firebase Console (Auth, Firestore, Storage, Crashlytics, Analytics) |
| **Control de Versiones** | Git + GitHub/GitLab |
| **Gestión de Estado** | `provider` (versión estable recomendada) |
| **Otras Utilidades** | Postman/Insomnia (para pruebas de API si se requiere), Figma to Flutter export, Lottie (animaciones), FlutterGen (assets) |

---

## 📋 Fases de Desarrollo (Paso a Paso)

### 🔹 Fase 0: Definición y Arquitectura
1. **Delimitar alcance funcional**: Catálogo de teléfonos, filtrado por marca/precio, detalle de producto, carrito de compras, checkout básico, historial de pedidos, perfil de usuario, panel administrador (opcional v1).
2. **Modelar flujos de usuario**: Registro/Login → Explorar catálogo → Ver detalle → Agregar al carrito → Confirmar compra → Ver historial.
3. **Definir arquitectura limpia por capas**: `data` (repositorios, modelos), `domain` (casos de uso, entidades), `presentation` (pantallas, widgets, providers).
4. **Estructurar carpetas del proyecto**: Crear directorios base siguiendo convención modular (`lib/core`, `lib/features/auth`, `lib/features/catalog`, etc.).

### 🔹 Fase 1: Diseño UI/UX
1. **Crear wireframes de baja fidelidad** en Figma para todas las pantallas clave.
2. **Diseñar mockups de alta fidelidad** aplicando guías de Material 3 o Cupertino (según prioridad multiplataforma).
3. **Definir Design System**: Paleta de colores, tipografía, espaciado, estados de botones, loaders, manejo de errores visuales.
4. **Preparar assets**: Iconos, imágenes de ejemplo, splash screen, logos. Exportar en formatos optimizados (`png`, `svg`, `lottie`).
5. **Validar usabilidad**: Simular flujos en Figma con prototipos interactivos. Ajustar antes de código.

### 🔹 Fase 2: Configuración del Proyecto y Dependencias
1. **Instalar Flutter SDK y Dart** en versión estable. Verificar con `flutter doctor`.
2. **Crear proyecto Flutter** en VS Code con nombre y organización definidos.
3. **Configurar `pubspec.yaml`** con las siguientes categorías de dependencias (sin versiones fijas, usar `any` o `^` según semver):
   - Core: `flutter`, `provider`
   - Firebase: `firebase_core`, `firebase_auth`, `cloud_firestore`, `firebase_storage` (opcional para imágenes de productos)
   - UI/UX: `cached_network_image`, `shimmer`, `google_fonts`, `lottie`
   - Navegación/Rutas: `go_router` o `auto_route`
   - Utilidades: `intl` (formato moneda/fechas), `formz` o `form_field_validator`, `uuid`, `shared_preferences` (cache local), `http` (si se requiere integración externa)
   - Dev: `flutter_lints`, `mockito`, `build_runner`
4. **Ejecutar `flutter pub get`** y verificar compatibilidad de versiones.
5. **Configurar linter y formateo** (`analysis_options.yaml` con reglas estrictas).

### 🔹 Fase 3: Integración de Firebase
1. **Crear proyecto en Firebase Console** y habilitar servicios: Authentication, Firestore Database.
2. **Registrar aplicaciones** (Android, iOS, Web) y descargar archivos de configuración (`google-services.json`, `GoogleService-Info.plist`).
3. **Colocar configuraciones** en las rutas nativas correspondientes (`android/app/`, `ios/Runner/`).
4. **Configurar Firestore**: Crear colecciones iniciales (`users`, `phones`, `orders`, `cart`). Definir reglas de seguridad en modo `testing` y planificar reglas de producción.
5. **Inicializar Firebase en Flutter** mediante `firebase_core.initializeApp()` antes de `runApp()`.
6. **Validar conexión** ejecutando la app en modo debug y verificando logs de inicialización.

### 🔹 Fase 4: Arquitectura de Estado (Provider) y Navegación
1. **Crear Providers principales**:
   - `AuthProvider`: manejo de sesión, token, estado de autenticación.
   - `CatalogProvider`: carga de teléfonos, filtros, búsqueda, paginación.
   - `CartProvider`: agregar, eliminar, actualizar cantidades, persistencia local/Firestore.
   - `UserProvider`: perfil, direcciones, historial.
2. **Implementar inyección de dependencias** con `Provider` o `MultiProvider` en el árbol de widgets raíz.
3. **Configurar enrutamiento protegido**: rutas públicas (login, registro, catálogo) vs rutas privadas (carrito, perfil, checkout).
4. **Definir gestión de errores globales**: `FlutterError.onError`, manejo de excepciones en repositorios, UI de fallback.

### 🔹 Fase 5: Desarrollo de Módulos Core
1. **Autenticación (Email/Password)**:
   - Formularios con validación en tiempo real.
   - Registro, inicio de sesión, cierre de sesión, recuperación de contraseña.
   - Manejo de estados de carga, errores y redirección.
2. **Catálogo de Teléfonos**:
   - Modelo `Phone` (id, marca, modelo, precio, specs, stock, imagen, categoría).
   - Repositorio `FirestorePhoneRepository` con métodos `getAll`, `getById`, `search`, `filter`.
   - UI: grid/listado, filtros desplegables, búsqueda, indicador de stock, imagen optimizada.
3. **Detalle de Producto**:
   - Galería de imágenes, especificaciones técnicas, selector de variante (color/almacenamiento), botón de agregar al carrito.
4. **Carrito y Checkout**:
   - Persistencia del carrito (local + sincronización con Firestore si usuario logueado).
   - Cálculo de totales, impuestos simulados, resumen de orden.
   - Flujo de confirmación (sin pasarela real en v1, pero preparado para integración futura).
5. **Perfil e Historial**:
   - Edición de datos básicos, dirección de envío.
   - Listado de pedidos con estado (`pending`, `shipped`, `delivered`).
6. **Panel Admin (Opcional v1)**:
   - CRUD de productos, gestión de stock, visualización de pedidos, cambio de estado.

### 🔹 Fase 6: Pruebas y Optimización
1. **Unit Tests**: lógica de repositorios, validadores, cálculo de carrito, parsing de modelos.
2. **Widget Tests**: pantallas de login, ítem de catálogo, carrito vacío/lleno.
3. **Integration Tests**: flujo completo registro → compra → ver historial.
4. **Pruebas de resiliencia**: modo offline, reconexión, permisos denegados, tiempo de espera agotado.
5. **Optimización**:
   - Compresión de imágenes, lazy loading, paginación en Firestore (`limit` + `startAfter`).
   - Minimizar rebuilds con `const`, `Provider.of(context, listen: false)`.
   - Revisión de tamaño de bundle y eliminación de dependencias no usadas.
6. **Auditoría de seguridad**: validar que ninguna clave sensible esté hardcodeada, reglas Firestore alineadas con roles, sanitización de inputs.

### 🔹 Fase 7: Despliegue y Post-Lanzamiento
1. **Compilación por plataforma**:
   - Android: `flutter build appbundle`
   - iOS: `flutter build ipa` (requiere macOS y certificados)
   - Web: `flutter build web`
2. **Publicación**:
   - Google Play Console (internas → producción)
   - App Store Connect (TestFlight → revisión)
   - Hosting estático para versión web (Firebase Hosting, Vercel, GitHub Pages)
3. **Monitoreo**:
   - Habilitar Crashlytics y Analytics en Firebase.
   - Configurar alertas de errores críticos y métricas de retención.
4. **Plan de mantenimiento**:
   - Backups automáticos de Firestore.
   - Estrategia de versiones semánticas y changelog.
   - Roadmap v2: pasarela de pagos, notificaciones push, reseñas, sincronización en tiempo real.

---

## ✅ Entregables por Fase

| Fase | Entregable |
|------|------------|
| 0 | Documento de alcance, diagrama de arquitectura, estructura de carpetas |
| 1 | Prototipo Figma interactivo, guía de estilo, assets exportados |
| 2 | Proyecto Flutter inicializado, `pubspec.yaml` validado, linter configurado |
| 3 | Proyecto Firebase activo, apps registradas, Firestore inicializado, conexión verificada |
| 4 | Providers implementados, ruteo protegido, gestión de errores global |
| 5 | Módulos funcionales (auth, catálogo, carrito, perfil, admin opcional) |
| 6 | Suite de pruebas, reporte de optimización, auditoría de seguridad |
| 7 | Builds compilados, apps publicadas, monitoreo activo, documentación v1 |

---

## 📌 Notas Finales para el Desarrollo
- **No avanzar a código** hasta tener el diseño UI/UX validado y la estructura de Firestore definida.
- **Firestore**: usar índices compuestos antes de implementar filtros complejos.
- **Provider**: mantener cada provider enfocado en una sola responsabilidad; evitar `ChangeNotifier` gigantes.
- **Seguridad**: nunca confiar solo en validación frontend; replicar reglas en Firestore Security Rules.
- **Versionado**: usar tags de Git por fase completada para facilitar rollback.

Cuando este plan esté aprobado o ajustado a tu visión, puedo proceder a generar la estructura de carpetas, el `pubspec.yaml` detallado o el flujo de implementación módulo por módulo con código listo para VS Code. ¿Deseas ajustar alguna fase o priorizar un módulo específico?
