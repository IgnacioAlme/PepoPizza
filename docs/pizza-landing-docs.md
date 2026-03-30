# 🍕 Documentación del Proyecto: Landing Page & Ecommerce para Pizzería

---

## Índice

1. [Descripción General del Proyecto](#1-descripción-general-del-proyecto)
2. [Objetivos](#2-objetivos)
3. [Alcance Funcional](#3-alcance-funcional)
4. [Arquitectura Técnica](#4-arquitectura-técnica)
5. [Diseño y Experiencia de Usuario (UX/UI)](#5-diseño-y-experiencia-de-usuario-uxui)
6. [Flujo de Pedido por WhatsApp](#6-flujo-de-pedido-por-whatsapp)
7. [Estructura de Páginas y Secciones](#7-estructura-de-páginas-y-secciones)
8. [Requisitos No Funcionales](#8-requisitos-no-funcionales)
9. [Fases del Proyecto](#9-fases-del-proyecto)
10. [Presupuesto](#10-presupuesto)
11. [Consideraciones Adicionales](#11-consideraciones-adicionales)

---

## 1. Descripción General del Proyecto

Se desarrollará una **landing page con funcionalidad de ecommerce liviano** para una pizzería. El sitio tiene como propósito principal presentar la identidad del negocio, mostrar el menú completo con precios, y permitir que los clientes armen su pedido de manera intuitiva. Una vez confirmado el pedido, el sistema redirigirá automáticamente al cliente a una conversación de WhatsApp con el detalle del pedido pre-cargado como mensaje de plantilla, listo para enviar al local.

No se requiere procesamiento de pagos online ni sistema de backend propio, lo que simplifica significativamente la infraestructura.

---

## 2. Objetivos

- Brindar presencia digital profesional a la pizzería.
- Facilitar el proceso de pedido sin necesidad de que el cliente llame o escriba manualmente.
- Reducir errores en la toma de pedidos al tener los ítems y precios estandarizados.
- Aumentar las conversiones mediante una experiencia de usuario fluida y atractiva.
- Minimizar los costos operativos evitando infraestructura de backend compleja.

---

## 3. Alcance Funcional

### ✅ Incluido en el proyecto

| Funcionalidad | Descripción |
|---|---|
| Página informativa | Nombre, descripción, horarios, dirección, redes sociales |
| Menú digital | Categorías, nombre, descripción, precio y foto de cada ítem |
| Carrito de compras | Agregar/quitar ítems, visualizar cantidades y total en tiempo real |
| Total dinámico | El precio total se actualiza automáticamente al modificar el carrito |
| Redirección a WhatsApp | Mensaje pre-armado con los ítems seleccionados, cantidades y total |
| Diseño responsive | Adaptado para celular, tablet y escritorio |
| Optimización SEO básica | Metaetiquetas, Open Graph para compartir en redes |

### ❌ Fuera del alcance

- Procesamiento de pagos online (tarjeta, transferencia, etc.)
- Panel de administración para editar el menú dinámicamente
- Sistema de usuarios / cuentas de cliente
- Seguimiento de pedidos en tiempo real
- Integración con sistemas de delivery de terceros

---

## 4. Arquitectura Técnica

### Stack recomendado

```
Frontend:  HTML5 + CSS3 + JavaScript (Vanilla) ó React (Next.js)
Hosting:   Vercel / Netlify (plan gratuito)
Dominio:   Registro externo (.com.ar o .com)
Imágenes:  Optimizadas y servidas estáticamente (WebP)
WhatsApp:  API de Click-to-Chat (wa.me) — sin costo, sin cuenta Business obligatoria
```

### Justificación del stack

Se opta por un sitio **estático o semi-estático** (sin base de datos ni servidor propio) para mantener costos bajos, garantizar velocidad de carga y simplificar el mantenimiento. El menú se gestiona editando un archivo de configuración (JSON o constantes JS), sin necesidad de CMS.

### Estructura de archivos (ejemplo)

```
/
├── index.html (ó pages/index.jsx en Next.js)
├── menu.json          ← datos del menú (ítems, precios, categorías)
├── assets/
│   ├── images/        ← fotos del negocio y los platos
│   └── icons/
├── styles/
│   └── main.css
└── scripts/
    ├── cart.js        ← lógica del carrito
    └── whatsapp.js    ← generación del mensaje y redirección
```

---

## 5. Diseño y Experiencia de Usuario (UX/UI)

### Paleta de colores sugerida

| Rol | Color | Uso |
|---|---|---|
| Primario | `#C0392B` (rojo tomate) | Botones, destacados |
| Secundario | `#F39C12` (naranja dorado) | Precios, badges |
| Fondo | `#1A1A1A` (negro carbón) ó `#FAFAF7` | Fondo principal |
| Texto | `#2C2C2C` / `#FFFFFF` | Tipografía |
| Acento | `#27AE60` (verde) | Botón de WhatsApp |

### Tipografía

- **Títulos:** fuente display con carácter (ej. Playfair Display, Oswald, o similar).
- **Cuerpo:** fuente legible para precios y descripciones (ej. Lato, Source Sans).

### Principios UX

- El carrito debe ser visible en todo momento (ícono flotante con contador de ítems).
- El botón de "Hacer pedido por WhatsApp" debe ser prominente y con color verde distintivo.
- Las categorías del menú deben ser navegables con scroll o tabs.
- Imágenes de los productos de alta calidad (o ilustraciones) para estimular el apetito.

---

## 6. Flujo de Pedido por WhatsApp

### Paso a paso del usuario

```
1. El usuario navega el menú
         ↓
2. Hace clic en "Agregar" en los ítems que desea
         ↓
3. El carrito flotante muestra los ítems y el total en tiempo real
         ↓
4. El usuario hace clic en "Hacer pedido por WhatsApp"
         ↓
5. Se genera un mensaje de texto con el resumen del pedido
         ↓
6. Se abre WhatsApp (app o web) con el número del local
   y el mensaje pre-cargado
         ↓
7. El cliente revisa y envía el mensaje
         ↓
8. El local recibe el pedido y confirma por WhatsApp
```

### Ejemplo de mensaje generado

```
¡Hola! Quisiera hacer el siguiente pedido:

🍕 Pizza Muzzarella x1 — $8.500
🍕 Pizza Napolitana x2 — $19.000
🥤 Coca-Cola 1.5L x1 — $3.200

💰 *Total: $30.700*

¿Pueden confirmarlo? ¡Gracias!
```

### Implementación técnica del link

```javascript
const numero = "5493624XXXXXX"; // número con código de país sin + ni espacios
const mensaje = encodeURIComponent(generarMensaje(carrito));
const url = `https://wa.me/${numero}?text=${mensaje}`;
window.open(url, "_blank");
```

---

## 7. Estructura de Páginas y Secciones

La web será de **una sola página (SPA / One Page)** con scroll por secciones:

### Sección 1 — Hero / Portada
- Logo del negocio
- Nombre y slogan
- Foto de portada de alta calidad
- Botón de llamada a la acción: "Ver Menú" → hace scroll al menú

### Sección 2 — Sobre Nosotros
- Breve descripción del negocio
- Historia o propuesta de valor
- Horarios de atención
- Dirección (con mapa embebido de Google Maps, opcional)

### Sección 3 — Menú
- Tabs o filtros por categoría (Pizzas, Empanadas, Bebidas, Postres, etc.)
- Cards de cada ítem con:
  - Foto
  - Nombre
  - Descripción breve
  - Precio
  - Botón "+ Agregar al carrito"

### Sección 4 — Carrito (flotante / sidebar)
- Lista de ítems agregados
- Cantidad por ítem (con +/-)
- Subtotal por ítem
- Total general
- Botón "Hacer pedido por WhatsApp"

### Sección 5 — Contacto / Footer
- Número de WhatsApp (con link directo)
- Redes sociales (Instagram, Facebook)
- Dirección y horarios
- Créditos del desarrollador (opcional)

---

## 8. Requisitos No Funcionales

| Requisito | Detalle |
|---|---|
| **Rendimiento** | Carga inicial < 3 segundos en conexión móvil 4G |
| **Compatibilidad** | Chrome, Firefox, Safari, Edge — versiones de los últimos 2 años |
| **Responsive** | Mobile-first, breakpoints en 768px y 1024px |
| **Accesibilidad** | Contraste adecuado, atributos `alt` en imágenes, navegación por teclado básica |
| **SEO** | Metaetiquetas básicas, Open Graph, descripción del negocio |
| **Seguridad** | HTTPS obligatorio (incluido en Vercel/Netlify gratis) |
| **Mantenibilidad** | Menú editable desde un único archivo de configuración |

---

## 9. Fases del Proyecto

### Fase 1 — Relevamiento y Diseño (Semana 1)
- Reunión de kickoff con el cliente
- Recopilación de contenido: logo, fotos, descripción, menú con precios
- Definición de paleta, tipografía y estilo visual
- Wireframes de baja fidelidad (bocetos de estructura)

### Fase 2 — Desarrollo (Semanas 2 y 3)
- Maquetado HTML/CSS base
- Implementación del menú estático con datos reales
- Desarrollo de la lógica del carrito (JavaScript)
- Integración del link de WhatsApp con mensaje dinámico
- Responsive design

### Fase 3 — Integración de Contenido y Ajustes (Semana 4)
- Carga de todas las fotos y textos reales del cliente
- Revisión y correcciones con el cliente (hasta 2 rondas de revisión)
- Pruebas en distintos dispositivos y navegadores

### Fase 4 — Lanzamiento (Semana 4 / inicio Semana 5)
- Configuración del dominio
- Deploy en Vercel o Netlify
- Prueba final end-to-end del flujo de pedido
- Entrega y capacitación básica al cliente

---

## 10. Presupuesto

> 💡 **Nota:** Los valores están expresados en **pesos argentinos (ARS)** y son orientativos a la fecha de elaboración de este documento (marzo 2026). Los precios de servicios de terceros pueden variar según cotización del dólar.

---

### 10.1 Costos de Desarrollo (Única vez)

| Ítem | Descripción | Precio (ARS) |
|---|---|---|
| Diseño UI/UX | Wireframes, paleta, tipografía, diseño visual completo | $150.000 |
| Desarrollo Frontend | Maquetado, responsive, animaciones, lógica del carrito | $280.000 |
| Integración WhatsApp | Lógica de generación de mensaje y redirección | $60.000 |
| Carga de contenido | Carga del menú, textos, imágenes y configuración final | $50.000 |
| Testing y QA | Pruebas en dispositivos, navegadores y flujo de pedido | $40.000 |
| Deploy y configuración | Configuración de hosting, dominio y HTTPS | $30.000 |
| **Subtotal Desarrollo** | | **$610.000** |

---

### 10.2 Costos Recurrentes (Anuales / Mensuales)

| Ítem | Descripción | Precio (ARS) |
|---|---|---|
| Dominio `.com.ar` | Registro anual en NIC.ar | $5.500 / año |
| Dominio `.com` (alternativa) | Registro en proveedor como Namecheap (~$15 USD) | ~$15.000 / año |
| Hosting | Vercel / Netlify plan Free (recomendado para este proyecto) | **$0** |
| Mantenimiento mensual | Actualización de precios del menú, soporte técnico | $25.000 / mes |
| Fotografía profesional | Sesión de fotos de platos (opcional, única vez) | $80.000 |

---

### 10.3 Resumen Presupuestario

| Concepto | Monto (ARS) |
|---|---|
| Desarrollo total (única vez) | $610.000 |
| Dominio (primer año, `.com.ar`) | $5.500 |
| Hosting (anual) | $0 |
| Fotografía (opcional) | $80.000 |
| **Total inversión inicial** | **$615.500** (sin foto) / **$695.500** (con foto) |
| Mantenimiento mensual estimado | $25.000 / mes |

---

### 10.4 Opciones de Presupuesto

Dependiendo del alcance y urgencia, se ofrecen tres variantes:

| Plan | Incluye | Precio (ARS) |
|---|---|---|
| 🥉 **Básico** | Diseño simple, menú sin fotos, carrito + WhatsApp, responsive | $380.000 |
| 🥈 **Estándar** | Todo lo anterior + diseño personalizado, fotos de stock, SEO básico | $610.000 |
| 🥇 **Premium** | Todo lo anterior + sesión fotográfica, animaciones avanzadas, 2 meses de mantenimiento incluido | $780.000 |

---

## 11. Consideraciones Adicionales

### Contenido a proveer por el cliente

Para iniciar el desarrollo, el cliente deberá entregar:

- [ ] Logo en alta resolución (PNG o SVG, fondo transparente)
- [ ] Listado completo del menú con precios actualizados
- [ ] Fotos de los platos (si las tiene) o autorización para usar fotos de stock
- [ ] Descripción del negocio y slogan
- [ ] Horarios de atención
- [ ] Dirección física
- [ ] Número de WhatsApp que recibirá los pedidos
- [ ] Cuentas de redes sociales (Instagram, Facebook, etc.)

### Edición del menú post-entrega

El menú se almacena en un archivo `menu.json` de fácil edición. El cliente podrá solicitar actualizaciones de precios o ítems como parte del mantenimiento mensual, o se le puede capacitar para hacerlo de forma autónoma a través de GitHub (sin costo adicional en ese caso).

### Escalabilidad futura

Si en el futuro el negocio requiere funcionalidades adicionales, el proyecto puede evolucionar hacia:

- **Panel de administración** para gestionar el menú sin tocar código (~$200.000 adicionales)
- **Integración con MercadoPago** para pagos online (~$150.000 adicionales)
- **Sistema de reservas** de mesa online (~$120.000 adicionales)

---

*Documento elaborado en marzo de 2026. Los precios son orientativos y sujetos a actualización.*
