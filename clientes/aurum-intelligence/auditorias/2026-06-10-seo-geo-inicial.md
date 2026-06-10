# Auditoría SEO + GEO — Aurum Intelligence

**Fecha:** 10 de junio de 2026  
**URL auditada:** https://www.aurumintelligence.net/  
**Tipo:** Auditoría inicial (técnica, on-page, contenido, GEO)  
**Auditor:** Documentación interna

---

## Resumen ejecutivo

| Área | Puntuación estimada | Estado |
|------|---------------------|--------|
| Seguridad | 🔴 Crítico | Comprometido |
| Indexación | 🔴 Crítico | Sin presencia en Google |
| SEO técnico | 🟠 Medio-bajo | Errores corregibles |
| On-page | 🟡 Medio | Mejorable |
| Contenido / marca | 🟠 Medio-bajo | Restos de plantilla |
| GEO (LLMs) | 🟡 Medio | llms.txt presente pero contaminado |

**Conclusión:** El sitio tiene buena propuesta de valor y contenido de blog relevante para su nicho (AI + packaging), pero hoy **no cumple su función como activo de marketing**. Hay que tratar la seguridad como urgencia #1, limpiar el contenido heredado del theme *Tecnologia*, reparar el hreflang ES y corregir la configuración de AIOSEO/schema antes de esperar tráfico orgánico o citaciones en motores generativos.

---

## 1. Hallazgos críticos (acción inmediata)

### 1.1 🔴 Sitio comprometido — inyección de JavaScript malicioso

En el `<head>` de la homepage se detectó un script ofuscado (`__performance_optimizer_v6`) que intenta cargar código remoto desde dominios sospechosos:

- `https://ntdnewts.shop`
- `https://dnsnewts.shop`
- Rutas tipo `/teamrepo?rnd=`

**Impacto:**
- Riesgo de malware, redirecciones, robo de datos de formularios
- Google puede desindexar o marcar el sitio como peligroso
- Daña la confianza de visitantes B2B y de motores de IA que crawlean el HTML

**Acción:** Auditoría completa de WordPress (plugins, theme, usuarios admin, archivos en `wp-content`), limpieza, cambio de credenciales, revisión en Google Search Console y solicitud de revisión si aplica.

---

### 1.2 🔴 Sin indexación visible en Google

Búsqueda `site:aurumintelligence.net` → **0 resultados** (al 10/06/2026).

Posibles causas combinadas:
- Sitio reciente o migrado (schema indica publicación oct 2025)
- Compromiso de seguridad
- Configuración incorrecta post-migración
- Falta de envío/verificación en Google Search Console
- Contenido duplicado/confuso por restos de plantilla

**Acción:** Verificar propiedad en GSC, revisar cobertura, comprobar que no haya `noindex` global, enviar sitemap y solicitar indexación de URLs clave.

---

### 1.3 🔴 Versión en español rota (`/es/`)

- Hreflang declarado: `en` → `/` y `es` → `/es/`
- La URL `/es/` muestra página 404 (“It seems we can't find what you're looking for”)
- Polylang activo (cookie `pll_language`)

**Impacto:** Señal hreflang inválida para Google, mala experiencia para mercado hispanohablante, riesgo de confusión para LLMs.

**Acción:** Crear y publicar todas las páginas ES en Polylang, o eliminar hreflang ES hasta que exista contenido real.

---

### 1.4 🔴 Restos del theme plantilla “Tecnologia”

El sitio usa el theme `tecnologia` (IT managed services) sin haber limpiado el contenido demo:

| Elemento encontrado | Problema |
|---------------------|----------|
| Testimonios homepage | Mencionan “Tecnologia”, “Integris” — no Aurum |
| Footer | Secciones marcadas **“Inactive”**, copy “Simplifying IT for a complex world” |
| Reseñas Clutch | Enlazan a `clutch.co/profile/red-key-solutions` (otra empresa) |
| Secciones genéricas | “Mobile Development”, “Cloud Services”, “Banks & Insurance”, “Non-Profit” |
| llms.txt | Docenas de templates Elementor con branding Tecnologia, emails `suport@tecnologia.com`, teléfono 1-800-356-8933 |

**Impacto:** Confusión de marca, E-E-A-T débil, contenido irrelevante para packaging/AI, contaminación en motores generativos.

**Acción:** Limpieza editorial completa + excluir templates Elementor del índice y de llms.txt.

---

## 2. SEO técnico

### 2.1 Infraestructura

