# VorticeDev — Agencia de Desarrollo Web Frontend

> Mi propio proyecto. No es para un cliente — es para mí.
> VorticeDev es el estudio con el que empiezo a ofrecer
> desarrollo web profesional y soluciones digitales optimizadas
> para aparecer en búsquedas de IA.

🔗 **Demo en vivo:**https://stanvanger.github.io/VorticeDev/)
📁 **GitHub:** https://github.com/Stanvanger

---

## ¿Qué es VorticeDev?

Una agencia de desarrollo web frontend especializada en dos cosas:
páginas web que Google e IAs como ChatGPT recomiendan,
y experiencias web que van más allá de una plantilla.

Este proyecto es mi carta de presentación técnica real —
no un ejercicio, sino la web con la que voy a conseguir clientes.

---

## Problemas técnicos que resolví y por qué importan

### 1. Intro 3D con Three.js que no bloqueaba la carga
**Problema:** quería una pantalla de entrada memorable con partículas 3D,
pero la primera versión bloqueaba la carga de la página entera.
El `renderer.render()` de Three.js en el hilo principal congelaba el resto
del JavaScript hasta que terminaba de inicializarse.

**Solución:** encapsulé todo el código de Three.js en una IIFE
(`(function(){ ... })()`) para aislarlo del scope global,
y cargué el script de Three.js justo antes del cierre del `</body>`
en lugar de en el `<head>`. Así el HTML y el CSS principales
cargan primero y Three.js se inicializa después.

**Impacto:** la página es usable mientras el intro carga.
El usuario ve contenido inmediatamente aunque Three.js tarde un poco más.

```javascript
// IIFE — el código de Three.js no contamina el scope global
(function() {
  var scene = new THREE.Scene();
  var camera = new THREE.PerspectiveCamera(70, W/H, 0.1, 1000);
  // ...
  // Liberar recursos cuando el intro termina
  function closeIntro() {
    cancelAnimationFrame(animId);
    renderer.dispose(); // importante para no tener memory leaks
  }
})();
```

---

### 2. SEO optimizado para aparecer en respuestas de IA
**Problema:** los buscadores tradicionales y las IAs (ChatGPT, Perplexity,
Google SGE) usan fuentes diferentes para recomendar negocios.
Una web puede estar bien posicionada en Google y ser invisible para las IAs.

**Solución:** implementé Schema.org con el tipo `Person` para el founder
y `Service` para cada línea de negocio, con descripciones redactadas
específicamente para que una IA pueda extraer y citar información concreta.
Añadí también meta tags específicos para LLMs (`ai-content-purpose`,
`business-type`) que algunos crawlers de IA ya leen.

**Impacto:** cuando alguien le pregunta a una IA "¿quién hace webs
con Three.js en España?", VorticeDev tiene más posibilidades de aparecer
porque los datos están estructurados de forma que una IA puede procesarlos.

```json
{
  "@type": "Person",
  "@id": "https://vorticedev.com/#founder",
  "name": "Stanvanger",
  "jobTitle": "Lead Developer & Founder",
  "url": "https://github.com/Stanvanger"
}
```

---

### 3. Sistema de planes y módulos dinámico sin framework
**Problema:** los planes de desarrollo web tienen precios, descripciones
y características que cambian. Escribir el HTML de cada plan a mano
lo hace difícil de mantener.

**Solución:** los datos de los planes (`var SVCS`) y los módulos extras
(`var EXTRAS`) están en arrays JavaScript. El HTML se genera dinámicamente
con funciones de renderizado. Para actualizar un precio o añadir un plan,
se toca solo el array de datos.

**Impacto:** mantenibilidad real sin depender de un CMS ni de React.
El mismo patrón que usaría con un framework, pero con JavaScript puro.

---

### 4. Carrito de pedido integrado en agencia de servicios
**Problema:** una agencia de servicios normalmente no tiene carrito —
eso es para tiendas. Pero en este caso el cliente puede seleccionar
varios servicios (plan web + módulo SEO + módulo analítica) y quería
que pudiera ver el resumen antes de contactar.

**Solución:** adapté el sistema de carrito para servicios en lugar de
productos físicos. El carrito muestra los servicios seleccionados
con sus precios y genera un mensaje de WhatsApp estructurado
con el presupuesto detallado.

**Impacto:** el cliente llega al WhatsApp con su selección ya hecha,
lo que reduce el tiempo de respuesta y filtra mejor las consultas serias.

---

### 5. Google Calendar para reuniones de presupuesto
**Problema:** el proceso de conseguir un cliente normalmente es:
contacto → llamada → presupuesto → decisión. Cada paso añade fricción
y tiempo muerto.

**Solución:** integré Google Calendar directamente en la sección
de contacto. El visitante puede reservar una reunión de 30 minutos
sin hablar con nadie primero. La reunión ya tiene el contexto
de lo que quiere (viene del carrito).

**Impacto:** el proceso se reduce a: seleccionar servicios →
reservar reunión → reunión con propuesta preparada.

---

## Stack técnico

| Tecnología | Uso |
|---|---|
| HTML5 semántico | Estructura con landmarks ARIA, roles y live regions |
| CSS3 vanilla | Variables, Grid, Flexbox, animaciones, responsive |
| JavaScript ES6 | Catálogo dinámico, carrito, módulos, formularios |
| Three.js | Intro 3D con partículas (encapsulado en IIFE) |
| AOS | Animaciones al hacer scroll |
| Schema.org | SEO para persona + servicio + optimización IA |
| Google Calendar | Sistema de reuniones integrado |
| Poppins + Barlow | Tipografía |
| GitHub Pages | Despliegue estático |

---

## Estructura del proyecto

```
vorticedev/
├── index.html         ← HTML, CSS y JS autocontenido
└── imagenes/
    ├── logo vortice.png
    └── favicon.png
```

---

## Lo que aprendí construyendo esto

- Que `renderer.dispose()` de Three.js no es opcional —
  sin él hay memory leaks que degradan el rendimiento
  mientras el usuario navega la página
- Que el SEO para IAs es diferente al SEO para Google —
  las IAs procesan Schema.org, no solo keywords
- Que un carrito tiene sentido en servicios, no solo en productos,
  si ayuda al cliente a articular lo que quiere antes de contactar
- Que construir tu propia web de agencia es más difícil que construir
  la de un cliente — tienes que ser juez y parte al mismo tiempo

---

## Proyectos de clientes

| Proyecto | Descripción | Demo |
|---|---|---|
| Gasomotores | Taller mecánico con calculadora de mantenimiento | [Ver](https://stanvanger.github.io/gasomotores/) |
| Zaira Masala | Restaurante indio con menú dinámico desde JSON | [Ver](https://stanvanger.github.io/demo-zaira-masala/) |
| PawStudio | Landing peluquería mascotas con reservas online | [Ver](https://stanvanger.github.io/pawstudioo/) |
| Digital Home | Imprenta Madrid con slider antes/después y carrito | [Ver](#) |

---

## Sobre mí

Soy **Carolina Quintero**, diseñadora publicitaria reconvertida en desarrolladora frontend.
VorticeDev es el estudio con el que transformo ese cambio en algo real.

📧 kinterocarolina0@gmail.com
📱 +34 655 607 610
🔗 github.com/Stanvanger
