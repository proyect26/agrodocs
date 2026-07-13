# AgroDocs — Sistema de Gestión y Etiquetado Agrícola

**AgroDocs** (anteriormente conocido como FLORELI) es una plataforma de software premium diseñada para la gestión logística, etiquetado comercial y facturación de exportadoras agrícolas, optimizado específicamente para la exportación de flores en *Angel's Blooms*.

El sistema combina herramientas avanzadas de generación de documentos físicos con un panel interactivo de administración administrativa, presentado bajo una estética futurista **Liquid Glass (estilo visionOS)**.

---

## 🌟 Características Principales y Módulos

El sistema está estructurado en 5 módulos integrados en una interfaz de una sola página (SPA):

### 1. Editor (Generador de Etiquetas e Invoices)
*   **Formulario Inteligente**: Permite el ingreso de información de embarque (AWB, HAWB, DAE, Marcación, Agencia, Cliente, Variedad de Flores, Longitud, etc.).
*   **Campos Inteligentes (`SmartField`)**: Los campos de texto memorizan y autocompletan datos ingresados anteriormente. Permiten guardar valores frecuentes en listas desplegables individuales y fijar valores predeterminados.
*   **Previsualización en Tiempo Real**: Muestra el diseño exacto del documento (sea etiqueta de caja o factura A4 comercial) a medida que se escribe.
*   **Impresión Optimizada**: Integrado con reglas CSS `@media print` de alta precisión. Al imprimir físicamente en papel o PDF, el fondo vibrante oscuro de la pantalla se oculta automáticamente para usar fondos blancos sólidos, garantizando contraste máximo y **cero desperdicio de tinta**.

### 2. Historial de Documentos
*   **Organización Cronológica**: Todos los documentos generados e impresos se guardan localmente y se agrupan en acordeones colapsables por fecha y tipo (Etiquetas, Invoices, Hojas Resumen, Hojas de Ruta).
*   **Envío de Correo Electrónico**: Permite enviar cualquier documento PDF del historial por correo electrónico directamente al cliente o agencia utilizando un servidor SMTP configurado.

### 3. Control de Proveedores
*   **Categorización**: Permite registrar y clasificar proveedores en categorías (Flores, Cartones, Capuchones, etc.).
*   **Registro de Entregas**: Registra entradas detalladas de insumos por fecha, bonches de flores, cantidades y costos asociados.

### 4. Registro de Pagos
*   **Flujo Financiero**: Permite llevar un registro de pagos realizados o pendientes a los proveedores, seleccionando el método de pago (Transferencia, Efectivo, etc.) y guardando notas de auditoría.

### 5. Configuración (Settings)
*   **Respaldos de Base de Datos**: Permite importar y exportar toda la base de datos del sistema en un solo archivo JSON, facilitando la migración de datos entre computadoras.
*   **Configuración SMTP**: Módulo de credenciales de correo electrónico para el envío automático de PDFs.

---

## 🎨 El Sistema de Diseño "Liquid Glass" (estilo visionOS)

La interfaz en pantalla está diseñada bajo principios de refracción física y profundidad esmerilada, imitando el sistema operativo visionOS de Apple. Se compone de:

### A. Fondo Dinámico con Refracción
1.  **Fondo de Pantalla Personalizado**: Utiliza una imagen base de alta resolución (`imagesfondo/fondopag.jpeg`) estirada para cubrir toda la pantalla.
2.  **Globos Líquidos de Luz (`.liquid-bg-blobs`)**: Tres esferas de degradado radial flotantes (celeste/cyan, rosa/magenta y morado) se mueven lentamente en segundo plano mediante animaciones CSS optimizadas por GPU (`@keyframes`).

### B. Tarjetas de Vidrio Líquido (`.card`)
Para lograr la ilusión de vidrio real sin empañar la lectura del texto, las tarjetas y la barra lateral usan una técnica de tres capas apiladas mediante un contexto aislado (`isolation: isolate`):
*   **Capa Base (Filtro de Distorsión SVG)**: El pseudo-elemento `::after` contiene un filtro SVG personalizado (`#glass-distortion`) que genera ruido fractal (`feTurbulence`) y deforma los píxeles del fondo (`feDisplacementMap`), simulando la refracción del cristal óptico junto con un desenfoque Gaussiano de `20px` (`backdrop-filter`).
*   **Capa Intermedia (Tinte Blanco Esmerilado)**: El pseudo-elemento `::before` aplica un fondo translúcido muy ligero (`rgba(255,255,255,0.05)`) y un borde interno brillante que imita los reflejos del bisel del vidrio.
*   **Capa Superior (Contenido Nítido)**: El texto, botones y campos de entrada se renderizan sobre estas capas, manteniéndose **100% nítidos, enfocados y con alto contraste** para una lectura sin fatiga visual.

---

## 🛠️ Arquitectura y Tecnologías Utilizadas

*   **Frontend (UI)**:
    *   **React (v18)**: Biblioteca principal para la gestión del estado reactivo de los formularios e historial.
    *   **Vite**: Compilador y empaquetador ultrarrápido para el desarrollo local.
    *   **Vanilla CSS3 / Custom Properties**: Sistema de variables CSS para colores adaptativos y tokens de vidrio.
    *   **Lucide React**: Biblioteca de iconos vectoriales consistentes y minimalistas.
*   **Entorno de Escritorio**:
    *   **Electron**: Empaqueta el frontend web en una aplicación nativa instalable para Windows (`.exe`), permitiendo la ejecución de código del lado del sistema operativo.
    *   **SQLite**: Motor de base de datos relacional ultraligero que almacena de forma persistente e in situ los datos al ejecutarse en modo de escritorio.
    *   **Nodemailer / SMTP Transport**: Módulo interno de Electron encargado de conectar de forma segura con servidores SMTP (Gmail, Outlook, etc.) para despachar los correos.
    *   **HTML5 LocalStorage**: Sistema de almacenamiento de respaldo en caso de ejecutar la plataforma únicamente desde un navegador web tradicional.

---

## 🚀 Guía de Desarrollo e Instalación

### Requisitos Previos
*   Tener instalado [Node.js](https://nodejs.org/) (versión 16 o superior).

### Modo Web (Desarrollo y Pruebas)
Instala las dependencias y corre el servidor de desarrollo local:
```bash
npm install
npm run dev
```
La aplicación estará disponible en `http://localhost:5173`.

### Modo Escritorio (Desarrollo con Electron)
Corre AgroDocs dentro de la ventana de Electron nativa:
```bash
npm run desktop:dev
```

### Compilar Instalador para Windows (`.exe`)
Genera el paquete instalador distribuible en la carpeta `release/`:
```bash
npm run desktop:build
```