| Check | Resultado |
|-------|-----------|
| HTTPS | ✅ Activo |
| Redirección non-www → www | ✅ 301 correcto |
| Canonical | ✅ Apunta a `https://www.aurumintelligence.net/` |
| robots.txt | ✅ Permite crawl, bloquea `/wp-admin/` |
| Sitemap XML (www) | ✅ Funciona — índice con post, page, category, language |
| Sitemap XML (non-www) | ❌ Error 500 — usar solo www |
| Plugin SEO | All in One SEO 4.9.7.2 |
| Multidioma | Polylang (EN activo, ES roto) |

### 2.2 Sitemap — URLs indexables públicas (~15)

**Páginas principales:**
- `/`, `/company/`, `/services/`, `/solutions/`, `/industries/`, `/partners/`, `/contact/`, `/articles/`, `/jobs/`, `/privacy-policy/`

**Posts / blog:**
- Expanding Wallet Share in Corrugated…
- Part III: AI, Agents, and Modular Automation…
- Exploring AI Technologies for Manufacturing…
- The Power of Data…
- Estimating Consultant – Packaging

**Nota:** Los templates Elementor (`?elementor_library=...`) **no** aparecen en el sitemap XML de páginas (correcto), pero **sí** en `llms.txt` (incorrecto para GEO).

### 2.3 Schema.org (JSON-LD)

Problemas detectados en homepage:

```json
"name": "Aurum Intelligence Aurum Intelligence"  // duplicado
"description": "#taglineAurum Intelligence delivers..."  // placeholder sin limpiar
"og:type": "article"  // debería ser "website" en homepage
```

**Presente:** Organization, WebSite, WebPage, BreadcrumbList  
**Falta:** Service, FAQPage, LocalBusiness (tienen oficinas físicas), Article en posts

### 2.4 Meta tags homepage

| Tag | Valor | Evaluación |
|-----|-------|------------|
| Title | AI for Packaging & Flexography \| Aurum Intelligence (~52 chars) | ✅ Bueno |
| Meta description | AI transformation for packaging converters… (~155 chars) | ✅ Aceptable |
| H1 | Digital transformation for Packaging Converters | 🟡 No alinea 100% con title (flexography vs converters) |
| robots | max-image-preview:large | ✅ OK |

### 2.5 Jerarquía de headings (homepage)

- 1× H1 ✅
- Múltiples H2 (normal en landing)
- Industrias en **H6** — demasiado bajo semánticamente; deberían ser H3/H4
- Saltos de nivel (H2 → H6)

### 2.6 Imágenes

- Logo y la mayoría de imágenes con `alt=""` vacío ❌
- Lazy load activo ✅
- OG image: logo PNG (2394×624) — funcional pero no optimizado para social

### 2.7 URLs rotas / inconsistentes

| URL | Estado |
|-----|--------|
| `/about/` | 404 (noindex) — usar `/company/` |
| `/es/` | 404 funcional |
| `/blog/` | No verificado en sitemap principal — existe `/articles/` |

---

## 3. SEO on-page y contenido

### 3.1 Fortalezas

- **Nicho claro:** packaging converters, AI, demand planning, pre-sales automation
- **Blog de calidad:** artículos largos, técnicos, orientados al buyer B2B
- **Propuesta de valor** bien articulada en Solutions y Company
- **CTA** visible: consulta gratuita, formulario de contacto
- **Partners** del sector packaging (PMMI, etc.)

### 3.2 Debilidades

| Problema | Detalle |
|----------|---------|
| Contenido genérico IT | Secciones heredadas no alineadas al negocio real |
| Testimonios falsos/heredados | Credibilidad perjudicada |
| Meta descriptions truncadas | Posts cortados a mitad de frase en SERP preview |
| Titles de posts muy largos | Ej. “Expanding Wallet Share in Corrugated: From Box Supplier to Growth Partner” (>60 chars) |
| Poca profundidad en landing pages de solución | Solutions es una sola página, no hub con URLs por servicio |
| Job posting indexado | `/estimating-consultant/` mezclado con contenido evergreen |

### 3.3 Keywords / intención (oportunidades)

Términos donde deberían posicionar con contenido dedicado:

- AI for packaging converters
- packaging estimating automation
- demand planning packaging / VMI packaging
- digital transformation flexible packaging
- agentic AI manufacturing packaging
- pre-sales automation corrugated

**Recomendación:** Crear landing pages por solución (`/solutions/pre-sales-automation/`, etc.) con schema `Service`.

---

## 4. Auditoría GEO (Generative Engine Optimization)

GEO = optimización para que LLMs (ChatGPT, Perplexity, Gemini, Claude, etc.) entiendan, citen y recomienden correctamente la marca.

