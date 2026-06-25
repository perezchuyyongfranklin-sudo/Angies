# Radar Angie.gt — de catálogo estático a descubridor automático

Resumen ejecutivo de la estrategia para que Angie deje de depender de curación manual y un "radar" descubra oportunidades de **todos los oficios** (no solo tech) en el internet guatemalteco.

---

## 1. El cambio de fondo: de "esclavo" a "explorador"

- **Hoy:** un listado estático que vos alimentás a mano.
- **Meta:** un sistema que **rastrea → limpia con IA → te deja una cola de aprobación**. Vos solo tocás *Aprobar* y se publica.

Flujo diario del bot:

```
[Rastreo por fuente] → [Extracción + OCR] → [Limpieza/clasificación con IA] →
[Deduplicado + verificación de enlace] → [Cola en panel admin] → [Aprobar/Descartar] → [Publicar]
```

## 2. El sesgo que había que romper (palabras clave)

Términos como **"bootcamp", "EdTech", "skills de alta demanda"** están **monopolizados por el sector tech** en internet → por eso el algoritmo se encasillaba en programación.

**Cura:** vocabulario **local y tradicional** por ruta → ver `angie_radar_diccionarios.json`. Ejemplos:
- Uñas → "curso de uñas acrílicas", "técnico en belleza", "se busca manicurista damos capacitación".
- Excel → "curso de computación", "ofimática", "secretariado computarizado".
- Oficios → "escuela de oficios", "capacitación técnica", "talleres libres", "competencias laborales".

## 3. El radar: 8 fuentes para barrer (semáforo legal)

| Fuente | Cómo | Estado legal/feasibilidad |
|---|---|---|
| **Dominios .gt** (Google Custom Search) | Barrer todo .gt nuevo/actualizado, filtrar por "cursos/convocatorias/pasantías" | ✅ Legítimo. La mejor base. |
| **Google Maps API** | Buscar "academia de belleza / escuela técnica / centro de capacitación" y revisar su web | ✅ Legítimo (con API key, tiene costo). |
| **Gobierno y munis** | SEGEPLAN, MINEDUC/DIGEEX, INTECAP, MINECO, Convivir/TuMuni, DMM departamentales | ✅ Legítimo (datos públicos). |
| **Cooperación / ONG** | USAID, Fund. Juan Bautista Gutiérrez, AGEXPORT, Glasswing | ✅ Legítimo. |
| **Prensa local** | Prensa Libre (Comunitario/Departamental), Soy502, La Hora, medios deptales | ✅ Legítimo vía RSS/secciones públicas. |
| **Empleo formal** | Tecoloco, Computrabajo, LinkedIn (junior/atención al cliente) | ⚠️ Respetar ToS de cada portal; preferir feeds/APIs. |
| **Clasificados** | Encuentra24 / OLX (Servicios, Cursos, Empleos) | ⚠️ Revisar ToS; lectura sí, scraping masivo puede no. |
| **Redes (FB/IG)** | Grupos y páginas locales por municipio + OCR de afiches | 🚫 **Scrapear viola los Términos de Meta y toca datos personales.** Usar API oficial, alianzas o curación manual. |

> **La joya:** los clasificados/redes con el patrón **"se busca [puesto], damos capacitación"** = empleos que **enseñan la habilidad mientras te pagan**. Oro para tu misión.

## 4. La IA del bot (lo que lo hace "explorador")

1. **OCR** para leer afiches/imágenes (cuando la fuente lo permite legalmente).
2. **Clasificación semántica:** a qué ruta pertenece, si es curso/empleo/beca, costo, fecha.
3. **Deduplicado** y verificación de que el enlace/teléfono siga vivo.
4. **Score de confianza** para priorizar tu cola de aprobación.

## 5. Lo que se necesita para construirlo (honesto)

Esto **no vive en el HTML actual** — es un backend aparte:
- Servidor + tareas programadas (cron) en Python.
- APIs: Google Custom Search, Google Maps, (opcional) Meta oficial.
- Modelo de IA (visión + texto) para OCR y clasificación.
- Base de datos + un **panel de administración** con la cola de aprobación.
- Costos: las APIs de Google/Maps y la IA se cobran por uso.

**Primer paso barato y útil ya:** usar estos diccionarios para **curación manual asistida** (buscás con los términos locales y agregás a mano) mientras se construye el motor. Multiplica la diversidad del catálogo sin backend.

## 6. Monetización del acompañamiento (referentes de Colombia)

| Modelo | Cómo | Para quién encaja |
|---|---|---|
| **ISA (ingreso compartido)** | El usuario paga solo al conseguir empleo sobre un salario meta | Rutas con empleo claro (inglés/call center, ventas) |
| **Membresía premium** | Ruta gratis; se cobra mensual por mentor humano y simulacros de entrevista | Usuarios que quieren acompañamiento |
| **B2B (empresas)** | Gratis para el usuario; se cobra comisión de reclutamiento a empresas por talento pre-filtrado | Tu insignia/bitácora ya entrega ese "talento verificado" |

> El **B2B** es el más alineado con lo que ya construimos: la **insignia de horas verificadas + consistencia + evidencia** es justo el "sello de garantía" que un reclutador pagaría.

---

### Próximo paso recomendado
1. Validar/expandir `angie_radar_diccionarios.json` (agregar municipios y sinónimos que conozcas).
2. Elegir el método de arranque: **curación manual asistida** (ya) vs. **piloto técnico** con Google Custom Search + Maps (cuando haya backend).
3. Definir el modelo de monetización a probar primero (sugerencia: B2B sobre la insignia).