### 4.1 llms.txt — presente pero problemático

- **URL:** https://www.aurumintelligence.net/llms.txt (~30 KB)
- **Generado por:** AIOSEO 4.9.7.2 ✅
- **Contenido útil:** Posts y páginas principales con descripciones ✅

**Problemas graves:**

1. Incluye sección **“My Templates”** con ~40+ URLs internas de Elementor
2. Esas URLs contienen copy de **Tecnologia** (competidor/plantilla IT)
3. Referencia sitemap roto en versión non-www
4. No hay `llms-full.txt`
5. Falta una definición clara de entidad al inicio (“Aurum Intelligence is…”)

**Impacto GEO:** Un LLM que lea llms.txt puede describir a Aurum como empresa de IT managed services de Tecnologia, con teléfonos y emails incorrectos.

### 4.2 Señales positivas para LLMs

- Contenido editorial profundo sobre AI en manufacturing/packaging
- LinkedIn en `sameAs` del schema
- Estructura de artículos con titulares descriptivos
- Datos cuantificables en homepage (25+ años, 100+ proyectos)

### 4.3 Señales negativas para LLMs

- Identidad de marca contradictoria (packaging vs IT genérico)
- Entidad Organization mal configurada en schema
- Sin página “About” canónica clara (`/company/` poco diferenciada)
- Sin FAQ estructurado
- Sin `speakable` ni datos de autor en artículos
- Español roto pese a hreflang

### 4.4 Recomendaciones GEO

| Prioridad | Acción |
|-----------|--------|
| Alta | Excluir templates Elementor de llms.txt en AIOSEO |
| Alta | Añadir bloque de entidad al inicio de llms.txt (quién son, qué hacen, para quién) |
| Alta | Limpiar schema Organization (nombre, descripción, logo, sameAs completo) |
| Media | Crear `/company/` robusta con facts verificables (fundación, equipo, casos) |
| Media | Añadir FAQ schema en Solutions y Contact |
| Media | Autoría visible en artículos (Person schema) |
| Baja | Publicar `llms-full.txt` con contenido extendido curado |

---

## 5. Checklist de prioridades

### Urgente (esta semana)

- [ ] Limpiar malware / inyección JS
- [ ] Cambiar passwords WP, FTP, hosting, DB
- [ ] Verificar sitio en Google Search Console
- [ ] Eliminar o corregir testimonios Tecnologia/Integris
- [ ] Quitar enlaces Clutch de Red Key Solutions
- [ ] Ocultar secciones “Inactive” del footer
- [ ] Reparar o eliminar hreflang ES

### Corto plazo (2–4 semanas)

- [ ] Corregir schema Organization y og:type homepage
- [ ] Completar alt text en imágenes clave
- [ ] Revisar meta descriptions (longitud, sin truncar)
- [ ] Excluir templates Elementor de llms.txt
- [ ] Crear landing pages por solución
- [ ] Unificar navegación: `/company/` vs `/about/`
- [ ] Revisar theme: considerar child theme o rebrand del theme tecnologia

### Medio plazo (1–3 meses)

- [ ] Estrategia de contenido: 2–4 artículos/mes en nicho packaging+AI
- [ ] Link building sectorial (PMMI, Flexible Packaging Association, etc.)
- [ ] Local SEO para oficinas NV y GA (Google Business Profile)
- [ ] Core Web Vitals y optimización de imágenes
- [ ] Versión ES completa si hay mercado objetivo

---

## 6. URLs de referencia

| Recurso | URL |
|---------|-----|
| Homepage | https://www.aurumintelligence.net/ |
| Sitemap | https://www.aurumintelligence.net/sitemap.xml |
| robots.txt | https://www.aurumintelligence.net/robots.txt |
| llms.txt | https://www.aurumintelligence.net/llms.txt |
| Blog / Articles | https://www.aurumintelligence.net/articles/ |
| Contact | https://www.aurumintelligence.net/contact/ |

---

## 7. Notas del auditor

- Auditoría realizada mediante revisión manual del HTML, headers, sitemaps, llms.txt y búsqueda `site:` en Google.
- No se ejecutó Lighthouse en este entorno; recomendable complementar con PageSpeed Insights y Screaming Frog.
- El sitemap en `aurumintelligence.net` (sin www) devolvió 500 en una comprobación; el canonical correcto es **www**.
- La fecha de publicación del schema (oct 2025) sugiere sitio relativamente nuevo o migración reciente.

---

*Próxima revisión recomendada: 30 días después de aplicar correcciones críticas.*
